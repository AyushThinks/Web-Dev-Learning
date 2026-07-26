# Presentation Topics (Backend Security Case Studies)

## Overview

This session covers five presentation topics built around real-world security incidents and the backend engineering principles behind them. Instead of focusing only on the attacks, it explains how backend developers should design secure systems by assuming that users can see and manipulate anything on the client side. The single lesson running through all five topics is that a backend developer assumes attack, never trusts the client, and verifies everything on the server.

---

# 1. The CBSE Portal, and How Marks Could Be Changed

### Description

In 2026, a 19-year-old named Nisarga Adhikary, who had just finished class 12, examined CBSE's On-Screen Marking (OSM) portal — the site where teachers evaluate scanned answer sheets online — and found a set of basic security holes. He reported them instead of exploiting them. CBSE first denied any breach, then later acknowledged vulnerabilities in the provider's portal and ordered an audit.

### Key Vulnerabilities

- A master password was written into the source code, visible in the browser.
- The OTP check could be bypassed.
- Password reset did not verify the old password — any user ID plus any gibberish reset it.
- An examiner's identity could be edited through browser storage, allowing impersonation of a teacher.
- Internal dashboards were open with no login.

### Backend Lessons

- Never put secrets in code that reaches the browser.
- A security check the client can skip is not a security check.
- The server must verify every step — never trust that the client did the checking.
- Never trust identity data from the browser; the server decides who you are.
- Every private route needs an access-control check, on the server, every time.

**One-sentence answer:** The marks were changeable because the system trusted the browser. Passwords, identities, and checks all lived where the user could see and edit them — as he put it, *"These aren't advanced defences. They're the basics."*

---

# 2. How 10,000+ Records Get Created in Minutes

### Description

10,000 records were not created by hand — that would take days. They were created by a **script**, a small program that repeats a "create record" request thousands of times in a loop, as fast as the server allows. This is not an advanced attack; it's a beginner-level loop pointed at an endpoint that was not protected.

### Key Issues

- No rate limit — the server accepted requests as fast as they came.
- No bot check on the form.
- The endpoint was open, with no login needed to create records.
- No validation catching obviously fake or duplicate data.

### Backend Lessons

- Implement rate limiting — cap how many requests one user or IP can make per minute.
- Add a CAPTCHA or similar check on public create-endpoints.
- Require authentication, so anonymous scripts cannot post at all.
- Add server-side validation and duplicate checks that reject junk.

**One-sentence answer:** Assume any open endpoint will be hit by a machine, not a polite human, and put limits in before it happens, not after.

---

# 3. What Information Is Kept in the UI

### Description

The UI is the HTML, CSS, and JavaScript that runs in the browser. The single most important fact about it: everything in it is visible to everyone. Anyone can right-click, choose "view source," or open developer tools, and read every line of it — nothing there is hidden.

### What Lives in the UI

- Page structure and text (HTML) — readable by anyone, always.
- Styling (CSS) — readable, harmless, fine to be public.
- Logic that runs in the browser (JavaScript) — fully readable; never hide a secret in it.
- Values stored in the browser (localStorage, cookies) — readable **and editable** by the user.

### Backend Lessons

- Never put a secret, a password, or a trust decision in the UI.
- Treat the browser as the user's territory, not yours.
- This is exactly the CBSE mistake from Topic 1: a master password sat in browser code, and browser storage decided identity.

**One-sentence answer:** The UI holds only what you are willing to show the whole world, because the whole world can read it.

---

# 4. Who Are Ethical Hackers

### Description

An ethical hacker uses the same skills as an attacker, but with permission, to find security holes so they can be fixed before a real attacker uses them. Same knowledge, opposite intent — done legally and reported responsibly. Nisarga from Topic 1 is the live example: he reported his findings to the authorities and CERT-In instead of exploiting them for money.

### Ethical Hacker vs. Malicious Hacker

| Ethical Hacker | Malicious Hacker |
|---|---|
| Has permission, or reports responsibly | No permission, hides the act |
| Finds a hole and tells the owner to fix it | Uses the hole for gain or damage |
| Makes systems safer | Makes victims |
| Paid by companies, bug bounties, security jobs | Faces jail |

### Backend Lessons

- The intent and legality are what separate an ethical hacker from a criminal, not the skills.
- Companies run bug bounty programs and hire security teams because they'd rather a friendly researcher find the hole first.

**One-sentence answer:** Ethical hackers are the people who find the holes before the criminals do, with permission and in the open, and it is a real, paid career.

---

# 5. The Movie "Pritam Pedro" — and What a Clicked Link Actually Reveals

### Description

The film *Pritam Pedro* (Rajkumar Hirani, 2026, JioHotstar) pairs two opposite cops — Pedro, who trusts instinct and traditional methods, and Pritam, a tech-savvy officer who trusts the system and code — as they solve cybercrimes starting from an abandoned ATM on a beach and building to a kidnapping case. The practical task paired with it is the location question: what does clicking a link actually reveal about someone?

### Things to Watch For

- The two mindsets: instinct vs. verifying the system — a backend developer needs both.
- Where trust breaks: a cybercrime happens because someone trusted something they shouldn't have.
- What is exposed vs. hidden: what an attacker can see, reach, or fake.
- How the attacker is caught: following data and spotting what doesn't add up.

### What People Believe vs. What Actually Happens

| What People Believe | What Actually Happens |
|---|---|
| A clicked link reveals exact location | It reveals only the public IP — city or ISP level at best |
| You can read their local address (192.168.x.x) | You cannot — it stays inside their router (NAT) and never reaches any server |

### How Exact Location Really Leaks

- The page asks "Allow Location?" and the person taps Allow — requires the person's consent.
- The person uploaded a photo or posted with location on — the person handed the data over themselves.
- Malware or a hacked account/phone — the device is already compromised.
- Police mapping an IP to a real address — a legal request to the ISP, not something a normal person can do.

### Backend Lessons

- A link on its own leaks only the public IP, never a street address and never the local IP.
- The server only sees what the request carries; everything precise stays behind a wall the user controls.

**One-sentence answer:** Exact location requires the person's consent, their own leaked data, or a compromised device — not the link itself.

---

# 6. The Biggest Lesson from All Five Topics

### Description

Although each topic is different, they all share the same root cause and fix: never trust the client, always verify on the server.

### Common Security Principles

- Marks were changed because the browser was trusted.
- Records were flooded because an endpoint had no limits.
- The UI exposes everything because it runs on the user's machine.
- Ethical hackers are the people who find these gaps first.
- The film makes the mindset stick: verify everything, trust nothing from the client.

---

## Learning Takeaways

- Backend security starts with **never trusting the client**.
- Sensitive information (passwords, master keys) must never be exposed in frontend code.
- Authentication and authorization should always be handled on the server.
- Every API request must be validated before processing.
- Rate limiting and bot checks help prevent automated attacks like mass record creation.
- Browser storage (localStorage, cookies) is readable **and editable** by the user — never store trust decisions there.
- Ethical hacking differs from malicious hacking in intent and legality, not skill.
- A clicked link reveals only a public IP (city/ISP level), never an exact location, due to NAT.
- Small security mistakes — a hardcoded password, a missing login check — can lead to major vulnerabilities.

---
