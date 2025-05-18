# Build and Secure a Multi-User Web Server With Containers on AlmaLinux

This practical project showcases a comprehensive RHCSA-level Linux system administration environment using AlmaLinux, designed to simulate real-world tasks covered in the Red Hat Certified System Administrator (RHCSA) exam. The setup involves deploying a primary AlmaLinux virtual machine as the target system and a lightweight Lubuntu VM as a remote client for SSH access, file transfer, and remote service testing. Within this environment, a wide range of system administration tasks are automated and manually executed - ranging from local storage management with LVM and partitions, service configuration, user and group administration, and SELinux policy troubleshooting to container management with Podman. A custom shell script automates routine operations like log backups, cron scheduling, and container startup via systemd. Security best practices are implemented through proper firewall management, secure SSH access, and SELinux enforcement.

- Here is the link to the [RHCSA Certification](https://www.redhat.com/en/services/certification/rhcsa)
- Here is the link to the [RHCSA Exam Objectives](https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam?section=objectives)


![build_and_secure_a_multi-user_web_server_with_containers_on_almalinux](https://github.com/user-attachments/assets/de00a1de-adc3-4645-9630-214c882f58cd)


1. [AlmaLinux VM Setup and Essential Tools](#almalinux-vm-setup-and-essential-tools)
2. [Create and Use Users, Groups and Permissions](#create-and-use-users-groups-and-permissions)
3. [Local Storage and File System Management](#local-storage-and-file-system-management)
4. [Network Configuration](#network-configuration)
5. [Secure Access, SSH and Firewall](#secure-access-ssh-and-firewall)
6. [System Services, Targets and Scheduling](#system-services-targets-and-scheduling)
7. [Logs, Performance and Journals](#logs-performance-and-journals)
8. [Shell Scripting Practice](#shell-scripting-practice)
9. [Containers with Podman](#containers-with-podman)
10. [SELinux Practice and Testing with Reboot](#selinux-practice-and-testing-with-reboot)


## AlmaLinux VM Setup and Essential Tools
- Visit [https://almalinux.org/get-almalinux/](https://almalinux.org/get-almalinux/) and download the AlmaLinux OS 9.5 DVD ISO image <br />
  ![image](https://github.com/user-attachments/assets/3e1c279c-7dad-44f9-982b-d85abb435782) <br />

- In VirtualBox, create a new VM and add the downloaded ISO image <br />
  ![image](https://github.com/user-attachments/assets/036b1818-2927-4c57-aeb1-352582034c5a) <br />

- Allocate 4GB of RAM and 2 CPUs <br />
  ![image](https://github.com/user-attachments/assets/c8aae1e1-5aaa-4e11-a0f0-02533d97542d) <br />

- The default storage size of 20GB should be enough <br />
  ![image](https://github.com/user-attachments/assets/452ade6b-f815-4497-aba4-b3f00a5868b9) <br />

- Finish the AlmaLinux VM setup <br />
  ![image](https://github.com/user-attachments/assets/b49778a3-90e8-4346-9708-b154c7748be3) <br />

- Power on AlmaLinux and begin the setup <br />
  ![image](https://github.com/user-attachments/assets/317a2c81-2e02-4069-9e2c-8146d4802786) <br />

  Choose the preferred language <br />
  ![image](https://github.com/user-attachments/assets/50f674d1-8769-4def-8954-1df48cee651b) <br />

  Complete the marked items to continue with the installation <br />
  ![image](https://github.com/user-attachments/assets/873a595a-8ed3-4bca-928e-a6d2e9310da7) <br />

  A simple root password such as `sysadmin` for this project would suffice <br />
  ![image](https://github.com/user-attachments/assets/087a7dcb-6f83-40a5-a9c9-eb1c4784aee9) <br />

  For the Installation Destination, select the disk to install the OS to. When returning to the Installation Summary, click Begin Installation <br />
  ![image](https://github.com/user-attachments/assets/08863ccc-c161-4e9b-a4ef-b3b23eae871e) <br />
  ![image](https://github.com/user-attachments/assets/839c781c-504d-4363-9eed-5980ac5bcf37) <br />

  Reboot the system once installation is completed <br />
  ![image](https://github.com/user-attachments/assets/651eebe1-4a06-4939-b5d1-3c5bc800dac0) <br />

- After reboot, start the setup <br />
  ![image](https://github.com/user-attachments/assets/e8e975ba-602f-4f71-84ba-8f53ee16e036) <br />

- Again, a simple name is used <br />
  ![image](https://github.com/user-attachments/assets/7f60dcf0-0c99-40b3-839f-f516c3cbab3e) <br />

- Use a simple password such as `sysadmin` for the created account, then the setup should be complete <br />
  ![image](https://github.com/user-attachments/assets/6a9de116-c292-46ce-a158-8df2cc3b3f1b) <br />
  ![image](https://github.com/user-attachments/assets/c77f69f7-254b-4ddc-956e-f6c040bfecc0) <br />

- Update and install the required packages
  ```
  sudo dnf update -y
  sudo dnf install -y vim tar bzip2 gzip net-tools podman skopeo wget firewalld man-pages
  ```
  ![image](https://github.com/user-attachments/assets/ae9ca394-6b3e-4a8a-8ea5-7ad93ba25c9d) <br />
  ![image](https://github.com/user-attachments/assets/8e68cb7b-1dad-4efc-a396-229c6894f450) <br />

- Use the essential CLI tools
  ```
  touch testfile
  echo "Hello World" > testfile
  grep Hello testfile
  cat /etc/passwd | grep root
  ```
  ![image](https://github.com/user-attachments/assets/34dc7df1-5312-462f-9dbf-177877328758) <br />

- Use `man` to get a concise manual page for any command
  ```
  man tar
  ```
  ![image](https://github.com/user-attachments/assets/68c31298-5062-44a4-800b-e3b29d47b3a4) <br />

  Press `/` then type a word to search. Press `q` to quit <br />
  ![image](https://github.com/user-attachments/assets/4b395cd1-a5eb-4b59-b9ff-3d353fdf9d25) <br />
  
- `info` is often more detailed and structured than `man` and has navigation links
  ```
  info coreutils 'ls invocation'
  ```
  ![image](https://github.com/user-attachments/assets/eaccd87d-1073-4a44-b065-4e32b2eb0dd4) <br />

- Access the `/usr/share/doc` directory that contains package-specific documentations
  ```
  cd /usr/share/doc/tar
  ls
  ```
  ![image](https://github.com/user-attachments/assets/cb608dc3-a8ba-4a88-b792-7141e92383f8) <br /> 

  Or search for installed docs
  ```
  ls /usr/share/doc | grep ssh
  ```
  ![image](https://github.com/user-attachments/assets/9b2f9ad7-97ff-494a-a32d-9c37df332747) <br />

- A good approach to understand a service (example: `firewalld` service) is to combine these commands to get concise help (`man`), in-depth guide (`info`) and upstream docs (`/usr/share/doc`)
  ```
  man firewalld
  info firewalld
  cd /usr/share/doc/firewalld
  less README.md
  ```
  ![image](https://github.com/user-attachments/assets/f4217dc5-a29f-493d-ae21-a2af5bdaa5a3) <br />
  ![image](https://github.com/user-attachments/assets/3bf60d81-c1ac-4e55-8c27-81a693e09fa5) <br />
  ![image](https://github.com/user-attachments/assets/0efd8a37-487d-470d-8d62-8b89b46e1b60) <br />
  ![image](https://github.com/user-attachments/assets/53bd0063-3533-4961-94d6-c42a370e6fa8) <br />



## Create and Use Users, Groups and Permissions

- Create users and groups
  ```
  sudo useradd dev1
  sudo useradd dev2
  sudo groupadd developers
  sudo usermod -aG developers dev1
  sudo usermod -aG developers dev2
  ```
  ![image](https://github.com/user-attachments/assets/77d1477d-a656-4551-b438-2841429a37f7) <br />

- Set passwords
  ```
  sudo passwd dev1
  sudo passwd dev2
  ```
  ![image](https://github.com/user-attachments/assets/de98748a-bf2a-4554-8eea-7de724a4e515) <br />
  Note that the recommended password for users are more than 8 characters <br />

- Create a shared directory with Set-GID
  ```
  sudo mkdir /opt/devshare
  sudo chown root:developers /opt/devshare
  sudo chmod 2775 /opt/devshare
  ```
  ![image](https://github.com/user-attachments/assets/90c46031-1f54-4384-a973-64c3ad51b383) <br />

- Test the configured permissions
  ```
  sudo -u dev1 touch /opt/devshare/file1
  sudo -u dev2 ls -l /opt/devshare
  ```
  ![image](https://github.com/user-attachments/assets/91bd926c-f44f-41fe-b186-9fad54ffc1ab) <br />
  

## Local Storage and File System Management

- Power off the AlmaLinux VM first to create a new disk in VM settings
- Open VirtualBox Manager and navigate to the storage settings of AlmaLinux VM, find the add Hard Disk button under the existing Controller: SATA <br />
  ![image](https://github.com/user-attachments/assets/e2321336-74be-43bc-baf9-96faa932792e) <br />
- Create a new disk and choose VDI (VirtualBox Disk Image) as the disk file type <br />
  ![image](https://github.com/user-attachments/assets/8d32b87f-be35-4450-aa53-286855ff9d82) <br />

  A full size won't be pre-allocated for the disk <br />
  ![image](https://github.com/user-attachments/assets/e52a3ef3-85a9-46b6-931d-3926d47e3127) <br />

  5GB of storage size should suffice. FInish and save the settings <br />
  ![image](https://github.com/user-attachments/assets/f4084b0d-6b32-4e1c-a107-246e7df80961) <br />
  ![image](https://github.com/user-attachments/assets/426a2e60-5e1e-41bd-ae77-9f31ab7c813d) <br />

- After powering on the AlmaLinux VM back, verify if the disk is detected
  ```
  lsblk
  ```
  ![image](https://github.com/user-attachments/assets/d2fd3a2c-67b8-4e58-a519-6633a06a8359) <br />
  Notice that the newly added disk is named `sdb`
  
- Partition the disk
  ```
  sudo fdisk /dev/sdb
  ```
  ![image](https://github.com/user-attachments/assets/4500d503-3b2a-40e0-a50a-e10e27d5fc91) <br />
  Note that the output here is normal and is expected on a brand new unformatted disk. The statement "Device does not contain a recognized partition table." means that it is empty has no existing partition table (like MBR or GPT). "Created a new DOS disklabel with disk identifier 0x275348bb." means fdisk auto-created a new MBR (DOS) partition table which is what is needed for this project, unless if GPT is used for disks over 2TB or specific configurations

- In the active `Command (m for help):` prompt, type the following in the correct order
  - Type `n` → New partition
  - Type `p` → Primary partition
  - Press `Enter` → Accept default partition number 1
  - Press `Enter` → Accept default first sector
  - Press `Enter` → Accept default last sector (uses entire disk)
  - Now the partition is created in memory
  - Type `w` → Write changes to disk and exit <br />
  ![image](https://github.com/user-attachments/assets/16931239-88fe-4a3d-aa9b-17cca083af85) <br />

- Create the LVM (Logical Volume Manager)
  ```
  sudo pvcreate /dev/sdb1  # turns the partition into a physical volume (PV) - the base unit for LVM
  sudo vgcreate devvg /dev/sdb1  # groups the physical volume into a volume group (VG) named devvg
  sudo lvcreate -n devlv -L 2G devvg  # creates a logical volume (LV) named devlv of size 2GB
  sudo mkfs.xfs /dev/devvg/devlv  # formats the logical volume with the XFS filesystem to be used like a regular drive
  ```
  ![image](https://github.com/user-attachments/assets/d929a8ff-1ecb-4ece-845d-d6cb1d8a276c) <br /> 

- Mount at boot using UUID
  ```
  sudo mkdir /mnt/dev  # creates a folder where the new volume will be mounted (like a virtual USB folder)
  sudo blkid /dev/devvg/devlv  # copy the UUID
  echo 'UUID=<UUID> /mnt/dev xfs defaults 0 0' | sudo tee -a /etc/fstab  # adds UUID entry so Linux mounts it automatically on every boot
  sudo mount -a
  ```
  ![image](https://github.com/user-attachments/assets/c1fef6d6-6763-4b8d-8599-b71de98280ba) <br />

- Add a swap of 1GB
  ```
  sudo lvcreate -L 1G -n swap devvg  # creates another logical volume in the same VG, but it's for swap space (used like virtual RAM)
  sudo mkswap /dev/devvg/swap  # formats it as a swap partition
  echo '/dev/devvg/swap swap swap defaults 0 0' | sudo tee -a /etc/fstab  # Adds it to /etc/fstab so it's used after reboot
  sudo swapon -a  # Activates all swap entries from /etc/fstab
  ```
  ![image](https://github.com/user-attachments/assets/e063c8a3-4ebb-4fc9-bfd9-85843f722a86) <br />


## Network Configuration
- To find the connection name, run
  ```
  nmcli con show
  ```
  ![image](https://github.com/user-attachments/assets/2ac3cb2a-18fb-4860-95e6-7522fd65d1de) <br />

  
- Set static IP and hostname. Replace the `<conn_name>` based on the previous output
  ```
  nmcli con mod <conn_name> ipv4.method manual ipv4.addresses "192.168.1.6/24" ipv4.gateway "192.168.1.1" ipv4.dns "8.8.8.8"
  nmcli con up <conn_name>
  hostnamectl set-hostname rhcsa-lab
  ```
  ![image](https://github.com/user-attachments/assets/c0765656-e334-46ac-b50c-6121b0e00df9) <br />



## Secure Access, SSH and Firewall

- Enable and configure SSH
  ```
  sudo systemctl enable --now sshd
  ```
  ![image](https://github.com/user-attachments/assets/4f5a1b36-8929-4400-9907-13ec69785b26) <br />

- Generate a key pair on the source machine (AlmaLinux VM)
  ```
  ssh-keygen
  ```
  When prompted where to save the key, press `Enter` to accept the default. Setting a passphrase is optional, pressing `Enter` makes no passphrase be chosen <br />
  ![image](https://github.com/user-attachments/assets/f49d9d63-590a-426b-8435-c5fc2f176e48) <br />
  
- Copy the Public Key to the target machine. In this project setup, the target machine is a separate Lubuntu VM. Retrieve the username and IP address of the Lubuntu VM
  ```
  ssh-copy-id <lubuntu_username>@<lubuntu_IP_address>
  ```
  ![image](https://github.com/user-attachments/assets/df8a790e-b7ab-4db3-98ee-ff5ed95ec0ee) <br />

- Test the SSH login from AlmaLinux VM to Lubuntu VM. A login should be successful without a password prompt
  ```
  ssh <lubuntu_username>@<lubuntu_IP_address>
  ```
  ![image](https://github.com/user-attachments/assets/179cd01e-1865-4f69-a5f7-dd64b67f1bd3) <br />

- Configure the firewall
  ```
  sudo systemctl enable --now firewalld
  sudo firewall-cmd --permanent --add-service=ssh
  sudo firewall-cmd --reload
  ```
  ![image](https://github.com/user-attachments/assets/a908c4f3-457f-4f93-9354-98787129a433) <br />


## System Services, Targets and Scheduling

- Change the default boot target for AlmaLinux VM system to `multi-user.target`. It is similar to runlevel 3 in older systems (text-only mode and no GUI)
  ```
  sudo systemctl set-default multi-user.target
  ```
  ![image](https://github.com/user-attachments/assets/c8aee4f1-0b8e-4f42-a003-62d7852b259f) <br />

- Reboot to test <br />
  ![image](https://github.com/user-attachments/assets/89f411f8-f4c6-4288-9d14-2513da54e911) <br />

- Create a cron job (recurring) and at job (one-time)
  ```
  echo "echo 'Hello from cron' >> /tmp/cron.log" | sudo tee /etc/cron.hourly/testjob
  echo "echo 'One time task' >> /tmp/atjob.log" | at now + 1 minute
  ```
  ![image](https://github.com/user-attachments/assets/e6b6468e-0bdc-4ce2-9264-f8263c86edc7) <br />

  Make the script executable using
  ```
  sudo chmod +x /etc/cron.hourly/testjob
  ```
  ![image](https://github.com/user-attachments/assets/5a98636f-3123-4444-a253-65b836bb4bce) <br />

  After an hour (or if it is manually tested with `run-parts`), the `Hello from cron` should be seen
  ```
  cat /tmp/cron.log
  ```
  For manual testing of the cron job, use
  ```
  sudo run-parts /etc/cron.hourly/
  cat /tmp/cron.log
  ```
  ![image](https://github.com/user-attachments/assets/a08f5a79-a6c0-4cd4-83ea-d314a7d6e816) <br />

  For the at job, wait 1-2 minutes then check
  ```
  cat /tmp/atjob.log
  ```
  It should output `One time task` <br />
  ![image](https://github.com/user-attachments/assets/4e9dd772-df2e-490d-81a4-7b4f5d32e037) <br />

  To confirm the the job is scheduled, use
  ```
  atq
  ```
  To view what was scheduled, use
  ```
  sudo ls -l /var/spool/at/
  ```
  ![image](https://github.com/user-attachments/assets/fa25fb99-b457-43de-9e9e-f5f42a587a7b) <br />

  Clean up both cron job and at job after the testig is completed
  ```
  sudo rm /etc/cron.hourly/testjob
  sudo rm /tmp/cron.log /tmp/atjob.log
  ```
  ![image](https://github.com/user-attachments/assets/6015eca1-0308-436d-96cb-58d96ace6e26) <br />


## Logs, Performance and Journals
- To view logs, use
  ```
  journalctl -xe
  sudo dmesg
  ```
  Log output of `journalctl -xe` command. `journalctl` is the tool to view logs collected by `systemd-journald`. `-x` adds extra explanation to log messages if available, while `-e` jumps to the end of the journal (the most recent logs). This command is used for diagnosing service failures, login issues, SELinux denials, troubleshooting after running `systemctl` commands and checking what happened when a cron job failed or a systemd service did not start <br />
  ![image](https://github.com/user-attachments/assets/4004a308-2e99-4cb6-9371-2eea115e989c) <br />

  Output of `sudo dmesg` command. `sudo dmesg` displays kernel ring buffer messages, which are logs directly from the Linux kernel, primarily hardware- and driver-related. `dmesg` which is short for 'display message' shows low-level system events <br />
  ![image](https://github.com/user-attachments/assets/a94a9820-ec1b-482e-808c-c4d1fa09e8f2) <br />


- Find and kill high CPU processes
  ```
  top
  kill -9 <PID>
  ```
  ![image](https://github.com/user-attachments/assets/8fed7f33-2ce6-4209-acb1-d182678ea6f1) <br />

- Set the tuning profile using the `tuned` package which is a service that dynamically adjusts system settings based on selected profile
  ```
  sudo dnf install -y tuned
  sudo tuned-adm list
  sudo tuned-adm profile balanced
  ```
  ![image](https://github.com/user-attachments/assets/8ba2b865-0710-4707-aaad-0efa0c8614ba) <br />


## Shell Scripting Practice
- Create a sample script using
  ```
  nano ~/service_control.sh
  ```
  Paste the following script
  ```
  #!/bin/bash
  if [ "$1" == "start" ]; then
      echo "Starting Service"
  elif [ "$1" == "stop" ]; then
      echo "Stopping Service"
  else
      echo "Usage: $0 start|stop"
  fi
  ```
  ![image](https://github.com/user-attachments/assets/bdb9bb63-7409-4764-bb3f-f6a0bb15268e) <br />

  Make the script executable using
  ```
  chmod +x ~/service_control.sh
  ```

  Test the script
  ```
  ~/service_control.sh start
  ~/service_control.sh stop
  ~/service_control.sh status
  ```
  ![image](https://github.com/user-attachments/assets/799f4b37-5aba-4c23-880f-f0fb53e54e94) <br />

- Create another script using
  ```
  nano ~/check_users.sh
  ```
  Example of a loop script
  ```
  #!/bin/bash
  for user in dev1 dev2; do
    echo "Checking files for $user"
    ls /home/$user
  done
  ```
  ![image](https://github.com/user-attachments/assets/51f012a5-3e5f-463a-995e-a1a59a6fc256) <br />

  Make the script executable
  ```
  chmod +x ~/check_users.sh
  ```
  Run the script to test it
  ```
  ~/check_users.sh
  ```
  ![image](https://github.com/user-attachments/assets/fa7d9a9d-c952-470f-8dbf-381760530158) <br />
  The screenshot shows that permission is required to check the files in the users personal directory. Note that no files are shown because the files are not created yet. Create the test files using
  ```
  sudo touch /home/dev1/file1.txt /home/dev2/file2.txt
  sudo chown -R dev1:dev1 /home/dev1
  sudo chown -R dev2:dev2 /home/dev2
  ```
  Then run and test the script again <br />
  ![image](https://github.com/user-attachments/assets/8d6293f1-be87-4331-b873-b7985d55dbd5) <br />


## Containers with Podman
- Install and run the container
  ```
  sudo dnf install -y podman
  podman pull httpd
  podman run -d --name web1 -p 8080:80 httpd
  ```

- Auto-start the container as systemd service
  ```
  podman generate systemd --name web1 --files --restart-policy=always
  sudo mv container-web1.service /etc/systemd/system/
  sudo systemctl daemon-reexec
  sudo systemctl enable --now container-web1.service
  ```

- Attach the storage
  ```
  sudo mkdir /webdata
  sudo podman volume create --opt type=none --opt device=/webdata --opt o=bind webvolume
  podman run -d --name web2 -p 8081:80 -v webvolume:/usr/local/apache2/htdocs httpd
  ```
    



## SELinux Practice and Testing with Reboot
- List and fix the contexts
  ```
  ls -Z /var/www/html
  sudo restorecon -Rv /var/www/html
  ```

- Set the SELinux booleans
  ```
  sudo setsebool -P httpd_can_network_connect on
  ```

- Modify the port labels
  ```
  sudo semanage port -a -t http_port_t -p tcp 8081
  ```

- View SELinux audit logs
  ```
  sudo ausearch -m avc -ts recent
  ```

- As a final validation, reboot and check the following:
  - SSH works with keys
  - Container starts via systemd
  - `/mnt/dev` auto-mounted
  - Users and shared folders remain
  - cron/at ran
  - Scripts work
  - SELinux and firewall rules applied





