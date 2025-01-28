# GitHub Timeline Assignment 📜📧

This repository contains a PHP-based application that allows users to subscribe with their email addresses and receive updates from the GitHub timeline every five minutes. The project adheres to clean coding principles and provides a robust, demo-ready solution for the assignment.

---

## 🖊️ Problem Description

The application solves the following:

1. **Subscription System**: Enables visitors to subscribe using their email addresses.
2. **Email Verification**: Ensures users verify their email to prevent unauthorized subscriptions.
3. **GitHub Timeline Updates**: Tracks changes on [GitHub Timeline](https://github.com/timeline) and sends updates.
4. **Automated Email Updates**: Sends new updates to subscribers every five minutes.
5. **Structured Emails**: Provides well-formatted and readable emails with navigation links.
6. **Unsubscribe Option**: Includes a link for users to opt out of receiving updates.

---

## 🛠️ Methodology

The project workflow is as follows:

1. **Subscription Form**:
   - The `index.php` file displays a form with input fields for First Name, Last Name, and Email Address.
   - Upon submission, the form data is sent to `validation.php`.
   - ![index.php](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/index.png?raw=true)

2. **Validation**:
   - In `validation.php`, the email address is validated against `filter_var($email, FILTER_VALIDATE_EMAIL)`.
   - If the email is valid, the flow proceeds to `helper/check.php`.
   - If invalid, an error message is displayed to the user.
   - ![validation.php](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/validation.png?raw=true)

3. **Email Authentication**:
   - In `check.php`, the email is checked against the database:
     - If the email exists and is authenticated, the user is shown a message on `subscribed.php` that they are already subscribed.
     - If the email exists but is not authenticated, the old entry is deleted.
     - A new database entry is created with `is_auth = 0`.
   - The flow moves to `verify.php`.

4. **Verification**:
   - In `verify.php`, a unique verification token is generated and stored in the `vauth` column of the database.
   - The token is stored as a **string** for security reasons to preserve leading zeros, ensuring consistent length and complexity. This reduces predictability, strengthening resistance against brute-force attacks.
   - An email containing the token is sent to the user.
   - The user inputs the token on the same page to verify their email address.
   - If the token matches, `is_auth` is set to `1` in the database, and the user is redirected to `subscribed.php`.
   - ![verify.php](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/verify_unsub_verify.png?raw=true)

5. **Unsubscribe Process**:
   - Users can unsubscribe using the link in the email or a button on `subscribed.php`.
   - ![subscribed.php](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/subscribed.png?raw=true)
   - The link directs them to `unsubscribe.php`, where they confirm their intention to unsubscribe.
   - ![unsubscribe.php](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/unsub.png?raw=true)
   - Upon confirmation, a new token is generated and stored in the `unsub_auth` column.
   - The flow moves to `unsub_verify.php`, where the user inputs the token to confirm unsubscription.
   - ![unsub_verify.php](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/verify_unsub_verify.png?raw=true)
   - If the token matches, the user is removed from the subscription list, and `unsub_auth` is cleared. The user is redirected to `unsub.php` with a confirmation message.
   - ![unsub_auth.php](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/unsubscribe.png?raw=true)

6. **Cron Job**:
   - A cron job runs every five minutes, sending emails to authenticated subscribers.
   - ![cron](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/cron.png?raw=true)
   - The email content is parsed and formatted from the latest entries of the GitHub timeline.
   - ![email](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/email.png?raw=true)
   - ![email unsub link](https://github.com/shrivastavanolo/shrivastavanolo/blob/main/images/email2.png?raw=true)

---

## 🗄️ Database Structure

| Column Name   | Data Type | Description                                   |
|---------------|-----------|-----------------------------------------------|
| `fname`       | STRING    | First name of the subscriber.                |
| `lname`       | STRING    | Last name of the subscriber.                 |
| `email`       | STRING    | Email address of the subscriber.             |
| `is_auth`     | INT       | Authentication status (0 = No, 1 = Yes).     |
| `vauth`       | STRING    | Verification token for email authentication. |
| `unsub_auth`  | STRING    | Token for email unsubscription.              |

---

## 🚒 Features

- **Object-Oriented Programming (OOP)**: Follows OOP principles for better maintainability.
- **Namespace Usage**: Organized code using PHP namespaces.
- **Inline Documentation**: Provides inline comments for clarity.
- **PHP Clean Code**: Adheres to clean code standards with proper indentation and spacing.
- **Cron Job**: Automates the email updates using a cron job.

---

## ⚙️ Technical Requirements

### **1. Email Verification**
- Verifies email addresses using tokens to ensure only valid subscriptions.

### **2. GitHub Timeline Updates**
- Tracks updates on [GitHub Timeline](https://github.com/timeline) for the latest changes.

### **3. Email Delivery**
- Sends updates to subscribers in a structured and navigable format.
- Includes an unsubscribe link in all emails.

### **4. Database Interaction**
- Stores subscriber information securely.
- Prevents SQL injection using prepared statements.

### **5. Security Practices**
- Escapes outputs and validates inputs to prevent XSS.
- Protects sensitive credentials and avoids hardcoding.

---

## 🔧 Project Setup

### **1. Prerequisites**
- PHP (>=8.2)
- MySQL
- A local mail server
- Cron access on the local machine

### **2. Installation**
1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```
2. Set up the database:
   - Create a MySQL database with the database structure above.
   - Update database credentials in the `config.php` file.

3. Configure mail settings in `php.ini` or the script.

4. Set up the cron job:
   ```bash
   */5 * * * * /path/to/php /path/to/cron_script.php >> /path/to/logfile.log 2>&1
   ```

---

## 🎤 Demo Instructions

1. Start your local server.
2. Use MailHog to capture and display emails.
3. Run the application and demonstrate:
   - Email subscription and verification.
   - GitHub timeline update delivery.
   - Unsubscribe functionality.

---

## 🔧 Troubleshooting

- **Emails Not Sending**: Ensure your local mail server is configured and running.
- **Cron Job Not Working**: Verify the cron job path and permissions.
- **Database Issues**: Check database credentials and connection.

---

Thank you for reviewing the GitHub Timeline Assignment!

