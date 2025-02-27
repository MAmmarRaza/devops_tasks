You can increase the size of **`/dev/root`** on your Ubuntu EC2 instance by following these steps:

---

### **Step 1: Increase the EBS Volume in AWS**  
1️⃣ **Go to AWS EC2 Console** → [AWS EC2](https://console.aws.amazon.com/ec2/)  
2️⃣ **Select the instance** → Click on **Storage**  
3️⃣ **Click on the attached volume (EBS volume ID)**  
4️⃣ **Modify the volume**:  
   - Click **Actions** → **Modify Volume**  
   - Increase the **size** (e.g., from **100GB → 150GB**)  
   - Click **Modify**, then **Confirm**  
5️⃣ **Wait a few minutes** for AWS to finish resizing the volume.  

---

### **Step 2: Verify the New Volume Size in Ubuntu**
Run:  
```bash
lsblk
```
Expected output (if you resized to 150GB):  
```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  150G  0 disk
└─xvda1 202:1    0   97G  0 part /
```
Here, **`xvda`** has increased to **150GB**, but **`xvda1` (your root partition) is still 97GB**.  

---

### **Step 3: Expand the Root Partition**
Run:  
```bash
sudo growpart /dev/xvda 1
```
This expands **partition 1** (`xvda1`) to use the available space.

---

### **Step 4: Resize the Filesystem**
Now, extend the filesystem to use the new space:  

For **ext4 filesystem (default on Ubuntu)**:  
```bash
sudo resize2fs /dev/xvda1
```
For **XFS filesystem (Amazon Linux 2, RHEL)**:  
```bash
sudo xfs_growfs /
```

---

### **Step 5: Confirm the New Size**
Check if the root partition has expanded:  
```bash
df -h
```
Expected output (if resized to 150GB):  
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       147G   62G   85G  42% /
```
Now, `/dev/root` has increased to **147GB (or close to your new volume size)**.

---

✅ **Done!** Your EC2 disk space is now increased. Let me know if you face any issues! 🚀
