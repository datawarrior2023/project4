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
