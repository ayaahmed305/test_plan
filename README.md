# Test Plan: Linphone (SIP Softphone / VoIP Application)

> **Portfolio project.** This is an independent QA exercise on the open-source Linphone application. It is not affiliated with or endorsed by the Linphone project. Targets, thresholds and schedules in this document are reasonable proposals for a portfolio; in a real project they must be agreed with the team and the Product Owner. Feature descriptions should be confirmed against the exact Linphone version under test.

---

## Table of Contents

1. [Document Control](#1-document-control)
2. [Introduction](#2-introduction)
3. [Scope](#3-scope)
4. [Test Items](#4-test-items)
5. [Features to be Tested](#5-features-to-be-tested)
6. [Assumptions, Constraints and Dependencies](#6-assumptions-constraints-and-dependencies)
7. [Risk Analysis](#7-risk-analysis)
8. [Test Approach](#8-test-approach)
9. [Item Pass/Fail Criteria](#9-item-passfail-criteria)
10. [Entry, Exit, Suspension and Resumption Criteria](#10-entry-exit-suspension-and-resumption-criteria)
11. [Test Environment](#11-test-environment)
12. [Test Cases (Selected Examples)](#12-test-cases-selected-examples)
13. [Defect Management](#13-defect-management)
14. [Metrics, Reporting and Traceability](#14-metrics-reporting-and-traceability)
15. [Roles and Responsibilities](#15-roles-and-responsibilities)
16. [Schedule](#16-schedule)
17. [Release Criteria (Go / No-Go)](#17-release-criteria-go--no-go)
18. [Test Deliverables](#18-test-deliverables)
19. [Approvals](#19-approvals)
20. [References](#20-references)

---

## 1. Document Control

| Field | Value |
|---|---|
| Test Plan Identifier | TP-LINPHONE-1.0 |
| Document Title | Linphone Test Plan |
| Plan Type | Master Test Plan (covers all test levels for one release) |
| Version | 1.0 |
| Author | `<Your Name>`, QA Engineer |
| Status | Draft / For Review |
| Related Documents | Requirements (derived), Release Notes, Risk Register, Test Cases, Bug Reports |
| Approvers | QA Lead, Dev Lead, Product Owner |

**Revision History**

| Version | Date | Author | Description |
|---|---|---|---|
| 0.1 | `<2026-10-20>` | `<Your Name>` | First draft |
| 1.0 | `<YYYY-MM-DD>` | `<Your Name>` | Updated after QA Lead review |

**How this plan maps to the IEEE 829 test plan structure**

This plan follows the section list of IEEE 829 (superseded by ISO/IEC/IEEE 29119, but still widely used as a checklist), adapted to a mobile/desktop VoIP product.

| IEEE 829 section | Where in this document |
|---|---|
| Test plan identifier | 1. Document Control |
| Introduction | 2. Introduction |
| Test items | 4. Test Items |
| Features to be tested | 5. Features to be Tested |
| Features not to be tested | 3.2 Out of Scope |
| Approach | 8. Test Approach |
| Item pass/fail criteria | 9. Item Pass/Fail Criteria |
| Suspension and resumption criteria | 10.3 |
| Test deliverables | 18. Test Deliverables |
| Testing tasks and schedule | 16. Schedule |
| Environmental needs | 11. Test Environment |
| Responsibilities | 15. Roles and Responsibilities |
| Risks and contingencies | 7. Risk Analysis |
| Approvals | 19. Approvals |

---

## 2. Introduction

### 2.1 Purpose

This document defines the scope, approach, resources, schedule, risks, and entry/exit criteria for testing Linphone. Its goal is to give confidence that Linphone delivers reliable voice, video, and messaging on every supported platform before a release.

In ISTQB terms, a test plan describes the scope, approach, resources, and schedule of the intended test activities. This plan is the Master Test Plan for one release.

### 2.2 Product Overview

Linphone is an open-source softphone built on **SIP** (Session Initiation Protocol). Media is carried over **RTP/SRTP**. Based on the public documentation, the application provides:

- Account registration on a SIP server
- Outgoing and incoming audio and video calls
- Instant messaging (SIP MESSAGE)
- Media encryption (SRTP, ZRTP) and signaling over TLS
- NAT traversal using ICE, STUN, and TURN
- Multiple accounts, call transfer, hold, and conference
- Incoming-call notifications (push) and integration with CallKit (iOS) and ConnectionService (Android)

### 2.3 Quality Objectives

1. Calls connect and stay connected under normal and degraded network conditions.
2. Audio and video quality stays acceptable across network profiles.
3. Credentials and media are protected; encryption works as designed.
4. The app is stable on all supported platforms and OS versions.
5. Battery, memory, and CPU usage stay within acceptable limits.

### 2.4 Requirements Basis

There is no formal SRS for this portfolio exercise. Requirements  are derived from the official Linphone documentation, release notes, and observed behavior of the build under test. Each Requirement is listed in the traceability matrix (section 14.2).

---

## 3. Scope

### 3.1 In Scope

| Area | Details |
|---|---|
| Account and Registration | Register, unregister, automatic re-registration, authentication failure (Digest Auth) |
| Audio Calls | Outgoing, incoming, reject, end, hold/resume, mute, DTMF |
| Video Calls | Enable/disable video mid-call, camera switch, rotation |
| Call Management | Transfer (blind/attended), conference, call waiting, multiple calls |
| Instant Messaging | Send/receive, delivery status, offline messages |
| Network Resilience | Wi-Fi to mobile data switch, network loss, packet loss, jitter, latency |
| Security | TLS signaling, SRTP/ZRTP, certificate validation, credential storage |
| Notifications | Push for incoming calls and messages, behavior when app is closed |
| Settings | Codecs, STUN/TURN, transport (UDP/TCP/TLS), audio and video settings |
| Compatibility | Android, iOS, Windows, macOS, Linux (per officially supported list) |
| Interoperability | Linphone with other SIP clients and SIP desk phones |
| Localization | Arabic translation, RTL layout, number and date formats |
| Accessibility | Screen-reader support (VoiceOver/TalkBack) within available limits |
| Installation and Upgrade | Fresh install, upgrade from previous version, uninstall |

### 3.2 Out of Scope (Features Not to be Tested)

| Item | Reason |
|---|---|
| Linphone backend and provisioning infrastructure | Owned by the backend team; separate plan |
| The SIP server itself (Asterisk / FreeSWITCH / Kamailio) | Infrastructure used only as a dependency |
| PSTN gateways and carrier integration | Depends on external agreements |
| Codecs that are not officially supported | Outside the requirements |
| Large-scale load testing (thousands of concurrent calls on the server) | Covered by a separate performance plan |
| Penetration testing of the server side | Outside the scope of client QA |

---

## 4. Test Items

| Item | Description | Version |
|---|---|---|
| Linphone Android | APK / Play Store build | 10.0.0.208` |
| Linphone Desktop | Windows, macOS, Linux installers | `26H2.26300.9711` |


---

## 5. Features to be Tested

| Feature | Priority | Notes |
|---|---|---|
| Registration and Authentication | P0 | Foundation for everything else |
| Outgoing audio call | P0 | |
| Incoming audio call | P0 | All app states: foreground, background, killed |
| Call end / reject | P0 | |
| Push notification for incoming call | P0 | |
| Network switching during a call | P0 | |
| TLS and SRTP/ZRTP | P0 | |
| Video call | P1 | |
| Hold / Resume / Mute | P1 | |
| DTMF | P1 | |
| Transfer (blind and attended) | P1 | |
| Conference | P1 | |
| Instant Messaging | P1 | |
| Codec negotiation | P1 | |
| Battery and performance | P1 | |
| Settings and Configuration | P2 | |
| Localization (Arabic / RTL) | P2 | |
| Accessibility | P2 | |

---

## 6. Assumptions, Constraints and Dependencies

**Assumptions**

- The build delivered to QA is the Release Candidate that will be published.
- Test accounts and the SIP server are available for the whole test period.
- Release notes and documentation are available before testing starts.

**Constraints**

- Limited device budget, so a device matrix based on market share or analytics is used.
- Time is fixed by the sprint or release date.
- No access to the real production environment.

**Dependencies**

- SIP server and TURN server ready and stable.
- Android and iOS devices with different OS versions.
- Push notification services (FCM / APNs) working in the test environment.
- SIP numbers and test accounts.

---

## 7. Risk Analysis

**Scoring:** Likelihood (1-5) x Impact (1-5) = Risk Score.

| Level | Score | Treatment |
|---|---|---|
| High | 15-25 | Tested first, most test cases, executed on real devices |
| Medium | 8-14 | Full coverage in the planned cycle |
| Low | 1-7 | Covered if time allows or through exploratory testing |

### 7.1 Product Risks

| ID | Risk | Likelihood | Impact | Score | Level | Mitigation | Coverage |
|---|---|---|---|---|---|---|---|
| R01 | Calls fail to connect or registration fails | 4 | 5 | **20** | High | Extensive registration tests and negative cases | TC-REG-*, TC-CALL-* |
| R02 | Call drops when switching network (Wi-Fi to mobile data) | 4 | 5 | **20** | High | Network-switch tests on real devices | TC-NET-* |
| R06 | Incoming call notification not received when app is closed | 4 | 5 | **20** | High | Push tests on iOS and Android in every app state | TC-PUSH-* |
| R03 | Poor audio quality (jitter, packet loss) | 4 | 4 | **16** | High | Simulated degraded networks | TC-QOS-*, TC-NET-* |
| R05 | Credentials leak in logs or storage | 3 | 5 | **15** | High | Inspect storage, logs, and network traffic | TC-SEC-* |
| R07 | Crash or memory leak during long calls | 3 | 4 | **12** | Medium | 60+ minute soak test with memory monitoring | TC-PERF-* |
| R08 | Compatibility problems on specific devices/OS versions | 4 | 3 | **12** | Medium | Device matrix, most-used devices first | TC-COMP-* |
| R10 | NAT / firewall prevents connection | 3 | 4 | **12** | Medium | Test behind different NAT types with TURN | TC-NET-* |
| R11 | Conflict with system call services (CallKit / ConnectionService) | 3 | 4 | **12** | Medium | Test alongside normal phone calls | TC-INT-* |
| R04 | Encryption or certificate validation weakness (MITM) | 2 | 5 | **10** | Medium | TLS, ZRTP, and fake-certificate tests | TC-SEC-* |
| R09 | High battery drain during calls or in background | 3 | 3 | **9** | Medium | Measure with Android Profiler / Xcode Energy | TC-PERF-* |
| R12 | Arabic translation or RTL layout defects | 3 | 2 | **6** | Low | Full Arabic UI pass | TC-L10N-* |

### 7.2 Project Risks

| ID | Risk | Likelihood | Impact | Mitigation / Contingency |
|---|---|---|---|---|
| PR1 | Late or unstable build | 3 | 4 | Daily smoke test; suspension criteria in 10.3 |
| PR2 | SIP/TURN/Push test environment down | 3 | 4 | Backup server; DevOps on call |
| PR3 | Not enough real devices | 4 | 3 | Prioritize the device matrix; use cloud device farm if available |
| PR4 | Requirements unclear or changing | 3 | 3 | Review requirements early; log assumptions; re-plan in change meetings |
| PR5 | Schedule compression | 3 | 4 | Execute by risk level: High first, then Medium, then Low |

---

## 8. Test Approach

### 8.1 Test Levels

| Level | Owner | Objective |
|---|---|---|
| Unit Testing | Developers | Internal logic (SIP parser, codec selection) |
| Integration Testing | Dev + QA | SIP stack, media engine, and UI working together |
| System Testing | QA | Full application in a production-like environment |
| Acceptance Testing | Product Owner | User stories are satisfied |

### 8.2 Test Types

| Type | Details |
|---|---|
| Functional | Each feature against the requirements |
| Negative | Wrong credentials, non-existent numbers, sudden disconnects |
| Boundary | Max video resolution, max call duration, max message length |
| Regression | Smoke suite on every build; full regression before release |
| Interoperability | Linphone to Linphone, to Asterisk/FreeSWITCH, to SIP desk phones |
| Network | Packet loss (1%, 5%, 10%), latency (100 ms, 300 ms), jitter, limited bandwidth |
| Security | TLS validation, SRTP/ZRTP, Digest auth, SIP injection, storage encryption |
| Performance | Call setup time, memory, CPU, battery, soak test |
| Compatibility | OS versions, device models, screen sizes, hardware codecs |
| Usability | Call flow clarity, button discoverability, settings access |
| Localization | Arabic and RTL, dates and numbers, complete translation |
| Accessibility | VoiceOver/TalkBack, font size, contrast |
| Installation and Upgrade | Fresh install, upgrade from previous version, uninstall with data |
| Exploratory | Charter-based sessions on call flows and network changes |

### 8.3 Test Design Techniques

- **Equivalence Partitioning:** valid SIP addresses, invalid addresses, very long input, international numbers.
- **Boundary Value Analysis:** password length, call duration before timeout, maximum number of accounts.
- **State Transition Testing:** call states (Idle, Ringing, Connected, Hold, Ended) and registration states (Unregistered, Registering, Registered, Failed).
- **Decision Table:** incoming call behavior by app state (foreground, background, killed), existing call, and Do Not Disturb.
- **Pairwise Testing:** combinations of device, OS, network, and codec to limit the number of combinations without losing coverage.
- **Error Guessing:** known VoIP issues such as audio route delay, speaker/Bluetooth switching, and echo.
- **Risk-Based Test Selection:** test cases are tagged with priority and risk ID; High-risk areas are executed first (section 7).

### 8.4 Manual vs Automation

| Area | Method | Reason |
|---|---|---|
| Registration and Login | Automation (Appium / SIPp) | Repetitive and stable |
| Basic call flow | Automation and Manual | Automation for smoke; manual for audio quality |
| Audio/Video quality | Manual with measuring tools | Human judgment matters |
| Network switching | Manual on real devices | Emulators are not accurate enough |
| Push notifications | Manual | Depends on app and OS state |
| SIP protocol conformance | Automation (SIPp scenarios) | Precisely repeatable |
| Security analysis | Tools (Wireshark, MITM proxy) | Technical analysis, not UI |
| Localization | Manual | Needs visual review |

### 8.5 Test Data

- At least 20 SIP test accounts (including accounts for conference and for different permission levels).
- Numbers: valid, invalid, international format, very long.
- Message samples: English, Arabic (RTL), emoji, very long text, special characters.
- Network profiles saved and named so they can be reused (for example `loss5_lat300_jit50`).

---

## 9. Item Pass/Fail Criteria

**A test case passes when** the actual result matches the expected result and no unexpected crash, hang, or data exposure occurs.

**A test case fails when** the result differs from the expected result, or the app crashes, freezes, or exposes sensitive data.

**A test item (build) passes testing when**

- All exit criteria in section 10.2 are satisfied.
- No open Blocker or Critical defect exists.
- Security test cases (TLS, SRTP/ZRTP, credential storage) all pass.

**A test item fails when** any of the above is not satisfied. A failed build goes back to development and a new build re-enters at the entry criteria.

---

## 10. Entry, Exit, Suspension and Resumption Criteria

### 10.1 Entry Criteria (start testing when)

- Build delivered to QA with release notes.
- Smoke test (Build Verification Test) passes on the build.
- Test environment ready (SIP server, TURN, Push, devices).
- Test cases reviewed and approved.
- Test accounts ready.

### 10.2 Exit Criteria (testing is complete when)

- 100% of P0 test cases executed, with pass rate of at least 98%.
- At least 95% of P1 test cases executed, with pass rate of at least 95%.
- No open Blocker or Critical defects.
- Open Major defects have an approved workaround.
- Regression suite passes on all target platforms.
- Soak test passes (60-minute call without crash or visible memory leak).
- Test Summary Report prepared and reviewed.

### 10.3 Suspension and Resumption Criteria

- **Suspend when:** a Blocker prevents testing (for example the app crashes on launch), the SIP server is down, or more than 30% of executed test cases fail in one cycle.
- **Resume when:** a new build fixes the Blocker (or the environment is restored) and the smoke test passes again.

---

## 11. Test Environment

### 11.1 Device and OS Matrix

| Category | Devices | Versions |
|---|---|---|
| Android (high usage) | Samsung Galaxy S-series, Pixel, Xiaomi | Android 12, 13, 14 |
| Android (low-end) | Mid-range or older device | Android 10 or 11 |
| iOS | iPhone (last two generations) | iOS 17, 18 |
| iPad | iPad Air or Pro | iPadOS per support list |
| Windows | Windows 10, 11 | Latest |
| macOS | Intel and Apple Silicon | Latest two versions |
| Linux | Ubuntu LTS | Latest LTS |

### 11.2 Network Environments

| Environment | Tool | Setup |
|---|---|---|
| Stable Wi-Fi | Standard router | No shaping |
| 4G / 5G | Real SIM or network profiles | Per carrier |
| Poor network | Network Link Conditioner (iOS), Android Emulator profiles, `tc netem` (Linux), Clumsy (Windows) | 5% loss, 300 ms latency, 50 ms jitter |
| NAT and Firewall | Router with different NAT types (including Symmetric NAT) | To test TURN and ICE |
| Network switching | Device moving between Wi-Fi and mobile data | Manual or scripted |

### 11.3 Server and Infrastructure

- **SIP Server:** Asterisk or Kamailio with TLS enabled
- **TURN/STUN Server:** coturn
- **Push:** FCM (Android) and APNs (iOS) in a test setup
- **Test accounts:** at least 20

### 11.4 Tools

| Tool | Purpose |
|---|---|
| Wireshark | Analyze SIP and RTP; inspect TLS |
| sngrep | Trace SIP messages in the terminal |
| SIPp | Protocol and load testing |
| Appium | UI automation |
| Android Studio Profiler / Xcode Instruments | CPU, memory, battery |
| Charles Proxy / mitmproxy | Inspect traffic and test MITM scenarios |
| Jira + Xray / TestRail | Test case and defect management |
| Git / GitHub | Version control for test scripts and documents |

---

## 12. Test Cases (Selected Examples)

The full suite lives in `/test-cases`. These examples show the format and depth expected.

| ID | Feature | Precondition | Steps | Expected Result | Priority | Risk |
|---|---|---|---|---|---|---|
| TC-REG-001 | Registration | Valid SIP account | 1. Open the app 2. Enter username and password 3. Tap Login | Status becomes Registered within 3 seconds | P0 | R01 |
| TC-REG-002 | Registration | Account with wrong password | 1. Enter wrong password 2. Tap Login | Clear error message; no crash; no unlimited automatic retries | P0 | R01 |
| TC-REG-003 | Re-registration | App registered | 1. Cut the network for 30 seconds 2. Restore it | App re-registers automatically within 30 seconds | P0 | R01 |
| TC-CALL-001 | Outgoing call | Registered on two devices | 1. Call the other device 2. Wait for answer | Call connects; audio clear both ways; call setup under 3 seconds on Wi-Fi | P0 | R01 |
| TC-CALL-002 | Incoming call | App in foreground | 1. Call the device | Incoming screen with ringtone; Accept starts the call | P0 | R01 |
| TC-CALL-003 | Call rejection | Incoming call | 1. Tap Decline | Caller hears busy or is sent to voicemail, per configuration | P0 | R01 |
| TC-CALL-004 | Call waiting | Active call | 1. Third party calls in | Call waiting is shown or handled per settings | P1 | R01 |
| TC-HOLD-001 | Hold / Resume | Active call | 1. Tap Hold 2. Wait 10 s 3. Tap Resume | Remote side hears hold state; audio resumes in both directions | P1 | R01 |
| TC-NET-001 | Network switch | Active call on Wi-Fi | 1. Turn off Wi-Fi 2. App moves to mobile data | Call continues or recovers within 5 seconds; no sudden termination | P0 | R02 |
| TC-NET-002 | Packet loss | Network with 5% loss | 1. Start a call 2. Talk for two minutes | Speech remains intelligible; no repeated drops | P0 | R03 |
| TC-PUSH-001 | Push (app killed) | App fully closed | 1. Call the device | Notification arrives and the call screen appears within 5 seconds | P0 | R06 |
| TC-SEC-001 | TLS certificate | SIP over TLS enabled | 1. Use a proxy with a self-signed certificate 2. Try to register | App refuses the connection and shows a warning | P0 | R04 |
| TC-SEC-002 | Credential storage | Successful login | 1. Inspect app files on the device 2. Inspect logs | Password is not visible in plain text | P0 | R05 |
| TC-SEC-003 | SRTP | Encrypted call | 1. Capture RTP with Wireshark | Payload is encrypted and unreadable | P0 | R04 |
| TC-VID-001 | Video call | Cameras available on both sides | 1. Start a video call 2. Switch camera 3. Stop and restart video | Video continues, switching is fast, audio is not interrupted | P1 | R03 |
| TC-IM-001 | Instant message | Both users registered | 1. Send an Arabic text message | Message arrives with delivered status; Arabic displays correctly | P1 | R12 |
| TC-IM-002 | Offline message | Recipient offline | 1. Send message 2. Recipient opens the app | Message is delivered on reconnect | P1 | R01 |
| TC-TRF-001 | Blind transfer | Active call A-B | 1. B performs blind transfer to C | C rings, call is transferred, A hears C | P1 | R01 |
| TC-CONF-001 | Conference | Three registered users | 1. A calls B 2. A adds C | All three parties hear each other | P1 | R01 |
| TC-PERF-001 | Soak test | Active call | 1. Hold a 60-minute call 2. Monitor memory and CPU | No crash; memory stable; no clear leak | P1 | R07 |
| TC-INST-001 | Upgrade | Previous version installed with account | 1. Install the new version over the old one | Account, settings, and history preserved; app starts normally | P1 | R08 |
| TC-L10N-001 | Arabic UI | Device language Arabic | 1. Open all screens | RTL layout correct; no truncated text; numbers clear | P2 | R12 |

---

## 13. Defect Management

### 13.1 Severity

| Level | Definition | Example |
|---|---|---|
| Blocker | Blocks testing or basic use | App crashes on launch |
| Critical | Core function lost, no workaround | Incoming calls never arrive |
| Major | Important function affected, workaround exists | Video freezes after 10 minutes |
| Minor | Small issue with no functional impact | Spelling error in translation |
| Trivial | Cosmetic | Slightly misaligned button |

### 13.2 Priority

P1 (urgent, fix before release) to P4 (defer if there is no time).

### 13.3 Bug Lifecycle

```
New -> Assigned -> In Progress -> Fixed -> Ready for QA -> Verified -> Closed
                                              |
                                              +-> Reopened (fix failed)
New -> Rejected / Duplicate / Not a Bug (with documented reason)
```

### 13.4 Bug Report Template

```
Title: [Platform][Feature] Short, clear description
Build Version:
Platform / OS / Device:
Network Condition:
SIP Server / Account used:
Severity / Priority:

Steps to Reproduce:
1.
2.
3.

Expected Result:
Actual Result:
Reproducibility: (Always / Sometimes / Once)

Attachments: (logs, screenshots, Wireshark capture, video)
Notes:
```

---

## 14. Metrics, Reporting and Traceability

### 14.1 Metrics

| Metric | Formula | Target |
|---|---|---|
| Test Execution Progress | Executed / Total | 100% before release |
| Pass Rate | Passed / Executed | 95% or higher |
| Defect Density | Defects / feature size | Track the trend |
| Defect Leakage | Defects found by customers / Total defects | As low as possible |
| Requirement Coverage | Requirements with test cases / Total requirements | 100% |
| Risk Coverage | High-risk items with executed test cases / All high-risk items | 100% |
| Automation Coverage | Automated test cases / Total | As agreed |
| Open Blocker/Critical | Count of open defects | Zero |

**Reporting cadence:** short daily status, weekly report for stakeholders, final Test Summary Report before release.

### 14.2 Traceability Matrix (Summary)

Status values are `Not Run`, `In Progress`, `Passed`, `Failed`. All start as `Not Run`.

| Requirement | Description | Risk | Test Cases | Status |
|---|---|---|---|---|
| REQ-01 | Account registration and authentication | R01 | TC-REG-001..003 | Not Run |
| REQ-02 | Audio calls (outgoing, incoming, reject, hold) | R01 | TC-CALL-001..004, TC-HOLD-001 | Not Run |
| REQ-03 | Network resilience | R02, R03, R10 | TC-NET-001..002 | Not Run |
| REQ-04 | Secure transport and media | R04, R05 | TC-SEC-001..003 | Not Run |
| REQ-05 | Push notification for incoming calls | R06 | TC-PUSH-001 | Not Run |
| REQ-06 | Video calls | R03 | TC-VID-001 | Not Run |
| REQ-07 | Instant messaging | R01, R12 | TC-IM-001..002 | Not Run |
| REQ-08 | Call transfer and conference | R01 | TC-TRF-001, TC-CONF-001 | Not Run |
| REQ-09 | Stability and performance | R07, R09 | TC-PERF-001 | Not Run |
| REQ-10 | Installation and upgrade | R08 | TC-INST-001 | Not Run |
| REQ-11 | Arabic localization | R12 | TC-L10N-001 | Not Run |

---

## 15. Roles and Responsibilities

| Role | Responsibilities |
|---|---|
| QA Lead | Planning, review, approval of exit criteria, risk management |
| QA Engineers | Write and execute test cases, log defects, report |
| Automation Engineer | Build and maintain automation suites |
| Developers | Fix defects, write unit tests, support debugging |
| DevOps | Prepare environments, SIP server, TURN, push |
| Product Owner | Set priorities, accept the release |

---

## 16. Schedule

Example for a two-week sprint.

| Phase | Days | Activity |
|---|---|---|
| Planning | 1-2 | Review requirements, write the plan, update risk register |
| Preparation | 3-4 | Set up environments, devices, and test accounts |
| Test Design | 3-5 | Write and review test cases |
| Smoke and Functional | 6-8 | Execute P0 and P1 |
| Network and Security | 8-9 | Network, security, and push testing |
| Regression | 10 | Full regression on all platforms |
| Soak and Performance | 10-11 | Stability and performance tests |
| Reporting and Sign-off | 11-12 | Test Summary Report and release approval |

If time is cut, execution order follows the risk level in section 7 (High, then Medium, then Low).

---

## 17. Release Criteria (Go / No-Go)

**Go** if

- All exit criteria are met.
- No Blocker or Critical defect is open.
- Major defects have an approved workaround.

**No-Go** if

- Any Blocker or Critical defect is open.
- Any security test fails (TLS, SRTP, credential exposure).
- The crash rate is above the agreed limit.

---

## 18. Test Deliverables

1. Test Plan (this document)
2. Test Cases (`/test-cases`)
3. Risk Register (kept in sync with section 7)
4. Test Execution Reports (daily and weekly)
5. Bug Reports (`/bug-reports`)
6. Traceability Matrix
7. Automation Scripts (`/automation`)
8. Test Summary Report

---

## 19. Approvals

| Name | Role | Signature | Date |
|---|---|---|---|
| | QA Lead | | |
| | Dev Lead | | |
| | Product Owner | | |

---

## 20. References

- IEEE 829, Standard for Software Test Documentation (test plan structure)
- ISO/IEC/IEEE 29119, Software Testing standards (successor to IEEE 829)
- ISTQB Glossary and Foundation Level Syllabus, https://www.istqb.org
- Linphone official documentation and release notes (for requirements and supported features)
- RFC 3261 (SIP), RFC 3550 (RTP), RFC 3711 (SRTP), RFC 8445 (ICE)
