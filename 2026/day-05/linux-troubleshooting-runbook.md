# Day 05 – Linux Troubleshooting Drill: CPU, Memory, and Logs

## Target service / process
- `systemctl list-units --type=service --state=running`: Filters and lists down the real-time service units which are currently running in your Linux system.
- Snapshot:  
<img width="700" height="600" alt="image" src="https://github.com/user-attachments/assets/09a5fec5-a47b-4dbc-8f9d-b029bd940098" />

- We'll choose 'ssh' service here to work on.

## Snapshot: CPU & Memory
### 1. Service status:
- `sudo systemctl status ssh`: To check the current status of 'ssh' service.
- Snapshot:
<img width="700" height="600" alt="image" src="https://github.com/user-attachments/assets/fc2d3dfc-cbcd-46fa-95c1-5a2134e241af" />

### 2. CPU & Memory:
- `ps aux | grep sshd`: Filter the process which is running 'ssh' and check its CPU and memory usage.
- `free -h`: To check the free space in human-readable format.
- Snapshot:
<img width="1115" height="203" alt="image" src="https://github.com/user-attachments/assets/0cf835fb-43ed-470e-af7c-4be5f22184be" />

## Snapshot: Disk & IO
- `df -h`: To check the disk usage.
- `iostat -x 1 3`: Monitors and displays extended disk (I/O) statistics on your system, taking 3 total samples spaced exactly 1 second apart.
- Snapshot:
<img width="1000" height="500" alt="image" src="https://github.com/user-attachments/assets/02148392-fae0-4ae9-ae3d-3ec6b7edc99a" />

## Snapshot: Network
- `ss -tlnp | grep ssh`: Checks if SSH server is actively listening for incoming network connections on your Linux system and displays the process details.
- Snapshot:
<img width="1069" height="125" alt="image" src="https://github.com/user-attachments/assets/ded3dac9-eb82-441a-9c14-d8ebe600ca1e" />

## Logs reviewed
- `sudo journalctl -u ssh -n 50`:
- Snapshot:
<img width="900" height="818" alt="image" src="https://github.com/user-attachments/assets/8ed60267-2f12-4409-95c9-784643b9e6a8" />

- `sudo journalctl -u ssh -p err -n 20`:
- Snapshot:
<img width="880" height="120" alt="image" src="https://github.com/user-attachments/assets/32b8abf0-d4b5-48c7-a995-673f0dd5e488" />


## Quick findings

## If this worsens (next steps)
