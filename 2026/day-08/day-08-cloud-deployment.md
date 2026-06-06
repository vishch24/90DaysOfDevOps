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

Run `sudo apt-get install nginx` to install nginx.

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

[List the key commands you used]

## Challenges Faced

[Describe any issues and how you solved them]

## What I Learned

[3-5 bullet points of key learnings]
