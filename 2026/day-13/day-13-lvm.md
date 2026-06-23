# Day 13 – Linux Volume Management (LVM)

## Task 1: Check Current Storage
Run: `lsblk`, `pvs`, `vgs`, `lvs`, `df -h`

<img width="600" height="620" alt="image" src="https://github.com/user-attachments/assets/66e54eb2-2b4d-4181-bb93-87cd2d18f0f4" />

**What I learned?**:-
- `lsblk`:- Lists the block of disks and volumes attached.
- `pvs`:- Lists the created physical volumes.
- `vgs`:- Lists the created volume groups.
- `lvs`:- Lists the created logical volume.
- `df -h`:- Lists the free disk space in human-readable format of the current system.

---

## Task 2: Create Physical Volume
```bash
pvcreate /dev/loop0
pvs
```

<img width="940" height="110" alt="image" src="https://github.com/user-attachments/assets/08769f20-caed-4e5f-b408-933dc540b06e" />

**What I learned?**:-
- `pvcreate`:- Creates a new physical volume i.e. `/dev/loop0`.
- `pvs`:- Lists the created physical volume.

---

## Task 3: Create Volume Group
```bash
vgcreate devops-vg /dev/loop0
vgs
```

<img width="940" height="105" alt="image" src="https://github.com/user-attachments/assets/b5c2daa5-74ae-42e6-98e1-a10c2a795bd8" />

**What I learned?**:-
- `vgcreate`:- Creates a new volume group i.e. `devops-vg` as volume name for physical volume `/dev/loop0`.
- `vgs`:- Lists the created volume groups.

---

## Task 4: Create Logical Volume
```bash
lvcreate -L 500M -n app-data devops-vg
lvs
```

<img width="940" height="129" alt="image" src="https://github.com/user-attachments/assets/080359e2-4a05-4bdd-8c89-0e07bb4dde45" />

**What I learned?**:-
- `lvcreate`:- Creates a new logical volume i.e. `-L 500M` as size and `-n app-data` as logical volume name for volume group `/dev/loop0`.
- `lvs`:- Lists the created logical volume.

---

## Task 5: Format and Mount
```bash
mkfs.ext4 /dev/devops-vg/app-data
mkdir -p /mnt/app-data
mount /dev/devops-vg/app-data /mnt/app-data
df -h /mnt/app-data
```

<img width="940" height="488" alt="image" src="https://github.com/user-attachments/assets/dbd377a7-966e-430b-b53b-f08f54f1c644" />

**What I learned?**:-
- `mkfs.ext4`:- Formats the filesystem of logical volume `/dev/devops-vg/app-data` to blank.
- `mkdir`:- Creates a nested directories using `-p` for `/mnt/app-data`.
- `mount`:- Used to mount  logical volume `/dev/devops-vg/app-data` to `/mnt/app-data`.
- `df -h`:- Lists free disk space in human-readable format for `/mnt/app-data`.

---

## Task 6: Extend the Volume
```bash
lvextend -L +200M /dev/devops-vg/app-data
resize2fs /dev/devops-vg/app-data
df -h /mnt/app-data
```

<img width="940" height="444" alt="image" src="https://github.com/user-attachments/assets/17ce5256-5dd0-4d0f-84b4-165ef3f93959" />

**What I learned?**:-
- `lvextend`:- Used to extend the size of the logical volume using `-L +200M /dev/devops-vg/app-data`.
- `resize2fs`:- Linux utility tool used to resize ext2, ext3, or ext4 file systems. It stretches the plastic organizer so it completely fills up the newly enlarged box i.e, `/dev/devops-vg/app-data`.
- `df -h`:- Lists free disk space in human-readable format for `/mnt/app-data`.
