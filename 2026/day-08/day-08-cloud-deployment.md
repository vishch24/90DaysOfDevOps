# Day 08 – Cloud Server Setup: Docker, Nginx & Web Deployment

## Part 1: Launch Cloud Instance & SSH Access

### Step 1: Create a Cloud Instance

Create an EC2 instance on AWS Cloud.

<img width="940" height="158" alt="image" src="https://github.com/user-attachments/assets/6e25bbae-10c5-4e1f-832e-a9b945e00976" />

### Step 2: Connect via SSH

Follow the instructions for the connection.

<img width="940" height="489" alt="image" src="https://github.com/user-attachments/assets/ec079f86-2c67-4672-a927-24731b519322" />

In the local terminal, run `ssh-keygen` command to create public/private ed25519 key pair.

<img width="918" height="762" alt="image" src="https://github.com/user-attachments/assets/59d7c941-8001-40ad-994c-8ae980ab2f03" />

---

## Part 2: Install Docker & Nginx

### Step 1: Update System

Run `sudo apt-get update` to update the system.

### Step 3: Install Nginx

Run `sudo apt-get install nginx` to install nginx and `sudo apt-get install docker.io` to install docker.

### Verify Nginx is running:

Run `systemctl status nginx` to check nginx is actively running.

---

## Part 3: Security Group Configuration

### Test Web Access: Open browser and visit: `http://<your-instance-ip>`

➡️ Go to your instances

➡️ Security

➡️ Click on security groups

➡️ Click on 'Edit Inbound Rules'

➡️ Select type as 'HTTP' and source 'My IP'

➡️ Save Rules

➡️ Go to `http://<public-ipv4-address>`

### You should see the Nginx welcome page!

<img width="940" height="349" alt="image" src="https://github.com/user-attachments/assets/255f53ee-b449-4670-87e4-fa76111439c9" />

---

## Part 4: Extract Nginx Logs

### Step 1: View Nginx Logs

SSH into your EC2 instance first, then:

```bash
sudo cat /var/log/nginx/access.log

sudo cat /var/log/nginx/error.log
```

<img width="940" height="232" alt="image" src="https://github.com/user-attachments/assets/05bd7bf0-c61e-4dd9-8b25-938077e3d354" />

### Step 2: Save Logs to File

Still on the EC2 instance:

```bash
sudo cat /var/log/nginx/access.log > ~/nginx-logs.txt
```

Verify it saved:

```bash
cat ~/nginx-logs.txt
```

### Step 3: Download Log File to Your Local Machine

On your local machine (new terminal window)

```bash
scp -i your-key.pem ubuntu@<your-instance-ip>:~/nginx-logs.txt .
```

Replace:

- `your-key.pem` → your actual key filename
- `<your-instance-ip>` → your EC2 public IP

## Commands Used

- `sudo apt-get update`:- Update the system before anything else.
- `sudo apt-get install nginx`:- Install nginx.
- `sudo cat /var/log/nginx/access.log`:- View access logs for nginx.
- `sudo cat /var/log/nginx/access.log > ~/nginx-logs.txt`:- Save the copy of 'access.log' to a new file 'nginx-logs.txt'.
- `cat ~/nginx-logs.txt`:- View the file if it's saved.
- `scp -i your-key.pem ubuntu@<your-instance-ip>:~/nginx-logs.txt .` Download the new created log file to local system.

## Challenges Faced

| Problems | Solutions |
| --- | --- |
| During running the ssh connection of AWS instance to local, got an error of permission denied. | Solved it by using `chmod 400 "cloud-server-key.pem"` by changing the permission of the file. |
| While, accessing the nginx page, the page didn't load | Solved it by changing type to 'http' which automatically selects port 80 and keeping the source type to 'My IP' only, on AWS. |

## What I Learned

- Nginx works only on port 80.
- SSH is the important part of TCP/IP.
- File permissions matter during establishing connections.
- You need to update system before installing any service.
- Logs are important part of DevOps and should never be ignored.
