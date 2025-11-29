# 🧪 Testing Guide - Beehive Attendance Automation

## Pre-Deployment Testing (1 Week)

### Day 1: Initial Setup & Unit Testing

#### ✅ Task 1.1: Installation Verification
- [ ] Tasker installed (version 6.0+)
- [ ] AutoInput installed
- [ ] Project imported without errors
- [ ] FirstTimeSetup completed
- [ ] Credentials stored (check VARS tab)

#### ✅ Task 1.2: SMS OTP Extraction Test
**Test**: Verify OTP regex works

```
Steps:
1. Ask colleague to send test SMS: "Your OTP is 123456"
2. Check if ExtractOTP_SMS task triggered
3. Verify %OTP_CODE variable = "123456"

Expected: Variable set correctly ✅
Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 1.3: Calendar Check Test
**Test**: Weekend detection

```
Steps:
1. Change phone date to Saturday
2. Run ExecuteMorningAttendance manually
3. Check for "Weekend Detected" notification

Expected: Automation exits early ✅
Actual: _______
Status: PASS / FAIL
```

---

### Day 2: Login Flow Testing

#### ✅ Task 2.1: App Launch Test
```
Steps:
1. Close Beehive app completely
2. Run PerformLogin task
3. Observe app opening

Expected: App launches within 3 seconds ✅
Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 2.2: Credential Auto-Fill Test
```
Steps:
1. Ensure logged out of Beehive
2. Run PerformLogin task
3. Watch for username/password entry

Expected: Both fields auto-filled ✅
Issues: _______
Status: PASS / FAIL
```

#### ✅ Task 2.3: OTP Flow End-to-End
```
Steps:
1. Run PerformLogin task
2. Wait for "Send OTP" click
3. Receive SMS
4. Verify OTP auto-filled
5. Check login success

Expected: Login completes in <2 minutes ✅
Actual time: _______
Status: PASS / FAIL
```

---

### Day 3: Attendance Submission Testing

#### ✅ Task 3.1: Regularize Attendance
```
Steps:
1. Manually login to Beehive
2. Run RegularizeAttendance task
3. Verify:
   - Navigation to regularize page ✅
   - Yesterday's date selected ✅
   - Reason entered: "Work from Home" ✅
   - Submit clicked ✅
   - Confirmation popup handled ✅

Expected: Previous day regularized ✅
Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 3.2: Punch-In Test
```
Steps:
1. Run MarkPunchIn task
2. Verify menu navigation
3. Check attendance marked

Expected: Punch recorded in Beehive ✅
Verify in app: _______
Status: PASS / FAIL
```

#### ✅ Task 3.3: Punch-Out Test
```
Steps:
1. Run MarkPunchOut task
2. Verify evening attendance marked

Expected: Punch-out recorded ✅
Verify in app: _______
Status: PASS / FAIL
```

---

### Day 4: End-to-End Integration Test

#### ✅ Task 4.1: Full Morning Flow
```
Steps:
1. Logout of Beehive
2. Clear %LAST_RUN_DATE
3. Run ExecuteMorningAttendance
4. Do NOT intervene
5. Wait for completion

Expected Flow:
1. Calendar check (weekday) ✅
2. App launch ✅
3. Login ✅
4. OTP handling ✅
5. Regularize yesterday ✅
6. Punch-in today ✅
7. Success notification ✅
8. Log entry created ✅

Completion time: ______ minutes
Issues encountered: _______
Status: PASS / FAIL
```

#### ✅ Task 4.2: Full Evening Flow
```
Steps:
1. Run ExecuteEveningAttendance
2. Observe full flow

Expected:
1. Login (if needed) ✅
2. Navigate to punch ✅
3. Punch-out marked ✅
4. Notification sent ✅

Status: PASS / FAIL
```

---

### Day 5: Error Handling & Edge Cases

#### ✅ Task 5.1: No Internet Test
```
Steps:
1. Turn OFF WiFi and mobile data
2. Run ExecuteMorningAttendance
3. Observe behavior

Expected: Graceful failure with retry ✅
Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 5.2: OTP Timeout Test
```
Steps:
1. Run automation
2. Do NOT send OTP (block SMS)
3. Wait 2 minutes

Expected: Timeout error notification ✅
Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 5.3: Already Marked Test
```
Steps:
1. Manually mark attendance
2. Run automation

Expected: Detection + skip duplicate ✅
Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 5.4: App Crash Test
```
Steps:
1. Run automation
2. Force-close Beehive app mid-flow
3. Observe recovery

Expected: Retry mechanism kicks in ✅
Actual: _______
Status: PASS / FAIL
```

---

### Day 6: Scheduled Automation Test

#### ✅ Task 6.1: Profile Trigger Test
```
Steps:
1. Change MorningPunchIn time to 2 minutes from now
2. Wait for trigger
3. Verify automation starts automatically

Expected: Automation runs without manual intervention ✅
Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 6.2: Battery Optimization Test
```
Steps:
1. Enable battery saver mode
2. Wait for scheduled time
3. Check if automation runs

Expected: Still runs despite battery saver ✅
Actual: _______
Status: PASS / FAIL
```

---

### Day 7: Production Readiness

#### ✅ Task 7.1: Weekend Skip Verification
```
Steps:
1. Wait until Saturday or Sunday
2. Verify no automation triggers
3. Check logs

Expected: No entries on weekend ✅
Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 7.2: Log File Check
```
Steps:
1. Review /sdcard/Tasker/BeehiveAttendance/logs/attendance.log
2. Verify all 5 weekdays logged

Expected Log:
Mon 11-25 09:27 | SUCCESS | Morning Punch-In ✅
Mon 11-25 18:27 | SUCCESS | Evening Punch-Out ✅
Tue 11-26 09:27 | SUCCESS | Morning Punch-In ✅
... (repeat for 5 days)

Actual: _______
Status: PASS / FAIL
```

#### ✅ Task 7.3: Failure Notification Test
```
Steps:
1. Intentionally break automation (disable AutoInput)
2. Run ExecuteMorningAttendance
3. Verify failure notification

Expected: "❌ Automation Failed" alert ✅
Actual: _______
Status: PASS / FAIL
```

---

## Test Summary Report

### Results Overview

| Test Category | Total Tests | Passed | Failed | Pass Rate |
|---------------|-------------|--------|--------|-----------|
| Installation | 5 | ___ | ___ | ___% |
| Login Flow | 3 | ___ | ___ | ___% |
| Attendance | 3 | ___ | ___ | ___% |
| End-to-End | 2 | ___ | ___ | ___% |
| Error Handling | 4 | ___ | ___ | ___% |
| Scheduling | 2 | ___ | ___ | ___% |
| Production | 3 | ___ | ___ | ___% |
| **TOTAL** | **22** | ___ | ___ | ___% |

### Critical Issues Found

1. Issue: _______________________________
   Severity: High / Medium / Low
   Status: Open / Fixed

2. Issue: _______________________________
   Severity: High / Medium / Low
   Status: Open / Fixed

### Recommendations

- [ ] All critical issues resolved
- [ ] Pass rate >90%
- [ ] Logs verified
- [ ] Notifications working
- [ ] Ready for production ✅

---

## Performance Benchmarks

| Metric | Target | Actual |
|--------|--------|--------|
| Full execution time | <2 min | _____ |
| OTP extraction time | <10 sec | _____ |
| Login time | <30 sec | _____ |
| Battery drain per run | <2% | ___% |
| Success rate (5 days) | >95% | ___% |

---

## Sign-Off

**Tester Name**: _______________________  
**Date**: _______________  
**Recommendation**: ✅ APPROVED / ⚠️ APPROVED WITH CONDITIONS / ❌ NOT APPROVED

**Notes**:
_______________________________________________
_______________________________________________
_______________________________________________

---

**Next Steps After Testing**:
1. If APPROVED → Enable profiles for production
2. If APPROVED WITH CONDITIONS → Address minor issues
3. If NOT APPROVED → Fix critical bugs and re-test

**Happy Testing! 🧪**
