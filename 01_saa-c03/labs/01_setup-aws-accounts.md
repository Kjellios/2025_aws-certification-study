# Lab 01 – Setup AWS Accounts

**Goal:**
Create and secure two AWS accounts (General and Production) with proper IAM configuration, MFA setup, billing alerts, and CLI access. This establishes the multi-account foundation for the entire course.

**Architecture Diagram:**
Link to file in `artifacts/FUNDAMENTALS/setup-diagram.png`

## Prerequisites
- Valid email address (Gmail recommended for plus trick)
- Credit/debit card for billing
- Mobile phone for MFA app (Google Authenticator, Authy, or similar)
- Computer with terminal/command prompt access

## Steps

### Part 1: Create General AWS Account

1. **Navigate to AWS Signup**
   - Go to https://aws.amazon.com
   - Click "Create an AWS Account"

2. **Enter Account Details**
   - Email: Use unique email (e.g., `yourname+aws-general@gmail.com`)
   - AWS Account name: `[YourInitials]-training-aws-general`
   - Password: Generate strong password and save in password manager

3. **Verify Email**
   - Check email for verification code
   - Enter code in AWS signup form
   - Click "Verify"

4. **Enter Contact Information**
   - Account type: Personal (for training)
   - Full name, phone, address
   - Accept AWS Customer Agreement
   - Click "Continue"

5. **Add Billing Information**
   - Enter credit/debit card details
   - Note: May see $1 pending charge for verification (disappears in 3-5 days)
   - Click "Verify and Continue"

6. **Identity Verification**
   - Choose SMS or Voice call
   - Enter phone number
   - Complete CAPTCHA if shown
   - Enter verification code received
   - Click "Continue"

7. **Select Support Plan**
   - Choose "Basic support - Free"
   - Click "Complete sign up"

8. **Wait for Activation**
   - Usually instant, can take up to 1 hour
   - Check email for activation confirmation

### Part 2: Secure General Account with MFA

9. **Login as Root User**
   - Use email and password from step 2
   - Complete CAPTCHA if prompted

10. **Enable Billing Access for IAM**
    - Click account dropdown → Account
    - Scroll to "IAM user and role access to billing"
    - Click "Edit"
    - Check "Activate IAM access"
    - Click "Update"

11. **Setup MFA for Root User**
    - Click account dropdown → Security credentials
    - Under "Multi-factor authentication", click "Assign MFA device"
    - Device name: `root-mfa-general`
    - Select "Authenticator app"
    - Click "Next"

12. **Configure MFA App**
    - Click "Show QR code"
    - Open authenticator app on phone
    - Add new account/scan QR code
    - Save as "AWS General Root"
    - Enter two consecutive codes from app
    - Click "Add MFA"

13. **Test MFA**
    - Sign out
    - Sign back in with email/password
    - Enter MFA code when prompted
    - Verify successful login

### Part 3: Create Budget

14. **Navigate to Billing Console**
    - Click account dropdown → Billing Dashboard

15. **Enable Billing Preferences**
    - Click "Billing preferences" in left menu
    - Check all boxes:
      - "Receive PDF invoice by email"
      - "Receive Free Tier usage alerts"
      - "Receive billing alerts"
    - Enter email for alerts
    - Click "Save preferences"

16. **Create Budget**
    - Click "Budgets" in left menu
    - Click "Create budget"
    - Choose template: "Zero spend budget" OR "Monthly cost budget"
    - If monthly: Enter amount (e.g., $10)
    - Budget name: `training-budget-general`
    - Email recipients: Your email
    - Click "Create budget"

### Part 4: Create IAM Admin User

17. **Navigate to IAM Console**
    - Search "IAM" in top search bar
    - Click on IAM service

18. **Create Account Alias**
    - On right side, click "Create" under Account Alias
    - Enter: `[unique-name]-general` (must be globally unique)
    - Click "Save changes"
    - Note the sign-in URL for IAM users

19. **Create IAM User**
    - Click "Users" in left menu
    - Click "Add users"
    - Username: `iamadmin`
    - Check "Provide user access to AWS Management Console"
    - Select "I want to create an IAM user"
    - Choose "Custom password"
    - Enter strong password (save in password manager)
    - Uncheck "Users must create new password at next sign-in"
    - Click "Next"

20. **Attach Admin Policy**
    - Select "Attach policies directly"
    - Search for "AdministratorAccess"
    - Check the box next to it
    - Click "Next"
    - Click "Create user"

21. **Add MFA to IAM Admin**
    - Sign out of root user
    - Navigate to IAM sign-in URL from step 18
    - Login as `iamadmin` with password from step 19
    - Click account dropdown → My security credentials
    - Under MFA, click "Assign MFA device"
    - Device name: `iamadmin-mfa-general`
    - Select "Authenticator app"
    - Scan QR code as new entry in app (don't reuse root MFA)
    - Enter two consecutive codes
    - Click "Add MFA"

22. **Test IAM Admin Login**
    - Sign out
    - Login with IAM URL, username, password, and MFA code
    - Verify successful login

### Part 5: Create Access Keys for CLI

23. **Create Access Keys**
    - While logged in as iamadmin
    - Account dropdown → Security credentials
    - Click "Create access key"
    - Select "Command Line Interface (CLI)"
    - Check acknowledgment box
    - Click "Next"
    - Description: `local-cli-iamadmin-general`
    - Click "Create access key"

24. **Save Credentials**
    - Click "Download .csv file"
    - Rename to: `iamadmin_accesskeys_general.csv`
    - Click "Done"
    - Store CSV securely (will delete after CLI config)

### Part 6: Create Production Account

25. **Repeat Steps 1-24 for Production Account**
    - Use different email: `yourname+aws-production@gmail.com`
    - Account name: `[YourInitials]-training-aws-production`
    - Account alias: `[unique-name]-production`
    - MFA entries: `root-mfa-production` and `iamadmin-mfa-production`
    - Access key file: `iamadmin_accesskeys_production.csv`
    - Budget name: `training-budget-production`

### Part 7: Install and Configure AWS CLI

26. **Install AWS CLI v2**
    - Download from: https://aws.amazon.com/cli/
    - Run installer for your OS (Windows/Mac/Linux)
    - Accept all defaults
    - Verify: `aws --version` (should show 2.x.x)

27. **Configure General Account Profile**
    ```bash
    aws configure --profile iamadmin-general
    ```
    - Access Key ID: [from general CSV]
    - Secret Access Key: [from general CSV]
    - Default region: `us-east-1`
    - Default output format: [press Enter for none]

28. **Configure Production Account Profile**
    ```bash
    aws configure --profile iamadmin-production
    ```
    - Access Key ID: [from production CSV]
    - Secret Access Key: [from production CSV]
    - Default region: `us-east-1`
    - Default output format: [press Enter for none]

29. **Delete CSV Files**
    - Securely delete both CSV files after configuration
    - Credentials are now stored in AWS CLI config

## Validation

Run these commands to verify setup:

```bash
# Test General account access
aws s3 ls --profile iamadmin-general
# Should return empty list or no error

# Test Production account access  
aws s3 ls --profile iamadmin-production
# Should return empty list or no error

# Verify CLI version
aws --version
# Should show: aws-cli/2.x.x

# Check configured profiles
aws configure list-profiles
# Should show: iamadmin-general and iamadmin-production
```

**MFA Verification:**
- You should have 4 entries in your authenticator app:
  1. AWS General Root
  2. AWS General IAMAdmin
  3. AWS Production Root
  4. AWS Production IAMAdmin

**Console Access URLs to Save:**
- General IAM: `https://[your-alias-general].signin.aws.amazon.com/console`
- Production IAM: `https://[your-alias-production].signin.aws.amazon.com/console`

## Gotchas

- **Email already in use**: AWS requires unique emails per account. Use Gmail plus trick or create new email
- **MFA code invalid**: Codes expire every 30 seconds. Wait for new code if near expiration
- **Access denied on CLI**: Typo in access keys. Rerun `aws configure --profile [name]` to fix
- **Can't see billing**: Forgot to enable IAM billing access in Account settings
- **Budget takes 24 hours**: Budget data population has delay, this is normal
- **Account alias taken**: Must be globally unique. Add random numbers/letters
- **Verification call not received**: Check country code, try SMS instead of voice
- **CLI command not found**: Restart terminal/computer after installation

## Cleanup

**DO NOT CLEANUP** - These accounts are used throughout the course.

For future reference, account closure process:
1. Delete all resources in account
2. Delete all IAM users/roles/policies
3. Close account from root user → Account settings
4. Remove payment method
5. Account enters 90-day suspension before permanent closure

## Time spent: 01:30

## Notes
- Keep all credentials in password manager
- Never share access keys or show in screenshots
- Always use MFA for any admin-level access
- Default to us-east-1 region unless specified otherwise
- Root user should not be used after initial setup