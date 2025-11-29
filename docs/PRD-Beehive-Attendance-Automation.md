# Product Requirements Document: Beehive HCM Attendance Automation System

**Version**: 1.0  
**Date**: November 24, 2025  
**Author**: Product Development Team  
**Status**: Draft for Review

---

## 1. Executive Summary

### Overview
This PRD outlines the development of an automated attendance submission system for SA Tech's Beehive HCM portal. The solution addresses the daily manual burden of submitting punch-in/out records while navigating OTP-based authentication and corporate IT restrictions.

### Business Value
- **Time Savings**: 15-20 minutes/day per employee
- **Error Reduction**: Eliminates missed attendance submissions
- **Compliance**: Maintains corporate IT policy adherence
- **User Experience**: Reduces repetitive manual tasks

### Recommended Solution
**Primary**: Android Tasker automation with SMS OTP extraction  
**Fallback**: GitHub Actions with email-based OTP handling

### Success Metrics
- 95%+ automated attendance submission success rate
- <2 minute average execution time
- Zero corporate IT policy violations
- 90%+ user satisfaction score

---

## 2. Problem Statement

### Current Pain Points
1. **Daily Manual Burden**: 22-step process required twice daily (9:30 AM, 6:30 PM)
2. **OTP Friction**: Two-factor authentication requires phone access
3. **Previous Day Regularization**: Must submit yesterday's attendance before today's punch-in
4. **Timing Constraints**: Late submissions may be flagged as absent
5. **Corporate Restrictions**: Work laptop blocks .bat, cmd, PowerShell scripts

### User Impact
- **Individual**: 5+ hours/month spent on attendance submission
- **Organizational**: Productivity loss across workforce
- **Risk**: Missed submissions lead to HR queries and potential payroll issues

---

## 3. Stakeholder Perspectives

### 🎯 Product Manager Perspective

**Key Requirements**:
- Solution must work with zero changes to Beehive HCM
- Support both web and mobile app interfaces
- Graceful degradation when automation fails
- Clear user notifications on success/failure

**Success Criteria**:
- MVP launch within 4 weeks
- 50+ user beta testing phase
- Production rollout to full organization within 3 months

---

### 🔒 Security Engineer Perspective

**Security Concerns**:
1. **Credential Storage**: Encryption at rest and in transit
2. **OTP Handling**: No logging of sensitive authentication codes
3. **Session Management**: Automatic timeout and re-authentication
4. **Audit Trail**: Complete logging of automation actions

**Mitigation Strategies**:
- Use Android Keystore for credential encryption
- Implement certificate pinning for API calls
- Enable 2FA backup codes for failover
- Regular security audits of automation scripts

**Risk Assessment**:
- **High Risk**: Stored credentials on personal devices → Mitigated by Android Keystore encryption
- **Medium Risk**: OTP interception → Mitigated by local-only SMS processing
- **Low Risk**: Session hijacking → Mitigated by short-lived tokens

---

### ⚙️ DevOps Engineer Perspective

**Deployment Considerations**:
- **Environment**: Android personal devices (no server infrastructure for primary solution)
- **CI/CD**: Version control for Tasker project XML exports
- **Monitoring**: Daily success/failure notifications via Telegram/Discord
- **Rollback**: Manual disable of Tasker profiles if issues arise

**Infrastructure** (Fallback GitHub Actions):
```yaml
# No-cost cloud infrastructure
- GitHub Actions: 2,000 minutes/month (free tier)
- Secrets Management: GitHub encrypted secrets
- Logging: GitHub Actions run history (90 days retention)
```

**Scalability**: Individual user deployments, no centralized infrastructure needed

---

### 👤 End User Perspective

**User Journey**:
```
Morning (9:25 AM):
├─ [Automated] Phone alarm triggers Tasker
├─ [Automated] Beehive app opens
├─ [Automated] Login with stored credentials
├─ [Automated] OTP extracted from SMS & auto-filled
├─ [Automated] Navigate to Regularize Attendance
├─ [Automated] Submit previous day with "Work from Home"
├─ [Automated] Navigate to Punch-In
├─ [Automated] Mark today's attendance
└─ [Notification] "✅ Attendance marked successfully at 9:27 AM"

Evening (6:25 PM):
└─ [Same automated flow for punch-out]
```

**User Concerns**:
- "What if my phone is dead?": Manual fallback via web portal
- "What if automation fails?": Instant notification with manual override button
- "Can I disable it on vacation?": Yes, simple toggle in Tasker widget

**Desired Experience**:
- "Set it and forget it" reliability
- Clear visibility into automation status
- Easy emergency manual override

---

### 🏢 IT Admin Perspective

**Compliance Checkpoints**:
- ✅ No software installation on corporate laptops
- ✅ No batch/cmd/PowerShell script execution
- ✅ Personal device usage (BYOD compliant)
- ✅ No corporate network traffic manipulation
- ✅ Audit-friendly logging

**Support Burden**:
- **Low**: User-managed personal devices
- **Training**: 1-hour onboarding session
- **Documentation**: Step-by-step setup guides

**Corporate Policy Alignment**:
```
Policy: No automated scripts on work laptop
Solution: Runs on personal Android phone ✅

Policy: No credential sharing
Solution: Individual user credentials in encrypted storage ✅

Policy: No unauthorized API access
Solution: Uses official Beehive mobile app ✅
```

---

### 🧪 QA Engineer Perspective

**Test Strategy**:

**Unit Tests** (Tasker Tasks):
- OTP regex extraction accuracy
- Date calculation for previous day
- Error handling on network failure

**Integration Tests**:
- End-to-end workflow from login to punch submission
- OTP SMS arrival timing variations
- Calendar verification (holidays, leaves)

**Edge Cases**:
1. **Already Marked**: Attendance already submitted manually
2. **Holiday**: Skip automation on public holidays
3. **Leave Applied**: Detect leave status and skip
4. **Network Failure**: Retry with exponential backoff
5. **App Update**: UI element selector changes

**Testing Matrix**:
| Scenario | Expected Behavior | Pass Criteria |
|---|---|---|
| Normal workday | Auto-submit attendance | ✅ Success notification |
| Weekend | Skip automation | 🛑 No action taken |
| Public holiday | Skip automation | 🛑 No action taken |
| Leave applied | Skip automation | 🛑 No action taken |
| Already marked | Skip duplicate | ⚠️ Warning notification |
| OTP delay | Wait & retry | ✅ Success after retry |
| Network failure | Retry 3x, then alert | ❌ Manual intervention |

---

## 4. User Personas

### Persona 1: Busy Professional
- **Name**: Rajesh, Software Engineer
- **Age**: 28
- **Tech Savvy**: High
- **Pain Point**: Forgets attendance during deep work sessions
- **Goal**: Complete automation with zero intervention
- **Device**: Android flagship (Samsung/OnePlus)

### Persona 2: Regular Employee
- **Name**: Priya, HR Manager
- **Age**: 35
- **Tech Savvy**: Medium
- **Pain Point**: Manual OTP entry is tedious
- **Goal**: Simple setup, reliable execution
- **Device**: Mid-range Android

---

## 5. Functional Requirements

### FR-1: Authentication & Security
- **FR-1.1**: Securely store Beehive credentials using Android Keystore
- **FR-1.2**: Auto-extract OTP from SMS using regex pattern `\b\d{4,6}\b`
- **FR-1.3**: Support both email and SMS OTP delivery methods
- **FR-1.4**: Implement session persistence with cookie storage
- **FR-1.5**: Auto-logout after 8 hours of inactivity

### FR-2: Attendance Submission
- **FR-2.1**: Daily 9:30 AM punch-in automation (weekdays only)
- **FR-2.2**: Daily 6:30 PM punch-out automation (weekdays only)
- **FR-2.3**: Previous day regularization before current day punch-in
- **FR-2.4**: Default reason: "Work from Home" (user-configurable)
- **FR-2.5**: Calendar verification (skip holidays/leaves)

### FR-3: User Interface (Tasker)
- **FR-3.1**: Configuration screen for credentials entry
- **FR-3.2**: Enable/Disable toggle widget
- **FR-3.3**: Manual trigger button for immediate execution
- **FR-3.4**: Status dashboard showing last 7 days' history
- **FR-3.5**: Settings: OTP timeout, retry attempts, notification preferences

### FR-4: Notifications & Alerts
- **FR-4.1**: Success notification with timestamp
- **FR-4.2**: Failure alert with error details
- **FR-4.3**: Daily summary (morning: "Regularized + Punched In")
- **FR-4.4**: Weekly report (Monday morning: "5/5 successful submissions")
- **FR-4.5**: Critical alerts: OTP timeout, network failure, app update needed

### FR-5: Error Handling & Recovery
- **FR-5.1**: 3 retry attempts with 30-second intervals
- **FR-5.2**: Fallback to manual mode on failure
- **FR-5.3**: Queue failed submissions for manual review
- **FR-5.4**: Auto-recovery after temporary failures
- **FR-5.5**: Detailed error logging for debugging

---

## 6. Non-Functional Requirements

### NFR-1: Performance
- **Execution Time**: <2 minutes end-to-end
- **OTP Extraction**: <10 seconds from SMS receipt
- **Network Latency**: Tolerate up to 30s delays
- **Battery Impact**: <2% per execution

### NFR-2: Reliability
- **Success Rate**: 95%+ over 30-day period
- **Uptime**: 99.5% (excluding planned maintenance)
- **Data Accuracy**: 100% (no incorrect submissions)
- **Failover**: Manual fallback always available

### NFR-3: Usability
- **Setup Time**: <15 minutes for first-time configuration
- **Learning Curve**: <1 hour training required
- **Accessibility**: Support for users with visual/motor impairments
- **Languages**: English (expandable to regional languages)

### NFR-4: Security
- **Encryption**: AES-256 for credentials at rest
- **TLS**: 1.3 for network communication
- **Audit Logs**: 90-day retention
- **Compliance**: GDPR, SOC 2 equivalent

### NFR-5: Maintainability
- **Code Quality**: Modular Tasker tasks
- **Documentation**: Inline comments, user manual
- **Version Control**: Git for Tasker XML exports
- **Update Frequency**: Monthly for UI selector updates

---

## 7. Technical Architecture

### Primary Solution: Android Tasker

```
┌─────────────────────────────────────────┐
│         Tasker Profile Layer            │
├─────────────────────────────────────────┤
│  Profile 1: Punch-In (9:30 AM, Mon-Fri) │
│  Profile 2: Punch-Out (6:30 PM, Mon-Fri)│
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│          Task Execution Flow            │
├─────────────────────────────────────────┤
│  1. Check Calendar (Holiday/Leave API)  │
│  2. Launch Beehive Mobile App           │
│  3. Enter Credentials (Keystore)        │
│  4. Intercept SMS (BroadcastReceiver)   │
│  5. Extract OTP (Regex)                 │
│  6. AutoInput: Fill OTP Field           │
│  7. Navigate to Regularize Attendance   │
│  8. Submit Previous Day                 │
│  9. Navigate to Punch-In                │
│ 10. Mark Attendance                     │
│ 11. Send Notification (Success/Fail)    │
└─────────────────────────────────────────┘
```

### Components

**1. Tasker Profiles** (Triggers)
```
Profile: MorningPunchIn
Event: Time 09:25 Mon,Tue,Wed,Thu,Fri
Task: ExecuteAttendance
```

**2. AutoInput Plugin** (UI Automation)
- Element detection by resource-id, text, xpath
- Tap, swipe, text entry actions
- Screenshot-based verification

**3. Secure Storage**
```
Encryption: Android Keystore System
Credentials: {
  "username": "encrypted",
  "password": "encrypted",
  "regularize_reason": "Work from Home"
}
```

**4. OTP Extraction**
```javascript
// Tasker JavaScript
var sms = global('SMSRB'); // Received SMS body
var otpPattern = /\b(\d{4,6})\b/;
var match = sms.match(otpPattern);
if (match) {
  setGlobal('OTP_CODE', match[1]);
  flash('OTP extracted: ' + match[1]);
}
```

---

### Fallback Solution: GitHub Actions + Playwright

```yaml
# .github/workflows/attendance.yml
name: Beehive Attendance Bot

on:
  schedule:
    - cron: '0 4 * * 1-5'    # 9:30 AM IST
    - cron: '0 13 * * 1-5'   # 6:30 PM IST
  workflow_dispatch:

jobs:
  mark-attendance:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - name: Run Automation
        env:
          BEEHIVE_USERNAME: ${{ secrets.BEEHIVE_USERNAME }}
          BEEHIVE_PASSWORD: ${{ secrets.BEEHIVE_PASSWORD }}
          EMAIL_HOST: ${{ secrets.EMAIL_HOST }}
          EMAIL_USER: ${{ secrets.EMAIL_USER }}
          EMAIL_PASSWORD: ${{ secrets.EMAIL_PASSWORD }}
        run: node src/mark-attendance.js
```

---

## 8. Implementation Roadmap

### Phase 1: MVP Development (Week 1-2)
**Deliverables**:
- Tasker project with core automation flow
- OTP extraction module
- Basic error handling
- Success/failure notifications

**Tasks**:
- [x] Research Beehive mobile app UI elements
- [ ] Create Tasker profiles for scheduling
- [ ] Develop AutoInput action sequences
- [ ] Implement SMS OTP interception
- [ ] Build credential encryption module
- [ ] Test end-to-end workflow

### Phase 2: Testing & Validation (Week 3)
**Deliverables**:
- Comprehensive test suite
- Edge case handling
- Performance optimization
- User documentation

**Tasks**:
- [ ] 5-day continuous testing
- [ ] Holiday/leave scenario testing
- [ ] Network failure resilience testing
- [ ] Battery impact measurement
- [ ] User acceptance testing (5 beta users)

### Phase 3: Production Rollout (Week 4)
**Deliverables**:
- Production-ready Tasker project export
- Setup guide with screenshots
- Video tutorial
- Support documentation

**Tasks**:
- [ ] Finalize deployment package
- [ ] Create installation wizard
- [ ] Record demo video
- [ ] Onboarding training session
- [ ] Launch monitoring dashboard

### Phase 4: Monitoring & Iteration (Ongoing)
- Weekly success rate analysis
- Monthly UI selector updates (if Beehive app updates)
- Quarterly feature enhancements
- User feedback incorporation

---

## 9. Risk Assessment & Mitigation

| Risk | Impact | Probability | Mitigation |
|---|---|---|---|
| **Beehive app UI changes** | High | Medium | Version monitoring, fallback to web portal |
| **OTP SMS delays** | Medium | Low | 5-minute buffer, 3 retry attempts |
| **Phone battery dead** | Medium | Low | Daily charge reminders, manual fallback |
| **Network outages** | Medium | Low | Offline queueing, retry logic |
| **Corporate policy changes** | High | Very Low | Legal review, compliance monitoring |
| **User credential compromise** | High | Very Low | Keystore encryption, regular password rotation |

---

## 10. Verification & Testing Strategy

### Automated Tests
```bash
# Unit Tests (Tasker Tasks)
- test_otp_extraction.tsk.xml
- test_date_calculation.tsk.xml
- test_network_retry.tsk.xml

# Run command: Import tasks and execute via Tasker
```

### Manual Testing Checklist
- [ ] Install Tasker + AutoInput on Android device
- [ ] Import attendance-automation.prj.xml
- [ ] Configure credentials in Settings task
- [ ] Trigger "Test Workflow" task manually
- [ ] Verify: Login → OTP → Regularize → Punch → Notification
- [ ] Expected result: "✅ Test successful" notification within 2 minutes

### User Acceptance Testing (UAT)
- 5 beta users across different Android devices
- 2-week trial period
- Daily feedback surveys
- Success criteria: 4/5 users report "satisfied" or "very satisfied"

---

## 11. Success Metrics & KPIs

### Primary Metrics
- **Automation Success Rate**: Target 95%+
- **Average Execution Time**: Target <2 minutes
- **User Satisfaction**: Target 4.5/5 stars
- **Manual Interventions Required**: Target <2/month

### Secondary Metrics
- Time saved per user: 15 min/day × 20 workdays = 5 hours/month
- ROI: ($20/hour × 5 hours) - $6 Tasker cost = $94/month value
- Adoption rate: Target 80% of eligible employees within 6 months

---

## 12. Future Enhancements (Post-MVP)

### V2.0 Features
- **Smart Scheduling**: ML-based optimal timing (avoid server load)
- **Multi-Account Support**: Manage attendance for family members
- **Voice Commands**: "Hey Google, mark my attendance"
- **Geo-Fencing**: Auto-trigger when leaving home
- **Integration**: Sync with Google Calendar, Notion, etc.

### V3.0 Features
- **Desktop Companion**: Browser extension for non-Android users
- **Beehive API Integration**: Direct API calls (pending company approval)
- **Advanced Analytics**: Monthly reports, trends, insights
- **Team Dashboard**: Organization-wide attendance insights

---

## 13. Appendices

### A. Glossary
- **OTP**: One-Time Password
- **Tasker**: Android automation app
- **AutoInput**: Tasker plugin for UI interactions
- **Keystore**: Android secure credential storage API
- **IMAP**: Email protocol for OTP extraction

### B. References
- Beehive HCM Portal: https://satech.beehivehcm.com/
- Tasker Documentation: https://tasker.joaoapps.com/
- AutoInput Guide: https://joaoapps.com/autoinput/
- Android Keystore: https://developer.android.com/training/articles/keystore

### C. Contact & Support
- **Product Owner**: [Name]
- **Technical Lead**: [Name]
- **Support Email**: attendance-automation@company.com
- **Slack Channel**: #beehive-automation

---

**END OF DOCUMENT**
