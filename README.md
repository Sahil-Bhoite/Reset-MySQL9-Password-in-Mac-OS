# Comprehensive MySQL 9 Password Reset Guide for macOS 15 (Sequoia)

By Sahil Bhoite
- Email: work.sahilbhoite@gmail.com
- LinkedIn: https://www.linkedin.com/in/sahil-bhoite

This detailed guide provides step-by-step instructions for resetting MySQL 9 database passwords on macOS 15 (Sequoia). It covers both non-root and root user account password resets, common errors, and troubleshooting tips.

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Common Errors and Solutions](#common-errors-and-solutions)
4. [Resetting Non-Root User Password](#resetting-non-root-user-password)
5. [Resetting Root User Password](#resetting-root-user-password)
6. [Verifying the Password Reset](#verifying-the-password-reset)
7. [Troubleshooting](#troubleshooting)
8. [Best Practices](#best-practices)
9. [Contact Information](#contact-information)

## Introduction

If you've forgotten your MySQL 9 database user password on macOS 15 (Sequoia), this comprehensive guide will help you reset it. We'll cover two scenarios: resetting a non-root user password and resetting the root user password. This guide assumes you have some familiarity with using the Terminal and basic MySQL commands.

## Prerequisites

Before starting, ensure you have:

1. macOS 15 (Sequoia) installed
2. MySQL 9 installed and previously configured
3. Administrative access to your macOS account
4. Terminal application ready

## Common Errors and Solutions

### Error 1: Can't connect to local MySQL server

```
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock'
```

This error occurs when your MySQL server instance is not running.

**Solution:**
1. Open System Settings (Apple menu > System Settings)
2. Search for "MySQL" in the search bar
3. Click on the MySQL preference pane
4. If the status shows "Stopped" or a red icon, click "Start MySQL Server"
5. Enter your macOS administrator password if prompted
6. Wait for the status to change to "Running" with a green icon

### Error 2: Access denied

```
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```

This error appears when you enter an invalid database user password.

**Solution:** Follow the password reset procedures outlined in the following sections.

## Resetting Non-Root User Password

1. Open Terminal (Applications > Utilities > Terminal)

2. Connect to MySQL as root:
   ```
   mysql -u root -p
   ```
   Enter your root password when prompted

3. If you can't log in as root, proceed to the "Resetting Root User Password" section

4. Once logged in, select the MySQL database:
   ```sql
   USE mysql;
   ```

5. View the list of users to confirm the username you want to reset:
   ```sql
   SELECT user, host FROM user;
   ```

6. Change the password for the desired user:
   ```sql
   ALTER USER 'username'@'hostname' IDENTIFIED BY 'new_password';
   ```
   Replace `username`, `hostname`, and `new_password` with appropriate values. 
   For example:
   ```sql
   ALTER USER 'john'@'localhost' IDENTIFIED BY 'new_secure_password123!';
   ```

7. Reload the privileges to ensure the changes take effect:
   ```sql
   FLUSH PRIVILEGES;
   ```

8. Exit MySQL:
   ```sql
   QUIT;
   ```

## Resetting Root User Password

1. Stop the MySQL server:
   - Open System Settings
   - Search for and select MySQL
   - Click "Stop MySQL Server"
   - Enter your macOS administrator password if prompted

2. Open Terminal and ensure all MySQL processes are terminated:
   ```
   sudo pkill mysqld
   ```
   Enter your macOS administrator password when prompted

3. Start MySQL in safe mode (this allows you to connect without a password):
   ```
   sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables --skip-networking
   ```
   Note: The path may vary depending on your MySQL installation. Adjust if necessary.

4. Open a new Terminal window while keeping the previous one running

5. Connect to MySQL without a password:
   ```
   /usr/local/mysql/bin/mysql -u root
   ```

6. Once connected, reload the grant tables:
   ```sql
   FLUSH PRIVILEGES;
   ```

7. Set the new root password:
   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_root_password';
   ```
   Replace `new_root_password` with a strong, secure password of your choice

8. Reload privileges and exit:
   ```sql
   FLUSH PRIVILEGES;
   QUIT;
   ```

9. Go back to the first Terminal window and stop the safe mode MySQL instance:
   Press Ctrl+C to stop the process

10. Restart MySQL normally:
    - Return to System Settings > MySQL
    - Click "Start MySQL Server"
    - Enter your macOS administrator password if prompted

## Verifying the Password Reset

1. Open a new Terminal window

2. Try connecting with the new password:
   ```
   mysql -u root -p
   ```
   or for a non-root user:
   ```
   mysql -u username -p
   ```

3. Enter the new password when prompted

4. If successful, you should see the MySQL prompt. Type `QUIT;` to exit

## Troubleshooting

If you encounter issues during this process, try the following:

- Ensure you're using the correct MySQL binary path. You can find it by running:
  ```
  which mysql
  ```
  
- Check MySQL server status in System Settings or via Terminal:
  ```
  sudo /usr/local/mysql/support-files/mysql.server status
  ```

- Verify that you're using the correct username and hostname combination

- If you get a "command not found" error, ensure MySQL bin directory is in your PATH:
  ```
  export PATH=$PATH:/usr/local/mysql/bin
  ```

- Check MySQL error logs for any specific issues:
  ```
  sudo tail -n 100 /usr/local/mysql/data/$(hostname).err
  ```

## Best Practices

1. Use strong, unique passwords for all MySQL users
2. Regularly update MySQL to the latest stable version
3. Limit root account usage and create specific users for different applications
4. Implement regular backups of your MySQL databases
5. Consider using a password manager to securely store your database credentials

Note: These instructions are based on MySQL 9 and macOS 15 (Sequoia). While every effort has been made to ensure accuracy, there might be slight variations depending on specific configurations or updates. Always refer to the official MySQL documentation for the most up-to-date and version-specific information.

For further assistance, consult the official MySQL 9 documentation or seek help in MySQL community forums.

## Contact Information

If you have any questions, suggestions, or need further clarification, please don't hesitate to reach out:

- **Author:** Sahil Bhoite
- **Email:** work.sahilbhoite@gmail.com
- **LinkedIn:** https://www.linkedin.com/in/sahil-bhoite

Your feedback is valuable and helps improve this guide for future users. Thank you for using this MySQL password reset guide!

