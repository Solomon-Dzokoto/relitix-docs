# Frontend Architecture & Design Map

Project: Relitix Contracts & Intelligence Platform

Updated: January 2026

Purpose: Map every Figma screen to a specific React component.
1. File Structure (Expanded)

This structure aligns with the domain-driven design and covers all feature modules visible in the Figma file.
Plaintext

```plaintext
src/
├── api/                        # HTTP Layer
│   ├── httpClient.ts           # Axios instance with Auth0 interceptors
│   ├── apiErrors.ts            # Standard error handling
│   ├── transactions.api.ts     # API calls for Transaction domain
│   └── credits.api.ts          # API calls for Credits/Billing
├── components/                 # Shared UI Components
│   ├── artifacts/              # PDF Viewer, Document Icons
│   ├── feedback/               # Toasts, Loaders, Error Messages
│   ├── layout/                 # Shell components
│   │   ├── Header.tsx
│   │   └── Sidebar.tsx
│   ├── timeline/               # Progress visualization
│   │   ├── TransactionTimeline.tsx
│   │   └── TimelineStep.tsx
│   └── ui/                     # Base shadcn/ui / Magic Ui library (Buttons, Cards)
├── domain/                     # TypeScript Types & Schemas
│   ├── apiTypes.ts
│   ├── credits/
│   │   └── credits.types.ts
│   └── transaction/
│       ├── transaction.types.ts
│       ├── transaction.state.ts
│       └── transaction.schema.ts
├── features/                   # Page Logic & Views
│   ├── dashboard/                  # "Contract Home Page"
│   │   ├── components/
│   │   │   ├── StatsRibbon.tsx     # The top KPI cards
│   │   │   ├── RenewalChart.tsx    # The main bar chart
│   │   │   └── RiskPieChart.tsx    # The pie charts at bottom
│   │   └── DashboardPage.tsx
│   ├── transactions/               # "Transactions 1-4"
│   │   ├── components/
│   │   │   ├── TransactionCard.tsx # For Grid View
│   │   │   ├── TransactionRow.tsx  # For Table View
│   │   │   └── FilterPanel.tsx     # The "Status/Type" filters
│   │   └── TransactionsListPage.tsx
│   ├── transaction-detail/         # "Transactions Detail - New 1-4"
│   │   ├── components/
│   │   │   ├── DetailHeader.tsx    # Title + "Generate Summary" button
│   │   │   ├── tabs/
│   │   │   │   ├── DetailsTab.tsx  # Address, County, Parcel
│   │   │   │   ├── PartiesTab.tsx  # Buyer, Seller, Agents
│   │   │   │   ├── TermsTab.tsx    # Contingencies & Clauses
│   │   │   │   └── SummaryTab.tsx  # AI Generated Summary
│   │   └── TransactionDetailPage.tsx
│   ├── ai-agent/                   # "AI Agent (New) 1-8"
│   │   ├── components/
│   │   │   ├── ChatBubble.tsx      # User vs AI message styles
│   │   │   ├── PromptInput.tsx     # The bottom text bar
│   │   │   └── AgentSidebar.tsx    # History of chats
│   │   └── AgentPage.tsx
│   ├── risk-assessment/            # "Risk Assessment 1-3"
│   │   ├── components/
│   │   │   ├── RiskScorecard.tsx   # The Green/Red indicators
│   │   │   └── RiskReport.tsx      # The long text document view
│   │   └── RiskAssessmentPage.tsx
│   ├── extraction/                 # "Extraction 1"
│   │   ├── components/
│   │   │   ├── ModuleGrid.tsx      # The 6 blue cards
│   │   │   └── ExtractionCard.tsx  # Individual blue card
│   │   └── ExtractionMenuPage.tsx
│   ├── credits/
│   │   ├── CreditsPage.tsx
│   │   ├── CreditsBalance.tsx
│   │   └── CheckoutRedirect.tsx
│   ├── review/                 # The "Correction View"
│   │   ├── ReviewPage.tsx
│   │   ├── ReviewForm.tsx
│   │   └── ReviewField.tsx
│   └── upload/                 # The "New Transaction" Wizard
│       ├── UploadPage.tsx
│       ├── UploadDropzone.tsx
│       └── UploadProgress.tsx
├── hooks/                      # Global Hooks
│   ├── useAuth.ts              # Auth0 wrapper
│   └── useFlag.ts              # Feature flagging
├── state/                      # Global State (React Query)
│   ├── queryClient.ts
│   └── queryKeys.ts
├── App.tsx                     # Main Component
├── SsoEntryPage.tsx            # Auth0 Handoff Logic
├── router.tsx                  # Application Routes
└── main.tsx                    # Entry Point
```

2. Design to Component Mapping


### Dashboard (Contract Home Page)

Figma Screen: Contract Home Page

File: src/features/dashboard/DashboardPage.tsx

Notes: Needs to handle the "Good Morning" header and the Grid of 3 charts.

<img width="549" height="306" alt="Screen Shot 2026-01-21 at 2 31 11 PM" src="https://github.com/user-attachments/assets/6653c19b-6a66-482f-9004-32b88df4b9e5" />
### Transactions List (Grid & Table)

Figma Screens: Transactions 1 - New, Transactions 2 - New

File: src/features/transactions/TransactionsListPage.tsx

Logic: This page needs a viewMode state ('grid' | 'table') to switch between the card view (Screen 1) and list view (Screen 2).

<img width="338" height="194" alt="Screen Shot 2026-01-21 at 2 33 51 PM" src="https://github.com/user-attachments/assets/0650d059-4c89-4eb1-a560-7eb4a02c1765" /> <img width="338" height="194" alt="Screen Shot 2026-01-21 at 2 37 55 PM" src="https://github.com/user-attachments/assets/d08a5e5e-c4df-4dd7-a47d-5ad9f662f21f" />
### Transaction Details (The Tabs)

Figma Screens: Transactions Detail - New 1 through New 4

File: src/features/transaction-detail/TransactionDetailPage.tsx

Logic: This is a complex page. We need to map the Figma tabs to these components:

    Screen New 1: DetailsTab.tsx (Property Info)
<img width="385" height="220" alt="Screen Shot 2026-01-21 at 2 39 51 PM" src="https://github.com/user-attachments/assets/677c71f2-5ce9-452c-8d66-b6755dca912e" />

    Screen New 2: PartiesTab.tsx (Buyer/Seller)

    Screen New 3: TermsTab.tsx (Clauses)

    Screen New 4: SummaryTab.tsx



### AI Agent (Chat)

Figma Screens: AI Agent (New) - 1 through 8

File: src/features/ai-agent/AgentPage.tsx

Notes: Since there are 8 screens, this is a full "Chat Application." It includes typing states, empty states, and results.
<img width="470" height="266" alt="Screen Shot 2026-01-21 at 3 54 36 PM" src="https://github.com/user-attachments/assets/f20dd593-9099-4c0d-b4e4-eff9be995d58" />
<img width="470" height="266" alt="Screen Shot 2026-01-21 at 3 55 05 PM" src="https://github.com/user-attachments/assets/6f25b89a-461f-414d-a445-d5f44e55277f" />
<img width="470" height="266" alt="Screen Shot 2026-01-21 at 3 55 23 PM" src="https://github.com/user-attachments/assets/ff8076c8-6814-418f-8668-cdaae87a5a59" />
<img width="470" height="266" alt="Screen Shot 2026-01-21 at 3 55 42 PM" src="https://github.com/user-attachments/assets/765f0951-4dad-4522-a0d4-7bc3f064edc9" />
<img width="470" height="266" alt="Screen Shot 2026-01-21 at 3 56 09 PM" src="https://github.com/user-attachments/assets/6958b6c9-8dd9-47fb-9b3e-1001c2bf264d" />
<img width="470" height="266" alt="Screen Shot 2026-01-21 at 3 56 26 PM" src="https://github.com/user-attachments/assets/a730c717-8743-4a0b-9186-2f0e2929b5bb" />
<img width="470" height="266" alt="Screen Shot 2026-01-21 at 3 57 09 PM" src="https://github.com/user-attachments/assets/6efab46c-fbd5-4603-9660-7e0742a81a2e" />
<img width="470" height="266" alt="Screen Shot 2026-01-21 at 3 57 26 PM" src="https://github.com/user-attachments/assets/a9863230-5870-4e2f-896b-47cfdceafb1a" />


### Risk Assessment Reports

Figma Screens: Risk Assessment 1 through 3

File: src/features/risk-assessment/RiskAssessmentPage.tsx

Notes: These is the document readers (long text with sidebars).
<img width="281" height="158" alt="Screen Shot 2026-01-21 at 3 59 30 PM" src="https://github.com/user-attachments/assets/b3c2bbba-cea5-4cd6-8d0f-3f4149f14c1f" />


### Extraction Menu

Figma Screen: Extraction 1

File: src/features/extraction/ExtractionMenuPage.tsx

Notes: The menu with the  (Contacts, Property, Terms, etc.).
<img width="376" height="218" alt="Screen Shot 2026-01-21 at 4 01 25 PM" src="https://github.com/user-attachments/assets/f7c8cf18-2bb0-40fc-9879-8ab66edf0552" />



### Upload Wizard

File: src/features/upload/UploadPage.tsx

Description: The multi-step wizard for dragging & dropping PDFs and selecting the contract type.

    Key Components: UploadDropzone.tsx (File input), UploadProgress.tsx (Extraction status bar).

    Logic: Handles the "Magic Bytes" validation before upload.



### Extraction & Review (The Core Feature)

File: src/features/review/ReviewPage.tsx

Description: The split-screen view where users verify AI data against the PDF.

Key Components:

    ReviewForm.tsx: The form on the right showing extracted fields.

    ReviewField.tsx: The individual inputs that toggle User_Override when edited.

    PDFViewer: The document panel on the left (from components/artifacts).



### Credits & Billing

File: src/features/credits/CreditsPage.tsx

Description: The page to purchase new transaction bundles.

    Key Components: CreditsBalance.tsx (Top right indicator), Pricing Table.

    Logic: Redirects to Zoho/Stripe checkout via CheckoutRedirect.tsx.



### Authentication Handoff

File: src/SsoEntryPage.tsx

Description: The headless page that handles the redirect from the Angular Portal.

Logic:
****
    Reads ?code=xyz from URL.

    Calls auth0Client.exchange(code).

    Stores Token.

    Redirects to /dashboard.



1. Figma Design Reference

Figma File: Relitix AI - Contract Management Module

Use this link to access all design screens and ensure accurate implementation of UI components.
