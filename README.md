# Automated Employee Offboarding Tool for IT Admins in Hybrid (AD & Microsoft Entra ID) Environments -digitally signed by esigner.

**NEW in v1.6.0:** Full support for cloud Microsoft Entra ID users, Active Directory, Hybrid and cloud environments, intelligent mailbox detection, confirmation dialogs, and 5x faster bulk processing!

---
<img width="1182" height="813" alt="adattgui-screen" src="https://github.com/user-attachments/assets/37800c00-7cf5-473a-9131-d638c61a95a3" />

========================================
  ADATT - Active Directory + Azure/M365 
  Termination Tool
  Version 1.6.0
========================================

WELCOME!

Thank you for trying ADATT! This professional tool automates user termination 
across Active Directory and Microsoft 365, including:

✓ Single user termination with auto-reply setup
✓ Bulk offboarding (up to 100 users)
✓ MFA reset
✓ Remove registered devices
✓ Session sign-out
✓ Manager auto-population
✓ Detailed audit logging and CSV reports

🆕 NEW IN v1.6.0:
✓ Remove Registered Devices - View and remove user devices with dates
✓ First-launch email setup wizard - Guided configuration
✓ Improved startup - No module reinstall every launch
✓ Cornflower blue UI theme - Modern, professional appearance
✓ Better button layout - Help moved to footer, all buttons visible
✓ Graceful connection handling - No errors on user cancellation
✓ Updated Discord community link

🌟 FEATURES FROM v1.5.0:
✓ App registration removal (auto-removes apps owned by terminated users)
✓ Service principal removal (removes associated service principals)
✓ Enhanced role assignment removal for privileged users
✓ Progress bar color feedback (green=success, red=errors)
✓ Sound notifications (audio feedback for operations)
✓ JSON audit log export (structured compliance records)
✓ Bulk termination enhancements (full feature parity with single)
✓ Cloud-Only Entra ID user support (no AD account required)
✓ Intelligent mailbox detection (skips Exchange ops when no mailbox)
✓ Confirmation dialog before termination (prevents accidents)
## 🚀 Features

### Core Capabilities
- ✅ **Single User Termination** - Complete offboarding in 60 seconds
- ✅ **Bulk Offboarding** - Process 2-100 users simultaneously with CSV/TXT import
- ✅ **Cloud-Only User Support** - Works with Entra ID-only accounts (no AD required) 🆕
- ✅ **Intelligent Mailbox Detection** - Skips Exchange ops when no mailbox assigned 🆕
- ✅ **MFA Reset** - Remove ALL authentication methods (Phone, Authenticator, FIDO2, etc.)
- ✅ **Session Management** - Sign out users from all active Microsoft 365 sessions
- ✅ **Auto-Reply Configuration** - Set mailbox auto-replies with custom messages
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

---

## 📋 Requirements

### System Requirements
- **OS**: Windows 10/11 or Windows Server 2016+
- **PowerShell**: 5.1 or later
- **Execution Policy**: RemoteSigned or Unrestricted
- **Privileges**: Domain Administrator or delegated permissions

### Required Modules
ADATT automatically installs and configures:
- **Active Directory Module** (RSAT-AD-PowerShell)
- **Microsoft Graph PowerShell SDK**
- **Exchange Online Management**

### Permissions Needed
- **Active Directory**: User account management, group membership modification
- **Microsoft 365**: Global Administrator or Exchange Administrator
- **Graph API**: User.ReadWrite.All, Group.ReadWrite.All, UserAuthenticationMethod.ReadWrite.All

---

## 📥 Installation

### Quick Start

1. **Download ADATT**
   ```powershell
   # Download from GitHub Releases
   # Extract ZIP to desired location
   ```

2. **Set Execution Policy** (if needed)
   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

3. **Install RSAT** (Windows 10/11 only)
   ```powershell
   # Option A: Via Settings
   Settings > Apps > Optional Features > Add a feature > 
   Search "RSAT: Active Directory Domain Services and Lightweight Directory Services Tools"
   
   # Option B: Via PowerShell (as Administrator)
   Get-WindowsCapability -Name RSAT.ActiveDirectory* -Online | Add-WindowsCapability -Online
   ```

4. **Run ADATT**
   ```powershell
   # Right-click ADATT-UX.ps1 > Run with PowerShell
   # OR
   .\ADATT-UX.ps1
   ```

5. **First Run**
   - ADATT will prompt to connect to Microsoft Graph and Exchange Online
   - Grant admin consent when prompted
   - Start your 14-day free trial

---

## 🎯 Usage

### Single User Termination

1. Enter the **sAMAccountName** (e.g., `jdoe`)
2. Manager auto-populates from Active Directory
3. Click **Terminate Access**
4. Customize auto-reply message (or use default)
5. View progress and results
6. Report saved to `C:\Temp\ADATT\Reports\UserTermination_Report.csv`

**What Happens:**
- AD account disabled ✅
- Mailbox converted to shared ✅
- Groups removed ✅
- MFA cleared ✅
- Sessions revoked ✅
- Auto-reply configured ✅
- Report generated ✅

### Bulk Termination

1. **Prepare File** (TXT or CSV format)
   
   **TXT Format** (one username per line):
   ```
   jdoe
   jsmith
   mjones
   ```
   
   **CSV Format**:
   ```csv
   SamAccountName,AutoReplyMessage
   jdoe,"Thank you for your message. This mailbox is no longer monitored."
   jsmith,"John Smith is no longer with the organization. Please contact hr@company.com"
   ```

2. Click **Choose File...** and select your TXT/CSV
3. Review the preview grid (validates against AD)
4. Click **Run Bulk Offboarding**
5. Watch real-time progress (Phase 1: AD, Phase 2: Cloud)
6. View detailed report or export results

**Bulk Processing:**
- Validates all users against Active Directory
- Shows preview with DisplayName, UPN, Status
- Identifies already-disabled accounts
- Processes AD operations first (fast)
- Handles cloud cleanup in Phase 2
- Generates comprehensive CSV report

### Reset MFA

1. Click **Reset MFA**
2. Enter sAMAccountName
3. Confirm user details
4. All MFA methods removed instantly
5. Optionally revoke sessions to force re-registration

### Sign Out User

1. Click **Sign Out**
2. Enter sAMAccountName
3. All Microsoft 365 sessions terminated
4. User must re-authenticate on all devices

---

## 📊 Reports & Logging

### Report Locations
All reports and logs saved to: **`C:\Temp\ADATT\`**

- **Reports**: `C:\Temp\ADATT\Reports\`
  - `UserTermination_Report.csv` - Single user terminations
  - `BulkTermination_Report.csv` - Bulk operations
  
- **Logs**: `C:\Temp\ADATT\`
  - `adatt-ui.log` - Application log
  - `Transcripts\` - PowerShell transcripts for audit

### Report Format (CSV)
```csv
SamAccountName,Status,DisabledBy,DisabledDate,ErrorMessage
jdoe,Disabled,DOMAIN\admin,2025-12-28 14:30:15,
jsmith,Disabled,DOMAIN\admin,2025-12-28 14:30:18,
mjones,Failed,N/A,N/A,User not found in Active Directory
```

### Open Reports Folder
Click the **Open Reports** button in the UI to instantly access your reports folder.

---

## 🔐 License & Pricing

### Trial
- **Duration**: 14 days
- **Features**: Full access to all features
- **No credit card required**


### One-Time Purchase - Lifetime Access

| Plan | Price | Devices | Best For |
|------|-------|---------|----------|
| **Solo** | $175 | 1 | Individual IT admins |
| **Team** | $599 | 5 | IT teams, small MSPs |
| **Business** | $1499 | 20 | Enterprises, large MSPs |

**🚀 LAUNCH SPECIAL**: First 100 customers get **20% OFF**  
Use code: **LAUNCH20** at checkout

### Purchase
Visit: https://adatt.lemonsqueezy.com

---

## 🛠️ Troubleshooting

### Common Issues

**"Active Directory Module not found"**
- Install RSAT (see Installation section)
- Restart PowerShell after installation

**"Execution Policy restricted"**
- Run: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

**"Microsoft Graph connection failed"**
- Ensure you have Global Admin or delegated permissions
- Check internet connectivity
- Try: `Connect-MgGraph -Scopes User.ReadWrite.All,Group.ReadWrite.All`

**"Exchange Online connection failed"**
- Verify Exchange Admin permissions
- Try: `Connect-ExchangeOnline`

**"User not found"**
- Verify sAMAccountName is correct
- Check user exists in Active Directory
- Ensure you're on domain-joined machine or have connectivity

**"License activation failed"**
- Check internet connection
- Verify license key from purchase email
- Wait 5 minutes if purchase just completed
- Contact: adatt@unifosec.com

### License Issues

**"Trial expired"**
- Purchase license from https://adatt.lemonsqueezy.com
- Click **Activate License** in UI
- Enter license key from email

**"Hardware changes detected"**
- ADATT allows 2 hardware changes
- After 3rd change, reactivation required
- Contact support if needed: adatt@unifosec.com

**"Offline grace period"**
- License revalidates every 7 days
- 30-day offline grace period
- Connect to internet before grace period ends

---

## 📖 Documentation

### Environment Support

**Supported Configurations:**
- ✅ Hybrid (On-premises AD + Entra ID)
- ✅ On-premises only (AD without cloud)
- ✅ Cloud-focused (Entra ID primary)

**ADATT automatically detects:**
- User exists in Entra ID → Full cloud operations
- User on-premises only → AD operations only
- Hybrid user → Both AD and cloud operations

### Best Practices

1. **Test First**: Try with test accounts before production
2. **Backup**: Have account recovery process ready
3. **Permissions**: Verify admin rights before bulk operations
4. **Communication**: Inform HR before terminations
5. **Documentation**: Review generated reports for compliance

### Security Considerations

- ADATT requires admin credentials (never stored)
- All actions are logged for audit compliance
- License files encrypted with Windows DPAPI
- No data sent to external servers (except license validation)
- Offline grace period: 30 days

---

## 🤝 Support

### Get Help

- **Email**: adatt@unifosec.com
- **Documentation**: https://adatt.lemonsqueezy.com/docs
- **Issues**: GitHub Issues (bug reports and feature requests)

### Response Times
- Email support: Within 24-48 hours
- Critical bugs: Priority response
- Feature requests: Tracked for future releases

---

## 📝 Changelog

### Version 1.4.1 (December 28, 2025)
- ✨ Added "Open Reports" button for easy access
- 🔧 Report location moved to C:\Temp\ADATT for visibility
- 🐛 Fixed Write-Log function initialization order
- 📧 Updated support email to adatt@unifosec.com
- ⚡ Improved form closing behavior (smart confirmation)
- 🔐 License key input now trims whitespace
- 📊 Success messages now show report file location

### Version 1.4.0 (December 2025)
- 🎉 Initial public release
- ✅ Single and bulk user termination
- ✅ MFA reset and session management
- ✅ Hybrid AD + Entra ID support
- ✅ Comprehensive audit logging
- ✅ License system with trial period

---

## ⚖️ License

**Commercial Software**  
Copyright © 2025 Jose Ernest / Unifosec

This software is licensed for use as described in the End User License Agreement (EULA).  
Unauthorized distribution, modification, or reverse engineering is prohibited.

Purchase required after 14-day trial period.

---

## 🙏 Acknowledgments

Built with:
- PowerShell & Windows Forms
- Microsoft Graph PowerShell SDK
- Exchange Online Management Module
- LemonSqueezy for licensing

Special thanks to the IT admin community for feedback and feature requests.

---

## 🚀 Get Started

1. **Download**: [Latest Release](../../releases/latest)
2. **Try Free**: 14-day trial, all features unlocked
3. **Purchase**: https://adatt.lemonsqueezy.com (use code **LAUNCH20** for 20% off)
4. **Support**: adatt@unifosec.com

---

**Made with ❤️ for IT Professionals who deserve better tools**

