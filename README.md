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

- Use man, info, /usr/share/doc, etc.

  

## Create and Use Users, Groups and Permissions

- Create users and groups
  ```
  sudo useradd dev1
  sudo useradd dev2
  sudo groupadd developers
  sudo usermod -aG developers dev1
  sudo usermod -aG developers dev2
  ```

- Set passwords
  ```
  sudo passwd dev1
  sudo passwd dev2
  ```

- Create a shared directory with Set-GID
  ```
  sudo mkdir /opt/devshare
  sudo chown root:developers /opt/devshare
  sudo chmod 2775 /opt/devshare
  ```

- Test the configured permissions
  ```
  sudo -u dev1 touch /opt/devshare/file1
  sudo -u dev2 ls -l /opt/devshare
  ```


## Local Storage and File System Management

- Create a new disk in VM settings

- Partition the disk
  ```
  sudo fdisk /dev/sdb
  ```
  Create a primary partition and write

- Create the LVM (Logical Volume Manager)
  ```
  sudo pvcreate /dev/sdb1
  sudo vgcreate devvg /dev/sdb1
  sudo lvcreate -n devlv -L 2G devvg
  sudo mkfs.xfs /dev/devvg/devlv
  ```

- Mount at boot using UUID
  ```
  sudo mkdir /mnt/dev
  sudo blkid /dev/devvg/devlv  # Copy UUID
  echo 'UUID=<UUID> /mnt/dev xfs defaults 0 0' | sudo tee -a /etc/fstab
  sudo mount -a
  ```

- Add a swap of 1GB
  ```
  sudo lvcreate -L 1G -n swap devvg
  sudo mkswap /dev/devvg/swap
  echo '/dev/devvg/swap swap swap defaults 0 0' | sudo tee -a /etc/fstab
  sudo swapon -a
  ```

  






## Network Configuration

- Set static IP and hostname
  ```
  nmcli con show
  nmcli con mod <conn_name> ipv4.method manual ipv4.addresses "192.168.1.100/24" ipv4.gateway "192.168.1.1" ipv4.dns "8.8.8.8"
  nmcli con up <conn_name>
  hostnamectl set-hostname rhcsa-lab
  ```
  




## Secure Access, SSH and Firewall

- Enable and configure SSH
  ```
  sudo systemctl enable --now sshd
  ```

- Set up SSH key-based authentication
  ```
  ssh-keygen
  ssh-copy-id dev1@192.168.1.100
  ```

- Configure the firewall
  ```
  sudo systemctl enable --now firewalld
  sudo firewall-cmd --permanent --add-service=ssh
  sudo firewall-cmd --reload
  ```




## System Services, Targets and Scheduling

- Change the default target
  ```
  sudo systemctl set-default multi-user.target
  ```

- Reboot to test
- Create a cron job and at job
  ```
  echo "echo 'Hello from cron' >> /tmp/cron.log" | sudo tee /etc/cron.hourly/testjob
  echo "echo 'One time task' >> /tmp/atjob.log" | at now + 1 minute
  ```




## Logs, Performance and Journals
- To view logs, use
  ```
  journalctl -xe
  sudo dmesg
  ```

- Find and kill high CPU processes
  ```
  top
  kill -9 <PID>
  ```

- Set the tuning profile
  ```
  sudo dnf install -y tuned
  sudo tuned-adm list
  sudo tuned-adm profile balanced
  ```






## Shell Scripting Practice
- Create a sample script
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

- Example of a loop script
  ```
  #!/bin/bash
  for user in dev1 dev2; do
    echo "Checking files for $user"
    ls /home/$user
  done
  ```






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





