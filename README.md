# DanskBuddy — Manual Test Report

## Flow 1: Maja Nielsen · Native Speaker

**App URL:** `localhost:5173`  
**Tester:** Maja Nielsen — Native Danish speaker in Copenhagen  
**Test type:** Manual functional and UI testing  
**Flow tested:** Login, match requests, messaging, browsing, public profile, localStorage verification, profile page, logout

---

## Status Legend

| Icon | Meaning |
|---|---|
| ✅ | Passed |
| ⚠️ | Works, but needs improvement |
| ❌ | Failed |

---

## 1. Overall Result

**Overall result:** ⚠️ Passed with UX and product-flow improvements needed.

The main app flow is functional and stable. Login, match requests, messaging, browsing, filtering, public profiles, localStorage persistence, logout, and protected routes worked correctly.

The main problems are related to feedback, clarity, and visual hierarchy.

---

## 2. Test Environment

| Item | Value |
|---|---|
| App URL | `localhost:5173` |
| Browser | Google Chrome |
| DevTools used | Application tab, Console tab |
| Storage inspected | `localStorage` |
| Test user | Maja Nielsen |
| Email | `maja@test.com` |
| Password | `password123` |

Before testing, all `localStorage` keys starting with `danskbuddy_` were deleted and the page was refreshed to reload seed data.

---

## 3. Test Results

| Step | Test Area | Status | Notes |
|---:|---|---:|---|
| 1 | Login as Maja | ✅ | Login worked. User landed on Browse. |
| 2 | Navbar readability | ✅ | Logo, links, avatar, name, and logout were readable. |
| 3 | Initial `danskbuddy_matches` check | ✅ | 6 match objects were found. Carlos and Priya had pending requests for Maja. |
| 4 | Pending tab | ✅ | Carlos Mendez and Priya Sharma appeared correctly. |
| 5 | Pending cards | ✅ | Cards showed avatar, name, city, Danish level, bio, Accept and Decline buttons. |
| 6 | Accept Carlos | ⚠️ | Carlos moved to Connected, but no toast notification appeared. |
| 7 | Decline Priya | ⚠️ | Priya disappeared from Pending, but no toast notification appeared. |
| 8 | Verify match state | ✅ | Carlos was accepted and Priya was handled as declined in `localStorage`. |
| 9 | UI and localStorage consistency | ✅ | No mismatch was found. |
| 10 | Connected tab | ✅ | Lars Andersen and Carlos Mendez appeared. Count was `(2)`. |
| 11 | Open chat with Lars | ✅ | Chat opened correctly with existing messages. |
| 12 | Message alignment | ✅ | Lars's messages were on the left. Maja's messages were on the right. |
| 13 | Send message to Lars | ✅ | Message sent with Enter and appeared correctly. |
| 14 | Input clearing | ✅ | Input cleared after sending. |
| 15 | Message saved in `localStorage` | ✅ | New message was saved in `danskbuddy_messages['1::2']`. |
| 16 | Messages list | ⚠️ | Latest message appeared, but preview needs `You:` or sender name. |
| 17 | Pending tab after actions | ✅ | Pending tab was empty after handling both requests. |
| 18 | Sent tab | ✅ | Anna Hansen and Sofie Christensen appeared after Sofie request. |
| 19 | Declined tab | ⚠️ | Current behavior is unclear for users. |
| 20 | Browse page | ✅ | Full user list appeared and Maja was excluded. |
| 21 | Browse filters | ✅ | Role and Availability filters worked together using AND logic. |
| 22 | Reset filters | ✅ | Dropdowns reset to All, search cleared, full list returned. |
| 23 | Sofie public profile | ✅ | Profile opened correctly and all data was readable. |
| 24 | Connect to Sofie | ✅ | Button changed to Pending and became disabled. |
| 25 | Sofie state in Browse | ⚠️ | State updated correctly, but Pending and Connected look too similar. |
| 26 | My Profile page | ⚠️ | Data and Edit Profile button were visible, but layout needs better visual hierarchy. |
| 27 | Logout | ✅ | User was redirected to Login. |
| 28 | Protected route redirect | ✅ | Direct navigation to `/browse` redirected back to `/login`. |

---

## 4. localStorage Verification

**Result:** ✅ No localStorage mismatches were found.

Verified keys:

```js
JSON.parse(localStorage.getItem('danskbuddy_users'))
JSON.parse(localStorage.getItem('danskbuddy_matches'))
JSON.parse(localStorage.getItem('danskbuddy_messages'))
```

Verified successfully:

| Check | Status |
|---|---:|
| Initial match data | ✅ |
| Pending requester IDs | ✅ |
| Carlos accepted state | ✅ |
| Priya declined handling | ✅ |
| Message sender ID | ✅ |
| Message text | ✅ |
| Message persistence | ✅ |
| Current user exclusion from Browse | ✅ |
| Sofie's public profile data | ✅ |

---

## 5. Issues and Improvements

All issues and recommendations are collected here to avoid duplicate comments.

| Priority | Status | Issue | Recommendation |
|---|---:|---|---|
| High | ⚠️ | User role model is unclear: `Learner`, `Native speaker`, `Both` | Keep two roles: `Learner`, `Native speaker`. |
| Medium | ❌ | No toast after Accept | Show a confirmation toast, for example: `Connected!`. |
| Medium | ❌ | No toast after Decline | Show a confirmation toast, for example: `Request declined`. |
| Medium | ⚠️ | Declined request behavior is confusing | Simplify request states. Best option: remove Declined as a main tab and show feedback through toast notifications. |
| Medium | ⚠️ | Pending and Connected states look too similar | Use distinct styles: `Connect` = primary, `Pending` = neutral/muted, `Connected` = success style. |
| Medium | ⚠️ | Registration page feels too heavy | Add `Already have an account? Log in` and reduce required fields. Move optional details to Edit Profile. |
| Medium | ⚠️ | Messages lack timestamp | Add time, date when needed. |
| Low / Medium | ⚠️ | Message preview lacks sender context | Prefix preview with `You:` or sender name. |
| Low | ⚠️ | My Profile page needs stronger visual hierarchy | Add better spacing, clearer sections, and a stronger profile header. |
| Low | ⚠️ | Browse filters could be easier to scan | Suggested order: Role → Danish level → Availability → City → Search by interests. |
| Low | ⚠️ | Emoji avatars feel temporary | Replace emoji avatars with uploaded photos or initials. |
| Low | ⚠️ | Danish level labels could be more standard | Use CEFR-style labels: A1, A2, B1, B2, C1, C2, Native. |
| Low | ⚠️ | Translate button looks too strong | Make Translate a small secondary action below each message. |
| Low | ⚠️ | Declined empty state uses a green check icon | Replace it with a neutral empty state or delete the icon. |
| Low | ⚠️ | Top navigation may become crowded | Consider a left sidebar for Browse, Matches, Messages, and account actions as it shown at the design system. |

---

## 6. Suggested Message Metadata

Messages preview should include lightweight metadata.

| Message age | Display format |
|---|---|
| Today | `14:35` |
| Yesterday | `June 25` |
| Older messages | `May 17` |


Example:

```text
You: Hej Lars! Hvad laver du i weekenden?           `June 25`
```


## 7. Final Conclusion

Flow 1 is functionally stable.

The following areas passed:

- Login
- Navbar readability
- Seed data loading
- Pending request display
- Match state updates
- Connected tab
- Existing chat
- Sending messages
- Message persistence
- Messages list ordering
- Browse page
- Browse filters
- Public profile display
- Sending a new connection request
- My Profile data display
- Logout
- Protected route redirect

No `localStorage` mismatches were found.

**Final result:** ⚠️ Passed with UX and product-flow improvements needed.
