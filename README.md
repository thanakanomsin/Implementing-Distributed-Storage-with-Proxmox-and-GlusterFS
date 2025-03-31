
# Network Programing Project
This project is about designing a VM and storage using Proxmox and GlusterFS, with the condition of using a total of 3 VMs to create server and client machines for storage. We will create a dispersed volume with 3 bricks.




## Features

- Designed VM and storage using Proxmox and GlusterFS.
- Utilizes 3 VMs to create server and client machines for storage.
- The storage is distributed, redundant, and scalable.
- Easy to implement changes in storage size (increase or decrease).

## Virtual Machine Configuration

**Memory:** 2.00 GiB

**Processors:** 2 cores

**Storage:** 10GB disk

**CD/DVD Drive:** Ubuntu Server 22.04 ISO
 
**Network:** VirtIO, bridge-vmbr0
 
## Volume Configuration

**Volume Name:** dispersed_vol

**Volume Type:** Disperse

**Number of Bricks:** 3 (1 x (2 + 1))

**Transport-type:** TCP

### Bricks

**Brick1:** ubuntu1:/gluster-brick/brick1

**Brick2:** ubuntu2:/gluster-brick/brick1

**Brick3:** ubuntu3:/gluster-brick/brick1

### Reconfigured Options

**nfs.disable:** on

**transport.address-family:** inet

**storage.fips-mode-rchecksum:** on

 

 
## How to Install Proxmox on a Server

To install Proxmox on a server, follow these steps:

**1. Download Proxmox ISO:**
   - Visit the Proxmox website (https://www.proxmox.com) and download the latest Proxmox Virtual Environment (VE) ISO image.

**2. Prepare Installation Media:**
   - Create a bootable USB drive using the downloaded ISO. You can use tools like **Rufus** (Windows) or **dd** (Linux) to write the ISO to the USB drive.

**3. Boot from USB:**
   - Insert the bootable USB drive into your server and boot the server. Ensure that the server’s BIOS is set to boot from USB.

**4. Start Installation:**
   - Once the server boots from the USB drive, you’ll see the Proxmox installation screen. Select the option to **Install Proxmox VE**.

**5. Accept the License Agreement:**
   - Read and accept the Proxmox VE End User License Agreement.

**6. Select Target Hard Drive:**
   - Choose the hard drive where Proxmox will be installed. It’s important to note that this process will erase all data on the selected drive.

**7. Configure Proxmox VE:**
   - Set up basic configurations:
     - **Country, Timezone, and Keyboard Layout**: Choose the appropriate settings for your location.
     - **Password**: Set the administrator password for the root account.
     - **Network Configuration**: Assign a static IP address for the server and configure the network settings (you can leave it as default if DHCP is fine for you).

**8. Start Installation:**
   - After completing the setup, click **Install** to begin the installation process. This may take a few minutes.

**9. Reboot and Remove the USB:**
   - Once the installation is finished, reboot the server and remove the USB drive.

**10. Access Proxmox Web Interface:**
    - After the system reboots, access the Proxmox web interface by typing the IP address of your Proxmox server in a browser, followed by the port `8006` (e.g., `https://your-server-ip:8006`).
    - Log in using the root account and the password you set earlier.

Now, you can start using Proxmox to manage your virtual machines (VMs) and containers.
## How to Install a Virtual Machine on Proxmox

To install a virtual machine (VM) on Proxmox with the specified configuration, follow these steps:

**1. Log into Proxmox Web Interface :**
   - Open a web browser and enter the IP address of your Proxmox server followed by port `8006` (e.g., `https://your-proxmox-ip:8006`).
   - Log in using your root account and password.

**2. Create a New VM:**
   - From the Proxmox dashboard, click on **Create VM** located at the top right corner.
   
**3. General Configuration:**
   - **Node**: Select the Proxmox node where the VM will be created.
   - **VM ID**: This will be automatically generated, but you can modify it if needed.
   - **Name**: Enter a name for the VM (e.g., `Ubuntu-22.04`).
   
**4. OS Installation:**
   - Under the **OS** tab, select **Use CD/DVD disc image file (iso)**.
   - In the **ISO Image** field, select the ISO image for Ubuntu (`ubuntu-22.04.2-live-server-amd64.iso`) from the dropdown list.
   
**5. System Configuration:**
   - Under the **System** tab:
     - **BIOS**: Leave as **Default (SeaBIOS)**.
     - **Machine**: Leave as **Default**.
   
**6. Hard Disk:**
   - Under the **Hard Disk** tab:
     - Select **SCSI** for the controller and choose **VirtIO SCSI single**.
     - In **Storage**, select **local**.
     - Set the **Disk size** to **10 GB**.
     - Choose the **Disk format** as **qcow2**.
     - Ensure **IOThread** is set to **1**.
   
**7. CPU Configuration:**
   - Under the **CPU** tab:
     - **Cores**: Set to **2** (1 socket, 2 cores).
     - **Type**: Select the default CPU model.
   
**8. Memory:**
   - Under the **Memory** tab:
     - Set **Memory** to **2 GB (2,048 MB)**.
   
**9. Network Configuration:**
   - Under the **Network** tab:
     - Select **virtio** for the **Model**.
     - Set **Bridge** to **vmbr0**.
     - **Firewall**: Enable (optional).
     - The **MAC Address** 
   
**10. CD/DVD Drive:**
   - Under the **CD/DVD Drive** tab:
     - In **ISO Image**, select the `ubuntu-22.04.2-live-server-amd64.iso` from your local storage.
     - Set **Media** to **cdrom**.

**11. Confirm Settings and Start Installation:**
   - Review all the configuration settings and click **Finish** to create the VM.

**12. Start the VM:**
   - Select the newly created VM from the left-hand menu.
   - Click **Start** to power on the VM.

**13. Install Ubuntu:**
   - Open the **Console** tab to access the VM.
   - Follow the installation steps for Ubuntu 22.04.2 (you can choose the default installation options).
   
Once the installation is complete, you can log into your VM and begin configuring it as needed.
## How to Set a Static IP Address on Ubuntu Using the Terminal

To fix a static IP on Ubuntu using the terminal, follow these steps:

**1. Open the terminal**.
   
**2. Edit the Netplan configuration file:**
   Netplan is the default network configuration tool for newer versions of Ubuntu (17.10 and above). You'll need to modify the netplan configuration file.

   Run the following command to open the netplan configuration file in a text editor (replace `00-default-nm-renderer.yaml` with your actual configuration file name if it differs):
   ```bash
   sudo nano /etc/00-default-nm-renderer.yaml
   ```

**3. Modify the file** to set a static IP. Here's an example of what the file might look like after modification:
   ```yaml
   network:
     version: 2
     ethernets:
       enp3s0:           # Replace with your network interface name
         dhcp4: no       # Disable DHCP
         addresses:
           - 192.168.1.100/24  # Replace with your desired IP address and subnet mask
         routes: 
           - to: defualt
             via: 192.168.1.1  # Replace with your gateway IP
         nameservers:
           addresses: [8.8.8.8, 8.8.4.4]      # Use your preferred DNS servers
   ```

**4. Apply the changes:**
   After editing the file, save and close the editor. Then, apply the configuration changes by running:
   ```bash
   sudo netplan apply
   ```

**5. Verify the IP address:**
   Check if the new static IP has been set correctly by running:
   ```bash
   ip a
   ```

   This should display your network interface with the new static IP address.

Let me know if you need more help with this!
## Steps to Install GlusterFS and Create a Dispersed Volume

Creating a Dispersed Volume on 3 Virtual Machines (VMs) using GlusterFS with Erasure Coding (EC) to distribute data and improve fault tolerance against disk or node failures

### Install GlusterFS on All VMs

**1. Add Repository and Install GlusterFS**
```bash
sudo apt update
sudo apt install -y glusterfs-server
```

**2. Start and Enable GlusterFS**
```bash
sudo systemctl enable --now glusterd
```

**3. Check Status**
```bash
sudo systemctl status glusterd
```

### Set Up GlusterFS Cluster

**1. Define Node Names on Each Machine**
On **each VM**, add the hostnames to `/etc/hosts`:
```bash
echo "ip1 node1" | sudo tee -a /etc/hosts
echo "ip1 node2" | sudo tee -a /etc/hosts
echo "ip1 node3" | sudo tee -a /etc/hosts
```

**2. Connect the Cluster**
On **node1**, run the following commands:
```bash
gluster peer probe node2
gluster peer probe node3
```

**3. Check the Cluster status**
```bash
gluster peer status
```

### Create and Set Up Dispersed Volume

**1. Create Directory for Bricks on All VMs**
```bash
sudo mkdir -p gluster-brick/brick1
```

**2. Create Dispersed Volume**
On **node1**, run the following command:
```bash
gluster volume create dispersed_vol \
  disperse 3 redundancy 1 \
  node1:gluster-brick/brick1 \
  node2:gluster-brick/brick1 \
  node3:gluster-brick/brick1 \
  force
```
- `disperse 3 redundancy 1` means 2 Data Shards + 1 Parity Shard  
- The **redundancy 1** setting allows the loss of 1 node while still being able to recover the data  

**3. Start the Volume**
```bash
gluster volume start dispersed_vol
```

**4. Check Status**
```bash
gluster volume info
```

### Mount Dispersed Volume on Client

**1. Install GlusterFS on Client**
If you want to use one of the VMs (or another machine) as a Client, install:
```bash
sudo apt install -y glusterfs-client
```

**2. Mount the Volume**
Create a directory to mount:
```bash
sudo mkdir -p /mnt/gluster-dispersed
```
Mount the volume:
```bash
sudo mount -t glusterfs node1:/dispersed_vol /mnt/gluster-dispersed
```

**3. Verify Mount**
```bash
df -h | grep gluster
```

**4. Set Up Automount**  
To automatically mount the volume upon boot, add the following line to `/etc/fstab`:

```bash
node1:/vol1 /mnt/gluster-dispersed glusterfs defaults,_netdev 0 0
```

**5. Test the Volume**  
Try creating a file at `/mnt/gluster-dispersed`:

```bash
echo "Hello GlusterFS" > /mnt/gluster-dispersed/test.txt
```

Then, check on `gluster-brick/brick1` on both **node2** and **node3** to see if the `test.txt` file is present.

**6. Check Volume Status**  
To check the status of the volume and perform healing if needed, run the following commands:

```bash
gluster volume status vol1
gluster volume heal vol1 info
```  

---

This will ensure that the volume is properly mounted at boot and provide verification steps to check the status and integrity of the GlusterFS volume.
