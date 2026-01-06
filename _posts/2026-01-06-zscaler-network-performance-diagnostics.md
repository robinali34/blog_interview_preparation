---
layout: post
title: "Zscaler Interview Preparation - Network Performance Diagnostics"
date: 2026-01-06 12:00:00 -0000
categories: interview-preparation zscaler networking performance diagnostics troubleshooting
tags: zscaler networking performance latency packet-loss diagnostics tools troubleshooting
excerpt: "Comprehensive guide to diagnosing and resolving network performance issues for Zscaler interviews covering latency, packet loss analysis, traffic tools, and troubleshooting techniques."
---

# Zscaler Interview Preparation - Network Performance Diagnostics

A comprehensive guide to diagnosing and resolving network performance issues for Zscaler technical interviews, covering latency analysis, packet loss detection, traffic analysis tools, and troubleshooting methodologies.

## 1. What is Network Performance Diagnostics?

### Network Performance Issues

**Network performance diagnostics** involves identifying, analyzing, and resolving issues that affect network communication quality, speed, and reliability.

**Common Performance Issues:**
- **Latency**: Delay in data transmission
- **Packet Loss**: Dropped packets during transmission
- **Jitter**: Variation in packet delay
- **Throughput**: Actual data transfer rate
- **Bandwidth**: Maximum data transfer capacity
- **Congestion**: Network overload
- **Bufferbloat**: Excessive buffering causing delays

### Key Performance Metrics

**Latency Metrics:**
- **RTT (Round-Trip Time)**: Time for packet to travel to destination and back
- **One-Way Latency**: Time for packet to reach destination
- **Jitter**: Variation in latency
- **Target**: <100ms for most applications, <20ms for real-time

**Packet Loss Metrics:**
- **Packet Loss Rate**: Percentage of packets lost
- **Acceptable**: <0.1% for most applications
- **Critical**: >1% causes noticeable degradation

**Throughput Metrics:**
- **Bandwidth Utilization**: Percentage of available bandwidth used
- **Goodput**: Actual useful data transferred (excluding overhead)
- **Target**: 80-90% of theoretical bandwidth

---

## 2. How Network Performance Diagnostics Works

### Latency Analysis

**Latency Components:**
1. **Propagation Delay**: Time for signal to travel (speed of light)
2. **Transmission Delay**: Time to put data on wire
3. **Processing Delay**: Time for routers/switches to process
4. **Queuing Delay**: Time waiting in buffers

**Latency Formula:**
```
Total Latency = Propagation + Transmission + Processing + Queuing
```

**Latency Sources:**
- **Geographic Distance**: Physical distance affects propagation
- **Network Hops**: Each router adds processing delay
- **Congestion**: Queuing delays in buffers
- **Protocol Overhead**: TCP/TLS handshakes add latency
- **Application Processing**: Server/client processing time

### Packet Loss Analysis

**Packet Loss Causes:**
1. **Network Congestion**: Buffer overflow in routers
2. **Physical Issues**: Cable problems, interference
3. **Router/Switch Failures**: Hardware issues
4. **MTU Mismatch**: Fragmentation issues
5. **Bufferbloat**: Excessive buffering
6. **Network Errors**: CRC errors, corruption

**Packet Loss Detection:**
- **ICMP**: Ping shows packet loss
- **TCP Retransmissions**: Indicates packet loss
- **SNMP**: Network device statistics
- **Packet Capture**: Direct analysis

### Network Troubleshooting Methodology

**OSI Model Approach:**
1. **Physical Layer**: Cables, connectors, signal quality
2. **Data Link Layer**: MAC addresses, switches, VLANs
3. **Network Layer**: IP routing, subnets, gateways
4. **Transport Layer**: TCP/UDP, ports, connections
5. **Application Layer**: Protocols, services, applications

**Systematic Approach:**
1. **Identify Symptoms**: What is the problem?
2. **Gather Data**: Collect metrics and logs
3. **Isolate Problem**: Narrow down to specific component
4. **Analyze Root Cause**: Determine why it's happening
5. **Implement Fix**: Apply solution
6. **Verify Resolution**: Confirm problem is solved
7. **Document**: Record solution for future reference

---

## 3. Tools to Use

### Latency Measurement Tools

#### ping

**Purpose**: Basic latency and connectivity testing

**Example Commands:**
```bash
# Basic ping
ping example.com

# Ping with count
ping -c 10 example.com

# Ping with interval
ping -i 0.5 example.com

# Ping with packet size
ping -s 1472 example.com

# Ping with timestamp
ping -D example.com

# Continuous ping
ping -f example.com  # Flood ping (requires root)

# Ping with TTL
ping -t 64 example.com
```

**Output Analysis:**
```
PING example.com (93.184.216.34): 56 data bytes
64 bytes from 93.184.216.34: icmp_seq=0 ttl=54 time=12.345 ms
64 bytes from 93.184.216.34: icmp_seq=1 ttl=54 time=11.234 ms
64 bytes from 93.184.216.34: icmp_seq=2 ttl=54 time=13.456 ms

--- example.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss
round-trip min/avg/max/stddev = 11.234/12.345/13.456/0.912 ms
```

#### traceroute / tracepath

**Purpose**: Identify network path and latency per hop

**Example Commands:**
```bash
# Basic traceroute
traceroute example.com

# Traceroute with TCP
traceroute -T example.com

# Traceroute with UDP
traceroute -U example.com

# Traceroute with ICMP
traceroute -I example.com

# Traceroute with max hops
traceroute -m 30 example.com

# Traceroute with packet size
traceroute -s 1472 example.com

# Tracepath (Linux alternative)
tracepath example.com
```

**Output Analysis:**
```
traceroute to example.com (93.184.216.34), 30 hops max
 1  gateway (192.168.1.1)  1.234 ms  1.123 ms  1.345 ms
 2  10.0.0.1  5.678 ms  5.567 ms  5.789 ms
 3  router.example.com (203.0.113.1)  10.123 ms  10.234 ms  10.345 ms
 ...
```

#### mtr (My Traceroute)

**Purpose**: Combines ping and traceroute with real-time stats

**Example Commands:**
```bash
# Basic mtr
mtr example.com

# MTR with report mode
mtr -r -c 10 example.com

# MTR with CSV output
mtr -r -c 10 --csv example.com

# MTR with interval
mtr -i 0.5 example.com

# MTR with packet size
mtr -s 1472 example.com
```

#### hping3

**Purpose**: Advanced ping with TCP/UDP/ICMP support

**Example Commands:**
```bash
# TCP ping
hping3 -S -p 80 example.com

# UDP ping
hping3 --udp -p 53 example.com

# ICMP ping
hping3 -1 example.com

# Flood ping
hping3 -i u1000 example.com  # 1000 microseconds interval

# Ping with data
hping3 -c 10 -d 1200 -S -p 80 example.com
```

### Packet Loss Detection Tools

#### iperf3

**Purpose**: Network performance testing

**Example Commands:**
```bash
# Server mode
iperf3 -s

# Client mode (TCP)
iperf3 -c server_ip

# Client mode (UDP)
iperf3 -c server_ip -u

# Test with specific bandwidth
iperf3 -c server_ip -b 100M

# Test with parallel streams
iperf3 -c server_ip -P 4

# Test with time limit
iperf3 -c server_ip -t 60

# Test with interval reports
iperf3 -c server_ip -i 1

# Test with packet size
iperf3 -c server_ip -l 1472
```

**Output Analysis:**
```
[ ID] Interval           Transfer     Bitrate         Retr
[  4]   0.00-10.00  sec  1.20 GBytes  1.03 Gbits/sec    0
[  4]  10.00-20.00  sec  1.18 GBytes  1.01 Gbits/sec    2
[  4]  20.00-30.00  sec  1.19 GBytes  1.02 Gbits/sec    1
```

#### netperf

**Purpose**: Network performance benchmarking

**Example Commands:**
```bash
# Server mode
netserver

# TCP stream test
netperf -H server_ip -t TCP_STREAM

# TCP request/response test
netperf -H server_ip -t TCP_RR

# UDP stream test
netperf -H server_ip -t UDP_STREAM

# Test with specific message size
netperf -H server_ip -t TCP_STREAM -- -m 8192
```

### Traffic Analysis Tools

#### Wireshark / tshark

**Purpose**: Deep packet inspection and analysis

**Example Commands:**
```bash
# Capture on interface
sudo tshark -i eth0

# Capture with filter
sudo tshark -i eth0 -f "tcp port 80"

# Save to file
sudo tshark -i eth0 -w capture.pcap

# Read from file
tshark -r capture.pcap

# Filter by protocol
tshark -r capture.pcap -Y "tcp"

# Filter by IP
tshark -r capture.pcap -Y "ip.addr == 192.168.1.1"

# Show statistics
tshark -r capture.pcap -q -z conv,tcp
tshark -r capture.pcap -q -z io,stat,1

# Analyze latency
tshark -r capture.pcap -Y "tcp.analysis.retransmission"
tshark -r capture.pcap -Y "tcp.analysis.duplicate_ack"
```

**Wireshark Filters for Performance:**
```
# Retransmissions
tcp.analysis.retransmission

# Duplicate ACKs
tcp.analysis.duplicate_ack

# Out-of-order packets
tcp.analysis.out_of_order

# Zero window
tcp.analysis.zero_window

# Slow TCP
tcp.analysis.flags && !tcp.analysis.window_update

# High latency
tcp.time_delta > 0.1
```

#### tcpdump

**Purpose**: Command-line packet capture

**Example Commands:**
```bash
# Capture all packets
sudo tcpdump -i any

# Capture with filter
sudo tcpdump -i eth0 "tcp port 80"

# Save to file
sudo tcpdump -i eth0 -w capture.pcap

# Read from file
tcpdump -r capture.pcap

# Verbose output
sudo tcpdump -i eth0 -v -n

# Show packet contents
sudo tcpdump -i eth0 -X

# Capture with snaplen
sudo tcpdump -i eth0 -s 0
```

#### iftop

**Purpose**: Real-time bandwidth monitoring

**Example Commands:**
```bash
# Monitor interface
sudo iftop -i eth0

# Show port numbers
sudo iftop -P -i eth0

# Show hostnames
sudo iftop -n -i eth0

# Set bandwidth scale
sudo iftop -B -i eth0
```

#### nethogs

**Purpose**: Per-process network usage

**Example Commands:**
```bash
# Monitor all interfaces
sudo nethogs

# Monitor specific interface
sudo nethogs eth0

# Refresh interval
sudo nethogs -d 1
```

#### bmon

**Purpose**: Bandwidth monitoring

**Example Commands:**
```bash
# Monitor interface
bmon -p eth0

# Show all interfaces
bmon
```

### Network Statistics Tools

#### ss

**Purpose**: Socket statistics

**Example Commands:**
```bash
# Show all sockets
ss -a

# Show TCP connections
ss -t

# Show listening sockets
ss -tlnp

# Show with process info
ss -tulpn

# Show statistics
ss -s

# Show connections to specific port
ss -t 'dport = :80'
```

#### netstat

**Purpose**: Network connections and statistics

**Example Commands:**
```bash
# Show all connections
netstat -a

# Show TCP connections
netstat -at

# Show listening ports
netstat -tlnp

# Show statistics
netstat -s

# Show routing table
netstat -r
```

#### ip

**Purpose**: Modern network configuration tool

**Example Commands:**
```bash
# Show interfaces
ip addr show

# Show routes
ip route show

# Show statistics
ip -s link show

# Show TCP connections
ss -t  # (uses ip internally)
```

#### ethtool

**Purpose**: Ethernet adapter settings and statistics

**Example Commands:**
```bash
# Show interface info
ethtool eth0

# Show statistics
ethtool -S eth0

# Show driver info
ethtool -i eth0

# Show link settings
ethtool eth0

# Test link
ethtool -t eth0
```

### Advanced Analysis Tools

#### tc (Traffic Control)

**Purpose**: Linux traffic shaping and QoS

**Example Commands:**
```bash
# Show qdiscs
tc qdisc show

# Add delay
tc qdisc add dev eth0 root netem delay 100ms

# Add packet loss
tc qdisc add dev eth0 root netem loss 1%

# Add jitter
tc qdisc add dev eth0 root netem delay 100ms 10ms

# Limit bandwidth
tc qdisc add dev eth0 root tbf rate 1mbit burst 32kbit latency 400ms

# Delete qdisc
tc qdisc del dev eth0 root
```

#### iptables

**Purpose**: Packet filtering and NAT

**Example Commands:**
```bash
# Show rules
sudo iptables -L -v -n

# Show statistics
sudo iptables -L -v -n | grep -E "packets|bytes"

# Drop packets (simulate loss)
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```

#### sar (System Activity Reporter)

**Purpose**: System and network statistics

**Example Commands:**
```bash
# Network statistics
sar -n DEV 1

# Network errors
sar -n EDEV 1

# Network sockets
sar -n SOCK 1

# All network stats
sar -n ALL 1
```

---

## 4. C/C++ Linux Libraries

### libpcap

**Purpose**: Packet capture library

#### Installation

```bash
sudo apt-get install libpcap-dev
gcc -o program program.c -lpcap
```

#### Packet Capture Example

```c
#include <pcap/pcap.h>
#include <stdio.h>
#include <string.h>

void packet_handler(u_char *user_data, const struct pcap_pkthdr *pkthdr,
                    const u_char *packet) {
    printf("Packet captured: %d bytes\n", pkthdr->len);
    printf("Timestamp: %ld.%06ld\n", pkthdr->ts.tv_sec, pkthdr->ts.tv_usec);
}

int main() {
    char errbuf[PCAP_ERRBUF_SIZE];
    pcap_t *handle;
    const char *device = "eth0";
    
    // Open device for capture
    handle = pcap_open_live(device, BUFSIZ, 1, 1000, errbuf);
    if (handle == NULL) {
        fprintf(stderr, "Couldn't open device %s: %s\n", device, errbuf);
        return 1;
    }
    
    // Set filter
    struct bpf_program fp;
    char filter_exp[] = "tcp port 80";
    if (pcap_compile(handle, &fp, filter_exp, 0, PCAP_NETMASK_UNKNOWN) == -1) {
        fprintf(stderr, "Couldn't parse filter %s: %s\n", filter_exp,
                pcap_geterr(handle));
        return 1;
    }
    
    if (pcap_setfilter(handle, &fp) == -1) {
        fprintf(stderr, "Couldn't install filter %s: %s\n", filter_exp,
                pcap_geterr(handle));
        return 1;
    }
    
    // Capture packets
    pcap_loop(handle, -1, packet_handler, NULL);
    
    // Cleanup
    pcap_freecode(&fp);
    pcap_close(handle);
    
    return 0;
}
```

### libnetfilter_queue

**Purpose**: Userspace packet queuing

```c
#include <netinet/ip.h>
#include <netinet/tcp.h>
#include <libnetfilter_queue/libnetfilter_queue.h>

static int callback(struct nfq_q_handle *qh, struct nfgenmsg *nfmsg,
                    struct nfq_data *nfa, void *data) {
    int id = 0;
    struct nfqnl_msg_packet_hdr *ph;
    unsigned char *packet_data;
    
    ph = nfq_get_msg_packet_hdr(nfa);
    if (ph) {
        id = ntohl(ph->packet_id);
    }
    
    nfq_get_payload(nfa, &packet_data);
    
    // Analyze packet
    // ...
    
    return nfq_set_verdict(qh, id, NF_ACCEPT, 0, NULL);
}

int main() {
    struct nfq_handle *h;
    struct nfq_q_handle *qh;
    int fd;
    char buf[4096];
    
    h = nfq_open();
    if (!h) {
        fprintf(stderr, "Error during nfq_open()\n");
        return 1;
    }
    
    if (nfq_unbind_pf(h, AF_INET) < 0) {
        fprintf(stderr, "Error during nfq_unbind_pf()\n");
        return 1;
    }
    
    if (nfq_bind_pf(h, AF_INET) < 0) {
        fprintf(stderr, "Error during nfq_bind_pf()\n");
        return 1;
    }
    
    qh = nfq_create_queue(h, 0, &callback, NULL);
    if (!qh) {
        fprintf(stderr, "Error during nfq_create_queue()\n");
        return 1;
    }
    
    if (nfq_set_mode(qh, NFQNL_COPY_PACKET, 0xffff) < 0) {
        fprintf(stderr, "Error during nfq_set_mode()\n");
        return 1;
    }
    
    fd = nfq_fd(h);
    
    while ((len = recv(fd, buf, sizeof(buf), 0)) && len >= 0) {
        nfq_handle_packet(h, buf, len);
    }
    
    nfq_destroy_queue(qh);
    nfq_close(h);
    
    return 0;
}
```

### Raw Sockets for Performance Testing

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <netinet/ip.h>
#include <netinet/tcp.h>
#include <arpa/inet.h>
#include <stdio.h>
#include <string.h>
#include <time.h>

struct timespec start_time, end_time;

void measure_latency(const char *dest_ip, int port) {
    int sockfd;
    struct sockaddr_in server_addr;
    struct timespec start, end;
    char buffer[1024];
    
    sockfd = socket(AF_INET, SOCK_STREAM, 0);
    
    memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(port);
    inet_pton(AF_INET, dest_ip, &server_addr.sin_addr);
    
    clock_gettime(CLOCK_MONOTONIC, &start);
    
    if (connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
        perror("connect");
        return;
    }
    
    clock_gettime(CLOCK_MONOTONIC, &end);
    
    double latency = (end.tv_sec - start.tv_sec) * 1000.0 +
                     (end.tv_nsec - start.tv_nsec) / 1000000.0;
    
    printf("Connection latency: %.2f ms\n", latency);
    
    close(sockfd);
}
```

---

## 5. Common Interview Questions & Answers

### Q1: How do you diagnose network latency issues?

**Answer:**
1. **Measure RTT**: Use `ping` to measure round-trip time
2. **Identify path**: Use `traceroute` to see each hop
3. **Analyze per-hop latency**: Identify which hop adds most delay
4. **Check for congestion**: Use `mtr` for continuous monitoring
5. **Analyze application**: Check application-level delays
6. **Use packet capture**: Wireshark to analyze packet timing

### Q2: What causes packet loss and how do you detect it?

**Answer:**
**Causes:**
- Network congestion (buffer overflow)
- Physical issues (cable problems)
- Router/switch failures
- MTU mismatches
- Bufferbloat

**Detection:**
- `ping` shows packet loss percentage
- `iperf3` shows retransmissions
- Wireshark shows TCP retransmissions
- SNMP statistics from network devices

### Q3: Explain the difference between latency and bandwidth.

**Answer:**
- **Latency**: Time for data to travel (measured in ms)
- **Bandwidth**: Maximum data transfer rate (measured in bps)
- **Throughput**: Actual data transfer rate achieved
- High bandwidth doesn't guarantee low latency
- Low latency is critical for real-time applications

### Q4: How do you troubleshoot high latency?

**Answer:**
1. **Measure baseline**: Use `ping` to establish normal latency
2. **Identify path**: `traceroute` to see network path
3. **Check each hop**: Identify which hop adds delay
4. **Analyze patterns**: Is latency consistent or variable?
5. **Check for congestion**: Use `mtr` for continuous monitoring
6. **Review routing**: Check routing tables and paths
7. **Analyze application**: Check application-level delays

### Q5: What is jitter and how do you measure it?

**Answer:**
**Jitter**: Variation in packet delay (latency variation)

**Measurement:**
- Calculate standard deviation of RTT measurements
- Use `ping` with multiple packets and analyze variation
- Use specialized tools that calculate jitter
- Target: <30ms for VoIP, <10ms for real-time gaming

### Q6: How do you identify network congestion?

**Answer:**
**Signs of congestion:**
- Increased latency
- Packet loss
- TCP retransmissions
- Bufferbloat (high latency with low utilization)

**Tools:**
- `mtr` for continuous monitoring
- `iperf3` for throughput testing
- Wireshark for retransmission analysis
- SNMP for interface statistics

### Q7: Explain bufferbloat and its impact.

**Answer:**
**Bufferbloat**: Excessive buffering in network devices causing high latency

**Impact:**
- High latency even with low bandwidth utilization
- Affects interactive applications
- Causes TCP to reduce throughput unnecessarily

**Solutions:**
- Active Queue Management (AQM) like CoDel, PIE
- Limit buffer sizes
- Use fq_codel or similar algorithms

### Q8: How do you measure network throughput?

**Answer:**
1. **iperf3**: Industry standard tool
   - Server: `iperf3 -s`
   - Client: `iperf3 -c server_ip`
2. **netperf**: Alternative benchmarking tool
3. **Application-level**: Measure actual application throughput
4. **SNMP**: Query interface statistics
5. **iftop/nethogs**: Real-time monitoring

### Q9: What tools do you use for packet analysis?

**Answer:**
- **Wireshark/tshark**: Deep packet inspection
- **tcpdump**: Command-line packet capture
- **libpcap**: Programmatic packet capture
- **tcpflow**: Reconstruct TCP streams
- **ngrep**: Network grep for packet content

### Q10: How do you diagnose TCP performance issues?

**Answer:**
1. **Check retransmissions**: Wireshark filter `tcp.analysis.retransmission`
2. **Check duplicate ACKs**: Indicates packet loss
3. **Analyze window size**: Check for zero window conditions
4. **Check RTT**: Measure round-trip time
5. **Review congestion control**: Check which algorithm is used
6. **Analyze connection establishment**: Check handshake timing

### Q11: Explain the OSI model approach to troubleshooting.

**Answer:**
Troubleshoot from bottom to top:
1. **Physical**: Cables, connectors, signal quality
2. **Data Link**: MAC addresses, switches, VLANs
3. **Network**: IP routing, subnets, gateways
4. **Transport**: TCP/UDP, ports, connections
5. **Application**: Protocols, services, applications

### Q12: How do you measure one-way latency?

**Answer:**
**Challenges**: Requires synchronized clocks

**Methods:**
1. **NTP-synchronized systems**: Use `ping` with timestamps
2. **Specialized tools**: Tools that use NTP for synchronization
3. **Packet capture**: Capture packets with high-precision timestamps
4. **Hardware timestamps**: Some NICs support hardware timestamping

### Q13: What is the difference between goodput and throughput?

**Answer:**
- **Throughput**: Total data transferred (including overhead)
- **Goodput**: Actual useful data transferred (excluding headers, retransmissions)
- **Formula**: Goodput = Throughput - Protocol Overhead - Retransmissions
- Goodput is always less than or equal to throughput

### Q14: How do you troubleshoot packet loss in a network?

**Answer:**
1. **Identify scope**: Is it specific to one connection or widespread?
2. **Check physical layer**: Cables, connectors, signal quality
3. **Analyze path**: Use `traceroute` to identify problematic hops
4. **Check for congestion**: Monitor interface statistics
5. **Analyze patterns**: Is loss consistent or bursty?
6. **Review device logs**: Check router/switch logs
7. **Use packet capture**: Wireshark to analyze lost packets

### Q15: Explain network performance optimization techniques.

**Answer:**
1. **Reduce latency**:
   - Optimize routing paths
   - Use CDN for content delivery
   - Minimize network hops
   - Use faster protocols (QUIC)

2. **Reduce packet loss**:
   - Increase buffer sizes (carefully)
   - Implement AQM
   - Fix physical issues
   - Optimize routing

3. **Increase throughput**:
   - Increase bandwidth
   - Optimize TCP settings
   - Use parallel connections
   - Compress data

4. **Optimize protocols**:
   - Use HTTP/2 or HTTP/3
   - Enable compression
   - Use connection pooling
   - Implement caching

---

## Summary

**Key Takeaways:**
- Network performance issues require systematic diagnosis
- Multiple tools are needed for comprehensive analysis
- Latency, packet loss, and throughput are key metrics
- Understanding OSI model helps in troubleshooting
- Both command-line and GUI tools are essential

**For Zscaler Interviews:**
- Focus on practical troubleshooting methodologies
- Know common tools and their usage
- Understand performance metrics and their significance
- Be prepared to analyze packet captures
- Understand both network and application-level issues

---

**Related Posts:**
- [Zscaler TCP/IP Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-tcp-ip-interview-preparation %})
- [Zscaler HTTP/HTTPS Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-http-https-interview-preparation %})
- [Zscaler Cloud Platforms Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-cloud-platforms-interview-preparation %})
- [Zscaler Kubernetes Containerization Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-kubernetes-containerization-interview-preparation %})

