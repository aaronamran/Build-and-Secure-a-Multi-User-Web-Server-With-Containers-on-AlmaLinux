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
  ![image](https://github.com/user-attachments/assets/a5325f0b-bd40-4ae8-9d53-fe9d7714f1ea) <br />

- Continue with the Unattended Guest OS Setup <br />
  ![image](https://github.com/user-attachments/assets/34346198-a499-4891-acaa-acdeb439959e) <br />

- Choose 4GB for the RAM and 2 cores for the number of processors <br />
  ![image](https://github.com/user-attachments/assets/d2f6637e-ffe6-49eb-b53a-d6cd4474cd2d) <br />

- 20GB of storage should be sufficient for this project <br />
  ![image](https://github.com/user-attachments/assets/5bb81429-2d86-433a-a609-ca7e7d76f80f) <br />

- 





- Update and install packages
  ```
  sudo dnf update -y
  sudo dnf install -y vim tar bzip2 gzip net-tools podman skopeo wget firewalld man-pages
  ```

- Use the essential CLI tools
  ```
  touch testfile
  echo "Hello World" > testfile
  grep Hello testfile
  cat /etc/passwd | grep root
  ```

- Use man, info, /usr/share/doc, etc.

  

## Create and Use Users, Groups and Permissions

## Local Storage and File System Management

## Network Configuration

## Secure Access, SSH and Firewall

## System Services, Targets and Scheduling

## Logs, Performance and Journals

## Shell Scripting Practice

## Containers with Podman

## SELinux Practice and Testing with Reboot



