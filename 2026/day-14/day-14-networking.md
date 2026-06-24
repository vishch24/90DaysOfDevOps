# Day 14 – Networking Fundamentals & Hands-on Checks

## `OSI layers (L1–L7)` vs `TCP/IP stack`

| OSI | TCP/IP |
| --- | --- |
| It has 7 layers. | It has 4 layers. |
| It's all theorotical to understand the internet flow. | It's actually used in everyday life. |
| <div align="center"> Application <br> ↓ <br> Presentation <br> ↓ <br> Session <br> ↓ <br> Transport <br> ↓ <br> Network <br> ↓ <br> Data Link <br> ↓ <br> Physical </div> | <div align="center"> Application (Application + Presentation + Session) <br> ↓ <br> Transport <br> ↓ <br> Internet / Network Layer <br> ↓ <br> Network Access / Link Layer (Data Link + Physical) </div> |
| **Application**: web browser, email app, etc. | **Application**: software, encryption, and the rules (protocols) like `HTTP`, `FTP`, and `DNS` |
| **Presentation**: Translates, encrypts, and formats the data. | **Transport Layer**: uses TCP to ensure data is reliably delivered. |
| **Session**: Opens, manages, and closes the connection between devices. | **Internet / Network Layer**: Identifies IP addresses and handles the routing of packets from the sender to the destination. |
| **Transport**: Breaks data into smaller chunks (packets) and ensures error-free delivery. | **Network Access / Link Layer**: handles the physical hardware, cables, and local network connections. |
| **Network**: Determines the best physical path to route the data to its destination. |-|
| **Data Link**: Packages raw data into frames and handles hardware MAC addresses. |-|
| **Physical**: Converts the data into raw electrical, optical, or radio signals to travel across wires or air. |-|

### Where `IP, TCP/UDP, HTTP/HTTPS, DNS` sit in the stack

| | OSI | TCP/IP |
| --- | --- | --- |
| `IP` | Network Layer | Internet Layer |
| `TCP/UDP` | Transport Layer | Transport Layer |
| `HTTP/HTTPS` | Application Layer | Application Layer |
| `DNS` | Application Layer | Application Layer |

Example - `curl https://example.com`

```text
[ Your Terminal: `curl https://example.com` ]
                      │
                      ▼
 ┌──────────────────────────────────────────┐
 │ 1. APPLICATION LAYER (HTTPS & DNS)       │ ◄── "What am I sending?"
 └────────────────────┬─────────────────────┘
                      │  Resolves example.com via DNS. Establishes encrypted
                      │  TLS tunnel. Generates an HTTP GET request.
                      ▼
 ┌──────────────────────────────────────────┐
 │ 2. TRANSPORT LAYER (TCP)                 │ ◄── "How do I securely deliver it?"
 └────────────────────┬─────────────────────┘
                      │  Initiates a TCP connection via port 443. Splits the 
                      │  encrypted HTTP stream into reliable TCP Segments.
                      ▼
 ┌──────────────────────────────────────────┐
 │ 3. INTERNET LAYER (IP)                   │ ◄── "Where is it going?"
 └────────────────────┬─────────────────────┘
                      │  Wraps the TCP segments inside an IP Packet. Attaches your
                      │  Source IP and example.com's Destination IP address.
                      ▼
 ┌──────────────────────────────────────────┐
 │ 4. LINK LAYER (Ethernet / Wi-Fi)         │ ◄── "How do I physically move the data?"
 └────────────────────┬─────────────────────┘
                      │  Encapsulates IP packets into local Data Frames with MAC addresses.
                      │  Converts frames into raw electrical/radio bits over the wire.
                      ▼
[ Physical Wire / Wi-Fi Signal ──► Sent to Router ──► Travels to example.com Server ]
```

### Identity: `hostname -I` (or `ip addr show`) — note your IP.

```bash
ubuntu@ip-172-31-10-33:~$ hostname -I
172.31.10.33
ubuntu@ip-172-31-10-33:~$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: enX0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc fq_codel state UP group default qlen 1000
    link/ether 0a:ae:0b:27:02:1b brd ff:ff:ff:ff:ff:ff
    altname enx0aae0b27021b
    inet 172.31.10.33/20 metric 100 brd 172.31.15.255 scope global dynamic enX0
       valid_lft 3537sec preferred_lft 3537sec
    inet6 fe80::8ae:bff:fe27:21b/64 scope link proto kernel_ll
       valid_lft forever preferred_lft forever
ubuntu@ip-172-31-10-33:~$
```

### Reachability: `ping <target>` — mention latency and packet loss.

```bash
ubuntu@ip-172-31-10-33:~$ ping google.com
PING google.com (142.251.33.206) 56(84) bytes of data.
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=1 ttl=117 time=5.47 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=2 ttl=117 time=5.69 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=3 ttl=117 time=5.74 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=4 ttl=117 time=5.81 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=5 ttl=117 time=5.52 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=6 ttl=117 time=5.96 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=7 ttl=117 time=5.92 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=8 ttl=117 time=6.02 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=9 ttl=117 time=5.61 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=10 ttl=117 time=5.59 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=11 ttl=117 time=5.49 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=12 ttl=117 time=5.68 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=13 ttl=117 time=6.85 ms
64 bytes from iad23s96-in-f14.1e100.net (142.251.33.206): icmp_seq=14 ttl=117 time=6.27 ms
...
...
...
```

### Path: `traceroute <target>` (or `tracepath`) — note any long hops/timeouts.

```bash
ubuntu@ip-172-31-10-33:~$ traceroute google.com
traceroute to google.com (142.250.73.110), 64 hops max
  1   242.16.83.237  8.431ms  7.106ms  7.394ms
  2   99.82.10.6  7.326ms  7.628ms  7.286ms
  3   99.82.10.9  6.324ms  6.308ms  6.084ms
  4   108.170.255.179  8.055ms  8.200ms  7.978ms
  5   142.251.64.87  5.646ms  6.390ms  5.878ms
  6   142.250.73.110  7.069ms  7.636ms  7.094ms
ubuntu@ip-172-31-10-33:~$ tracepath

Usage
  tracepath [options] <destination>

Options:
  -4             use IPv4
  -6             use IPv6
  -b             print both name and IP
  -l <length>    use packet <length>
  -m <hops>      use maximum <hops>
  -n             no reverse DNS name resolution
  -p <port>      use destination <port>
  -V             print version and exit
  <destination>  DNS name or IP address

For more details see tracepath(8).
```
### Ports: `ss -tulpn` (or `netstat -tulpn`) — list one listening service and its port.

```bash
ubuntu@ip-172-31-10-33:~$ ss -tulpn
Netid      State       Recv-Q      Send-Q               Local Address:Port           Peer Address:Port     Process
udp        UNCONN      0           0                       127.0.0.54:53                  0.0.0.0:*
udp        UNCONN      0           0                    127.0.0.53%lo:53                  0.0.0.0:*
udp        UNCONN      0           0                172.31.10.33%enX0:68                  0.0.0.0:*
udp        UNCONN      0           0                        127.0.0.1:323                 0.0.0.0:*
udp        UNCONN      0           0                            [::1]:323                    [::]:*
tcp        LISTEN      0           4096                    127.0.0.54:53                  0.0.0.0:*
tcp        LISTEN      0           4096                       0.0.0.0:22                  0.0.0.0:*
tcp        LISTEN      0           4096                 127.0.0.53%lo:53                  0.0.0.0:*
tcp        LISTEN      0           4096                          [::]:22                     [::]:*
ubuntu@ip-172-31-10-33:~$ netstat -tulpn
(No info could be read for "-p": geteuid()=1000 but you should be root.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
udp        0      0 127.0.0.54:53           0.0.0.0:*                           -
udp        0      0 127.0.0.53:53           0.0.0.0:*                           -
udp        0      0 172.31.10.33:68         0.0.0.0:*                           -
udp        0      0 127.0.0.1:323           0.0.0.0:*                           -
udp6       0      0 ::1:323                 :::*                                -
```

### Name resolution: `dig <domain>` or `nslookup <domain>` — record the resolved IP.

```bash
ubuntu@ip-172-31-10-33:~$ dig google.com

; <<>> DiG 9.20.18-1ubuntu2.1-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 41545
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             249     IN      A       142.251.45.142

;; Query time: 1 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Wed Jun 24 08:04:18 UTC 2026
;; MSG SIZE  rcvd: 55

ubuntu@ip-172-31-10-33:~$ nslookup google.com
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   google.com
Address: 142.251.45.142
Name:   google.com
Address: 2607:f8b0:400a:809::200e
```

### HTTP check: `curl -I <http/https-url>` — note the HTTP status code.

```bash
ubuntu@ip-172-31-10-33:~$ curl -I https://google.com
HTTP/2 301
location: https://www.google.com/
content-type: text/html; charset=UTF-8
content-security-policy-report-only: object-src 'none';base-uri 'self';script-src 'nonce-8moiSgLQxnIBXrkNA3AcJg' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri https://csp.withgoogle.com/csp/gws/other-hp
date: Wed, 24 Jun 2026 08:05:50 GMT
expires: Fri, 24 Jul 2026 08:05:50 GMT
cache-control: public, max-age=2592000
server: gws
content-length: 220
x-xss-protection: 0
x-frame-options: SAMEORIGIN
alt-svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
```

### Connections snapshot: `netstat -an | head` — count ESTABLISHED vs LISTEN (rough).

```bash
ubuntu@ip-172-31-10-33:~$ netstat -an | head
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0    748 172.31.10.33:22         49.43.25.11:54311       ESTABLISHED
tcp6       0      0 :::22                   :::*                    LISTEN
udp        0      0 127.0.0.54:53           0.0.0.0:*
udp        0      0 127.0.0.53:53           0.0.0.0:*
udp        0      0 172.31.10.33:68         0.0.0.0:*
```

---

## Port Probe & Interpret

### Identify one listening port from `ss -tulpn` (e.g., SSH on 22 or a local web app).

```bash
tcp        LISTEN      0           4096                    127.0.0.54:53                  0.0.0.0:*
tcp        LISTEN      0           4096                       0.0.0.0:22                  0.0.0.0:*
tcp        LISTEN      0           4096                 127.0.0.53%lo:53                  0.0.0.0:*
tcp        LISTEN      0           4096                          [::]:22                     [::]:*
```

### From the same machine, test it: `nc -zv localhost <port>` (or `curl -I http://localhost:<port>`)

```bash
ubuntu@ip-172-31-10-33:~$ nc -zv localhost 22
Connection to localhost (127.0.0.1) 22 port [tcp/ssh] succeeded!
ubuntu@ip-172-31-10-33:~$ curl -I http://localhost:22
curl: (1) Received HTTP/0.9 when not allowed
```

### Write one line: is it reachable? If not, what’s the next check? (e.g., service status, firewall).

Yes, it's reachable.
If it is not reachable because DNS resolution failed; the next check is to run `dig google.com` to see `ANSWER` section and if it returned any IP address.
I can also use `nslookup google.com` to get a non-authoritative answer.

---

## Reflection

### Which command gives you the fastest signal when something is broken?

```bash
curl -I -s --max-time 3 https://google.com | head -n 1
```
**Why it's fast**: The `--max-time 3` flag stops the command after `3` seconds if the server hangs, and `head -n 1` shows you just the status code (like `200 OK` or `500 Internal Server Error`).

### What layer (OSI/TCP-IP) would you inspect next if DNS fails? If HTTP 500 shows up?
- **Application Layer**, because `DNS` and `HTTP` stays on this layer.

### Two follow-up checks you’d run in a real incident.

1. Check Application and System Logs (For HTTP 500)
```bash
sudo tail -n 100 /var/log/nginx/error.log || journalctl -u <application-service> -n 100 --no-pager
```

2. Check Resource Utilization and Disk Space
```bash
df -h && free -m && top -b -n 1 | head -n 20
```
