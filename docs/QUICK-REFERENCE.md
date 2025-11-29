# 📝 Beehive Attendance Automation - Quick Reference

## 🎯 Common Tasks

### Enable/Disable Automation

**Disable for vacation**:
1. Open Tasker → PROFILES tab
2. Long-press **MorningPunchIn** → Toggle OFF
3. Long-press **EveningPunchOut** → Toggle OFF

**Re-enable**:
- Repeat above steps and toggle ON

### Change Regularize Reason

1. Tasker → VARS tab → `%REGULARIZE_REASON`
2. Tap to edit → Enter new reason
3. Save

### Update Credentials

1. Tasker → TASKS → **FirstTimeSetup**
2. Tap ▶ (Play button)
3. Enter new username/password

### Manual Trigger

**Test automation right now**:
1. Tasker → TASKS → **ExecuteMorningAttendance**
2. Tap ▶ button
3. Watch automation run

---

## 🔍 Quick Troubleshooting

| Problem | Quick Fix |
|---------|-----------|
| OTP not extracted | Check SMS permissions |
| App doesn't open | Verify battery optimization OFF |
| Login fails | Run FirstTimeSetup again |
| Already run today | Clear %LAST_RUN_DATE variable |
| No automation at schedule | Check profiles enabled (green ✓) |

---

## 📱 Widget Setup (Optional)

Create home screen widgets for quick control:

### 1. Enable/Disable Widget

1. Long-press home screen → Widgets
2. Find "Tasker" → "Task Shortcut"
3. Drag to home screen
4. Select **Toggle Automation** (create this task)
5. Customize icon

### 2. Manual Trigger Widget

1. Add "Task Shortcut" widget
2. Select **ExecuteMorningAttendance**
3. Label: "Mark Attendance Now"

---

## 🔔 Notification Examples

### Success
```
✅ Attendance Marked
Morning punch-in completed at 09:27 AM
```

### Failure
```
❌ Automation Failed
Error: OTP timeout
Action: Mark manually
```

### Already Run
```
⏭️ Already Run Today
Attendance already submitted for 2025-11-25
```

---

## 📊 Log File Location

```
/sdcard/Tasker/BeehiveAttendance/logs/attendance.log
```

View with any text editor or file manager.

---

## ⚡ Performance Tips

1. **Keep phone charged**: Automation needs phone ON
2. **Stable internet**: Ensure WiFi/4G active at trigger times
3. **Close resource-heavy apps**: Free up RAM for smoother automation
4. **Update apps**: Keep Tasker, AutoInput, Beehive app updated

---

## 🛠️ Customization Examples

### Change to 9:00 AM instead of 9:25 AM

1. Tasker → PROFILES → **MorningPunchIn**
2. Tap Time trigger (09:25)
3. Change to 09:00
4. Save

### Add Telegram notification

1. Install Tasker Telegram plugin
2. Edit **ExecuteMorningAttendance**
3. After success notification, add Telegram send action
4. Configure bot token and chat ID

### Skip regularization

If you don't need previous day regularization:

1. Edit **ExecuteMorningAttendance**
2. Long-press **RegularizeAttendance** action
3. Toggle to disabled (yellow ⚠️ icon)

---

## 📞 Emergency Manual Attendance

If automation fails:

1. **Web Portal**: https://satech.beehivehcm.com/
2. **Mobile App**: Open Beehive app manually
3. **Fallback**: Email HR/Manager

---

## 🗓️ Maintenance Schedule

- **Weekly**: Check logs for any failures
- **Monthly**: Verify automation still working
- **Quarterly**: Review and update selectors if app updates
- **Yearly**: Change Beehive password (then re-run setup)

---

## 🔗 Useful Links

- **Tasker Wiki**: https://tasker.joaoapps.com/userguide/en/
- **AutoInput Guide**: https://joaoapps.com/autoinput/
- **Beehive Portal**: https://satech.beehivehcm.com/
- **GitHub Repo**: [Your repo link]

---

**Need help?** Check [INSTALLATION.md](INSTALLATION.md) for detailed troubleshooting.
