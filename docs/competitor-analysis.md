# 🕵️ Competitor Analysis & Recommendations: ListedKit

**Target:** Relitix Contract Intelligence Platform  
**Source:** ListedKit (Competitor/Inspiration)

## 1. Core Philosophy: Dynamic vs. Static Workflows

The most critical differentiator in ListedKit's logic is that they do not use static templates. Instead of forcing users to pick a "Buyer Checklist," their AI reads the contract and dynamically builds the task list.

### Observation

- **Logic:** If a contract does not have an Inspection Contingency, ListedKit never creates an "Inspection" task.
- **Impact:** The UI is never cluttered with "N/A" fields. The user feels the system actually "read" the deal.

!!! tip "Screenshot Opportunity"
    **Action:** Go to the Walkthrough Video at 0:45.  
    **Capture:** The moment where the AI auto-populates the "Tasks" list from the PDF.  
    **Caption:** ListedKit automatically generating tasks based on specific contract clauses.

### 🚀 Recommendation for Relitix

We should implement **"Conditional UI Rendering"** for our dashboard cards.

- **Current Plan:** Show "Financing," "Property," and "Terms" cards for every deal.
- **Proposed Upgrade:**
    - If `Payment_Type == 'Cash'`, hide the "Financing" card entirely.
    - If `Property_Type == 'Condo'`, show a "HOA Docs" card that is normally hidden.
    - **Benefit:** Reduces cognitive load for the agent.

---

## 2. Feature Gap: Operational vs. Risk Intelligence

ListedKit focuses on **"When do I do this?"** (Operations), whereas Relitix focuses on **"Is this bad?"** (Risk).

### Observation

ListedKit visualizes the deal as a **Linear Timeline** relative to "Today." They calculate "Business Days" vs. "Calendar Days" automatically to set accurate deadlines.

!!! tip "Screenshot Opportunity"
    **Action:** Go to the Walkthrough Video at 1:15.  
    **Capture:** The "Timeline" view showing the horizontal progress bar of dates.  
    **Caption:** Visualizing the deal as a timeline creates urgency.

### 🚀 Recommendation for Relitix

We can win by combining **Risk with Time**. A risky clause is only a problem if you miss the deadline to fix it.

**Proposed UI Change:** Instead of just a "Risk Score" (Green/Red), add a **"Risk Deadline"** column to our dashboard.

- **Example:** "Missing Radon Addendum (Expires in 2 days)."
- **Action:** Sync these specific "Risk Deadlines" to the agent's Google Calendar.

---

## 3. The "Contextual Action" Pattern

ListedKit moves beyond "Viewing" data to "Acting" on it.

### Observation

They offer a **"Draft Email"** button inside the deal context. It doesn't just open a blank email; it pre-fills the message with the Loan Amount, Lender Name, and Address extracted from the PDF.

!!! tip "Screenshot Opportunity"
    **Action:** Go to the Walkthrough Video at 1:50.  
    **Capture:** The "Draft Email" popup window.  
    **Caption:** AI drafting an email using specific data points from the extracted contract.

### 🚀 Recommendation for Relitix

Transform our **"Recommendations"** panel into an **"Action Center."**

| Risk Identified | Current Relitix Action | Proposed "Contextual" Action |
|----------------|------------------------|------------------------------|
| Missing Addendum | Display Warning text | Button: "Draft Request to Seller" |
| Low Earnest Money | Display Risk Score | Button: "Draft Amendment PDF" |

**Logic:**

```typescript
// If the AI finds a risk, it should return a 'suggestedAction' object
if (risk.type === 'MISSING_DOC') {
  showButton('Draft Email Request', { 
    template: 'request_doc', 
    recipient: 'listing_agent' 
  });
}
```

---

## 4. Summary of Strategic Logic

| Feature | ListedKit Logic | Relitix Implementation |
|---------|----------------|------------------------|
| Intake Cost | Pay-Per-Deal ($9.99) | Consider a "Per Upload" tier for low-volume agents. |
| Data Extraction | Extracts for Tasks (To-Do) | Extract for Compliance (Risk Audit). |
| User Onboarding | "Upload your first contract" (Instant value) | Ensure our "New Transaction" wizard is the first thing they see. |

---

!!! quote "Closing Thought"
    "Agents don't want to just see the risks; they want tools to fix them before they become lawsuits. By adopting ListedKit's 'Action' logic, Relitix becomes an assistant, not just an auditor."
