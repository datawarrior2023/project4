### Checking if Nikto is Installed on Kali Linux

To check if Nikto is already installed on your Kali Linux system, you can follow these steps:

1. **Open the Terminal:** You can do this by searching for "Terminal" in your applications menu or by pressing `Ctrl + Alt + T`.

2. **Check for Nikto:**
   - Type the following command and press Enter:
     ```
     nikto -h
     ```
   - This command attempts to run Nikto. If it's installed, you'll see a help screen that lists all command options. If not, you'll receive an error indicating that Nikto is not found.

Side NOTEW

<details>

   ![image](https://github.com/user-attachments/assets/3eb2a9ee-1642-4753-b10f-0847716e5d8f)

   
</details>
### Installing Nikto on Kali Linux

If Nikto is not installed, you can install it using the following steps:

1. **Update Your Package List:**
   - Run the following command to ensure your package list is up-to-date:
     ```
     sudo apt-get update
     ```

2. **Install Nikto:**
   - Use the following command to install Nikto:
     ```
     sudo apt-get install nikto
     ```

3. **Verify Installation:**
   - Once the installation is complete, verify it by running:
     ```
     nikto -h
     ```
   - This should display the help screen for Nikto, confirming that it is properly installed.

### Installing Damn Vulnerable Web Application (DVWA)

For DVWA, you need a web server environment that supports PHP and a database like MySQL. Here’s how to install DVWA on your Kali Linux:

1. **Install Dependencies:**
   - You need Apache, MySQL, and PHP. Install them using:
     ```
     sudo apt-get install apache2 mysql-server php php-mysqli php-gd libapache2-mod-php
     ```

2. **Start Apache and MySQL Services:**
   - Ensure the services are running:
     ```
     sudo systemctl start apache2
     sudo systemctl start mysql
     ```

3. **Download DVWA:**
   - Clone the DVWA repository from GitHub:
     ```
     cd /var/www/html
     sudo git clone https://github.com/digininja/DVWA.git
     ```

4. **Permissions and Configuration:**
   - Change the permissions of the DVWA folder:
     ```
     sudo chown -R www-data:www-data /var/www/html/DVWA
     ```
   - Copy the config file:
     ```
     sudo cp /var/www/html/DVWA/config/config.inc.php.dist /var/www/html/DVWA/config/config.inc.php
     ```

5. **Configure the Database Connection:**
   - Edit the configuration file:
     ```
     sudo nano /var/www/html/DVWA/config/config.inc.php
     ```
   - Update the database connection settings inside the file:
     ```
     $_DVWA[ 'db_user' ] = 'root';
     $_DVWA[ 'db_password' ] = 'p@ssw0rd';  # change this to your MySQL root password
     $_DVWA[ 'db_database' ] = 'dvwa';
     ```

6. **Create DVWA Database:**
   - Log into MySQL:
     ```
     sudo mysql -u root -p
     ```
   - Create the DVWA database:
     ```
     CREATE DATABASE dvwa;
     EXIT;
     ```

7. **Setup DVWA:**
   - Open your browser and navigate to:
     ```
     http://localhost/DVWA/setup.php
     ```
   - Click on the 'Create / Reset Database' button. This will set up the tables needed for DVWA.

8. **Verify DVWA Installation:**
   - After setting up the database, go to the DVWA login page:
     ```
     http://localhost/DVWA/login.php
     ```
   - The default credentials are usually `admin` for the username and `password` for the password.

### Validation Steps
- **Nikto:** Running `nikto -h` should display the help options.
- **DVWA:** After logging in to DVWA, you should be able to access various vulnerability exercises from the DVWA home screen.

This setup provides a controlled environment for practicing web application penetration testing safely. Let me know if you need any further assistance!

## TROUBLSHOOTING
### Resetting MySQL
When you install MySQL on Kali Linux or any other system, you typically set the root password during the installation process. If you've installed MySQL but don't recall setting a password, or if you're unsure what it is, you can follow these steps to reset it:

### Checking Your MySQL Root Password

If you're unsure whether you have set a MySQL root password, you can try logging into MySQL with a blank password:

1. **Open Terminal:**
   - Launch your terminal by pressing `Ctrl + Alt + T`.

2. **Try Logging In:**
   - Enter the following command:
     ```
     mysql -u root -p
     ```
   - When prompted for a password, try pressing Enter without typing anything (this tests for a blank password).

If you can't log in, or if you want to reset the password just in case, follow these steps:

### Resetting Your MySQL Root Password

1. **Stop MySQL Service:**
   - First, stop the MySQL service to allow for safe changes:
     ```
     sudo systemctl stop mysql
     ```

2. **Start MySQL in Safe Mode:**
   - Start MySQL with the `--skip-grant-tables` option, which allows you to access the database without a password:
     ```
     sudo mysqld_safe --skip-grant-tables &
     ```

3. **Log into MySQL:**
   - Now, connect to MySQL as the root user:
     ```
     mysql -u root
     ```

4. **Change the Root Password:**
   - Use the following MySQL commands to reset the root password:
     ```sql
     FLUSH PRIVILEGES;
     ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpassword';
     FLUSH PRIVILEGES;
     ```
   - Replace `newpassword` with your new password.

5. **Exit MySQL and Restart the Service:**
   - Type `exit` to leave MySQL.
   - Stop the MySQL safe mode and start the normal service:
     ```
     sudo killall mysqld
     sudo systemctl start mysql
     ```

6. **Verify the New Password:**
   - Try logging in to MySQL again with your new password:
     ```
     mysql -u root -p
     ```
   - Enter your new password when prompted.

### Setting the Password in DVWA Configuration

After resetting and verifying your MySQL root password, update the DVWA configuration file (`config.inc.php`) with the new password:

- Open the DVWA configuration file:
  ```
  sudo nano /var/www/html/DVWA/config/config.inc.php
  ```
- Update the password line to reflect your new MySQL root password:
  ```
  $_DVWA[ 'db_password' ] = 'newpassword';  # Use the new root password you set
  ```
- Save and close the file.

By following these steps, you can ensure that your MySQL installation has a secure root password that you know, and you can correctly configure DVWA to connect to MySQL. If you need further assistance with any of these steps, feel free to ask!

Question
![image](https://github.com/user-attachments/assets/538773ea-b089-48a0-924b-d06bcfd88023)

It looks like you encountered an error trying to access MySQL, and then when you tried to restart MySQL in safe mode, it couldn't find the socket. This typically happens if MySQL isn't running or there's a misconfiguration in the MySQL socket path. Let's address these issues:

### Step 1: Verify MySQL Service Status
First, ensure that MySQL is actually running:

```bash
sudo systemctl status mysql
```

If it's not running:

```bash
sudo systemctl start mysql
```

### Step 2: Locate the MySQL Socket
Sometimes, the socket file might be in a different location or not properly configured. You can specify the socket path directly when connecting to MySQL:

1. **Find the MySQL Socket:**
   - Often, the socket file is located at `/var/run/mysqld/mysqld.sock`. Check if it exists:
     ```bash
     ls /var/run/mysqld/mysqld.sock
     ```

2. **If the Socket Exists:**
   - Try connecting using the socket path:
     ```bash
     mysql -u root -p --socket=/var/run/mysqld/mysqld.sock
     ```

### Step 3: Restart MySQL in Safe Mode with Specified Socket
If MySQL is running but you still need to reset the password and are facing socket issues, start MySQL in safe mode with the specified socket path:

```bash
sudo systemctl stop mysql
sudo mysqld_safe --skip-grant-tables --socket=/var/run/mysqld/mysqld.sock &
```

### Step 4: Reset the Password
Once in safe mode, follow the previous instructions to reset your MySQL root password.

### Step 5: Configuration File Check
Also, ensure that the MySQL configuration file points to the correct socket. Open the MySQL configuration file:

```bash
sudo nano /etc/mysql/my.cnf
```

Look for the lines under `[mysqld]` that specify `socket`. It should look like this:
```
[mysqld]
socket=/var/run/mysqld/mysqld.sock
```

If changes are made, restart MySQL:
```bash
sudo systemctl restart mysql
```

Try these steps and let me know if you're able to proceed or if you encounter further issues!
