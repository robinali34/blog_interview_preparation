---
layout: post
title: "Zscaler Interview Preparation - Multi-Threading & Multi-Processing in C/C++"
date: 2026-01-06 12:00:00 -0000
categories: interview-preparation zscaler multithreading multiprocessing c-cpp concurrency
tags: zscaler multithreading multiprocessing c-cpp pthreads mutex semaphore process communication
excerpt: "Comprehensive guide to multi-threading and multi-processing in C/C++ for Zscaler interviews covering threads, processes, synchronization, IPC, and common design patterns."
---

# Zscaler Interview Preparation - Multi-Threading & Multi-Processing in C/C++

A comprehensive guide to multi-threading and multi-processing in C/C++ for Zscaler technical interviews, covering thread management, process creation, synchronization primitives, inter-process communication, and common design patterns.

## 1. What is Multi-Threading and Multi-Processing?

### Multi-Threading

**Multi-threading** is the ability of a CPU (or a single core) to execute multiple threads concurrently within a single process.

**Key Characteristics:**
- **Shared Memory**: All threads share the same address space
- **Lightweight**: Threads are cheaper to create than processes
- **Fast Communication**: Shared memory enables fast data sharing
- **Synchronization Needed**: Requires mutexes, semaphores, etc.

**Use Cases:**
- I/O-bound operations (network, file I/O)
- Parallel computation
- Responsive user interfaces
- Server handling multiple clients

### Multi-Processing

**Multi-processing** is the use of multiple processes to execute tasks concurrently.

**Key Characteristics:**
- **Separate Memory**: Each process has its own address space
- **Heavyweight**: Processes are more expensive to create
- **Isolation**: Process failures don't affect others
- **IPC Required**: Inter-process communication needed

**Use Cases:**
- Fault isolation
- Security boundaries
- Distributed systems
- Independent tasks

### Comparison

| Feature | Threads | Processes |
|---------|---------|-----------|
| **Memory** | Shared address space | Separate address space |
| **Creation Cost** | Low | High |
| **Communication** | Shared memory (fast) | IPC (slower) |
| **Fault Isolation** | Weak (one thread can crash process) | Strong (isolated) |
| **Synchronization** | Mutex, semaphore, condition variable | IPC mechanisms |
| **Context Switch** | Fast | Slower |

---

## 2. Multi-Threading in C/C++

### POSIX Threads (pthreads)

**pthreads** is the POSIX standard for thread management in C/C++.

#### Basic Thread Operations

**Creating a Thread:**
```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

void* thread_function(void* arg) {
    int thread_id = *(int*)arg;
    printf("Thread %d is running\n", thread_id);
    return NULL;
}

int main() {
    pthread_t thread1, thread2;
    int id1 = 1, id2 = 2;
    
    // Create threads
    pthread_create(&thread1, NULL, thread_function, &id1);
    pthread_create(&thread2, NULL, thread_function, &id2);
    
    // Wait for threads to complete
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);
    
    printf("All threads completed\n");
    return 0;
}
```

**Thread Attributes:**
```c
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_JOINABLE);
pthread_attr_setstacksize(&attr, 1024 * 1024);  // 1MB stack

pthread_t thread;
pthread_create(&thread, &attr, thread_function, NULL);

pthread_attr_destroy(&attr);
```

#### C++11 Threads (std::thread)

**Modern C++ threading:**
```cpp
#include <thread>
#include <iostream>
#include <vector>

void thread_function(int id) {
    std::cout << "Thread " << id << " is running\n";
}

int main() {
    std::vector<std::thread> threads;
    
    // Create threads
    for (int i = 0; i < 5; i++) {
        threads.emplace_back(thread_function, i);
    }
    
    // Wait for all threads
    for (auto& t : threads) {
        t.join();
    }
    
    std::cout << "All threads completed\n";
    return 0;
}
```

### Thread Synchronization

#### Mutex (Mutual Exclusion)

**Purpose**: Protect shared resources from concurrent access.

**pthread_mutex:**
```c
#include <pthread.h>

int counter = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void* increment(void* arg) {
    for (int i = 0; i < 100000; i++) {
        pthread_mutex_lock(&mutex);
        counter++;
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    
    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    
    printf("Counter: %d\n", counter);  // Should be 200000
    
    pthread_mutex_destroy(&mutex);
    return 0;
}
```

**C++11 std::mutex:**
```cpp
#include <thread>
#include <mutex>
#include <iostream>

int counter = 0;
std::mutex mtx;

void increment() {
    for (int i = 0; i < 100000; i++) {
        std::lock_guard<std::mutex> lock(mtx);
        counter++;
    }
}

int main() {
    std::thread t1(increment);
    std::thread t2(increment);
    
    t1.join();
    t2.join();
    
    std::cout << "Counter: " << counter << std::endl;  // Should be 200000
    return 0;
}
```

**Mutex Types:**
- **Normal Mutex**: Basic mutual exclusion
- **Recursive Mutex**: Same thread can lock multiple times
- **Read-Write Lock**: Multiple readers or one writer
- **Timed Mutex**: Lock with timeout

#### Semaphore

**Purpose**: Control access to a resource with a counter.

**POSIX Semaphore:**
```c
#include <semaphore.h>
#include <pthread.h>

#define BUFFER_SIZE 10

int buffer[BUFFER_SIZE];
int in = 0, out = 0;
sem_t empty, full, mutex;

void* producer(void* arg) {
    for (int i = 0; i < 20; i++) {
        sem_wait(&empty);  // Wait for empty slot
        sem_wait(&mutex);  // Lock buffer
        
        buffer[in] = i;
        in = (in + 1) % BUFFER_SIZE;
        
        sem_post(&mutex);  // Unlock buffer
        sem_post(&full);   // Signal item available
    }
    return NULL;
}

void* consumer(void* arg) {
    for (int i = 0; i < 20; i++) {
        sem_wait(&full);   // Wait for item
        sem_wait(&mutex);  // Lock buffer
        
        int item = buffer[out];
        out = (out + 1) % BUFFER_SIZE;
        
        sem_post(&mutex);  // Unlock buffer
        sem_post(&empty);  // Signal empty slot
        
        printf("Consumed: %d\n", item);
    }
    return NULL;
}

int main() {
    sem_init(&empty, 0, BUFFER_SIZE);
    sem_init(&full, 0, 0);
    sem_init(&mutex, 0, 1);
    
    pthread_t prod, cons;
    pthread_create(&prod, NULL, producer, NULL);
    pthread_create(&cons, NULL, consumer, NULL);
    
    pthread_join(prod, NULL);
    pthread_join(cons, NULL);
    
    sem_destroy(&empty);
    sem_destroy(&full);
    sem_destroy(&mutex);
    
    return 0;
}
```

#### Condition Variables

**Purpose**: Allow threads to wait for conditions and notify when conditions change.

**pthread_cond:**
```c
#include <pthread.h>

int count = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t condition = PTHREAD_COND_INITIALIZER;

void* consumer(void* arg) {
    pthread_mutex_lock(&mutex);
    
    while (count == 0) {
        pthread_cond_wait(&condition, &mutex);
    }
    
    count--;
    printf("Consumed, count: %d\n", count);
    
    pthread_mutex_unlock(&mutex);
    return NULL;
}

void* producer(void* arg) {
    pthread_mutex_lock(&mutex);
    
    count++;
    printf("Produced, count: %d\n", count);
    
    pthread_cond_signal(&condition);
    
    pthread_mutex_unlock(&mutex);
    return NULL;
}
```

**C++11 std::condition_variable:**
```cpp
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>

std::queue<int> queue;
std::mutex mtx;
std::condition_variable cv;

void producer() {
    for (int i = 0; i < 10; i++) {
        std::unique_lock<std::mutex> lock(mtx);
        queue.push(i);
        cv.notify_one();
    }
}

void consumer() {
    while (true) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, []{ return !queue.empty(); });
        
        int item = queue.front();
        queue.pop();
        lock.unlock();
        
        if (item == 9) break;  // Exit condition
        std::cout << "Consumed: " << item << std::endl;
    }
}
```

#### Read-Write Locks

**Purpose**: Allow multiple readers or one writer.

```c
#include <pthread.h>

pthread_rwlock_t rwlock = PTHREAD_RWLOCK_INITIALIZER;
int shared_data = 0;

void* reader(void* arg) {
    pthread_rwlock_rdlock(&rwlock);
    printf("Reader: %d\n", shared_data);
    pthread_rwlock_unlock(&rwlock);
    return NULL;
}

void* writer(void* arg) {
    pthread_rwlock_wrlock(&rwlock);
    shared_data++;
    printf("Writer: %d\n", shared_data);
    pthread_rwlock_unlock(&rwlock);
    return NULL;
}
```

**C++17 shared_mutex:**
```cpp
#include <shared_mutex>
#include <thread>

std::shared_mutex rw_mutex;
int shared_data = 0;

void reader() {
    std::shared_lock<std::shared_mutex> lock(rw_mutex);
    std::cout << "Reader: " << shared_data << std::endl;
}

void writer() {
    std::unique_lock<std::shared_mutex> lock(rw_mutex);
    shared_data++;
    std::cout << "Writer: " << shared_data << std::endl;
}
```

### Thread Safety Patterns

#### Thread-Safe Singleton

```cpp
#include <mutex>

class Singleton {
private:
    static Singleton* instance;
    static std::mutex mtx;
    
    Singleton() {}  // Private constructor
    
public:
    static Singleton* getInstance() {
        if (instance == nullptr) {
            std::lock_guard<std::mutex> lock(mtx);
            if (instance == nullptr) {  // Double-check
                instance = new Singleton();
            }
        }
        return instance;
    }
    
    // Delete copy constructor and assignment
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};

Singleton* Singleton::instance = nullptr;
std::mutex Singleton::mtex;
```

#### Thread Pool

```cpp
#include <thread>
#include <queue>
#include <vector>
#include <functional>
#include <mutex>
#include <condition_variable>

class ThreadPool {
private:
    std::vector<std::thread> workers;
    std::queue<std::function<void()>> tasks;
    std::mutex queue_mutex;
    std::condition_variable condition;
    bool stop;
    
public:
    ThreadPool(size_t num_threads) : stop(false) {
        for (size_t i = 0; i < num_threads; i++) {
            workers.emplace_back([this] {
                while (true) {
                    std::function<void()> task;
                    {
                        std::unique_lock<std::mutex> lock(queue_mutex);
                        condition.wait(lock, [this] { 
                            return stop || !tasks.empty(); 
                        });
                        
                        if (stop && tasks.empty()) {
                            return;
                        }
                        
                        task = tasks.front();
                        tasks.pop();
                    }
                    task();
                }
            });
        }
    }
    
    template<class F>
    void enqueue(F&& f) {
        {
            std::unique_lock<std::mutex> lock(queue_mutex);
            tasks.emplace(std::forward<F>(f));
        }
        condition.notify_one();
    }
    
    ~ThreadPool() {
        {
            std::unique_lock<std::mutex> lock(queue_mutex);
            stop = true;
        }
        condition.notify_all();
        for (std::thread& worker : workers) {
            worker.join();
        }
    }
};
```

---

## 3. Multi-Processing in C/C++

### Process Creation

#### fork() System Call

**Purpose**: Create a new process by duplicating the calling process.

```c
#include <unistd.h>
#include <sys/wait.h>
#include <stdio.h>

int main() {
    pid_t pid = fork();
    
    if (pid < 0) {
        perror("fork failed");
        return 1;
    } else if (pid == 0) {
        // Child process
        printf("Child process (PID: %d)\n", getpid());
        exit(0);
    } else {
        // Parent process
        printf("Parent process (PID: %d), Child PID: %d\n", 
               getpid(), pid);
        wait(NULL);  // Wait for child to complete
    }
    
    return 0;
}
```

#### exec() Family

**Purpose**: Replace current process with a new program.

```c
#include <unistd.h>

int main() {
    pid_t pid = fork();
    
    if (pid == 0) {
        // Child process - execute new program
        execl("/bin/ls", "ls", "-l", NULL);
        perror("execl failed");
        exit(1);
    } else {
        wait(NULL);
    }
    
    return 0;
}
```

**exec() Variants:**
- `execl()`: List of arguments
- `execv()`: Array of arguments
- `execle()`: With environment
- `execvp()`: Search PATH

### Inter-Process Communication (IPC)

#### Pipes

**Purpose**: Unidirectional communication between related processes.

**Unnamed Pipes:**
```c
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main() {
    int pipefd[2];
    char buffer[100];
    
    if (pipe(pipefd) == -1) {
        perror("pipe failed");
        return 1;
    }
    
    pid_t pid = fork();
    
    if (pid == 0) {
        // Child: Write to pipe
        close(pipefd[0]);  // Close read end
        const char* msg = "Hello from child!";
        write(pipefd[1], msg, strlen(msg) + 1);
        close(pipefd[1]);
    } else {
        // Parent: Read from pipe
        close(pipefd[1]);  // Close write end
        read(pipefd[0], buffer, sizeof(buffer));
        printf("Parent received: %s\n", buffer);
        close(pipefd[0]);
        wait(NULL);
    }
    
    return 0;
}
```

**Named Pipes (FIFO):**
```c
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

// Create FIFO
mkfifo("/tmp/myfifo", 0666);

// Writer
int fd = open("/tmp/myfifo", O_WRONLY);
write(fd, "Hello", 5);
close(fd);

// Reader
int fd = open("/tmp/myfifo", O_RDONLY);
char buffer[100];
read(fd, buffer, sizeof(buffer));
close(fd);
```

#### Shared Memory

**Purpose**: Fast communication via shared memory segment.

**POSIX Shared Memory:**
```c
#include <sys/mman.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

#define SHM_NAME "/my_shm"
#define SHM_SIZE 4096

// Writer process
int main() {
    // Create shared memory
    int shm_fd = shm_open(SHM_NAME, O_CREAT | O_RDWR, 0666);
    ftruncate(shm_fd, SHM_SIZE);
    
    // Map to address space
    void* ptr = mmap(0, SHM_SIZE, PROT_WRITE, MAP_SHARED, shm_fd, 0);
    
    // Write data
    const char* msg = "Hello from shared memory!";
    memcpy(ptr, msg, strlen(msg) + 1);
    
    // Cleanup
    munmap(ptr, SHM_SIZE);
    close(shm_fd);
    
    return 0;
}

// Reader process
int main() {
    // Open shared memory
    int shm_fd = shm_open(SHM_NAME, O_RDONLY, 0666);
    
    // Map to address space
    void* ptr = mmap(0, SHM_SIZE, PROT_READ, MAP_SHARED, shm_fd, 0);
    
    // Read data
    printf("Read: %s\n", (char*)ptr);
    
    // Cleanup
    munmap(ptr, SHM_SIZE);
    close(shm_fd);
    shm_unlink(SHM_NAME);
    
    return 0;
}
```

#### Message Queues

**Purpose**: Structured message passing between processes.

**POSIX Message Queue:**
```c
#include <mqueue.h>
#include <stdio.h>
#include <string.h>

#define QUEUE_NAME "/my_queue"
#define MSG_SIZE 100

// Sender
int main() {
    mqd_t mq = mq_open(QUEUE_NAME, O_CREAT | O_WRONLY, 0666, NULL);
    
    const char* msg = "Hello from message queue!";
    mq_send(mq, msg, strlen(msg) + 1, 0);
    
    mq_close(mq);
    return 0;
}

// Receiver
int main() {
    mqd_t mq = mq_open(QUEUE_NAME, O_RDONLY);
    
    char buffer[MSG_SIZE];
    mq_receive(mq, buffer, MSG_SIZE, NULL);
    
    printf("Received: %s\n", buffer);
    
    mq_close(mq);
    mq_unlink(QUEUE_NAME);
    return 0;
}
```

#### Sockets

**Purpose**: Network communication (can also be used for local IPC).

**Unix Domain Sockets:**
```c
#include <sys/socket.h>
#include <sys/un.h>
#include <unistd.h>
#include <stdio.h>

#define SOCKET_PATH "/tmp/my_socket"

// Server
int main() {
    int server_fd = socket(AF_UNIX, SOCK_STREAM, 0);
    
    struct sockaddr_un addr;
    memset(&addr, 0, sizeof(addr));
    addr.sun_family = AF_UNIX;
    strcpy(addr.sun_path, SOCKET_PATH);
    
    unlink(SOCKET_PATH);  // Remove if exists
    bind(server_fd, (struct sockaddr*)&addr, sizeof(addr));
    listen(server_fd, 5);
    
    int client_fd = accept(server_fd, NULL, NULL);
    char buffer[100];
    read(client_fd, buffer, sizeof(buffer));
    printf("Server received: %s\n", buffer);
    
    close(client_fd);
    close(server_fd);
    unlink(SOCKET_PATH);
    return 0;
}

// Client
int main() {
    int sock_fd = socket(AF_UNIX, SOCK_STREAM, 0);
    
    struct sockaddr_un addr;
    memset(&addr, 0, sizeof(addr));
    addr.sun_family = AF_UNIX;
    strcpy(addr.sun_path, SOCKET_PATH);
    
    connect(sock_fd, (struct sockaddr*)&addr, sizeof(addr));
    write(sock_fd, "Hello from client!", 19);
    
    close(sock_fd);
    return 0;
}
```

---

## 4. Shared Resources and Memory

### Shared Memory in Multi-Threading

**Threads automatically share memory** within the same process. All threads in a process share:
- Global variables
- Static variables
- Heap memory (dynamically allocated)
- File descriptors
- Process address space

**Example - Shared Global Variable:**
```c
#include <pthread.h>
#include <stdio.h>

// Shared global variable
int shared_counter = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void* thread_function(void* arg) {
    for (int i = 0; i < 100000; i++) {
        pthread_mutex_lock(&mutex);
        shared_counter++;  // All threads access same variable
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    pthread_create(&t1, NULL, thread_function, NULL);
    pthread_create(&t2, NULL, thread_function, NULL);
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    
    printf("Shared counter: %d\n", shared_counter);
    return 0;
}
```

**Example - Shared Heap Memory:**
```cpp
#include <thread>
#include <vector>
#include <mutex>
#include <iostream>

class SharedBuffer {
private:
    std::vector<int> buffer;  // Shared heap memory
    std::mutex mtx;
    
public:
    void add(int value) {
        std::lock_guard<std::mutex> lock(mtx);
        buffer.push_back(value);
    }
    
    int get(size_t index) {
        std::lock_guard<std::mutex> lock(mtx);
        return buffer[index];
    }
    
    size_t size() {
        std::lock_guard<std::mutex> lock(mtx);
        return buffer.size();
    }
};

SharedBuffer shared_buffer;  // Shared by all threads

void producer() {
    for (int i = 0; i < 10; i++) {
        shared_buffer.add(i);
    }
}

void consumer() {
    for (size_t i = 0; i < shared_buffer.size(); i++) {
        std::cout << shared_buffer.get(i) << " ";
    }
}

int main() {
    std::thread t1(producer);
    std::thread t2(consumer);
    
    t1.join();
    t2.join();
    
    return 0;
}
```

### Shared Memory in Multi-Processing

**Processes have separate memory spaces**, so shared memory must be explicitly created using IPC mechanisms.

**Memory Layout:**
```
Process 1                    Process 2
┌─────────────────┐        ┌─────────────────┐
│ Stack           │        │ Stack           │
│ Heap            │        │ Heap            │
│ Data            │        │ Data            │
│ Code            │        │ Code            │
└─────────────────┘        └─────────────────┘
         │                         │
         └─────────┬─────────────────┘
                  │
         ┌────────▼────────┐
         │ Shared Memory   │
         │ Segment         │
         └─────────────────┘
```

### Memory Visibility and Consistency

**Problem**: Without proper synchronization, changes made by one thread may not be visible to other threads immediately.

**Memory Visibility Issues:**
```cpp
// Thread 1
bool flag = false;
int data = 0;

// Thread 2
void writer() {
    data = 42;        // Write data
    flag = true;      // Signal ready
}

// Thread 1
void reader() {
    while (!flag) {   // Wait for flag
        // May never see flag = true due to CPU cache!
    }
    int value = data; // May read stale value!
}
```

**Solution - Memory Barriers:**
```cpp
#include <atomic>
#include <thread>

std::atomic<bool> flag(false);
std::atomic<int> data(0);

void writer() {
    data.store(42, std::memory_order_relaxed);
    flag.store(true, std::memory_order_release);  // Release barrier
}

void reader() {
    while (!flag.load(std::memory_order_acquire)) {  // Acquire barrier
        // Spin wait
    }
    int value = data.load(std::memory_order_relaxed);
    // Guaranteed to see data = 42
}
```

### Memory Ordering

**C++ Memory Order Semantics:**

1. **memory_order_relaxed**: No ordering guarantees
   ```cpp
   std::atomic<int> x(0);
   x.store(1, std::memory_order_relaxed);  // No ordering
   ```

2. **memory_order_acquire**: Acquire semantics (read barrier)
   ```cpp
   int value = x.load(std::memory_order_acquire);
   // All writes before this are visible
   ```

3. **memory_order_release**: Release semantics (write barrier)
   ```cpp
   x.store(1, std::memory_order_release);
   // All writes before this are visible to acquire
   ```

4. **memory_order_seq_cst**: Sequential consistency (default)
   ```cpp
   x.store(1, std::memory_order_seq_cst);  // Strongest ordering
   ```

**Example - Producer-Consumer with Memory Ordering:**
```cpp
#include <atomic>
#include <thread>

std::atomic<int> data(0);
std::atomic<bool> ready(false);

void producer() {
    data.store(42, std::memory_order_relaxed);
    ready.store(true, std::memory_order_release);  // Release: make data visible
}

void consumer() {
    while (!ready.load(std::memory_order_acquire)) {  // Acquire: see all writes
        // Wait
    }
    int value = data.load(std::memory_order_relaxed);
    // Guaranteed to see data = 42
}
```

### Cache Coherency

**Problem**: Each CPU core has its own cache. Without proper synchronization, threads on different cores may see different values.

**Cache Coherency Mechanisms:**
- **MESI Protocol**: Modified, Exclusive, Shared, Invalid
- **Memory Barriers**: Ensure cache synchronization
- **Volatile**: Prevents compiler optimizations (not sufficient for thread safety)

**Example - Cache Coherency Issue:**
```c
// Without synchronization - may have cache coherency issues
int shared_var = 0;

void thread1() {
    shared_var = 1;  // May only update local cache
}

void thread2() {
    int value = shared_var;  // May read from local cache (stale)
}
```

**Solution - Proper Synchronization:**
```c
int shared_var = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void thread1() {
    pthread_mutex_lock(&mutex);
    shared_var = 1;  // Mutex ensures cache coherency
    pthread_mutex_unlock(&mutex);
}

void thread2() {
    pthread_mutex_lock(&mutex);
    int value = shared_var;  // Guaranteed to see updated value
    pthread_mutex_unlock(&mutex);
}
```

### Shared Memory Patterns

#### 1. Shared Counter with Atomic Operations

```cpp
#include <atomic>
#include <thread>
#include <vector>

std::atomic<int> counter(0);

void increment() {
    for (int i = 0; i < 100000; i++) {
        counter.fetch_add(1, std::memory_order_relaxed);
    }
}

int main() {
    std::vector<std::thread> threads;
    for (int i = 0; i < 4; i++) {
        threads.emplace_back(increment);
    }
    
    for (auto& t : threads) {
        t.join();
    }
    
    std::cout << "Counter: " << counter.load() << std::endl;
    return 0;
}
```

#### 2. Shared Data Structure with Mutex

```cpp
#include <map>
#include <mutex>
#include <string>

class ThreadSafeMap {
private:
    std::map<std::string, int> data;
    mutable std::shared_mutex mtx;
    
public:
    void insert(const std::string& key, int value) {
        std::unique_lock<std::shared_mutex> lock(mtx);
        data[key] = value;
    }
    
    int get(const std::string& key) const {
        std::shared_lock<std::shared_mutex> lock(mtx);
        auto it = data.find(key);
        return (it != data.end()) ? it->second : -1;
    }
    
    size_t size() const {
        std::shared_lock<std::shared_mutex> lock(mtx);
        return data.size();
    }
};
```

#### 3. Shared Memory Pool

```cpp
#include <vector>
#include <mutex>
#include <memory>

template<typename T>
class MemoryPool {
private:
    std::vector<std::unique_ptr<T>> pool;
    std::vector<bool> in_use;
    std::mutex mtx;
    size_t pool_size;
    
public:
    MemoryPool(size_t size) : pool_size(size), in_use(size, false) {
        for (size_t i = 0; i < size; i++) {
            pool.push_back(std::make_unique<T>());
        }
    }
    
    T* acquire() {
        std::lock_guard<std::mutex> lock(mtx);
        for (size_t i = 0; i < pool_size; i++) {
            if (!in_use[i]) {
                in_use[i] = true;
                return pool[i].get();
            }
        }
        return nullptr;  // Pool exhausted
    }
    
    void release(T* ptr) {
        std::lock_guard<std::mutex> lock(mtx);
        for (size_t i = 0; i < pool_size; i++) {
            if (pool[i].get() == ptr) {
                in_use[i] = false;
                return;
            }
        }
    }
};
```

### Shared Memory Best Practices

**1. Always Synchronize:**
- Use mutexes for shared data
- Use atomic operations for simple counters
- Use memory barriers for visibility

**2. Minimize Shared State:**
- Reduce amount of shared data
- Use thread-local storage when possible
- Prefer message passing over shared memory

**3. Lock Granularity:**
- Fine-grained locks: Better concurrency, more complex
- Coarse-grained locks: Simpler, less concurrency

**4. Avoid False Sharing:**
```cpp
// Bad: False sharing (same cache line)
struct Data {
    int counter1;  // Thread 1 modifies
    int counter2;  // Thread 2 modifies
    // Both on same cache line - causes cache invalidation
};

// Good: Padding to avoid false sharing
struct Data {
    alignas(64) int counter1;  // Align to cache line (64 bytes)
    alignas(64) int counter2;  // Separate cache lines
};
```

**5. Use Thread-Local Storage:**
```cpp
#include <thread>

thread_local int thread_id = 0;  // Each thread has its own copy

void thread_function(int id) {
    thread_id = id;  // Only this thread sees this value
    // No synchronization needed
}
```

### Practical Example: Concurrent HTTP Requests with shared_ptr and Lambda

**Use Case**: Send multiple HTTP requests (GET, POST with different headers) concurrently using shared_ptr for shared resources and lambda functions for different request types.

```cpp
#include <memory>
#include <thread>
#include <vector>
#include <string>
#include <map>
#include <functional>
#include <iostream>
#include <curl/curl.h>  // Using libcurl for HTTP

// HTTP Response structure
struct HttpResponse {
    int status_code;
    std::string body;
    std::map<std::string, std::string> headers;
};

// Shared HTTP client configuration
class HttpClient {
private:
    CURL* curl;
    std::mutex curl_mutex;  // Protect curl handle (not thread-safe)
    
public:
    HttpClient() {
        curl = curl_easy_init();
        curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L);
        curl_easy_setopt(curl, CURLOPT_TIMEOUT, 30L);
    }
    
    ~HttpClient() {
        curl_easy_cleanup(curl);
    }
    
    // Thread-safe HTTP request
    HttpResponse request(const std::string& url, 
                        const std::string& method,
                        const std::map<std::string, std::string>& headers,
                        const std::string& body = "") {
        std::lock_guard<std::mutex> lock(curl_mutex);
        
        HttpResponse response;
        std::string response_body;
        
        curl_easy_setopt(curl, CURLOPT_URL, url.c_str());
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, writeCallback);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, &response_body);
        
        // Set method
        if (method == "POST") {
            curl_easy_setopt(curl, CURLOPT_POST, 1L);
            curl_easy_setopt(curl, CURLOPT_POSTFIELDS, body.c_str());
        } else if (method == "GET") {
            curl_easy_setopt(curl, CURLOPT_HTTPGET, 1L);
        }
        
        // Set headers
        struct curl_slist* header_list = nullptr;
        for (const auto& [key, value] : headers) {
            std::string header = key + ": " + value;
            header_list = curl_slist_append(header_list, header.c_str());
        }
        curl_easy_setopt(curl, CURLOPT_HTTPHEADER, header_list);
        
        // Perform request
        CURLcode res = curl_easy_perform(curl);
        
        if (res == CURLE_OK) {
            curl_easy_getinfo(curl, CURLINFO_RESPONSE_CODE, &response.status_code);
            response.body = response_body;
        }
        
        curl_slist_free_all(header_list);
        return response;
    }
    
private:
    static size_t writeCallback(void* contents, size_t size, size_t nmemb, void* userp) {
        ((std::string*)userp)->append((char*)contents, size * nmemb);
        return size * nmemb;
    }
};

// Request configuration
struct RequestConfig {
    std::string url;
    std::string method;
    std::map<std::string, std::string> headers;
    std::string body;
};

int main() {
    // Create shared HTTP client (shared_ptr for automatic memory management)
    auto http_client = std::make_shared<HttpClient>();
    
    // Define different request configurations
    std::vector<RequestConfig> requests = {
        {
            "https://api.example.com/users",
            "GET",
            { {"Accept", "application/json"} },
            ""
        },
        {
            "https://api.example.com/users",
            "POST",
            {
                {"Content-Type", "application/json"},
                {"Authorization", "Bearer token123"}
            },
            R"({"name": "John", "email": "john@example.com"})"
        },
        {
            "https://api.example.com/users",
            "POST",
            {
                {"Content-Type", "application/json"},
                {"Authorization", "Bearer token456"},
                {"X-Custom-Header", "custom-value"}
            },
            R"({"name": "Jane", "email": "jane@example.com"})"
        },
        {
            "https://api.example.com/data",
            "GET",
            { {"Accept", "application/json"}, {"X-Request-ID", "req-123"} },
            ""
        }
    };
    
    // Vector to store thread objects and results
    std::vector<std::thread> threads;
    std::vector<std::shared_ptr<HttpResponse>> responses(requests.size());
    std::mutex results_mutex;  // Protect results vector
    
    // Launch concurrent requests using lambda functions
    for (size_t i = 0; i < requests.size(); i++) {
        // Capture shared_ptr by value (safe for concurrent access)
        // Lambda captures: http_client (shared_ptr), request config, response storage
        threads.emplace_back([http_client, &requests, i, &responses, &results_mutex]() {
            const auto& req = requests[i];
            
            // Execute HTTP request
            auto response = std::make_shared<HttpResponse>(
                http_client->request(req.url, req.method, req.headers, req.body)
            );
            
            // Store result (thread-safe)
            {
                std::lock_guard<std::mutex> lock(results_mutex);
                responses[i] = response;
            }
            
            // Print result
            std::cout << "Request " << i << " (" << req.method << " " << req.url 
                      << ") completed with status: " << response->status_code << std::endl;
        });
    }
    
    // Wait for all threads to complete
    for (auto& thread : threads) {
        thread.join();
    }
    
    // Process results
    std::cout << "\nAll requests completed:\n";
    for (size_t i = 0; i < responses.size(); i++) {
        if (responses[i]) {
            std::cout << "Response " << i << ": Status " << responses[i]->status_code 
                      << ", Body length: " << responses[i]->body.length() << std::endl;
        }
    }
    
    return 0;
}
```

**Key Concepts:**

1. **shared_ptr for Shared Resource:**
   - `http_client` is shared across all threads
   - Automatic memory management
   - Thread-safe reference counting

2. **Lambda Functions for Different Operations:**
   - Each lambda captures the request configuration
   - Different headers and methods per request
   - Concurrent execution

3. **Thread Safety:**
   - Mutex protects curl handle (not thread-safe)
   - Mutex protects results vector
   - shared_ptr reference counting is thread-safe

**Alternative: Using std::function for Request Handlers**

```cpp
#include <functional>
#include <memory>
#include <thread>
#include <vector>

// Request handler type
using RequestHandler = std::function<std::shared_ptr<HttpResponse>(
    std::shared_ptr<HttpClient> client
)>;

int main() {
    auto http_client = std::make_shared<HttpClient>();
    
    // Define different request handlers using lambdas
    std::vector<RequestHandler> handlers = {
        // GET request
        [](std::shared_ptr<HttpClient> client) {
            std::map<std::string, std::string> headers = {
                {"Accept", "application/json"}
            };
            return std::make_shared<HttpResponse>(
                client->request("https://api.example.com/users", "GET", headers)
            );
        },
        
        // POST request with JSON
        [](std::shared_ptr<HttpClient> client) {
            std::map<std::string, std::string> headers = {
                {"Content-Type", "application/json"},
                {"Authorization", "Bearer token123"}
            };
            std::string body = R"({"name": "John", "email": "john@example.com"})";
            return std::make_shared<HttpResponse>(
                client->request("https://api.example.com/users", "POST", headers, body)
            );
        },
        
        // POST request with different headers
        [](std::shared_ptr<HttpClient> client) {
            std::map<std::string, std::string> headers = {
                {"Content-Type", "application/json"},
                {"Authorization", "Bearer token456"},
                {"X-Custom-Header", "custom-value"},
                {"X-Request-ID", "req-789"}
            };
            std::string body = R"({"name": "Jane", "email": "jane@example.com"})";
            return std::make_shared<HttpResponse>(
                client->request("https://api.example.com/users", "POST", headers, body)
            );
        },
        
        // GET request with custom header
        [](std::shared_ptr<HttpClient> client) {
            std::map<std::string, std::string> headers = {
                {"Accept", "application/json"},
                {"X-Request-ID", "req-123"},
                {"X-Client-Version", "1.0"}
            };
            return std::make_shared<HttpResponse>(
                client->request("https://api.example.com/data", "GET", headers)
            );
        }
    };
    
    // Execute all handlers concurrently
    std::vector<std::thread> threads;
    std::vector<std::shared_ptr<HttpResponse>> results(handlers.size());
    
    for (size_t i = 0; i < handlers.size(); i++) {
        threads.emplace_back([&handlers, &results, http_client, i]() {
            results[i] = handlers[i](http_client);
            std::cout << "Handler " << i << " completed, status: " 
                      << results[i]->status_code << std::endl;
        });
    }
    
    for (auto& thread : threads) {
        thread.join();
    }
    
    return 0;
}
```

**Benefits of This Approach:**

1. **shared_ptr Benefits:**
   - Automatic memory management
   - Thread-safe reference counting
   - Shared ownership across threads
   - No manual cleanup needed

2. **Lambda Benefits:**
   - Encapsulate different request configurations
   - Easy to add new request types
   - Type-safe function objects
   - Can capture context

3. **Concurrency Benefits:**
   - All requests execute in parallel
   - Faster overall execution
   - Efficient resource usage
   - Scalable design

### Shared Memory Debugging

**Common Issues:**
1. **Race Conditions**: Concurrent access without synchronization
2. **Data Races**: Unsynchronized access to shared memory
3. **Deadlocks**: Circular waiting on locks
4. **False Sharing**: Unnecessary cache invalidation

**Detection Tools:**
```bash
# Thread Sanitizer (TSan)
g++ -fsanitize=thread -g program.cpp -o program
./program

# Valgrind Helgrind
valgrind --tool=helgrind ./program

# Valgrind DRD
valgrind --tool=drd ./program
```

---

## 5. Synchronization Primitives

### Mutex Types

**1. Normal Mutex:**
```c
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```

**2. Recursive Mutex:**
```c
pthread_mutexattr_t attr;
pthread_mutexattr_init(&attr);
pthread_mutexattr_settype(&attr, PTHREAD_MUTEX_RECURSIVE);
pthread_mutex_t mutex;
pthread_mutex_init(&mutex, &attr);
```

**3. Error-Checking Mutex:**
```c
pthread_mutexattr_settype(&attr, PTHREAD_MUTEX_ERRORCHECK);
```

### Deadlock Prevention

**Deadlock Conditions:**
1. Mutual exclusion
2. Hold and wait
3. No preemption
4. Circular wait

**Prevention Strategies:**
- **Lock Ordering**: Always acquire locks in same order
- **Timeout**: Use timed mutex locks
- **Lock-Free**: Use atomic operations
- **Deadlock Detection**: Detect and recover

**Example - Lock Ordering:**
```c
// Always lock in same order: mutex1, then mutex2
pthread_mutex_lock(&mutex1);
pthread_mutex_lock(&mutex2);
// ... critical section ...
pthread_mutex_unlock(&mutex2);
pthread_mutex_unlock(&mutex1);
```

### Atomic Operations

**Purpose**: Lock-free operations for simple updates.

**C11 atomics:**
```c
#include <stdatomic.h>

atomic_int counter = ATOMIC_VAR_INIT(0);

void increment() {
    atomic_fetch_add(&counter, 1);
}

int get_value() {
    return atomic_load(&counter);
}
```

**C++11 atomics:**
```cpp
#include <atomic>

std::atomic<int> counter(0);

void increment() {
    counter.fetch_add(1);
}

int get_value() {
    return counter.load();
}
```

---

## 6. Common Design Patterns

### Producer-Consumer Pattern

**Using Condition Variables:**
```cpp
#include <queue>
#include <thread>
#include <mutex>
#include <condition_variable>

template<typename T>
class ProducerConsumer {
private:
    std::queue<T> queue;
    std::mutex mtx;
    std::condition_variable cv;
    size_t max_size;
    bool done;
    
public:
    ProducerConsumer(size_t max) : max_size(max), done(false) {}
    
    void produce(T item) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [this] { return queue.size() < max_size; });
        queue.push(item);
        cv.notify_one();
    }
    
    T consume() {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [this] { return !queue.empty() || done; });
        if (queue.empty() && done) {
            return T{};  // Sentinel value
        }
        T item = queue.front();
        queue.pop();
        cv.notify_one();
        return item;
    }
    
    void setDone() {
        std::lock_guard<std::mutex> lock(mtx);
        done = true;
        cv.notify_all();
    }
};
```

### Reader-Writer Pattern

```cpp
#include <shared_mutex>
#include <vector>

class DataStore {
private:
    std::vector<int> data;
    mutable std::shared_mutex rw_mutex;
    
public:
    int read(size_t index) const {
        std::shared_lock<std::shared_mutex> lock(rw_mutex);
        return data[index];
    }
    
    void write(size_t index, int value) {
        std::unique_lock<std::shared_mutex> lock(rw_mutex);
        data[index] = value;
    }
};
```

### Worker Pool Pattern

```cpp
#include <thread>
#include <vector>
#include <queue>
#include <functional>
#include <mutex>
#include <condition_variable>

class WorkerPool {
private:
    std::vector<std::thread> workers;
    std::queue<std::function<void()>> tasks;
    std::mutex mtx;
    std::condition_variable cv;
    bool stop;
    
public:
    WorkerPool(size_t num_workers) : stop(false) {
        for (size_t i = 0; i < num_workers; i++) {
            workers.emplace_back([this] {
                while (true) {
                    std::function<void()> task;
                    {
                        std::unique_lock<std::mutex> lock(mtx);
                        cv.wait(lock, [this] { 
                            return stop || !tasks.empty(); 
                        });
                        
                        if (stop && tasks.empty()) return;
                        
                        task = tasks.front();
                        tasks.pop();
                    }
                    task();
                }
            });
        }
    }
    
    template<typename F>
    void submit(F&& f) {
        {
            std::lock_guard<std::mutex> lock(mtx);
            tasks.emplace(std::forward<F>(f));
        }
        cv.notify_one();
    }
    
    ~WorkerPool() {
        {
            std::lock_guard<std::mutex> lock(mtx);
            stop = true;
        }
        cv.notify_all();
        for (auto& worker : workers) {
            worker.join();
        }
    }
};
```

---

## 7. Tools to Use

### Debugging Tools

**gdb (GNU Debugger):**
```bash
# Debug multi-threaded program
gdb ./program
(gdb) set follow-fork-mode child  # Follow child process
(gdb) info threads                 # List threads
(gdb) thread 2                     # Switch to thread 2
(gdb) bt                           # Backtrace
```

**Valgrind:**
```bash
# Check for memory errors
valgrind --tool=memcheck ./program

# Check for race conditions
valgrind --tool=helgrind ./program

# Check for data races
valgrind --tool=drd ./program
```

**Thread Sanitizer (TSan):**
```bash
# Compile with TSan
g++ -fsanitize=thread -g program.cpp -o program

# Run
./program
```

### Performance Analysis

**perf:**
```bash
# Profile program
perf record ./program
perf report

# Profile specific events
perf stat -e cache-misses,cache-references ./program
```

**strace:**
```bash
# Trace system calls
strace -f ./program  # -f to follow forks

# Trace specific calls
strace -e trace=clone,fork,execve ./program
```

**htop/top:**
```bash
# Monitor processes and threads
htop
# Press H to show threads
```

---

## 8. Common Interview Questions & Answers

### Q1: What is the difference between threads and processes?

**Answer:**
- **Threads**: Share memory space, lightweight, fast communication, weak isolation
- **Processes**: Separate memory, heavyweight, IPC required, strong isolation
- **Use threads** for: I/O-bound tasks, parallel computation, shared data
- **Use processes** for: Fault isolation, security, independent tasks

### Q2: Explain race conditions and how to prevent them.

**Answer:**
**Race condition**: When outcome depends on timing of thread execution.

**Prevention:**
- **Mutexes**: Lock critical sections
- **Atomic operations**: Lock-free updates
- **Synchronization primitives**: Semaphores, condition variables
- **Lock-free data structures**: Use atomic operations

### Q3: What is a deadlock and how do you prevent it?

**Answer:**
**Deadlock**: Two or more threads waiting for each other indefinitely.

**Prevention:**
- **Lock ordering**: Always acquire locks in same order
- **Timeout**: Use timed mutex locks
- **Lock-free**: Avoid locks when possible
- **Deadlock detection**: Detect and recover

### Q4: Explain mutex vs semaphore.

**Answer:**
- **Mutex**: Binary semaphore (0 or 1), ownership concept, same thread must unlock
- **Semaphore**: Counting semaphore (0 to N), no ownership, any thread can signal
- **Mutex**: Protect critical sections
- **Semaphore**: Control resource access, signaling

### Q5: What is a condition variable?

**Answer:**
**Condition variable**: Allows threads to wait for conditions and notify when conditions change.

**Use cases:**
- Producer-consumer problems
- Waiting for resources
- Synchronizing thread execution

**Operations:**
- `wait()`: Wait for condition (releases mutex, blocks)
- `signal()`: Wake one waiting thread
- `broadcast()`: Wake all waiting threads

### Q6: Explain the producer-consumer problem.

**Answer:**
**Problem**: Producer produces items, consumer consumes items, need synchronization.

**Solution:**
- **Semaphores**: `empty` (available slots), `full` (available items)
- **Mutex**: Protect shared buffer
- **Condition variables**: Wait/notify on buffer state

### Q7: What is thread safety?

**Answer:**
**Thread safety**: Code can be safely executed by multiple threads simultaneously.

**Characteristics:**
- No race conditions
- No data corruption
- Correct behavior under concurrent access

**Achieving thread safety:**
- Synchronization primitives (mutex, semaphore)
- Atomic operations
- Immutable data
- Thread-local storage

### Q8: Explain fork() vs pthread_create().

**Answer:**
- **fork()**: Creates new process (separate memory, expensive)
- **pthread_create()**: Creates new thread (shared memory, lightweight)

**fork() characteristics:**
- Duplicates entire process
- Separate address space
- Copy-on-write optimization
- Returns 0 in child, PID in parent

**pthread_create() characteristics:**
- Creates thread in same process
- Shares address space
- Faster creation
- Returns thread ID

### Q9: What is shared memory and when would you use it?

**Answer:**
**Shared memory**: Memory segment accessible by multiple processes.

**Use cases:**
- High-performance IPC
- Large data sharing
- Real-time systems

**Advantages:**
- Fast (no kernel copying)
- Direct memory access
- Efficient for large data

**Disadvantages:**
- Requires synchronization
- No automatic cleanup
- Platform-specific

### Q10: Explain the difference between mutex and read-write lock.

**Answer:**
- **Mutex**: Exclusive access (one thread at a time)
- **Read-Write Lock**: Multiple readers OR one writer

**Use cases:**
- **Mutex**: When all operations need exclusive access
- **Read-Write Lock**: When reads are frequent, writes are rare

**Performance:**
- Read-write lock better for read-heavy workloads
- Mutex simpler, lower overhead for write-heavy

### Q11: What is a thread pool and why use it?

**Answer:**
**Thread pool**: Pre-created pool of worker threads.

**Benefits:**
- Avoid thread creation overhead
- Control resource usage
- Better performance
- Easier management

**Use cases:**
- Server handling requests
- Parallel task processing
- I/O-bound operations

### Q12: Explain process synchronization methods.

**Answer:**
**IPC Methods:**
1. **Pipes**: Unidirectional, related processes
2. **FIFOs**: Named pipes, unrelated processes
3. **Shared Memory**: Fast, requires synchronization
4. **Message Queues**: Structured messages
5. **Sockets**: Network or local communication
6. **Signals**: Asynchronous notifications

### Q13: What is a semaphore and how does it work?

**Answer:**
**Semaphore**: Counter that controls access to resources.

**Operations:**
- `wait()` (P): Decrement counter, block if zero
- `post()` (V): Increment counter, wake waiting thread

**Types:**
- **Binary semaphore**: 0 or 1 (like mutex)
- **Counting semaphore**: 0 to N

**Use cases:**
- Resource counting
- Producer-consumer
- Rate limiting

### Q14: Explain memory models and visibility.

**Answer:**
**Memory model**: Defines how memory operations are visible to threads.

**Concepts:**
- **Sequential consistency**: Operations appear in program order
- **Happens-before**: Ordering guarantees
- **Memory barriers**: Enforce ordering

**C++ memory orders:**
- `memory_order_relaxed`: No ordering guarantees
- `memory_order_acquire`: Acquire semantics
- `memory_order_release`: Release semantics
- `memory_order_seq_cst`: Sequential consistency

### Q15: How do you debug multi-threaded programs?

**Answer:**
1. **Thread Sanitizer**: Detect data races
2. **Valgrind Helgrind**: Race condition detection
3. **gdb**: Debug threads, set breakpoints
4. **Logging**: Add thread IDs to logs
5. **Assertions**: Check invariants
6. **Static analysis**: Find potential issues

---

## Summary

**Key Takeaways:**
- Multi-threading: Shared memory, lightweight, requires synchronization
- Multi-processing: Separate memory, isolated, requires IPC
- Synchronization: Mutex, semaphore, condition variables, atomics
- IPC: Pipes, shared memory, message queues, sockets
- Design patterns: Producer-consumer, reader-writer, worker pool

**For Zscaler Interviews:**
- Focus on practical implementation
- Understand synchronization primitives
- Know when to use threads vs processes
- Be familiar with common patterns
- Understand debugging and performance analysis

---

**Related Posts:**
- [Zscaler TCP/IP Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-tcp-ip-interview-preparation %})
- [Zscaler Network Performance Diagnostics]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-network-performance-diagnostics %})
- [Zscaler System Reliability & Scalability Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-system-reliability-scalability-interview-preparation %})

