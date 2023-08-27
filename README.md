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

<details>
  <summary><h2><b>Section 1: Pre-Installation Checks</b></h2></summary>
  <br>

  **Step 1: Validate Domain Controller (DC) Settings:**
  - Ensure that the Windows Server 2019 Domain Controller is up and running.
  - Validate that DHCP and DNS services are functional on the DC.

  **Step 2: Confirm Network Interface Card (NIC) Settings:**
  - On `UbuntuServer00`, set the NIC to "Internal Network".
  - Make sure it aligns with the DC's internal network settings.

</details>

<details>
  <summary><h2><b>Section 2: Installing UbuntuServer00</b></h2></summary>
  <br>

  **Step 1: Begin Installations:**
  - Boot up the `UbuntuServer00` VM from the ISO images and start the installation process.

  **Step 2: Network Connections:**
  - During the installation, reach the "Network Connections" section.
  - Ensure that you are provided an IP within the range of the DC, which is between `10.2.22.100-200`.
  - In this example, we were allocated the IP `10.2.22.104`.

  **Step 3: SSH Setup:**
  - Proceed to the SSH setup and select "Install OpenSSH server".

  **Step 4: Complete Installation and Access Login:**
  - Once the installation is completed, select "Reboot Now".
  - After the system reboots, press Enter, and your login prompt will appear.

Awesome! We've successfully installed UbuntuServer00!
