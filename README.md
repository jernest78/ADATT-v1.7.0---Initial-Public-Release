# ADATT - Active Directory Automated Termination Tool

![Version](https://img.shields.io/badge/version-1.6.0-blue)
![License](https://img.shields.io/badge/license-Commercial-green)
![Platform](https://img.shields.io/badge/platform-Windows-lightgrey)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue)

## 👋 Welcome

**ADATT** (Active Directory Automated Termination Tool) is a professional employee offboarding solution designed for IT administrators managing hybrid environments. Transform your user termination process from 35 minutes of manual clicking to 60 seconds of automated perfection.

ADATT handles Active Directory, Microsoft 365, MFA, groups, sessions, and generates compliance-ready reports automatically—all through an intuitive graphical interface.

**What's New in v1.6.0:** Enhanced email notifications for terminated users, automated reporting to IT admins, and improved cloud-only user support with intelligent mailbox detection!
<img width="2800" height="1857" alt="adatt-Interface" src="https://github.com/user-attachments/assets/cecca0be-8b9a-4867-b34d-236e458625e2" />

---

## 🚀 Features

### Core Capabilities
- ✅ **Single User Termination** - Complete offboarding in 60 seconds
- ✅ **Bulk Offboarding** - Process 2-100 users simultaneously with CSV/TXT import
- ✅ **Universal Account Support** - Works with On-Premises AD, Hybrid (AD + Entra ID), and Cloud-Only (Entra ID) accounts 🆕
- ✅ **Intelligent Mailbox Detection** - Skips Exchange ops when no mailbox assigned 🆕
- ✅ **MFA Reset** - Remove ALL authentication methods (Phone, Authenticator, FIDO2, etc.)
- ✅ **Session Management** - Sign out users from all active Microsoft 365 sessions
- ✅ **Auto-Reply Configuration** - Set mailbox auto-replies with custom messages
- ✅ **Email Notifications** - Automated reports sent to IT admins with CSV attachments 🆕
- ✅ **Audit Reports** - Auto-generate CSV reports with timestamps and account types

### Account Type Support 🆕
ADATT now supports ALL Microsoft identity scenarios:
- **Cloud-Only** - Users created directly in Entra ID (no Active Directory account)
- **Hybrid** - Users synchronized from Active Directory to Entra ID
- **On-Premises** - Users in Active Directory only (not synced to cloud)

All operations automatically detect account type and adjust accordingly!

### What ADATT Does Automatically
1. **Active Directory**
   - Disables user account
   - Updates description with termination details
   - Removes all group memberships (except primary)
   - Clears manager field

2. **Microsoft 365 / Exchange Online**
   - Intelligently checks for mailbox existence 🆕
   - Converts mailbox to shared (if exists)
   - Hides from Global Address List (if exists)
   - Sets automatic reply message (if exists)
   - Removes Office 365 licenses
   - Revokes all active sessions

3. **Entra ID (Azure AD)**
   - Disables cloud-only accounts 🆕
   - Removes ALL MFA methods:
     - Phone authentication
     - Microsoft Authenticator
     - Software OATH tokens
     - FIDO2 security keys
     - Windows Hello for Business
     - Temporary Access Pass
   - Removes registered devices
   - Clears authentication methods

4. **Compliance & Audit**
   - Generates timestamped CSV reports
   - Records account type (Cloud-Only, Hybrid, On-Premises) 🆕
   - Logs all operations for audit trail
   - Records who performed termination and when
   - Tracks success/failure for each operation
   - Documents mailbox operation status 🆕
   - Sends email notifications to IT admins with report attachments 🆕
   - Auto-detects technician email from Active Directory
   - Optional manager notification

---

## 📋 Requirements

### System Requirements
- **Operating System**: Windows 10/11 or Windows Server 2016+
- **PowerShell**: Version 5.1 or later
- **Execution Policy**: RemoteSigned or Unrestricted
- **Administrator Privileges**: Domain Administrator or delegated permissions
- **Network**: Internet connectivity for Microsoft 365/Graph operations
- **RAM**: Minimum 4GB (8GB recommended for bulk operations)
- **Disk Space**: 100MB minimum

### Required PowerShell Modules
ADATT automatically checks and installs these modules on first run:
- **ActiveDirectory** (RSAT-AD-PowerShell) - For on-premises AD operations
- **Microsoft.Graph** - For Entra ID and Microsoft 365 operations
- **ExchangeOnlineManagement** - For Exchange Online mailbox operations

### Permissions Required

#### Active Directory (On-Premises)
- User account management (read, write, disable)
- Group membership modification
- Organizational Unit (OU) move permissions
- Password reset rights

#### Microsoft 365 / Entra ID
- **Global Administrator** (recommended) or combination of:
  - User Administrator
  - Groups Administrator
  - Exchange Administrator
  - Application Administrator (for app removal)

#### Microsoft Graph API Scopes
ADATT requests these scopes during first connection:
- `User.ReadWrite.All` - Read/write users, disable accounts
- `Directory.ReadWrite.All` - Manage directory objects
- `Group.ReadWrite.All` - Remove group memberships
- `UserAuthenticationMethod.ReadWrite.All` - Remove MFA methods
- `Device.ReadWrite.All` - Remove registered devices
- `Application.ReadWrite.All` - Remove owned app registrations
- `Mail.Send` - Send email notifications
- `RoleManagement.ReadWrite.Directory` - Manage privileged roles (optional)

---

## 📥 Installation

### Method 1: MSI Installer (Recommended)

1. **Download the Package**
   - Visit: [ADATT Releases](https://adatt.lemonsqueezy.com)
   - Download: `ADATT-v1.6.0-Package.zip`
   - Extract and run `ADATT-v1.6.0.msi` from the package

2. **Run the MSI Installer**
   ```powershell
   # Double-click the ADATT-v1.6.0.msi file
   # Or run from command line:
   msiexec /i ADATT-v1.6.0.msi
   ```

3. **Follow Installation Wizard**
   - Accept license agreement
   - Choose installation directory (default: `C:\Program Files\ADATT\`)
   - Select Start Menu shortcuts
   - Click Install

4. **Launch ADATT**
   - From Start Menu: Search "ADATT"
   - From Desktop: Double-click ADATT shortcut
   - From installation folder: Run `ADATT-UX.ps1`

**Default Installation Path**: `C:\Program Files\ADATT\`

### Method 2: Portable (No Installation)

1. **Download the Same Package**
   - Download: `ADATT-v1.6.0-Package.zip` (same as Method 1)
   - Contains both MSI installer and portable files

2. **Extract Files for Portable Use**
   ```powershell
   # Recommended locations:
   # C:\Tools\ADATT\
   # D:\Software\ADATT\
   # \\server\share\ADATT\  (network share)
   
   Expand-Archive -Path "ADATT-v1.6.0-Package.zip" -DestinationPath "C:\Tools\ADATT"
   ```

3. **Set Execution Policy** (if needed)
   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

4. **Run ADATT**
   ```powershell
   cd "C:\Tools\ADATT"
   .\ADATT-UX.ps1
   ```

### First-Time Setup

#### Step 1: Install RSAT Tools (Windows 10/11 only)

**Option A: Via Settings**
1. Open Settings → Apps → Optional Features
2. Click "Add a feature"
3. Search "RSAT: Active Directory"
4. Install "RSAT: Active Directory Domain Services and Lightweight Directory Services Tools"

**Option B: Via PowerShell (as Administrator)**
```powershell
# Windows 10/11
Add-WindowsCapability -Online -Name Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0

# Windows Server
Install-WindowsFeature -Name RSAT-AD-PowerShell
```

#### Step 2: Install Required Modules

ADATT automatically prompts to install missing modules, or install manually:

```powershell
# Microsoft Graph SDK
Install-Module Microsoft.Graph -Scope CurrentUser -Force

# Exchange Online Management
Install-Module ExchangeOnlineManagement -Scope CurrentUser -Force
```

#### Step 3: First Launch

1. **Run ADATT**
   - Right-click `ADATT-UX.ps1` → "Run with PowerShell"
   - Or launch from Start Menu

2. **Activate License**
   - Click "Activate License" button
   - Enter license key (or start 14-day free trial)
   - No credit card required for trial

3. **Connect to Microsoft 365** (on first cloud operation)
   - ADATT prompts for Graph authentication
   - Grant admin consent when requested
   - Connection persists across sessions

4. **Configure Email Notifications** (optional)
   - On first termination, prompted for IT admin email
   - Email address saved for future operations
   - Can be reconfigured in settings

---

## 🚀 Core Capabilities

ADATT provides comprehensive employee offboarding automation across hybrid environments:

### 1. Single User Termination
Complete offboarding of individual users in under 60 seconds:
- Disable Active Directory account
- Convert mailbox to shared (with intelligent detection)
- Remove all group memberships
- Reset MFA authentication methods
- Revoke all active sessions
- Remove licenses and registered devices
- Generate timestamped audit report

### 2. Bulk Offboarding
Process 2-100 users simultaneously with CSV/TXT import:
- Import from CSV or TXT files
- Real-time validation and preview
- Batch processing for optimal performance
- Comprehensive success/failure reporting
- 5x faster than sequential processing

### 3. MFA Reset
Instantly remove all MFA methods for locked-out users:
- Phone authentication
- Microsoft Authenticator app
- Software OATH tokens
- FIDO2 security keys
- Windows Hello for Business
- Temporary Access Pass

### 4. Session Management
Force users to re-authenticate across all devices:
- Sign out from all active Microsoft 365 sessions
- Revoke refresh tokens
- Immediate effect across web, mobile, and desktop

### 5. Intelligent Account Type Detection
Automatically identifies and handles:
- **Cloud-Only** - Users created directly in Entra ID
- **Hybrid** - Users synchronized from AD to Entra ID
- **On-Premises** - Users in Active Directory only
- Operations automatically adjust based on account type

### 6. Email Notifications
Automated email reporting to IT administrators:
- Termination confirmation emails
- CSV report attachments
- Sent to configured IT admin email
- Optional technician/manager notifications

---

## ✨ Features

### Automation Features
- ✅ **One-Click Termination** - Complete offboarding in 60 seconds
- ✅ **Bulk CSV Import** - Process up to 100 users simultaneously
- ✅ **Intelligent Mailbox Detection** - Skips Exchange ops when no mailbox assigned
- ✅ **Confirmation Dialogs** - Prevents accidental terminations
- ✅ **Progress Tracking** - Real-time status updates during operations
- ✅ **Error Handling** - Graceful failure recovery with detailed logs

### Active Directory Operations
- ✅ Disable user accounts
- ✅ Reset passwords with secure random generation
- ✅ Clear manager fields
- ✅ Add termination timestamps to descriptions
- ✅ Move to Disabled Users OU
- ✅ Remove all group memberships (except primary)

### Microsoft 365 / Exchange Online Operations
- ✅ Convert mailbox to shared (if exists)
- ✅ Hide from Global Address List
- ✅ Configure automatic Out-of-Office replies
- ✅ Grant manager mailbox access
- ✅ Remove Office 365 licenses
- ✅ Revoke all active sessions instantly

### Entra ID (Azure AD) Operations
- ✅ Disable cloud-only accounts
- ✅ Remove ALL MFA methods (phone, app, FIDO2, etc.)
- ✅ Remove registered devices
- ✅ Remove owned app registrations
- ✅ Clear authentication methods
- ✅ Remove group memberships

### Compliance & Reporting
- ✅ Generate timestamped CSV reports
- ✅ Record account type (Cloud-Only, Hybrid, On-Premises)
- ✅ Log all operations for audit trail
- ✅ Track who performed termination and when
- ✅ Document success/failure for each operation
- ✅ Email notifications with report attachments
- ✅ PowerShell transcript logging

### Security Features
- ✅ License validation with hardware fingerprinting
- ✅ 30-day offline grace period
- ✅ Encrypted license storage (Windows DPAPI)
- ✅ No credential storage (uses Windows authentication)
- ✅ Audit logging for compliance

---

## 💼 Usage

### Single User Termination

#### Step 1: Enter Username
1. In the **"Single Termination"** section, enter one of:
   - **sAMAccountName**: `john.doe` (for hybrid/on-premises users)
   - **User Principal Name (UPN)**: `john.doe@contoso.com` (for any user type)
   - **Email Address**: `john.doe@company.com` (for cloud-only users)

#### Step 2: Click "Terminate"
- ADATT performs automatic detection:
  - Searches Active Directory (if hybrid/on-premises)
  - Falls back to Entra ID (if cloud-only)
  - Displays confirmation dialog

#### Step 3: Review Confirmation Dialog
Displays:
- User's display name
- Account type (Cloud-Only, Hybrid, On-Premises)
- Operations that will be performed
- Estimated completion time

**Example Confirmation**:
```
Terminate User: John Doe
Account Type: Hybrid
UPN: john.doe@contoso.com

Operations to be performed:
✓ Disable AD account
✓ Reset AD password
✓ Clear manager field
✓ Add termination date to description
✓ Disable Entra ID account
✓ Revoke all sessions
✓ Remove group memberships
✓ Remove Office 365 licenses
✓ Set mailbox Out-of-Office
✓ Convert to shared mailbox

Continue?
```

#### Step 4: Confirm or Cancel
- **Click "Yes, Terminate"**: Executes all operations
- **Click "Cancel"**: Aborts operation (no changes made)

#### Step 5: View Results
- Success message displays with report location
- Report saved to: `Reports\UserTermination_Report.csv`
- Email notification sent to configured IT admin (if enabled)

### Bulk Termination

#### Step 1: Prepare CSV File

**CSV Format (Recommended)**:
```csv
SamAccountName,AutoReplyMessage
john.doe,"Thank you for your message. This mailbox is no longer monitored."
jane.smith,"Jane Smith is no longer with the organization. Contact hr@company.com"
cloud.user@contoso.com,"User has left the company."
```

**TXT Format** (simple, one username per line):
```
john.doe
jane.smith
bob.jones
cloud.user@contoso.com
```

**Supported Formats**:
- sAMAccountName (for hybrid/on-premises): `john.doe`
- User Principal Name (for any type): `john.doe@contoso.com`
- Email address (for cloud-only): `user@company.com`

#### Step 2: Load File in ADATT
1. Click **"Choose File..."** button
2. Select your CSV or TXT file
3. ADATT validates users:
   - Searches Active Directory
   - Falls back to Entra ID for cloud-only users
   - Displays progress: "Processing user 5 of 20..."

#### Step 3: Review Preview Grid
Preview displays:
- **sAMAccountName** - Username
- **DisplayName** - User's full name
- **UserPrincipalName** - Email/UPN
- **Status** - Found / Not Found / Already Disabled

**Color Coding**:
- 🟢 **Green rows** - User found and ready
- 🔴 **Red rows** - User not found (check username)
- 🟡 **Yellow rows** - Already disabled

#### Step 4: Execute Bulk Termination
1. Click **"Run Bulk Offboarding"** button
2. Confirm: "Terminate X users?"
3. Watch progress bar and status updates
4. Completion: "Bulk termination completed: X succeeded, Y failed"

#### Step 5: Review Report
- Report location: `Reports\BulkTermination_Report.csv`
- Click **"Open Reports"** button for quick access
- Email notification sent with CSV attachment (if enabled)

**Report Columns**:
```csv
SamAccountName,DisplayName,AccountType,Status,DisabledBy,DisabledDate,ErrorMessage
john.doe,John Doe,Hybrid,Disabled,DOMAIN\admin,2026-01-14 10:30:15,
jane.smith,Jane Smith,Cloud-Only,Disabled,admin@contoso.com,2026-01-14 10:30:18,
bob.jones,Bob Jones,On-Premises,Disabled,DOMAIN\admin,2026-01-14 10:30:20,
invalid.user,Unknown,Unknown,Failed,N/A,N/A,User not found in AD or Entra ID
```

### Reset MFA

1. Click **"Reset MFA"** tab
2. Enter username (sAMAccountName or UPN)
3. Click **"Reset MFA"** button
4. Confirm user details
5. All MFA methods removed instantly
6. User prompted to re-register on next sign-in

**Supported Account Types**: Cloud-Only, Hybrid (requires Entra ID)

### Sign Out User Sessions

1. Click **"Sign Out"** tab
2. Enter username (sAMAccountName or UPN)
3. Click **"Sign Out Sessions"** button
4. All Microsoft 365 sessions terminated
5. User must re-authenticate on all devices

**Use Cases**: Security incidents, before termination, force MFA re-registration

---

## 📊 Reports

### Report Locations
All reports saved relative to script location:

**Path Structure**:
```
<ADATT Installation Folder>\Reports\
├── UserTermination_Report.csv        (Single user operations)
├── BulkTermination_Report_*.csv      (Bulk operations, timestamped)
└── Archive\                          (Optional: older reports)
```

**Example**: If installed to `C:\Program Files\ADATT\`, reports save to `C:\Program Files\ADATT\Reports\`

### Report Formats

#### Single User Termination Report
```csv
SamAccountName,DisplayName,UserPrincipalName,Status,DisabledBy,DisabledDate,AccountType,CloudOperations,ContactUser
jdoe,John Doe,jdoe@contoso.com,Disabled,DOMAIN\admin,2026-01-14 14:30:15,Hybrid (AD + Entra ID),Completed,manager@contoso.com
```

**Columns**:
- **SamAccountName** - Username
- **DisplayName** - Full name
- **UserPrincipalName** - Email/UPN
- **Status** - Disabled / Failed
- **DisabledBy** - Administrator who performed action
- **DisabledDate** - Timestamp of termination
- **AccountType** - Cloud-Only, Hybrid, or On-Premises
- **CloudOperations** - Completed / Skipped / Failed
- **ContactUser** - Manager email (for mailbox access)

#### Bulk Termination Report
```csv
SamAccountName,DisplayName,AccountType,Status,DisabledBy,DisabledDate,ErrorMessage,MailboxConverted,LicensesRemoved,MFACleared
john.doe,John Doe,Hybrid,Disabled,DOMAIN\admin,2026-01-14 14:30:15,,Yes,Yes,Yes
jane.smith,Jane Smith,Cloud-Only,Disabled,admin@contoso.com,2026-01-14 14:30:18,,Yes,Yes,Yes
invalid.user,Unknown,Unknown,Failed,N/A,N/A,User not found in AD or Entra ID,No,No,No
```

**Columns**:
- **SamAccountName** - Username
- **DisplayName** - Full name
- **AccountType** - Cloud-Only, Hybrid, On-Premises, or Unknown
- **Status** - Disabled / Failed
- **DisabledBy** - Administrator
- **DisabledDate** - Timestamp
- **ErrorMessage** - Details if failed
- **MailboxConverted** - Yes / No / N/A
- **LicensesRemoved** - Yes / No
- **MFACleared** - Yes / No

### Email Reports

ADATT can automatically email CSV reports to IT administrators:

#### Email Configuration
1. **First-Time Setup**:
   - On first termination, prompted for IT admin email
   - Email saved to: `%APPDATA%\ADATT\email_config.json`
   - No re-entry needed for future operations

2. **Email Contents**:
   - **Subject**: `[ADATT] User Termination Report - [Username]`
   - **Body**: 
     - Termination summary
     - User details
     - Operations performed
     - Timestamp and administrator
   - **Attachment**: CSV report file

3. **Recipients** (configurable):
   - IT admin email (required)
   - Technician who performed action (auto-detected from AD)
   - Technician's manager (optional)

#### Email Requirements
- Microsoft Graph connection required
- `Mail.Send` API scope granted
- Sent from authenticated Graph user account

### Open Reports Folder
Click the **"Open Reports"** button in ADATT UI to instantly open the Reports folder in File Explorer.

---

## 🔐 Security Requirements

### Authentication & Authorization

#### Windows Authentication
- ADATT uses Windows integrated authentication
- No credentials stored on disk
- Leverages current user's domain credentials for AD operations

#### Microsoft Graph Authentication
- Interactive browser-based authentication (OAuth 2.0)
- Refresh tokens cached securely by Microsoft.Graph SDK
- Token location: `%USERPROFILE%\.graph\`
- Tokens encrypted using Windows DPAPI

#### Exchange Online Authentication
- Modern authentication (OAuth 2.0)
- No password storage
- Session tokens managed by ExchangeOnlineManagement module

#### License Validation
- Hardware fingerprinting (non-reversible hash)
- License keys validated against LemonSqueezy Cloudflare Worker
- Revalidation every 7 days (configurable)
- 30-day offline grace period

#### Hardware Change Protection
- Allows up to 2 hardware changes (e.g., RAM upgrade, NIC replacement)
- Tracks changes via hardware fingerprint
- Exceeding limit requires license reactivation

### Data Protection

#### No Data Exfiltration
- No user data sent to external servers
- Only license validation calls to LemonSqueezy API
- All operations local or to your Microsoft 365 tenant

#### Audit Logging
- All operations logged to: `adatt-ui.log`
- PowerShell transcripts: `Transcripts\` folder
- Logs include: timestamp, user, action, result
- No sensitive data (passwords, MFA secrets) logged

### Execution Policy
ADATT requires PowerShell execution policy:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**Security Implications**:
- `RemoteSigned`: ADATT has been digitally signed - Pubisher: UNIF


### Compliance Considerations

#### SOC 2 / ISO 27001
- Comprehensive audit logging
- Timestamped CSV reports for evidence
- No credential storage
- Encrypted license storage

#### GDPR
- No personally identifiable information (PII) sent to ADATT servers
- All user data remains in your Microsoft 365 tenant
- Reports stored locally on your infrastructure

#### Industry Best Practices
- Principle of least privilege (use delegated permissions where possible)
- Defense in depth (multiple confirmation dialogs)
- Audit trail for all terminations
- Secure credential handling (Windows integrated auth)

---

## 🆘 Get Help

### Support Channels

#### Email Support
**Primary Contact**: adatt@unifosec.com

**Response Times**:
- General inquiries: 24-48 hours
- Technical issues: 24-48 hours
- Critical bugs: Priority response
- Feature requests: Tracked for future releases

**When emailing, include**:
- ADATT version (shown in UI footer)
- Error message or screenshot
- Steps to reproduce issue
- Relevant log excerpts (from `adatt-ui.log`)

#### Discord Community
**Join our Discord server**: https://discord.gg/3WRyWACa7a

**Community Benefits**:
- Faster response times for urgent issues
- Connect with other IT administrators
- Share best practices and workflows
- Get notified of new releases
- Beta testing opportunities
- Feature request discussions

**Discord Channels**:
- `#general` - General discussion and introductions
- `#support` - Technical help and troubleshooting
- `#feature-requests` - Suggest new features
- `#announcements` - Release notes and updates
- `#show-and-tell` - Share your ADATT workflows

---

## ⚖️ License

### Software License

**Commercial Software**  
Copyright © 2025 Jose Ernest / Unifosec

This software is licensed, not sold. By installing and using ADATT, you agree to the terms of the End User License Agreement (EULA).

**License Types**:
- **Trial**: 14 days, all features, no credit card required
- **Solo Admin**: 1 device, $119 (one-time payment, lifetime access)
- **Team**: 5 devices, $279 (one-time payment, lifetime access)
- **Business**: 20 devices, $479 (one-time payment, lifetime access)

**🚀 LAUNCH SPECIAL**: First 100 customers get **20% OFF** - Use code: **LAUNCH20**

### Key Terms

#### What You CAN Do
- ✅ Use ADATT for employee offboarding in your organization
- ✅ Install on the number of devices specified by your license
- ✅ Receive free updates and bug fixes
- ✅ Use for internal IT operations

#### What You CANNOT Do
- ❌ Reverse engineer, decompile, or disassemble the software
- ❌ Rent, lease, or lend the software to third parties
- ❌ Transfer license without written consent
- ❌ Use on more devices than permitted
- ❌ Remove or modify copyright notices
- ❌ Share license keys with others
- ❌ Provide ADATT as a service to other organizations (MSPs: contact us for special licensing)

### Warranty Disclaimer

THE SOFTWARE IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED. Licensor does not warrant that the software will be uninterrupted, error-free, or meet all your requirements.

### Limitation of Liability

Licensor shall not be liable for any special, incidental, indirect, or consequential damages arising from use or inability to use the software, including but not limited to loss of business data or profits.

**Full EULA**: See [LICENSE.txt](LICENSE.txt) for complete terms and conditions.

### Purchase

Visit: **https://adatt.lemonsqueezy.com**

---

## 🙏 Acknowledgments

ADATT is built on the shoulders of giants. Special thanks to:

### Technologies & Frameworks
- **PowerShell** - Automation foundation
- **Windows Forms** - Modern GUI framework
- **Microsoft Graph PowerShell SDK** - Azure AD / Entra ID operations
- **Exchange Online Management Module** - Mailbox and Exchange operations
- **Active Directory Module** - On-premises AD operations

### Third-Party Services
- **LemonSqueezy** - License management and payment processing
- **Cloudflare Workers** - Secure license validation infrastructure
- **GitHub** - Source control and issue tracking (private repository)

### Community
- **IT Administrators Worldwide** - For invaluable feedback, feature requests, and real-world testing
- **r/sysadmin subreddit** - For workflow insights and pain point identification
- **PowerShell Community** - For scripting best practices and optimization techniques
- **Microsoft MVP Program** - For guidance on Graph API and Exchange Online best practices

### Beta Testers
Special thanks to our beta testing community who helped refine ADATT v1.6.0:
- Organizations that provided hybrid environment testing
- Cloud-only environment testers for Entra ID-only scenarios
- MSPs who tested bulk offboarding workflows
- Security teams who validated audit logging and compliance features

### Inspiration
This tool was born from frustration with manual offboarding processes and the lack of comprehensive, user-friendly automation tools for hybrid environments. ADATT represents thousands of hours of development, testing, and refinement to create the tool we all wish we had.

---

## 🚀 Get Started Today

### Quick Start Checklist

1. **Download ADATT**
   - Visit: https://adatt.lemonsqueezy.com
   - Download: `ADATT-v1.6.0-Package.zip`

2. **Install & Launch**
   - Run `ADATT-v1.6.0.msi` for installation, or
   - Extract ZIP for portable use
   - Launch `ADATT-UX.ps1`

3. **Start Free Trial**
   - Click "Activate License"
   - Select "Start 14-Day Trial"
   - No credit card required

4. **Connect to Microsoft 365**
   - ADATT prompts on first cloud operation
   - Grant admin consent
   - Begin offboarding users instantly

5. **Offboard Your First User**
   - Enter username
   - Review confirmation
   - Watch ADATT work its magic
   - View comprehensive report

### Pricing

| Plan | Devices | Price | Best For |
|------|---------|-------|----------|
| **Trial** | Any | FREE for 14 days | Evaluation |
| **Solo Admin** | 1 | $175 | Individual IT admins |
| **Team** | 5 | $599 | IT teams, small MSPs |
| **Business** | 1499 | $479 | Enterprises, large MSPs |

**One-time payment. Lifetime access. Free updates.**

**🎉 LAUNCH OFFER**: Use code **LAUNCH20** for 20% off (first 100 customers)
