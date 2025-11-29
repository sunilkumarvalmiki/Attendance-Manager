# 📱 Beehive Attendance Automation - Installation Guide

**Version**: 1.0  
**Platform**: Android 6.0+  
**Estimated Setup Time**: 15 minutes

---

## 📋 Prerequisites Checklist

Before starting, ensure you have:

- [ ] Android phone (version 6.0 or higher)
- [ ] Beehive HCM mobile app installed
- [ ] Tasker app purchased ($3.50) - [Download from Google Play](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm)
- [ ] AutoInput plugin purchased ($2.50) - [Download from Google Play](https://play.google.com/store/apps/details?id=com.joaomgcd.autoinput)
- [ ] Beehive username and password ready
- [ ] SMS/Email OTP access confirmed

---

## 🚀 Installation Steps

### Step 1: Install Required Apps

1. **Install Tasker**
   - Open Google Play Store
   - Search for "Tasker"
   - Purchase and install (₹350 / $3.50)
   - Grant all requested permissions

2. **Install AutoInput**
   - Open Google Play Store
   - Search for "AutoInput"
   - Purchase and install (₹200 / $2.50)
   - Grant Accessibility permission when prompted

3. **Verify Beehive App**
   - Ensure Beehive HCM app is installed
   - Test manual login once to verify credentials
   - Close the app

---

### Step 2: Download Tasker Project

1. **Download the project file**
   ```bash
   # Option A: Clone repository
   git clone https://github.com/your-repo/Attendance-Manager.git
   cd Attendance-Manager/tasker
   
   # Option B: Direct download
   # Download BeehiveAttendance.prj.xml to your phone
   ```

2. **Transfer to phone** (if downloaded on computer)
   - Connect phone via USB
   - Copy `BeehiveAttendance.prj.xml` to `/sdcard/Tasker/projects/`
   - Alternatively, use Google Drive/Dropbox

---

### Step 3: Import Project into Tasker

1. **Open Tasker**
   - Launch the Tasker app
   - If seeing the beginner mode, switch to regular mode

2. **Import Project**
   - Tap the **Home icon** (bottom left)
   - Tap **Import Project** (3-dot menu → Import)
   - Navigate to `/sdcard/Tasker/projects/`
   - Select `BeehiveAttendance.prj.xml`
   - Tap **Import**

3. **Verify Import**
   - You should see "BeehiveAttendance" project in the project list
   - Tap on it to view profiles and tasks

---

### Step 4: Run First-Time Setup

1. **Launch Setup Task**
   - In Tasker, go to **TASKS** tab
   - Find and tap **FirstTimeSetup**
   - Tap the ▶ (Play) button at bottom

2. **Enter Configuration**
   - **Username**: Enter your Beehive username (e.g., `5876`)
   - **Password**: Enter your Beehive password
   - **Regularize Reason**: Keep default "Work from Home" or customize

3. **Complete Setup**
   - Tap **OK** after each input
   - Wait for "✅ Setup Complete!" message

---

### Step 5: Grant Permissions

1. **AutoInput Accessibility**
   - Settings → Accessibility → AutoInput
   - Toggle ON

2. **Tasker Permissions**
   - Settings → Apps → Tasker → Permissions
   - Grant: Storage, Phone, SMS, Location (if needed)

3. **Battery Optimization** (Critical!)
   - Settings → Battery → Battery Optimization
   - Find **Tasker** → Select "Don't optimize"
   - Find **AutoInput** → Select "Don't optimize"
   - This prevents Android from killing the automation

---

### Step 6: Test the Automation

1. **Manual Test Run**
   - In Tasker, go to **TASKS** tab
   - Find **ExecuteMorningAttendance**
   - Tap the ▶ button to run manually

2. **What to Expect**
   - Beehive app opens automatically
   - Credentials are auto-filled
   - OTP SMS arrives and is extracted
   - OTP auto-filled
   - Navigation to Regularize Attendance
   - Previous day attendance submitted
   - Punch-in marked
   - Success notification appears

3. **Troubleshooting Test**
   - If any step fails, note the error
   - Check the **Troubleshooting** section below

---

### Step 7: Enable Automated Scheduling

1. **Enable Profiles**
   - Go to **PROFILES** tab
   - Ensure**MorningPunchIn** profile has green checkmark
   - Ensure **EveningPunchOut** profile has green checkmark
   - Ensure **SMSOTPReceiver** profile has green checkmark

2. **Verify Schedule**
   - **Morning**: 9:25 AM (Mon-Fri)
   - **Evening**: 6:25 PM (Mon-Fri)

3. **Test Scheduling** (Optional)
   - Change time to 1 minute from now
   - Wait and verify automation triggers
   - Change back to 9:25 AM / 6:25 PM

---

## ⚙️ Configuration Options

### Customizing Variables

Edit global variables in Tasker:

1. **VARS** tab → Find **BeehiveAttendance** project
2. Tap any variable to edit:

| Variable | Default | Description |
|----------|---------|-------------|
| `%REGULARIZE_REASON` | Work from Home | Attendance regularization reason |
| `%OTP_TIMEOUT` | 120 | Seconds to wait for OTP SMS |
| `%RETRY_ATTEMPTS` | 3 | Number of retry attempts on failure |

---

## 🧪 Testing Checklist

Before relying on automation:

- [ ] Manual test run successful
- [ ] OTP extraction working (SMS received and parsed)
- [ ] Login flow completes without errors
- [ ] Regularize attendance submits correctly
- [ ] Punch-in marks successfully
- [ ] Success notification appears
- [ ] Log file created at `/sdcard/Tasker/BeehiveAttendance/logs/attendance.log`
- [ ] Profiles enabled for automated scheduling
- [ ] Battery optimization disabled for Tasker & AutoInput

---

## 🐛 Troubleshooting

### Issue 1: OTP Not Extracted

**Symptoms**: Automation waits forever for OTP

**Solutions**:
1. Check SMS permissions granted to Tasker
2. Manually trigger OTP → Check if SMS contains word "OTP"
3. Test OTP extraction:
   - TASKS → **ExtractOTP_SMS**
   - Trigger OTP manually
   - Run task
   - Check if %OTP_CODE variable is set

### Issue 2: AutoInput Not Clicking Elements

**Symptoms**: App opens but no automation happens

**Solutions**:
1. Verify AutoInput Accessibility enabled
2. Update selectors (Beehive app may have changed UI):
   - In Tasker, edit tasks
   - Update `resource_id` or `text` values
   - Use AutoInput UI Query to find correct selectors

### Issue 3: Already Run Today

**Symptoms**: "Already Run Today" notification

**Solutions**:
- This is by design to prevent duplicate submissions
- To test again, clear `%LAST_RUN_DATE` variable
- Or wait until next day

### Issue 4: Login Fails

**Symptoms**: Credentials not auto-filled

**Solutions**:
1. Re-run **FirstTimeSetup** task
2. Verify credentials are correct
3. Check if Beehive app login screen changed
4. Update AutoInput selectors in **PerformLogin** task

### Issue 5: Automation Doesn't Run at Scheduled Time

**Symptoms**: No automation at 9:25 AM or 6:25 PM

**Solutions**:
1. **Check profiles are enabled** (green checkmark)
2. **Disable battery optimization**:
   - Settings → Battery → Tasker → Don't optimize
3. **Check Do Not Disturb** not blocking Tasker
4. **Verify time triggers**:
   - Edit profile → Check time is 9:25 / 18:25
   - Check days (Mon-Fri selected)

---

## 📊 Monitoring & Logs

### View Attendance Log

1. Open file manager
2. Navigate to `/sdcard/Tasker/BeehiveAttendance/logs/`
3. Open `attendance.log`

**Sample log entry**:
```
2025-11-25 09:27 | SUCCESS | Morning Punch-In
2025-11-25 18:27 | SUCCESS | Evening Punch-Out
```

### Daily Summary Notification

Every morning after successful automation:
```
✅ Attendance Marked
Morning punch-in completed at 09:27 AM
- Regularized: 2025-11-24
- Punched In: 2025-11-25
```

---

## 🔒 Security Best Practices

1. **Credential Protection**
   - Tasker stores variables in encrypted storage
   - Never share your Tasker project export with others (contains credentials)

2. **Lock Your Phone**
   - Set up fingerprint/PIN lock
   - Automation runs even when phone is locked

3. **Regular Password Changes**
   - If you change Beehive password, re-run **FirstTimeSetup**

---

## 🛡️ Privacy & Data

### What Data is Stored?

- Beehive username (encrypted)
- Beehive password (encrypted)
- Regularize reason (plain text)
- OTP codes (temporary, cleared after use)
- Attendance log (dates and status only)

### What Data is NOT Stored?

- OTP codes are never logged
- No data sent to external servers
- Everything runs locally on your phone

---

## ⚠️ Important Notes

1. **Weekend & Holidays**
   - Automation skips Saturdays and Sundays automatically
   - For public holidays, manually disable profiles or use "already run today" logic

2. **Leave Days**
   - If on leave, manually disable profiles
   - Future: Auto-detect leave from Beehive calendar

3. **App Updates**
   - If Beehive app updates, UI selectors may break
   - You'll need to update AutoInput actions
   - Check logs if automation suddenly fails

4. **Battery Usage**
   - Minimal battery impact (<2% per day)
   - Ensure Tasker not optimized for battery

---

## 🆘 Getting Help

### Debug Mode

To enable detailed logging:

1. Edit **ExecuteMorningAttendance** task
2. Add "Flash" actions after each step
3. Re-run and observe step-by-step execution

### Export Logs for Support

```bash
# Copy logs from phone
adb pull /sdcard/Tasker/BeehiveAttendance/logs/attendance.log
```

### Community Support

- GitHub Issues: [Report bugs](https://github.com/your-repo/issues)
- Email: attendance-automation@example.com

---

## 🎓 Advanced Configuration

### Custom OTP Regex

If your OTP format is different (e.g., alphanumeric):

1. Edit **ExtractOTP_SMS** task
2. Find Variable Search Replace action
3. Update regex pattern:
   - Current: `\b(\d{4,6})\b` (4-6 digits)
   - Example: `\b([A-Z0-9]{6})\b` (6 alphanumeric)

### Email OTP Fallback

Not implemented in v1.0. Coming in v2.0.

### Multiple Accounts

To manage multiple Beehive accounts:

1. Duplicate project (rename to BeehiveAttendance_AccountName)
2. Run setup with different credentials
3. Adjust profile times to avoid conflicts

---

## ✅ Post-Installation Checklist

After setup, verify:

- [ ] Tasker installed and configured
- [ ] AutoInput installed with accessibility enabled
- [ ] Project imported successfully
- [ ] FirstTimeSetup completed
- [ ] Test run passed
- [ ] Profiles enabled (green checkmarks)
- [ ] Battery optimization disabled
- [ ] Scheduled times verified (9:25 AM, 6:25 PM)
- [ ] Log file created and readable
- [ ] Phone alarm/notification confirmed

---

## 🎉 You're All Set!

Your automation is now ready. It will run automatically at:
- **9:25 AM** (Mon-Fri) - Regularize yesterday + Punch-in today
- **6:25 PM** (Mon-Fri) - Punch-out

**What to do on Day 1**:
1. Keep phone charged and nearby at 9:25 AM
2. Wait for success notification
3. Verify attendance in Beehive app manually
4. If successful, let it run autonomously going forward!

---

**Questions? Issues?** Check the Troubleshooting section or open a GitHub issue.

**Happy automating! 🚀**
