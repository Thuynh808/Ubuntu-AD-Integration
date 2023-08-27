# Ubuntu-Active Directory Integration Lab

## Objective

The goal of this project is to integrate an Ubuntu Server (`UbuntuServer00`) into an existing Active Directory environment (`Streetrack.com`). This will enable centralized management of user credentials, enhanced security, and a seamless user experience. 

## Components

- **VirtualBox**: For creating and running virtual machines
- **Windows Server 2019**: Serves as the Domain Controller for the `Streetrack.com` domain
- **Ubuntu Server 20.04.3**: To be integrated into the Active Directory environment
- **SSH**: Secure Shell for remote management of Ubuntu Server
- **PAM**: Pluggable Authentication Module for Unix/Linux authentication
- **SSSD**: System Security Services Daemon for AD integration
- **Kerberos**: For secure authentication between Ubuntu Server and AD
- **net-tools**: Network utilities for network troubleshooting and configuration

<details>
  <summary><b>Section 1: Pre-Installation Checks</b></summary>
  <br>

  - **Validate Domain Controller (DC) Settings**:  
    Ensure that the Windows Server 2019 Domain Controller is up and running.
    Validate that DHCP and DNS services are functional on the DC.

  - **Confirm Network Interface Card (NIC) Settings**:  
    On `UbuntuServer00`, set the NIC to "Internal Network".
    Make sure it aligns with the DC's internal network settings.

</details>

<details>
  <summary><b>Section 2: Installing UbuntuServer00</b></summary>
  <br>

  - **Begin Installations**:  
    Boot up the `UbuntuServer00` VM from the ISO images and start the installation process.

  - **Network Connections**:  
    During the installation, reach the "Network Connections" section.
    Ensure that you are provided an IP within the range of the DC, which is between `10.2.22.100-200`.
    In this example, we were allocated the IP `10.2.22.104`.

  - **SSH Setup**:  
    Proceed to the SSH setup and select "Install OpenSSH server".

  - **Complete Installation and Access Login**:  
    Once the installation is completed, select "Reboot Now".
    After the system reboots, press Enter, and your login prompt will appear.

  Awesome! We've successfully installed UbuntuServer00!

</details>

<details>
  <summary><b>Section 3: Initial Server Updates</b></summary>
  <br>

  - **Log in to the Ubuntu Server**:  
    Use the username and password created during the installation to log in.
  
  - **Update the System**:  
    Run the following command to update the package list and install the latest versions.
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

  - **Install net-tools**:  
    Run the following command to install net-tools, which provide network troubleshooting and configuration utilities.
    ```bash
    sudo apt install net-tools
    ```

</details>

<details>
  <summary><h2><b>Section 4: Accessing UbuntuServer00 via SSH from Domain Controller</b></h2></summary>
  <br>

  **Step 1: Confirm Server IP Address:**
  - Run `ifconfig` on `UbuntuServer00` to display the network details and confirm its IP address.
    ```bash
    ifconfig
    ```

  **Step 2: SSH from Domain Controller:**
  - Open the Command Prompt on the Domain Controller.
  - Use the `ssh` command to initiate a connection to `UbuntuServer00`.
    ```bash
    ssh thuynh808@10.2.22.104
    ```
    Replace `thuynh808` with your Ubuntu Server username.

  **Step 3: Accept Host Key and Complete Connection:**
  - Upon connecting for the first time, you will be prompted to accept the host key. Verify the fingerprint, type `yes`, and press Enter.

  **Step 4: Enter Password:**
  - After accepting the host key, you will be prompted for your password. Enter the password you set up for `UbuntuServer00`.

</details>

<details>
  <summary><h2><b>Section 5: Setting Date, Time, and Time Zone</b></h2></summary>
  <br>

- **Step 1: Switch to Root User**:
  - Switch to the root user to have the necessary permissions for changing the date, time, and time zone.
    ```bash
    sudo su -
    ```
    
- **Step 2: Set Date and Time Manually**:
  - Set the date and time manually using the `date` command. Replace `YYYY-MM-DD` with the desired date and `HH:MM:SS` with the desired time in 24-hour format.
    ```bash
    date -s "YYYY-MM-DD HH:MM:SS"
    ```
    
- **Step 3: Set Time Zone to US/Hawaii**:
  - Change the system's time zone to "US/Hawaii" using the `timedatectl` command.
    ```bash
    timedatectl set-timezone US/Hawaii
    ```
    
- **Step 4: Verify Domain Time Sync**:
  - Verify if the time on your Ubuntu server is synced with the domain controller's time.
    ```bash
    date
    ```
    
</details>
