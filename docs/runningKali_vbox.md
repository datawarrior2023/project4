## DVWA on Kali
Kali (kali/kali)
Metasploit2 (msfadmin/msfadmin)

<details>

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

## Using Nikto on DVWA

<details>

![image](https://github.com/user-attachments/assets/d253a0b5-05a6-4ef1-a63b-2980d5bb58f3)
![image](https://github.com/user-attachments/assets/37897a65-b4f2-4471-80f6-a82e26ef6e7e)
The results of your Nikto scan on the Damn Vulnerable Web Application (DVWA) provide several insights into potential security vulnerabilities and misconfigurations. Here's a breakdown of the findings:

### 1. Missing HTTP Headers
- **X-Frame-Options**: This header is not present, which means that this site could be vulnerable to clickjacking attacks. Clickjacking involves an attacker tricking a user into clicking something different from what the user perceives, potentially revealing confidential information or allowing others to take control of their computer while clicking seemingly innocuous web pages.
- **X-Content-Type-Options**: This header is also missing. Without this header, browsers might try to guess the MIME type of a document and will execute the document if they guess it to be executable. This can lead to XSS (Cross-Site Scripting) attacks.

### 2. Server and Directory Information
- **Apache/2.4.58 (Debian)**: The server is running on Apache version 2.4.58 on Debian. While this doesn’t indicate a direct vulnerability, it's crucial to keep the software up-to-date as new patches are released.
- Various directories are found to be indexable, which means that the files and subdirectories within them are listed when there is no index file (like index.html). This can lead to information disclosure about the file structure and potential access to sensitive files:
  - **/DVWA/config/**
  - **/DVWA/config/js/**
  - **/DVWA/tests/**
  - **/DVWA/database/**
  - **/DVWA/docs/**
  - **/DVWA/login/**
  - **/DVWA/phpmyadmin/**

### 3. Git Repository Exposure
- **.git Exposure**: The presence of .git directories and files (like .git/index, .git/config) being accessible from the web server could potentially expose sensitive information about the source code and its history. An attacker could download the entire repository to analyze it for vulnerabilities, hardcoded secrets, or configuration details.

### 4. PHP Details
- **/DVWA/phpinfo.php**: The presence of a phpinfo() file that is accessible can reveal a wealth of information about the PHP environment, server configuration, and more, which can be useful for an attacker to craft further attacks.

### 5. Docker File Exposure
- **.dockerignore File**: The exposure of .dockerignore file can give an attacker information about patterns of files and directories marked for omission in Docker build contexts, potentially revealing sensitive files that are not included in the build context.

### Action Steps:
- **Headers**: Add the missing HTTP headers (`X-Frame-Options` and `X-Content-Type-Options`) to improve security against common web attacks.
- **Directory Listing**: Disable directory listing on the server to prevent attackers from discovering files and directories.
- **Secure Git**: Ensure that .git directories are not accessible via the web server. This might involve configuring server rules or .htaccess files to deny access to .git paths.
- **Secure PHP Info**: Restrict access to the `phpinfo.php` file as it should not be accessible in a production environment due to the detailed information it provides.
- **Review Docker Files**: Ensure that sensitive information is not inadvertently exposed through Docker configuration files.

These findings underscore the importance of proper server configuration and the need for regular security reviews to ensure that no sensitive information is unnecessarily exposed.

### Attack

</details>

## Metaspliotable

<details>

To scan your Metasploitable2 instance using Nikto from your Kali Linux system, you will need the IP address of the Metasploitable2 virtual machine, which from your screenshot appears to be `10.0.2.15`. Here's how to proceed with the scan:

### Steps to Run Nikto on Metasploitable2

1. **Open Terminal**:
   - You can do this by pressing `Ctrl + Alt + T` or by searching for "Terminal" in your applications menu on Kali Linux.

2. **Run Nikto**:
   - Use the following command to initiate the Nikto scan against the Metasploitable2 IP:
     ```bash
     nikto -h 10.0.2.15
     ```
   - This command tells Nikto to scan the host located at `10.0.2.15`.

3. **Review the Output**:
   - Nikto will output its findings directly in the terminal. This will include any common vulnerabilities found on the Metasploitable2 server.
   - Look for issues related to server misconfigurations, vulnerable scripts, outdated server software, and potential exposure of sensitive files.

4. **Analyze and Act**:
   - Once the scan is complete, analyze the output to understand the vulnerabilities.
   - Determine which issues are critical and plan for patches or configurations to mitigate these vulnerabilities.

### Additional Options

- **Detailed Scan**:
  You can increase the detail and scope of the scan by using additional options such as:
  ```bash
  nikto -h 10.0.2.15 -Tuning x
  ```
  Replace `x` with numbers 0-9 to specify what types of tests to perform, depending on what you are focusing on (e.g., SQL Injection, XSS, etc.).

- **Port Specification**:
  If Metasploitable2 services run on non-standard ports that you wish to test, you can specify the port with:
  ```bash
  nikto -h 10.0.2.15 -p 8080
  ```
  Replace `8080` with the port number of interest.

### Considerations

- **Permissions**: Make sure you have the right to scan the Metasploitable2 machine. Even though it's a local and intentional vulnerable machine designed for practice, it's good practice to consider the ethics and legality.
- **Network Settings**: Ensure that both your Kali and Metasploitable2 VMs are on the same network or are configured to see each other on your virtual box settings. Typically, setting network adapters to 'Host-Only' or 'Internal Network' will work if you are not using 'Bridged Adapter'.

Following these steps, you can effectively use Nikto to scan your Metasploitable2 VM from your Kali Linux setup, identifying potential vulnerabilities and learning more about how these vulnerabilities can be exploited and mitigated.

![image](https://github.com/user-attachments/assets/d22f3d1e-ee1a-480d-bc96-69d1be95879b)
The results from your Nikto scan against the Metasploitable2 machine reveal a couple of common vulnerabilities associated with web server configurations. Here's a breakdown of the findings and their potential impact:

### 1. Missing HTTP Headers

- **X-Frame-Options Header Missing**:
  - **Vulnerability**: This header helps protect your website against clickjacking attacks. Without it, attackers can embed your website in a frame on their malicious site, potentially tricking users into interacting with your site in unintended ways.
  - **Impact**: Users could be tricked into executing actions on your site without their knowledge, such as changing their passwords or making transactions.

- **X-Content-Type-Options Header Missing**:
  - **Vulnerability**: This header prevents the browser from interpreting files as a different MIME type than what is specified by the server. Without this header, browsers can MIME-sniff the content and execute non-executable MIME types, leading to XSS (Cross-Site Scripting) attacks.
  - **Impact**: Allows an attacker to perform XSS attacks by uploading specially crafted files that the browser might execute as an executable.

### 2. Server Configuration

- **ETags (Entity Tags) Configuration**:
  - **Vulnerability**: The server's use of ETags can leak inode information through the HTTP headers. Inodes are identifiers for files and directories in UNIX-like operating systems.
  - **Impact**: An attacker can gain insights into the system's file system structure, which could help in designing further attacks or understanding the server setup.

### 3. Allowed HTTP Methods

- **OPTIONS Method Enabled**:
  - **Vulnerability**: By allowing various HTTP methods (GET, POST, OPTIONS, HEAD), particularly OPTIONS, an attacker can discover potential points of interaction with the server that might be vulnerable.
  - **Impact**: OPTIONS method reveals the HTTP methods that the server supports on a particular URL, which can aid an attacker in crafting specially designed requests that could exploit other vulnerabilities.

### Mitigation Strategies

- **Configure HTTP Headers**:
  - **X-Frame-Options**: Set this to "SAMEORIGIN" or "DENY" to prevent your site from being framed by other sites. This can be configured in your Apache or Nginx configuration file.
    ```apache
    Header always append X-Frame-Options SAMEORIGIN
    ```
  - **X-Content-Type-Options**: Set this to "nosniff" to prevent MIME-sniffing.
    ```apache
    Header set X-Content-Type-Options nosniff
    ```

- **Secure ETags**:
  - Modify the Apache configuration to either not use ETags or configure them not to leak inode information.
    ```apache
    FileETag None
    ```

- **Restrict HTTP Methods**:
  - Restrict the use of HTTP methods to only those necessary for your application. This can be done via server configuration.
    ```apache
    <LimitExcept GET POST>
      Order Deny,Allow
      Deny from all
    </LimitExcept>
    ```

If the recommended security headers (`X-Frame-Options` and `X-Content-Type-Options`) are not configured on your Apache server, several security vulnerabilities can be exploited. Here's a breakdown of the potential attacks and their implications:

### 1. **Clickjacking Attacks**
- **Header**: `X-Frame-Options`
- **Risk**: Without the `X-Frame-Options` header, your website can be embedded in a frame or iframe on another site. Malicious actors can use this technique, known as clickjacking, to trick users into clicking on elements that appear to be on their site but are actually on another site, potentially leading to unintended actions such as liking a page, making a purchase, or changing their password.

### 2. **MIME Sniffing Attacks**
- **Header**: `X-Content-Type-Options`
- **Risk**: Without the `X-Content-Type-Options: nosniff` header, browsers might try to infer the MIME type of a resource, overriding the server-specified `Content-Type` header. This can allow an attacker to upload a malicious file with a disguised MIME type (like a disguised executable file with a `.jpg` extension) that the browser executes. This vulnerability can lead to Cross-Site Scripting (XSS) attacks and other types of code execution vulnerabilities.

### 3. **Cross-Site Scripting (XSS)**
- **Related Headers**: Both headers indirectly help prevent XSS:
  - **`X-Frame-Options`**: By preventing your site from being framed, it reduces the attack surface for XSS attacks that rely on framing techniques.
  - **`X-Content-Type-Options`**: Prevents script execution from non-script MIME types, reducing the possibility of XSS exploits via MIME type confusion.

### 4. **Cross-Site Request Forgery (CSRF)**
- **Related Headers**: Mainly `X-Frame-Options`
- **Risk**: While primarily defended against by other means (like CSRF tokens), clickjacking can be part of a CSRF attack where an attacker tricks a user into executing unwanted actions on a web application in which they are authenticated. If an application can be framed, it might be possible to engineer a clickjacking attack that also doubles as a CSRF exploit.

### 5. **Code Injection**
- **Related Headers**: Mainly `X-Content-Type-Options`
- **Risk**: Allowing the browser to sniff MIME types can lead to unexpected script execution when files with ambiguous extensions or content are interpreted as executable code, leading to potential malicious code injections.

### 6. **Information and Identity Theft**
- **Related Headers**: Both headers
- **Risk**: Through clickjacking, attackers could potentially redirect users to malicious websites or overlay transparent frames to capture keystrokes and sensitive information. 

### Conclusion
Implementing these headers is a straightforward but crucial aspect of securing web applications. It helps ensure that content is displayed as intended, without unwanted manipulation, and prevents the browser from making potentially dangerous assumptions about the data it handles. By configuring these headers, you significantly reduce the risk of these types of web attacks, enhancing the overall security posture of your application.
Implementing these mitigations will help secure your server against the vulnerabilities identified in the Nikto scan, enhancing the overall security posture of your system.

![image](https://github.com/user-attachments/assets/649e75b8-6568-4dfa-88e8-0cccc61af0fd)

</details>

![image](https://github.com/user-attachments/assets/afb1ec5c-396c-4de5-aed9-3a6f08b0e3c6)

![image](https://github.com/user-attachments/assets/1210b175-8b41-41b9-8471-ada032bf8945)

From your Nikto scan results, we can identify the changes and implications when security headers are enabled versus when they are commented out. Let’s break down the differences between the two scans and explain their significance:

### Scan with Security Configurations Enabled

- **Key Findings:**
  - Security headers such as `X-Frame-Options` and `X-Content-Type-Options` are likely properly set as Nikto does not report their absence.
  - The scan reports vulnerabilities associated with outdated server software and enabled HTTP TRACE method, but there is no mention of missing `X-Frame-Options` or `X-Content-Type-Options`, indicating that these headers were effective.

### Scan with Security Configurations Disabled (Headers Commented Out)

- **Key Findings:**
  - **`X-Frame-Options` Missing**: Nikto specifically points out that the `X-Frame-Options` header is missing. This header is crucial for preventing clickjacking attacks by restricting how content can be framed. Its absence means that the site is potentially vulnerable to such attacks.
  - **`X-Content-Type-Options` Missing**: The absence of this header allows browsers to perform MIME type sniffing, which can lead to security issues if the browser incorrectly interprets the content type of files, potentially executing non-executable MIME types.
  - Both of these findings suggest a reduction in security due to the headers being commented out.

### Consistent Findings in Both Scans

- **Outdated Apache Version**: Both scans alert about the use of an outdated Apache version, suggesting susceptibility to vulnerabilities known in those versions.
- **Enabled HTTP TRACE Method**: Both versions indicate that the HTTP TRACE method is active, which could be exploited in Cross-Site Tracing (XST) attacks.
- **Mod Negotiation and MultiViews**: Reported in both scans, indicating that the server allows attackers to brute force file names, which could lead to unauthorized access or information disclosure.

### Conclusion

The direct comparison shows that commenting out the security headers (`X-Frame-Options` and `X-Content-Type-Options`) directly impacts the security posture reported by Nikto, highlighting their importance in protecting against specific web-based attacks like clickjacking and MIME type sniffing vulnerabilities. 

### Recommendations

1. **Re-enable Security Headers**: Ensure that both `X-Frame-Options` and `X-Content-Type-Options` are enabled to safeguard against the respective vulnerabilities they are designed to mitigate.
2. **Update Server Software**: Consider updating the Apache server to the latest supported version to protect against vulnerabilities known in older versions.
3. **Disable HTTP TRACE**: If not needed, disable the HTTP TRACE method to protect against XST attacks.
4. **Review and Harden Apache Configuration**: Regularly review and update the Apache configuration to minimize vulnerabilities, especially concerning file access and server information disclosure.

Implementing these measures will enhance your server’s defense against common web security threats.
