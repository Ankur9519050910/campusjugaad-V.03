# Campus Jugaad: The Hyper-Local Campus Marketplace
 **"Summon a Hero. Delegate your chaos."**
 
---

## ◆ Problem Statement
University campuses are isolated economies with inefficient "favor" systems. Students struggle to find help for small tasks like:
* A professor needs medicine but is stuck in back-to-back labs.
* A student needs a cycle for 1 hour.
* Printing, food delivery, laundry pickup, and more.

Currently, students rely on spammy WhatsApp groups or favors from friends in the middle of a busy schedule. There is no organized system to request help, no way to find someone available immediately, and no trusted way to pay for such small tasks. While others have free time but no way to monetize it. Existing platforms cannot enter hostels or classrooms.

---

## ◆ The Solution
**Campus Jugaad** is a tech-driven marketplace exclusively for university campuses where students can outsource tasks to verified peers ("Heroes") safely. It connects those who have money but no time, and those who have time and want to earn money.

---

## Key Features (Phase 1: Core)

### The Gamified Experience
* **Gamified Quest System**: Tasks are "Quests" with bounties, deadlines, and urgency levels (Low/Medium/Urgent).
* **Hero Feed**: A real-time marketplace where students filter quests by location and reward to become the campus "Hero".
* **Legendary Dashboard**: Track your journey with total earnings, completion stats, and a level-based **XP System**.
* **Trust Score**: A 5-star rating system builds a Hero's reputation, ensuring quality work.

### Security & Architecture
* **Secure OTP Handshake**: We don't rely on trust. When a Hero arrives, the Task Master gives them a secret 4-digit Code. The Hero must enter this code to unlock the payment.
* **Zero-Fraud Escrow**: Payments are deducted before the task goes live, guaranteeing the Hero gets paid. Funds are held in a secure Escrow State within the database and released only when the task provider ("Task Master") shares the secret OTP.
* **Auto-Refunds**: If a task is not accepted by anyone or a Hero accepts it but vanishes (doesn't complete it by the deadline), the smart contract automatically refunds the money to the Task Master. No admin needed.
* **In-App Hero Chat**: A private communication channel for Task Masters and Heroes to negotiate details without sharing personal phone numbers.

### Design & Accessibility
* **Mobile-First Design**: Fully responsive layout optimized for on-the-go campus use between classes.
* **Glassmorphic UI**: A modern, sleek interface featuring fluid animations and full Dark/Light mode support.

---
**Demo Video**:
* Round 1: https://www.youtube.com/watch?v=1uiUTcwBCmg
* Round 2:
https://www.youtube.com/watch?v=hWEl94z8alw
## Feasibility & Impact

| Metric / Area | Stat / Status | Details |
| :--- | :--- | :--- |
| **Market Size** | ~5,000 Students | Per average technical campus (Dense Ecosystem). |
| **Economic Volume** | ₹50,000+ / Month | Projected transaction volume in the pilot phase. |
| **Time Saving** | ~3 Hours / Week | Average time saved per student by outsourcing chores. |
| **Operations** | Binary Resolution | 2-Factor OTP confirms success; Jury System handles edge cases. |
---

## Tech Stack

| Component | Technology | Highlights |
| :--- | :--- | :--- |
| **Frontend** | React.js (Vite) | Component-based, Fast HMR |
| **Styling** | Tailwind CSS | Glassmorphism, Mobile-First Design |
| **Backend** | Node.js + Express | REST API, Stateless Architecture |
| **Database** | MongoDB Atlas | NoSQL, Atomic Transactions |
| **Auth** | OTP | Secure Account Management |
| **Services** | Nodemailer | Transactional Emails & Dispute Bridge |


## Phase 2 Features (Now Live!)
*In Round 1, we built the economy. In Round 2, we built the negotiation and justice systems that make it sustainable.*

### 1. Dynamic Bidding Wars
Fixed prices don't always work. Maybe it's raining, or the printer line is long. Heroes can now "Bid" different amounts on a quest. Task Masters can accept the best offer, with automatic wallet adjustments.

**⚙️ Technical Implementation:**
* **Atomic Pushes**: We use MongoDB's `$push` operator to add bids to a quest array without locking the entire document. This allows multiple heroes to bid simultaneously without race conditions.
* **Smart Escrow Top-Up**: When a Task Master accepts a *higher* bid (e.g., ₹70 for a ₹50 quest), the backend calculates the difference (`bid - originalReward`). It atomically deducts the *extra* ₹20 from their wallet. If the wallet balance is insufficient, the entire transaction rolls back using a `try-catch` block, ensuring no money is lost.

### 2. Community Justice System
A full **Dispute Resolution Center** is now active. Users can raise disputes, locking the funds, while Admins review evidence via a dashboard to issue refunds or splits.

**Technical Implementation:**
* **State Locking**: When a user clicks "Raise Dispute", the quest status atomically flips from `active` -> `disputed`. This **freezes** the funds in Escrow. Neither the Hero nor the Task Master can touch the money.
* **Privacy-First Email Bridge**: We integrated **Nodemailer** to act as a bridge. When a dispute is raised, the system emails both parties.
* **Crucial Detail**: The system uses `Reply-To: admin@campusjugaad` and `BCC`. This means the Hero and Task Master can email their evidence *without revealing their personal email addresses to each other*.

**Admin Email for Testing**: admin@cj.com  
**Password**: admin123

**Resolution Logic (The Jury):**
Admins have a special dashboard to trigger one of three outcomes:
1. **Refund Poster**: Funds return to the Task Master (e.g., Hero didn't show up).
2. **Pay Hero**: Funds release to the Hero (e.g., Task Master was unresponsive).
3. **50/50 Split**: A compromise where the loss is shared.

### 3. Instant Revocation (Atomic Cancellation)
Users can now cancel a quest if they find a friend to do it instead.

**Technical Implementation:**
* **Conditional Deletion**: The API endpoint `DELETE /api/quests/:id` performs a strict check:
  ```typescript
  findOneAndDelete({ _id: id, status: 'open' })
  ```
## Limitations & Constraints

### 1. Financial Compliance (Regulatory)
* **No Real Banking API**: Currently, the "Wallet" uses a closed-loop virtual currency system. We have not integrated a real Payment Gateway (like Razorpay/Stripe) due to **KYC (Know Your Customer)** and legal requirements for handling real money between peers.
* **Cash-Out Logic**: Students cannot currently withdraw their virtual earnings to a bank account; this feature requires banking partnerships planned for **Phase 3** (the final production version).


---
*For detailed architecture and diagrams, see [TECHNICAL_ARCHITECTURE.md](./TECHNICAL_ARCHITECTURE.md)*
