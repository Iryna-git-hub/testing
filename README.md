# DanskBuddy — Manual Test Report

## Flow 1: Maja Nielsen · Native Speaker

**App URL:** `localhost:5173`
**Tester:** Maja Nielsen — Native Danish speaker in Copenhagen
**Test type:** Manual functional and UI testing
**Flow tested:** Login, match requests, messaging, browsing, public profile, localStorage verification, profile page, logout

---

## 1. Test Scope

This report covers Flow 1 for Maja Nielsen.

The tested areas include:

* Login
* Navbar readability
* Pending match requests
* Accept / Decline actions
* Match state verification in `localStorage`
* Connected matches
* Messaging
* Message persistence in `localStorage`
* Message list preview
* Browse filters
* Public profile
* New connection request
* My Profile page
* Logout and protected route redirect
* UI and product-flow concerns

---

## 2. Test Environment

| Item              | Value                        |
| ----------------- | ---------------------------- |
| App URL           | `localhost:5173`             |
| Browser           | Google Chrome                |
| DevTools used     | Application tab, Console tab |
| Storage inspected | `localStorage`               |
| Test user         | Maja Nielsen                 |
| Email             | `maja@test.com`              |
| Password          | `password123`                |

Before testing, all `localStorage` keys starting with `danskbuddy_` were deleted and the page was refreshed to reload seed data.

---

## 3. Overall Result

**Overall result:** Passed with UX and product-flow improvements needed.

Most core functionality works correctly. No localStorage mismatches were found.

Main problems:

* No toast notifications after Accept / Decline.
* Declined request behavior is confusing.
* Pending and Connected states look too similar.
* Message previews need sender context.
* Messages need timestamps and sent / delivered / read status.
* Registration and user role logic need improvement.
* Several UI areas need clearer visual hierarchy and styling.

---

## 4. Step-by-Step Test Results

| Step    | Test Area                          | Result                                         | Notes                                                                                                                         |
| ------- | ---------------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 1       | Login as Maja                      | Passed                                         | Login worked. User landed on Browse. Maja's avatar and name were visible in the navbar.                                       |
| 1 UI    | Navbar readability                 | Passed                                         | Logo, center links, avatar/name, and logout were readable.                                                                    |
| 2       | Initial `danskbuddy_matches` check | Passed                                         | 6 match objects were found. Two pending requests for Maja had `receiverId: "1"`.                                              |
| 2 Data  | Requester IDs                      | Passed                                         | Requesters matched expected users: Carlos `id: "7"` and Priya `id: "6"`.                                                      |
| 3       | Pending tab                        | Passed                                         | Carlos Mendez and Priya Sharma appeared correctly.                                                                            |
| 3 UI    | Pending cards                      | Passed                                         | Cards showed avatar, name, city, Danish level, bio, Accept and Decline buttons.                                               |
| 3 UI    | Accept / Decline button clarity    | Passed                                         | Buttons were understandable and clearly labeled.                                                                              |
| 4       | Accept Carlos                      | Partially passed                               | Carlos was accepted and moved to Connected, but no toast notification appeared.                                               |
| 4 UI    | Toast after Accept                 | Failed                                         | Expected `Connected!` toast did not appear.                                                                                   |
| 5       | Decline Priya                      | Partially passed                               | Priya disappeared from Pending, but no toast notification appeared.                                                           |
| 5 UI    | Toast after Decline                | Failed                                         | Expected `Declined` toast did not appear.                                                                                     |
| 5 UX    | Declined user behavior             | Needs improvement                              | The request disappears without feedback, which is unclear.                                                                    |
| 6       | Verify Carlos match state          | Passed                                         | Carlos had `status: "accepted"` in localStorage.                                                                              |
| 6       | Verify Priya match state           | Passed / UI unclear                            | Priya was handled as declined, but she disappeared from the UI.                                                               |
| 6 Data  | UI and localStorage consistency    | Passed                                         | No localStorage mismatch was found.                                                                                           |
| 7       | Connected tab                      | Passed                                         | Lars Andersen and Carlos Mendez appeared. Count was `(2)`.                                                                    |
| 7 UI    | Send Message buttons               | Passed                                         | Each connected card had a Send Message button.                                                                                |
| 8       | Open chat with Lars                | Passed                                         | Chat opened correctly with 5 existing messages.                                                                               |
| 8 UI    | Message alignment                  | Passed                                         | Lars's messages were on the left, Maja's messages were on the right.                                                          |
| 8 UI    | Chat header                        | Passed                                         | Header showed Lars's name, Online chip, role chip, and level badge.                                                           |
| 9       | Send message to Lars               | Passed                                         | Message sent with Enter and appeared on the right.                                                                            |
| 9 UI    | Input clearing                     | Passed                                         | Input cleared after sending.                                                                                                  |
| 10      | Verify message in localStorage     | Passed                                         | New message was saved in `danskbuddy_messages['1::2']`.                                                                       |
| 10 Data | Message sender and text            | Passed                                         | Last message had `senderId: "1"` and exact expected text.                                                                     |
| 11      | Back to Messages list              | Passed                                         | Returned to Messages list. Lars appeared at the top.                                                                          |
| 11 UI   | Message preview                    | Needs improvement                              | Preview text matched the latest message, but it was not clear that it was the latest chat message. Add `You:` or sender name. |
| 12      | Pending tab after actions          | Passed                                         | Pending tab was empty after handling both requests.                                                                           |
| 12 UI   | Empty state                        | Passed                                         | Empty state appeared.                                                                                                         |
| 13      | Sent tab                           | Passed                                         | Anna Hansen appeared as awaiting reply. Carlos and Priya did not appear.                                                      |
| 14      | Declined tab                       | Passed according to current logic / UX unclear | Tab was empty, but this behavior is not intuitive. Green check icon should be removed.                                        |
| 15      | Browse page                        | Passed                                         | Full user list appeared and Maja was excluded. Count looked sensible.                                                         |
| 16      | Verify Maja user object            | Passed                                         | Maja's localStorage user data matched expected current-user exclusion logic.                                                  |
| 17      | Browse filters                     | Passed                                         | Role + Availability filters worked together using AND logic. Count updated correctly.                                         |
| 18      | Reset filters                      | Passed                                         | Dropdowns reset to All, search cleared, full list returned.                                                                   |
| 19      | Sofie public profile               | Passed                                         | Profile opened correctly and all data was readable. No overlap or cut-off content.                                            |
| 19 UI   | Connect button on Sofie profile    | Passed                                         | Connect button was enabled because Maja had no existing match with Sofie.                                                     |
| 20      | Verify Sofie user object           | Passed                                         | Name, city, Danish level, bio, and topics matched localStorage.                                                               |
| 21      | Connect to Sofie                   | Passed                                         | Button changed to Pending and became disabled.                                                                                |
| 22      | Sofie state in Browse              | Passed                                         | Sofie's Browse card also showed Pending.                                                                                      |
| 22 UI   | Pending / Connected visual clarity | Needs improvement                              | Pending and Connected use the same or very similar color. They should be visually different.                                  |
| 23      | Sent tab after Sofie request       | Passed                                         | Sent tab showed Anna Hansen and Sofie Christensen. Count was `(2)`.                                                           |
| 24      | My Profile page                    | Passed with UI concern                         | Read-only profile data and Edit Profile button were visible. Page needs stronger visual design.                               |
| 25      | Logout                             | Passed                                         | User was redirected to Login. No Maja data was visible.                                                                       |
| 25 Auth | Protected route redirect           | Passed                                         | Direct navigation to `/browse` redirected back to `/login`.                                                                   |

---

## 5. localStorage Verification

No localStorage mismatches were found.

Verified keys:

```js
JSON.parse(localStorage.getItem('danskbuddy_users'))
JSON.parse(localStorage.getItem('danskbuddy_matches'))
JSON.parse(localStorage.getItem('danskbuddy_messages'))
```

Verified successfully:

* Initial match data
* Pending requester IDs
* Carlos accepted state
* Priya declined handling
* Message sender ID
* Message text
* Message persistence
* Current user exclusion from Browse
* Sofie's public profile data

---

## 6. What Worked Well

The main user flow worked correctly.

Passed areas:

* Login
* Navbar readability
* Seed data loading
* Pending request display
* localStorage match data
* Accepting Carlos
* Connected tab
* Existing chat with Lars
* Message alignment
* Sending a new message
* Message persistence
* Messages list ordering
* Empty Pending tab after actions
* Sent requests
* Browse page
* Browse filters with AND logic
* Reset filters
* Public profile display
* Public profile data matching localStorage
* Sending a new request to Sofie
* Pending state consistency between profile and Browse
* My Profile data display
* Logout
* Protected route redirect

---

## 7. Issues Found

### Issue 1 — No toast notification after Accept or Decline

**Severity:** Medium
**Related steps:** 4, 5
**Area:** Matches / Pending requests

When accepting Carlos or declining Priya, no toast notification appeared.

Expected:

* Accept should show a confirmation like `Connected!`
* Decline should show a confirmation like `Request declined`

Actual:

* The state changed silently.
* Carlos moved to Connected.
* Priya disappeared from Pending.
* The user did not receive clear feedback.

**Recommendation:**
Add toast notifications for both Accept and Decline actions.

---

### Issue 2 — Declined request behavior is confusing

**Severity:** Medium
**Related steps:** 5, 6, 12, 14
**Area:** Matches / Request management

After declining Priya, she disappeared from Pending. The Declined tab stayed empty because it only shows requests sent by Maja that were declined by others.

This is technically explainable, but not intuitive. If I decline someone, I expect either clear feedback or a visible declined state.

**Recommendation:**

Simplify the flow:

* Remove the Declined tab entirely.
* Show a toast when a request is declined.
* Do not show declined incoming or outgoing requests in a main tab.
* After declining someone, allow the user to send a new Connect request later.
* On that user's Browse card or profile, show `Connect` again.

Alternative, if declined requests must stay visible:

* Split them into `Declined by me` and `Declined by others`.

However, this may be too complex for this app.

---

### Issue 3 — Message preview needs sender context

**Severity:** Low / Medium
**Related step:** 11
**Area:** Messages list

The latest message preview appears under Lars's name, but it looks like a generic subtitle instead of the latest chat message.

Current:

`Hej Lars! Hvad laver du i weekenden?`

Recommended:

`You: Hej Lars! Hvad laver du i weekenden?`

or:

`Maja: Hej Lars! Hvad laver du i weekenden?`

**Recommendation:**
Add the sender name or `You:` before the preview text.

---

### Issue 4 — Pending and Connected states look too similar

**Severity:** Medium
**Related step:** 22
**Area:** Browse / Profile / Matches

The `Pending` and `Connected` button states use the same or very similar color, so they are not easy to distinguish.

**Recommendation:**

| State     | Suggested Style                                      |
| --------- | ---------------------------------------------------- |
| Connect   | Main brand color                                     |
| Pending   | Neutral, muted, or outlined disabled style           |
| Connected | Success style, badge style, or distinct filled color |

---

### Issue 5 — Registration page needs improvement

**Severity:** Medium
**Area:** Registration / Onboarding

The registration page needs a clear link for users who already have an account.

Suggested text:

`Already have an account? Log in`

The form also appears to require too many fields, which makes onboarding feel heavy.

**Recommendation:**

* Add a Login link at the bottom of the Registration page.
* Reduce required fields.
* Move optional profile details to Edit Profile after account creation.

---

### Issue 6 — User role model is unclear

**Severity:** High
**Area:** Product logic / Registration flow

Current roles:

* Learner
* Native speaker
* Both

The meaning of `Both` is unclear. It is not obvious whether this means:

* The user is learning Danish and helping others.
* The user is fluent enough to support learners.
* The user is a non-native advanced speaker.
* The user is a native Danish speaker looking for language exchange.

This part of the user flow is not fully designed.

**Recommendation:**
Clarify or redesign the role model.

Possible options:

* I want to learn Danish
* I want to help others learn Danish
* I want to do both

Or:

* Learner
* Danish speaker / Helper
* Language exchange partner

If `Both` remains, add helper text:

`Both — I want to practice Danish and also help other learners when I can.`

If the main goal is matching Danish learners with native speakers, consider removing `Both` until the product logic is clearer.

---

### Issue 7 — Messages need timestamp and delivery/read status

**Severity:** Medium
**Area:** Messages / Chat UX

Messages are displayed correctly, but they do not show whether an outgoing message was sent, delivered, or read. There is also no visible timestamp or date context.

**Recommendation:**
Add message metadata:

* Sent
* Delivered
* Read
* Time
* Date when needed

Suggested date/time behavior:

| Message age    | Display format     |
| -------------- | ------------------ |
| Today          | `14:35`            |
| Yesterday      | `Yesterday, 18:20` |
| Older messages | `12 Jun 2026`      |
| Different year | `12 Jun 2025`      |

Example:

`Hej Lars! Hvad laver du i weekenden?`
`14:35 · Read`

Metadata should be small, visually secondary, and placed consistently below or next to the message bubble.

---

## 8. UI Concerns and Recommendations

### 8.1 Browse page filter order

Recommended filter order:

1. Role
2. Danish level
3. Availability
4. City
5. Search by interests

`Search by interests` should appear below the dropdown filters.

---

### 8.2 Navigation

Consider replacing the top navbar with a left sidebar.

Recommended sidebar:

Main navigation:

* Browse
* Matches
* Messages

Bottom account section:

* Avatar / initials
* User name
* View Profile
* Edit Profile
* Logout

This would make account actions easier to access and reduce top-navbar clutter.

---

### 8.3 Avatars

Current emoji-style avatars should be replaced.

Recommended behavior:

* Show uploaded photo if available.
* If no photo exists, show initials with a colored background.
* Remove emoji icons from avatars.

Examples:

* Maja Nielsen → MA
* Carlos Mendez → CA
* Priya Sharma → PR

Also add photo upload to Edit Profile:

* Upload photo
* Remove photo
* Preview current avatar

---

### 8.4 My Profile page

The page works functionally, but needs better visual design.

Recommended improvements:

* Better spacing
* Stronger hierarchy
* Profile header section
* Clear sections for personal info, topics, goals, and availability
* More consistency with public profile pages

---

### 8.5 Danish level labels

Current labels:

* Beginner
* Intermediate
* Advanced
* Native

Recommended CEFR-style labels:

* A1 — Beginner
* A2 — Elementary
* B1 — Intermediate
* B2 — Upper Intermediate
* C1 — Advanced
* C2 — Proficient
* Native

This better matches the way language levels are usually described in Europe.

---

### 8.6 Matches tabs and color system

The active tab in Matches should use the main brand color. The current interface does not have a strong, consistent purple/brand color.

Recommended color system:

| Purpose           | Example Usage                  |
| ----------------- | ------------------------------ |
| Brand / primary   | Main buttons, active tabs      |
| Success           | Connected state                |
| Warning / neutral | Pending state                  |
| Danger            | Decline action                 |
| Muted             | Disabled buttons, empty states |
| Background        | Cards and page sections        |

---

### 8.7 Translate button

The Translate button in Messages should look like the mockup: a small secondary row under the message, not a primary-looking button.

Suggested layout:

`Message text here`
`Translate`

or:

`Message text here`
`↳ Translate`

---

### 8.8 Empty states

The empty state in the Declined tab includes a green check icon. It feels unnecessary and may imply success.

Recommendation:

* Remove the green check icon.
* Use a neutral empty state.
* Keep the text simple and clear.

---

## 9. Product Flow Concerns

### 9.1 Declined requests

The current distinction between requests I declined and requests others declined is not obvious to users.

Recommendation:

* Keep Matches focused on actionable states: Pending, Connected, Sent.
* Remove Declined as a main tab, or redesign it clearly.
* Use toast notifications for declined requests.
* Allow users to send a new request later if needed.

---

### 9.2 Registration and role selection

The account creation flow needs more product definition.

Open questions:

* Who is the main user: learners, native speakers, or both?
* Are native speakers volunteers, tutors, or exchange partners?
* What exactly does `Both` mean?
* Should users be able to switch roles later?
* Should native speakers also have a Danish level?
* Should learners be able to help other learners?

Before improving the UI, the role model and matching logic should be clarified.

---

## 10. Recommended Priority Fixes

### High Priority

1. Clarify the role model: Learner / Native speaker / Both.
2. Add toast notifications for Accept and Decline actions.
3. Redesign declined request behavior.
4. Make Pending and Connected visually different.

### Medium Priority

5. Add sender label to message previews.
6. Add timestamps and sent/delivered/read statuses to messages.
7. Add Login link to Registration page.
8. Reduce required registration fields.
9. Improve Browse filter layout.
10. Improve My Profile page.
11. Add profile photo upload.

### Low Priority

12. Replace emoji avatars with initials or uploaded photos.
13. Improve Matches active tab styling.
14. Remove green check icon from neutral empty states.
15. Redesign Translate as an inline message action.
16. Consider replacing the top navbar with a left sidebar.

---

## 11. Final Conclusion

Flow 1 is functionally stable. The main app logic works: login, match updates, messaging, localStorage persistence, browsing, filtering, public profiles, sending connection requests, profile view, logout, and protected routes all passed.

No localStorage mismatches were found.

The main improvements should focus on UX clarity and product decisions:

* Add feedback after Accept and Decline.
* Make declined-request behavior easier to understand.
* Improve visual distinction between connection states.
* Add sender labels, timestamps, and read/delivery statuses in Messages.
* Clarify the role model, especially `Both`.
* Improve registration, avatars, profile design, Browse filters, and the overall color system.

Overall result: **Passed with UX and product-flow improvements needed.**
