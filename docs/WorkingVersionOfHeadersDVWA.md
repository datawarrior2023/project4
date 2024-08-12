## Steps to get DVWA up on Kali

<details>
  <summary>Click for Details</summary>

Here's a simplified step-by-step guide on how you set up Damn Vulnerable Web Application (DVWA) on Kali Linux, including troubleshooting some common issues encountered during the installation and configuration process:

### Step-by-Step Guide to Set Up DVWA

#### **Preparation and Installation**
1. **Checking Nikto Installation**
   - **Action**: Open Terminal and type `nikto -h`.
   - **Expected Outcome**: Displays Nikto help screen if installed.
   - **Troubleshooting**: If not found, run `sudo apt-get update` followed by `sudo apt-get install nikto`.

2. **Installing DVWA Dependencies**
   - **Action**: Install Apache, MySQL, PHP, and other necessary libraries with:
     ```
     sudo apt-get install apache2 mysql-server php php-mysqli php-gd libapache2-mod-php
     ```
   - **Note**: Ensure all services are properly installed and can start without errors.

3. **Starting Services**
   - **Action**: Start Apache and MySQL services:
     ```
     sudo systemctl start apache2
     sudo systemctl start mysql
     ```
   - **Troubleshooting**: If services fail to start, check the status using `sudo systemctl status apache2` or `mysql` for error messages.

#### **Configuration and Setup**
4. **Downloading and Configuring DVWA**
   - **Action**: Clone DVWA from GitHub:
     ```
     cd /var/www/html
     sudo git clone https://github.com/digininja/DVWA.git
     ```
   - **Action**: Set appropriate permissions:
     ```
     sudo chown -R www-data:www-data /var/www/html/DVWA
     ```
   - **Action**: Copy and configure the DVWA configuration file:
     ```
     sudo cp /var/www/html/DVWA/config/config.inc.php.dist /var/www/html/DVWA/config/config.inc.php
     sudo nano /var/www/html/DVWA/config/config.inc.php
     ```
     Update database settings (`db_user`, `db_password`, `db_database`).

5. **Database Configuration**
   - **Action**: Secure MySQL and set up DVWA database:
     ```
     sudo mysql_secure_installation
     sudo mysql -u root -p
     CREATE DATABASE dvwa;
     ```
   - **Troubleshooting**: If unable to login to MySQL, reset the root password or ensure that MySQL service is running.

#### **Finalizing DVWA Setup**
6. **Accessing and Setting Up DVWA**
   - **Action**: Open a browser and navigate to `http://localhost/DVWA/setup.php`.
   - **Action**: Initialize the DVWA database by clicking 'Create / Reset Database'.
   - **Expected Outcome**: DVWA is ready to use at `http://localhost/DVWA/login.php`.
   - **Troubleshooting**: If you encounter connection issues, confirm the Apache and MySQL services are running and that the DVWA configuration file points to the correct database details.

#### **Validation and Verification**
7. **Testing with Nikto**
   - **Action**: After setup, run Nikto to scan for vulnerabilities:
     ```
     nikto -h localhost
     ```
   - **Expected Outcome**: Nikto should report potential issues or confirm no critical vulnerabilities, depending on your security settings.

#### **Common Issues and Solutions**
- **MySQL Access Denied**: Common if the MySQL root password is forgotten or not set correctly during installation. Resetting the password using safe mode can resolve this.
- **Apache/MySQL Services Not Starting**: Often due to misconfigurations or port conflicts. Checking the status and logs can help identify and resolve these issues.
- **Permission Issues with DVWA**: Ensuring the web server user (`www-data`) has proper permissions on the DVWA directory is crucial for functionality.

This guide not only outlines the steps to get DVWA up and running but also incorporates troubleshooting steps that were critical during your setup process. The insights gained here are valuable for ensuring a smooth setup experience and for understanding common pitfalls that may occur during similar installations.

</details>

## Steps to start up DVWA aftet it is installed on Kali

<details>
  <summary>Click for Details</summary>

To properly shut down and restart the Damn Vulnerable Web Application (DVWA) along with its underlying services like Apache and MySQL on your Kali Linux system, follow these steps:

### Shutting Down DVWA and Services

1. **Stop Apache and MySQL Services**:
   - To stop the Apache web server and MySQL database, you can use the following commands:
     ```bash
     sudo systemctl stop apache2
     sudo systemctl stop mysql
     ```

### Restarting DVWA and Services

1. **Start Apache and MySQL Services**:
   - To restart the services, you use similar commands:
     ```bash
     sudo systemctl start apache2
     sudo systemctl start mysql
     ```

2. **Verify Services are Running**:
   - After restarting the services, you can check their status to ensure they are running properly:
     ```bash
     sudo systemctl status apache2
     sudo systemctl status mysql
     ```
   - This will provide you with the active status and any current activity logs for these services.

3. **Access DVWA**:
   - Once the services are up and running, you can access DVWA by navigating to `http://localhost/DVWA` or `http://localhost/DVWA/login.php` in your web browser.
   - Log in with the credentials you set up (default is usually `admin` for the username and `password` for the password).

### Regular Maintenance and Checks

- **Check Apache and MySQL Logs**:
  - Regularly checking the logs can help you identify and resolve issues quickly. Logs for Apache are typically found in `/var/log/apache2/`, and for MySQL in `/var/log/mysql/`.
  
- **Update System and Applications**:
  - Keep your system and applications like DVWA, Apache, and MySQL updated to ensure you have the latest security patches and features:
    ```bash
    sudo apt-get update
    sudo apt-get upgrade
    ```

- **Backup Configuration Files**:
  - Before making significant changes, especially when updating or reconfiguring, it’s a good practice to backup configuration files like Apache’s `apache2.conf` or DVWA’s `config.inc.php`.

By following these steps, you can ensure that DVWA and its dependent services are properly shut down and restarted, maintaining a secure and stable environment for your cybersecurity testing and practice. If you have any further questions or need additional details, feel free to ask!

</details>
