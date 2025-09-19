# Notes – AWS Fundamentals

## 1. AWS Accounts Basics

**Key Concepts**
- AWS Account = Container for identities (users) and resources
- Every account has one Account Root User created with the account
- Account Root User has unrestricted access and cannot be limited
- Each account requires a unique email address (but payment method can be shared)
- AWS Accounts provide isolation boundaries for security and damage containment

**Rules of Thumb**
- Account Root User → Only use for initial setup, then create IAM Admin
- Multiple accounts → Better isolation between dev/test/prod environments
- Gmail plus trick → user+suffix@gmail.com creates unique addresses
- Security principle → Default deny for external access unless explicitly allowed

**Recall Drill Questions**
1. What three things are required to create an AWS account?
2. Can you restrict the permissions of an Account Root User?
3. Why should businesses use multiple AWS accounts instead of one?

---

## 2. Multi-Factor Authentication (MFA)

**Key Concepts**
- Factors: Knowledge (password), Possession (MFA device), Inherent (biometrics), Location
- MFA adds a second factor beyond username/password
- Virtual MFA uses apps like Google Authenticator; Physical MFA uses hardware tokens
- AWS generates secret key + QR code when activating MFA
- Each identity needs its own MFA configuration

**Rules of Thumb**
- Single factor (password only) → High risk if leaked
- MFA code → Changes every 30 seconds, single use only
- Virtual MFA → One entry per identity per account in your app
- Lost MFA → Need account recovery process, so keep backup codes

**Recall Drill Questions**
1. What are the four types of authentication factors?
2. How many MFA devices can you have per IAM identity?
3. What happens if you lose access to your MFA device?

---

## 3. Cost Management & Free Tier

**Key Concepts**
- AWS Free Tier: Free trials, 12-month free, and always free allocations
- Budgets alert you when approaching spending thresholds
- Billing preferences enable email invoices and usage alerts
- Zero spend budget notifies on any charges; Monthly cost budget sets spending limit

**Rules of Thumb**
- New accounts → Set up budget immediately
- Free tier examples → 750 hrs/month EC2, 5GB S3, 750 hrs RDS
- Billing access → Must enable "IAM user and role access to billing"
- Budget alerts → Takes up to 24 hours to populate spend data

**Recall Drill Questions**
1. What are the three types of AWS Free Tier offers?
2. Where do you enable IAM access to billing information?
3. What's the difference between zero spend and monthly cost budgets?

---

## 4. IAM (Identity and Access Management) Basics

**Key Concepts**
- IAM = Globally resilient service, one database per account
- Three identity types: Users (humans/apps), Groups (collections), Roles (services/external)
- Policies = JSON documents that allow/deny permissions (only work when attached)
- IAM identities start with NO permissions (explicit grant required)
- IAM provides: Identity Provider (IDP), Authentication, Authorization

**Rules of Thumb**
- Account Root User → Never use after initial setup
- IAM Users → For identifiable individuals or specific applications
- IAM Roles → For uncertain number of entities or AWS services
- Policies → Must be attached to have effect
- Best practice → Least privilege access (minimum required permissions)

**Recall Drill Questions**
1. What are the three main jobs of IAM?
2. Do IAM identities have any permissions by default?
3. When would you use an IAM Role instead of an IAM User?

---

## 5. IAM Access Keys

**Key Concepts**
- Long-term credentials for CLI/API access (not console)
- Two parts: Access Key ID (public) and Secret Access Key (private)
- Maximum 2 access keys per IAM user (can have 0, 1, or 2)
- Secret key only shown once at creation - cannot be retrieved later
- Access keys can be Active, Inactive, or Deleted

**Rules of Thumb**
- Console access → Username/password + MFA
- CLI/API access → Access keys
- Rotating keys → Create second set, update apps, delete first set
- Lost secret key → Must delete and recreate entire key pair
- Security → Never share or expose secret access keys

**Recall Drill Questions**
1. How many access keys can an IAM user have?
2. What happens if you lose the secret access key?
3. What does "rotating access keys" mean?

---

## 6. AWS CLI Configuration

**Key Concepts**
- AWS CLI v2 uses named profiles for multiple accounts
- Configuration: access key ID, secret key, default region, output format
- Named profiles: --profile profilename (e.g., iamadmin-general)
- Default profile used if no --profile specified

**Rules of Thumb**
- Profile naming → iamadmin-general, iamadmin-production
- Default region → Always use us-east-1 (Northern Virginia)
- Credentials storage → Delete CSV files after configuring CLI
- Testing → aws s3 ls --profile profilename
- Security → Never expose credentials in screenshots or logs

**Recall Drill Questions**
1. What four pieces of information does AWS CLI configuration require?
2. How do you specify a named profile when running AWS CLI commands?
3. Where should you store access key CSV files after configuring CLI?

---

## Account Setup Checklist

**For Each AWS Account:**
- [ ] Create account with unique email
- [ ] Enable MFA on Account Root User
- [ ] Set account alias for friendly signin URL
- [ ] Enable IAM user billing access
- [ ] Create budget (zero spend or monthly)
- [ ] Enable billing alerts and preferences
- [ ] Create IAMAdmin user with AdministratorAccess policy
- [ ] Add MFA to IAMAdmin user
- [ ] Create access keys for IAMAdmin
- [ ] Configure CLI named profile
- [ ] Stop using Account Root User

**Security Best Practices:**
- Use unique, strong passwords for each identity
- Enable MFA on all admin-level accounts
- Rotate access keys regularly
- Never commit credentials to version control
- Delete credential files after CLI configuration
- Use separate AWS accounts for dev/test/prod

**By End of Fundamentals Section:**
- 2 AWS Accounts (General & Production)
- 4 MFA entries in authenticator app (2 root users, 2 IAMAdmin users)
- 2 CLI named profiles configured
- All root users secured and not in regular use