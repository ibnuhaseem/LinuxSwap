# LinuxSwap
Add or recreate Linux swap memory in debian btrfs System File
# LinuxSwap
Add or recreate Linux swap memory in debian btrfs System File
# 🔎 Step 1: Double-Check the No COW Attribute
The most common reason for failure on Btrfs is that the swap file is not set as No COW (Copy-On-Write).

$ lsattr /swapfile

You should see something like:
-----------------C---- /swapfile

# 🛠️ Step 2: Recreate the Swap File with Correct Steps
Turn off Swap (if any is active):

$ sudo swapoff -a

Delete the Existing Swap File:

$ sudo rm /swapfile

Create an Empty File:

$ sudo touch /swapfile

Disable Copy-On-Write (COW) on the Swap File:

$ sudo chattr +C /swapfile

Allocate Space:

$ sudo dd if=/dev/zero of=/swapfile bs=1M count=12288 status=progress

Set Correct Permissions:

$ sudo chmod 600 /swapfile

Format the File as Swap:

$ sudo mkswap /swapfile

Enable Swap:

$ sudo swapon /swapfile

# 📝 Step 3: Make It Permanent in /etc/fstab
Make sure the entry in /etc/fstab is correct:

$ echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# ✅ Step 4: Verify the Swap
$ swapon --show
$ free -h
