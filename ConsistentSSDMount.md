# Raspberry Pi SSD + Plex + qBittorrent Stable Setup Guide

## Goal
A stable setup where:
- SSD always mounts at `/mnt/ssd`
- No auto-mount conflicts
- Survives reboot reliably

---

## 1. Identify SSD UUID

```bash
lsblk -f
```

Copy the UUID for `/dev/sda1`.

---

## 2. Create mount point

```bash
sudo mkdir -p /mnt/ssd
```

---

## 3. Configure /etc/fstab

Edit:

```bash
sudo nano /etc/fstab
```

Add:

```
UUID=<your-drive-uuid>  /mnt/ssd  ntfs-3g  defaults,uid=<user-id>,gid=<group-id>,umask=002,nofail,x-systemd.automount  0  0
```

## fstab Options Explained

- `UUID=<your-drive-uuid>` → The unique identifier for your SSD (find it using `blkid`)
- `/mnt/ssd` → The directory where the drive will be mounted (mount point)
- `ntfs-3g` → Filesystem type (change if using ext4, exfat, etc.)
- `defaults` → Standard mount options
- `uid=<user-id>` → Sets file ownership to a specific user (use `id -u <username>`)
- `gid=<group-id>` → Sets group ownership (use `id -g <username>` or `getent group <groupname>`)
- `umask=002` → Allows read/write access for owner and group
- `nofail` → Prevents boot failure if the drive is missing
- `x-systemd.automount` → Mounts the drive automatically when accessed
- `0 0` → Disables dump and filesystem check order---

## 4. Apply mount

```bash
sudo systemctl daemon-reload
sudo mount -a
```

---

## 5. Verify mount

```bash
findmnt /mnt/ssd
```

Expected:
- mounted at `/mnt/ssd`

---

## Result
- Stable mount at /mnt/ssd
- No auto-mount issues
- Survives reboot cleanly
