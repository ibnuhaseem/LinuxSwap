# LinuxSwap
Add or recreate Linux swap memory in debian btrfs System File

# 🔎 Step 1: Double-Check the No COW Attribute
The most common reason for failure on Btrfs is that the swap file is not set as No COW (Copy-On-Write).
```bash
lsattr /swapfile
```
You should see something like:
-----------------C---- /swapfile

# 🛠️ Step 2: Recreate the Swap File with Correct Steps
Turn off Swap (if any is active):
```bash
sudo swapoff -a
```
Delete the Existing Swap File:
```bash
sudo rm /swapfile
```
Create an Empty File:
```bash
sudo touch /swapfile
```
Disable Copy-On-Write (COW) on the Swap File:
```bash
sudo chattr +C /swapfile
```
Allocate Space:
```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=12288 status=progress
```
Set Correct Permissions:
```bash
sudo chmod 600 /swapfile
```
Format the File as Swap:
```bash
sudo mkswap /swapfile
```
Enable Swap:
```bash
sudo swapon /swapfile
```
# 📝 Step 3: Make It Permanent in /etc/fstab
Make sure the entry in /etc/fstab is correct:
```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
# ✅ Step 4: Verify the Swap
```bash
swapon --show
free -h
```
