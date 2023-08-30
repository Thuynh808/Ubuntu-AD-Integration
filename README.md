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
  <summary><h2><b>Section 1: Pre-Installation Checks</b></h2></summary>
  <br>
  Before beginning the installation process, we need to perform some preliminary checks to ensure a smooth setup.

  - **Step 1: Validate Domain Controller (DC) Settings**:  
    Ensure that the Windows Server 2019 Domain Controller is up and running.
    Validate that DHCP and DNS services are functional on the DC.

  - **Step 2: Confirm Network Interface Card (NIC) Settings**:  
    On `UbuntuServer00`, set the NIC to "Internal Network".
    Make sure it aligns with the DC's internal network settings.

</details>

<details>
  <summary><h2><b>Section 2: Installing UbuntuServer00</b></h2></summary>
  <br>
  In this section, we will go through the installation process for Ubuntu Server and prepare it for integration with the Active Directory environment.

  - **Step 1: Begin Installations**:  
    Boot up the `UbuntuServer00` VM from the ISO images and start the installation process.

  - **Step 2: Network Connections**:  
    During the installation, reach the "Network Connections" section.
    Ensure that you are provided an IP within the range of the DC, which is between `10.2.22.100-200`.
    In this example, we were allocated the IP `10.2.22.104`.
  
  - **Step 3: Profile Setup**:  
    Here we will setup our profile:
      - Your name: Thong Huynh
      - Your server's name: ubuntuserver00
      - Pick a username: thuynh808
      - Password: ************

  - **Step 3: SSH Setup**:  
    Proceed to the SSH setup and select "Install OpenSSH server".

  - **Step 4: Complete Installation and Login**:  
    Once the installation is completed, select "Reboot Now".
    After the system reboots, press Enter, and your login prompt will appear.

  Awesome! We've successfully installed UbuntuServer00!

</details>

<details>
  <summary><h2><b>Section 3: Initial Server Updates and Installing net-tools</b></h2></summary>
  <br>
  After installing Ubuntu Server, we'll ensure that it's up to date and install additional network tools for troubleshooting and configuration.

  - **Step 1: Log in to the Ubuntu Server**:  
    Use the username and password created during the installation to log in.
  
  - **Step 2: Update the System**:  
    Run the following command to update the package list and install the latest versions.
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

  - **Step 3: Install net-tools**:  
    Run the following command to install net-tools, which provide network troubleshooting and configuration utilities.
    ```bash
    sudo apt install net-tools
    ```

</details>

<details>
  <summary><h2><b>Section 4: Accessing UbuntuServer00 via SSH from Domain Controller</b></h2></summary>
  <br>
  Now that our server is updated and equipped with necessary tools, let's establish a secure SSH connection to it from the Domain Controller.

  - **Step 1: Confirm Server IP Address**:
    Run `ifconfig` on `UbuntuServer00` to display the network details and confirm its IP address.
    ```bash
    ifconfig
    ```
  
  - **Step 2: SSH from Domain Controller**:
    Open the Command Prompt on the Domain Controller.
    Use the `ssh` command to initiate a connection to `UbuntuServer00`.
    ```bash
    ssh thuynh808@10.2.22.104
    ```
    Replace `thuynh808` with your Ubuntu Server username.
  
  - **Step 3: Accept Host Key and Complete Connection**:
    Upon connecting for the first time, you will be prompted to accept the host key. Verify the fingerprint, type `yes`, and press Enter.
  
  - **Step 4: Enter Password**:
    After accepting the host key, you will be prompted for your password. Enter the password you set up for `UbuntuServer00`.

</details>

<details>
  <summary><h2><b>Section 5: Setting Date, Time, and Time Zone</b></h2></summary>
  <br>
  To ensure accurate time synchronization within the domain, we'll set the date, time, and time zone for the Ubuntu Server.

  - **Step 1: Switch to Root User**:
    Switch to the root user to have the necessary permissions for changing the date, time, and time zone.
    ```bash
    sudo su -
    ```
    
  - **Step 2: Set Date and Time Manually**:
    Set the date and time manually using the `date` command. Replace `YYYY-MM-DD` with the desired date and `HH:MM:SS` with the desired time in 24-hour format.
    ```bash
    date -s "YYYY-MM-DD HH:MM:SS"
    ```
    
  - **Step 3: Set Time Zone to US/Hawaii**:
    Change the system's time zone to "US/Hawaii" using the `timedatectl` command.
    ```bash
    timedatectl set-timezone US/Hawaii
    ```
    
  - **Step 4: Verify Domain Time Sync**:
    Verify if the time on your Ubuntu server is synced with the domain controller's time
    ```bash
    date
    ```
</details>

<details>
  <summary><h2><b>Section 6: Installing Packages for Active Directory Integration</b></h2></summary>
  <br>

  In this section, we'll be installing the required packages that are essential for integrating UbuntuServer00 into the Active Directory domain.

  - **Step 1: Install Packages**:  
    Open a terminal on `UbuntuServer00`.

    Run the following command to install the necessary packages for Active Directory integration:
    ```bash
    sudo apt install sssd-ad sssd-tools realmd packagekit krb5-user adcli
    ```
  
    This command will install various packages required for interacting with Active Directory services.

</details>

<details>
  <summary><h2><b>Section 6: Discovering and Joining the Active Directory Domain</b></h2></summary>
  <br>

  In this section, we'll discover the Active Directory domain and join it using the packages we installed earlier. Joining the domain will enable seamless authentication and access to domain resources.

  - **Step 1: Discover the Domain**:  
    Run the following command to discover the Active Directory domain:
    ```bash
    sudo realm discover STREETRACK.COM
    ```
    This command will provide information about the Active Directory realm, such as its domain controllers and supported authentication mechanisms.

  - **Step 2: Join the Domain**:  
    Run the following command to join the Ubuntu Server to the Active Directory domain:
    ```bash
    sudo realm join -v STREETRACK.COM
    ```
    We'll then input our domain Administrator password

  - **Step 3: Verify the Joining**:  
    After successful domain joining, you can verify it using the following command:
    ```bash
    sudo realm list
    ```
    This command should display the details of the joined domain, including its name, domain controller, and configured realm.

  - **Step 4: Update PAM Configuration**:  
    Run the following command to update the Pluggable Authentication Module (PAM) configuration:
    ```bash
    sudo nano /etc/pam.d/common-session
    ```
    We're going to add an entry"
      - session optional    pam_mkhomedir.so

    This configuration will auto create a home directory for a user's first time log in.

    Save and Exit with:
    ```bash
    Ctrl + O , Enter , Ctrl + X
    ```

  - **Step 5: Update krb45.conf**:
    Run the following command to update the krb5.conf file:
    ```bash
    sudo nano /etc/krb5.conf
    ```
    Here we'll add 4 entries:
      - udp_preference_limit = 0
      - rdns = False
      - dns_lookup_kdc = True
      - dns_lookup_realms = True

    Save and Exit with:
    ```bash
    Ctrl + O , Enter , Ctrl + X

  - **Step 6: Update SSSD Service**:
    Run the following command to update the System Security Servicess Daemon (SSSD):
    ```bash
    sudo nano /etc/sssd/sssd.conf
    ```
    Here we'll add 2 entries:
      - krb5_keytab = /etc/krb5.keytab
      - ldap_keytab_init_creds = True

    Save and Exit with:
    ```bash
    Ctrl + O , Enter , Ctrl + X
    ```

  After updating the configuration, restart the System Security Services Daemon (SSSD) for changes to take effect and check its status to make sure its configured properly:
    ```bash
    sudo systemctl restart sssd
    ```

    ```bash
    sudo systemctl status sssd
    ```
</details>

<details>
  <summary><h2><b>Section 8: Verifying Active Directory Authentication</b></h2></summary>
  <br>

  To ensure that Active Directory authentication is working properly, we will perform the following steps:

  - **Step 1: Logging in with Domain Admin Account:**
    Log in to the Ubuntu Server (`UbuntuServer00`).
  - We'll use our Active Directory domain admin credentials to log in:
    ```bash
    sudo login thuynh@streetrack.com
    ```
    Great! We're in! Now let's confirm that we were issued a kerberos ticket for authentication:
    ```bash
    klist
    ```

    Looks like our ticket has been issued for us!

  - **Step 2: Adding Domain Admin to sudoers List:**
    To allow your domain admin to execute administrative commands, add the domain admin to the `sudoers` list.
    Edit the sudoers file using the `visudo` command.
    ```bash
    sudo visudo
    ```
    Add the following line to the file, replacing `thuynh` with your domain admin username:
    ```plaintext
    thuynh ALL=(ALL:ALL) ALL
    ```
    Save and exit the editor.
   
    Here we've confirmed that (`thuynh@Streetrack.com`) has sudo priveleges.

  - **Step 3: Log Out and Log In with Regular AD User:**
    Log out from the current session with the domain admin account.
    ```bash
    exit
    ```
    Log in again using a different Active Directory user account to verify that general AD users can also authenticate and access the server.
    ```bash
    su - pcoulson@streetrack.com
    ```

    Excellent! 

</details>

## __Conclusion__
  
  In conclusion, our successful integration of Ubuntu Server into the Active Directory domain showcases our prowess in bridging diverse systems. Thanks to Kerberos and LDAP, authentication is seamless, enhancing security and user interaction. This success reflects our preparedness for intricate IT landscapes, propelling us forward on our journey.


</details>

