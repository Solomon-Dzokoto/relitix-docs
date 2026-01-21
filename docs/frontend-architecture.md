#  Frontend Architecture & Design Map

**Project:** Relitix Contracts & Intelligence Platform  
**Updated:** January 2026  
**Purpose:** Map the codebase structure directly to our Figma UI designs.

---

## 1. Project Structure (File Tree)

This structure reflects the domain-driven design we agreed upon.

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
│   └── ui/                     # Base shadcn/ui library (Buttons, Cards)
├── domain/                     # TypeScript Types & Schemas
│   ├── apiTypes.ts
│   ├── credits/
│   │   └── credits.types.ts
│   └── transaction/
│       ├── transaction.types.ts
│       ├── transaction.state.ts
│       └── transaction.schema.ts
├── features/                   # Page Logic & Views
│   ├── credits/
│   │   ├── CreditsPage.tsx
│   │   ├── CreditsBalance.tsx
│   │   └── CheckoutRedirect.tsx
│   ├── dashboard/
│   │   └── DashboardPage.tsx
│   ├── report/
│   │   └── ReportPage.tsx
│   ├── review/                 # The "Correction View"
│   │   ├── ReviewPage.tsx
│   │   ├── ReviewForm.tsx
│   │   └── ReviewField.tsx
│   ├── transactions/
│   │   ├── TransactionsListPage.tsx
│   │   ├── TransactionDetailPage.tsx
│   │   └── TransactionLayout.tsx
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

---

## 2. Feature & Design Mapping

Paste the relevant Figma screenshots below each section to confirm the UI implementation.

---

###  Dashboard Feature

**File:** `src/features/dashboard/DashboardPage.tsx`

**Description:** The landing page showing "Good Morning, Derek," contract renewal timelines, and risk widgets.

- **Key Components:** `StatsGrid`, `RiskChart` (Recharts).
- **Data Source:** `useDashboardQuery` (aggregates transaction states).

!!! note "ACTION"
    Paste Screenshot of "Dashboard / Home" Design Here
    
    ![Dashboard Design Placeholder]

---

### Transaction List

**File:** `src/features/transactions/TransactionsListPage.tsx`

**Description:** The grid/list view of all uploaded contracts with their status badges (Active, Closed, Review Needed).

- **Key Components:** `ModuleCard` (Blue Cards), Search Bar, Filter Toggles.
- **Data Source:** `transactions.api.ts` → `getTransactions()`.

!!! note "ACTION"
    Paste Screenshot of "Transaction Grid View" Here
    
    ![Transaction List Placeholder]

---

###  Upload Wizard

**File:** `src/features/upload/UploadPage.tsx`

**Description:** The multi-step wizard for dragging & dropping PDFs and selecting the contract type.

- **Key Components:** `UploadDropzone.tsx` (File input), `UploadProgress.tsx` (Extraction status bar).
- **Logic:** Handles the "Magic Bytes" validation before upload.

!!! note "ACTION"
    Paste Screenshot of "New Transaction / Upload" Modal Here
    
    ![Upload Screen Placeholder]

---

### Extraction & Review (The Core Feature)

**File:** `src/features/review/ReviewPage.tsx`

**Description:** The split-screen view where users verify AI data against the PDF.

**Key Components:**

- `ReviewForm.tsx`: The form on the right showing extracted fields.
- `ReviewField.tsx`: The individual inputs that toggle `User_Override` when edited.
- `PDFViewer`: The document panel on the left (from `components/artifacts`).

!!! note "ACTION"
    Paste Screenshot of "Extraction / Correction View" Here
    
    ![Extraction View Placeholder]

---

### Credits & Billing

**File:** `src/features/credits/CreditsPage.tsx`

**Description:** The page to purchase new transaction bundles.

- **Key Components:** `CreditsBalance.tsx` (Top right indicator), Pricing Table.
- **Logic:** Redirects to Zoho/Stripe checkout via `CheckoutRedirect.tsx`.

!!! note "ACTION"
    Paste Screenshot of "Buy Credits" Banner/Page Here
    
    ![Credits Design Placeholder]

---

###  Authentication Handoff

**File:** `src/SsoEntryPage.tsx`

**Description:** The headless page that handles the redirect from the Angular Portal.

**Logic:**

1. Reads `?code=xyz` from URL.
2. Calls `auth0Client.exchange(code)`.
3. Stores Token.
4. Redirects to `/dashboard`.

!!! note "ACTION (Optional)"
    Paste Screenshot of Login/Loading Screen Here
    
    ![Login Screen Placeholder]
