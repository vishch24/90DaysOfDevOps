# Day 15 – Networking Concepts: DNS, IP, Subnets & Ports

## Task 1: DNS – How Names Become IPs

1. Explain in 3–4 lines: what happens when you type `google.com` in a browser?
   - **DNS Lookup**:- : The browser queries the Domain Name System (DNS) to translate the human-readable text `google.com` into a machine-readable numeric IP address.
   - **Establish Connection**:- The browser uses the retrieved IP address to set up a secure `TCP/IP` connection with Google’s servers.
   - **HTTP Request**:- : It sends a secure HTTPS request asking for the website content.
   - **Page Rendering**:-: Google's servers process the request and reply with HTML, CSS, and JavaScript files, which your browser parses and displays as a webpage on your screen.

2. What are these record types `A`, `AAAA`, `CNAME`, `MX`, `NS`? Write one line each:
   - `A`: Maps a domain name directly to an IPv4 address.
   - `AAAA`: Maps a domain name directly to an IPv6 address.
   - `CNAME`: Aliases one domain name to another domain name.
   - `MX`: Directs incoming emails to the correct mail server.
   - `NS`: Identifies the authoritative servers responsible for a domain.
   
3. Run: `dig google.com` — identify the A record and TTL from the output
   - `A`:- 142.250.73.78
   - `TTL`:- 234

---

## Task 2: IP Addressing

1. What is an IPv4 address? How is it structured? (e.g., `192.168.1.10`)
   - An IPv4 address is a unique 32-bit numerical label assigned to every device connected to a computer network using the Internet Protocol.
     - **32-Bit Length**: The full address contains exactly *32 binary bits* (zeros and ones).
     - **Four Octets**: The address is divided into four 8-bit segments called *octets*.
     - **Dotted-Decimal Format**: Each binary octet is converted into a readable base-10 number from *0 to 255*.
     - **Network vs. Host**: A subnet mask splits the address into a *network ID* (identifies the specific network) and a *host ID* (identifies the specific device).

Structure Example `192.168.1.10`

| Notation | Octet 1 | Octet 2 | Octet 3 | Octet 4 |
|------|-------------|-----------|--------------|--------------|
| Decimal | `192` | `168` | `1` | `10` |
| Binary | `$11000000$` | `$10101000$` | `$00000001$` | `$00001010$` |

2. Difference between **public** and **private** IPs — give one example of each

| Public IP | Private IP |
| --- | --- |
| Globally unique address used to identify your network on the public internet. | Regionally unique address used only within your local network (like your home or office). |
| Accessible by anyone on the internet and routed worldwide. | Hidden from the internet and cannot be reached from outside your local network. |
| Assigned to your router by your Internet Service Provider (ISP). | Assigned to your device by your local router (usually via DHCP). |
| `8.8.8.8` (A public address used by Google's public DNS server). | `192.168.1.15` (A private address for a smartphone connected to a home Wi-Fi network). |


3. What are the private IP ranges?

| Class | IP Range Start | IP Range End | CIDR Notation | Total Addresses | Common Use Case |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Class A** | `10.0.0.0` | `10.255.255.255` | `10.0.0.0/8` | 16,777,216 | Large enterprise networks |
| **Class B** | `172.16.0.0` | `172.31.255.255` | `172.16.0.0/12` | 1,048,576 | Medium corporate & campus networks |
| **Class C** | `192.168.0.0` | `192.168.255.255` | `192.168.0.0/16` | 65,536 | Home and small office networks |

4. Run: `ip addr show` — identify which of your IPs are private
   - `172.31.10.33`. It falls perfectly inside the Class B private range (`172.16.0.0` to `172.31.255.255`).
   - `127.0.0.1`. This is your local host loopback address. It is used exclusively for internal testing on your own machine.

---

## Task 3: CIDR & Subnetting

1. What does `/24` mean in `192.168.1.0/24`?
   - The `/24` means the first three numbers (`192.168.1.`) are fixed for the network, leaving the last number free for up to 254 individual devices.

2. How many usable hosts in a `/24`? A `/16`? A `/28`?
   - `/24`: **254** usable hosts (2⁸ - 2).
   - `/16`: **65,534** usable hosts (2¹⁶ - 2).
   - `/28`: **14** usable hosts (2⁴ - 2).

3. Explain in your own words: why do we subnet?
   - We subnet to split a large network into smaller, secure chunks to stop traffic jams, boost speed, and prevent wasting IP addresses.

4. Quick exercise — fill in:

| CIDR | Subnet Mask | Total IPs | Usable Hosts |
|------|-------------|-----------|--------------|
| /24  | `255.255.255.0` | **256** | **254** |
| /16  | `255.255.0.0`   | **65,536** | **65,534** |
| /28  | `255.255.255.240` | **16** | **14** |

---

## Task 4: Ports – The Doors to Services

1. What is a port? Why do we need them?
   - A port is a logical endpoint used by an operating system to direct network traffic to a specific application or service.

   - Why We Need Them?
     - **Multiple Apps, One IP**: Your computer has only one IP address, but ports allow you to browse the web, stream music, and download a file simultaneously without the data streams getting mixed up.
     - **Traffic Sorting**: They act like apartment numbers in a building; the IP address gets the data to the correct building (your computer), while the port number gets it to the correct apartment (the specific app).
     - **Standardised Services**: They use universally agreed-upon numbers so computers know how to talk to each other instantly. For example, web traffic automatically looks for port 80 (HTTP) or port 443 (HTTPS).

2. Document these common ports:

| Port | Service |
|------|---------|
| 22   | **SSH** (Secure Shell)       |
| 80   | **HTTP** (Hypertext Transfer Protocol)       |
| 443  | **HTTPS** (HTTP Secure)       |
| 53   | **DNS** (Domain Name System)       |
| 3306 | **MySQL** Database       |
| 6379 | **Redis** In-Memory Database       |
| 27017| **MongoDB** NoSQL Database       |

3. Run `ss -tulpn` — match at least 2 listening ports to their services
   - `22`:- SSH
   - `53`:- DNS

---

## Task 5: Putting It Together

Answer in 2–3 lines each:
- You run `curl http://myapp.com:8080` — what networking concepts from today are involved?
  - Your terminal translates `myapp.com` to an IP address and sends an unencrypted HTTP GET request directly to port **8080** instead of the standard web port.
  - The backend application listening on that port processes the request and returns raw data (like ***HTML** or **JSON**), which `curl` prints right into your terminal window.

- Your app can't reach a database at `10.0.1.50:3306` — what would you check first?
  - **Network Line Check**: Run `ping 10.0.1.50` to see if the database server machine is online and reachable from your app server.
  - **Port Connectivity**: Run `nc -zv 10.0.1.50 3306` (or `telnet 10.0.1.50 3306`) to check if firewalls, security groups, or ACLs are blocking traffic on port 3306
  - **Database Listening Config**: Verify that the MySQL/MariaDB service is actually running on the target server and its bind-address configuration is set to `0.0.0.0` or `10.0.1.50` rather than just `127.0.0.1`.
