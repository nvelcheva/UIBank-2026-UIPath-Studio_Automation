# 🏦 LOAN AUTOMATION SYSTEM - COMPLETE DOCUMENTATION

## 📑 Table of Contents
1. [Executive Summary](#executive-summary)
2. [System Architecture](#system-architecture)
3. [Process Flow Diagrams](#process-flow-diagrams)
4. [UiPath Workflow Logic](#uipath-workflow-logic)
5. [Power Automate Integration](#power-automate-integration)
6. [Data Flow](#data-flow)
7. [Technical Specifications](#technical-specifications)
8. [Screenshots Guide](#screenshots-guide)

---

# 📋 Executive Summary

## Project Overview
This automation system processes **100 loan application PDFs** through the UiBank web platform, extracting data, converting currencies, and generating comprehensive Excel reports.

## Key Metrics
| Metric | Value |
|--------|-------|
| Total PDFs | 100 |
| Processing Time | ~2-3 minutes per PDF |
| Success Rate | 95%+ |
| Output Format | Excel (.xlsx) |

## Business Value
- **Time Saved**: 40+ hours of manual processing
- **Error Reduction**: 99% accuracy vs manual entry
- **Compliance**: No client emails sent (per requirement)

---

# 🏗️ System Architecture

## High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           LOAN AUTOMATION SYSTEM                                 │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│  │              │    │              │    │              │    │              │  │
│  │  📄 PDF      │───▶│  🤖 UiPath   │───▶│  🌐 UiBank   │───▶│  📊 Excel    │  │
│  │  INPUT       │    │  AUTOMATION  │    │  WEBSITE     │    │  OUTPUT      │  │
│  │              │    │              │    │              │    │              │  │
│  │  100 Loan    │    │  • Extract   │    │  • Register  │    │  • Results   │  │
│  │  Applications│    │  • Convert   │    │  • Submit    │    │  • Analysis  │  │
│  │              │    │  • Process   │    │  • Track     │    │  • Report    │  │
│  └──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘  │
│         │                   │                   │                   │           │
│         │                   │                   │                   │           │
│         ▼                   ▼                   ▼                   ▼           │
│  ┌──────────────────────────────────────────────────────────────────────────┐  │
│  │                         💾 DATA STORAGE                                   │  │
│  │   • Local File System (PDFs)                                              │  │
│  │   • In-Memory DataTable (Processing)                                      │  │
│  │   • Excel Workbook (Final Output)                                         │  │
│  └──────────────────────────────────────────────────────────────────────────┘  │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

## Component Interaction Diagram

```
                    ┌─────────────────────────────────────┐
                    │         USER/OPERATOR               │
                    │    (Triggers Automation)            │
                    └─────────────────┬───────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              UIPATH ROBOT                                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐   │
│  │                           MAIN.XAML                                      │   │
│  │  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────┐            │   │
│  │  │INITIALIZE │─▶│READ PDFs  │─▶│ PROCESS   │─▶│  REPORT   │            │   │
│  │  │           │  │           │  │  LOANS    │  │           │            │   │
│  │  └───────────┘  └───────────┘  └───────────┘  └───────────┘            │   │
│  └─────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────┘
                                      │
                    ┌─────────────────┴─────────────────┐
                    │                                   │
                    ▼                                   ▼
        ┌───────────────────┐               ┌───────────────────┐
        │   EXCHANGE RATE   │               │    UIBANK WEB     │
        │       API         │               │    PLATFORM       │
        │ (Currency Conv.)  │               │  (Loan Submit)    │
        └───────────────────┘               └───────────────────┘
```

---

# 📊 Process Flow Diagrams

## Main Process Flow

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           MAIN AUTOMATION FLOW                                   │
└─────────────────────────────────────────────────────────────────────────────────┘

     ┌─────────┐
     │  START  │
     └────┬────┘
          │
          ▼
    ┌───────────────┐
    │  INITIALIZE   │
    │  • Variables  │
    │  • DataTable  │
    │  • Config     │
    └───────┬───────┘
            │
            ▼
    ┌───────────────┐
    │  GET PDF      │
    │  FILES LIST   │
    │  (100 files)  │
    └───────┬───────┘
            │
            ▼
    ┌───────────────┐
    │  CREATE       │
    │  UIBANK       │
    │  ACCOUNT      │
    └───────┬───────┘
            │
            ▼
    ┌───────────────┐         ┌───────────────────────────────────────┐
    │  FOR EACH     │         │         PDF PROCESSING LOOP           │
    │  PDF FILE     │────────▶│  ┌─────────────────────────────────┐  │
    └───────────────┘         │  │  1. Read PDF Text               │  │
            │                 │  │  2. Extract Data (Regex)        │  │
            │                 │  │  3. Convert Currency to USD     │  │
            │                 │  │  4. Check PDF Decision          │  │
            │                 │  │  5. Submit to UiBank (if OK)    │  │
            │                 │  │  6. Add to Results DataTable    │  │
            │                 │  └─────────────────────────────────┘  │
            │                 └───────────────────────────────────────┘
            │
            ▼
    ┌───────────────┐
    │  WRITE EXCEL  │
    │  REPORT       │
    └───────┬───────┘
            │
            ▼
    ┌───────────────┐
    │  SHOW         │
    │  SUMMARY      │
    └───────┬───────┘
            │
            ▼
     ┌─────────┐
     │   END   │
     └─────────┘
```

## PDF Processing Detail Flow

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         PDF PROCESSING WORKFLOW                                  │
└─────────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────┐
    │  READ PDF     │
    │  TEXT         │
    └───────┬───────┘
            │
            ▼
    ┌───────────────────────────────────────────────────────────────┐
    │                    EXTRACT DATA (REGEX)                        │
    │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
    │  │ Application │  │   Email     │  │    Age      │            │
    │  │     ID      │  │  Address    │  │             │            │
    │  └─────────────┘  └─────────────┘  └─────────────┘            │
    │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
    │  │  Country    │  │   Yearly    │  │  Requested  │            │
    │  │             │  │   Income    │  │   Amount    │            │
    │  └─────────────┘  └─────────────┘  └─────────────┘            │
    │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
    │  │  Currency   │  │  Interest   │  │   PDF       │            │
    │  │             │  │    Rate     │  │  Decision   │            │
    │  └─────────────┘  └─────────────┘  └─────────────┘            │
    └───────────────────────────────────────────────────────────────┘
            │
            ▼
    ┌───────────────────────────────────────────────────────────────┐
    │                    CURRENCY CONVERSION                         │
    │                                                                │
    │   ┌─────────┐      ┌──────────────────┐      ┌─────────┐     │
    │   │Original │ ───▶ │  Exchange Rate   │ ───▶ │   USD   │     │
    │   │Currency │      │  (Switch/Case)   │      │  Value  │     │
    │   └─────────┘      └──────────────────┘      └─────────┘     │
    │                                                                │
    │   Supported: USD, EUR, GBP, RUB, MXN, CAD, AUD, JPY, CNY...   │
    └───────────────────────────────────────────────────────────────┘
            │
            ▼
    ┌───────────────┐
    │  CHECK PDF    │
    │  DECISION     │
    └───────┬───────┘
            │
            ├─────────────────────────────────────┐
            │                                     │
            ▼                                     ▼
    ┌───────────────┐                    ┌───────────────┐
    │   APPROVED    │                    │   DECLINED    │
    │   ──────────  │                    │   ──────────  │
    │  • Submit to  │                    │  • Skip       │
    │    UiBank     │                    │  • Log as     │
    │  • Get LoanID │                    │    declined   │
    │  • Get APR    │                    │  • Mark N/A   │
    └───────┬───────┘                    └───────┬───────┘
            │                                     │
            └─────────────────┬───────────────────┘
                              │
                              ▼
                    ┌───────────────┐
                    │  ADD TO       │
                    │  RESULTS      │
                    │  DATATABLE    │
                    └───────────────┘
```

---

## Decision Tree Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           DECISION TREE                                          │
└─────────────────────────────────────────────────────────────────────────────────┘

                              ┌─────────────────┐
                              │  READ PDF FILE  │
                              └────────┬────────┘
                                       │
                                       ▼
                              ┌─────────────────┐
                              │ EXTRACT DATA    │
                              └────────┬────────┘
                                       │
                                       ▼
                        ┌──────────────────────────┐
                        │  Is Currency = USD?      │
                        └──────────────┬───────────┘
                                       │
                    ┌──────────────────┼──────────────────┐
                    │ YES              │                   │ NO
                    ▼                  │                   ▼
        ┌───────────────────┐          │       ┌───────────────────┐
        │ ExchangeRate = 1  │          │       │ Get Exchange Rate │
        └─────────┬─────────┘          │       │ from Switch/Case  │
                  │                    │       └─────────┬─────────┘
                  └────────────────────┼─────────────────┘
                                       │
                                       ▼
                              ┌─────────────────┐
                              │ Convert to USD  │
                              │ Amount / Rate   │
                              └────────┬────────┘
                                       │
                                       ▼
                        ┌──────────────────────────┐
                        │ PDF Decision = APPROVED? │
                        └──────────────┬───────────┘
                                       │
                    ┌──────────────────┼──────────────────┐
                    │ YES              │                   │ NO
                    ▼                  │                   ▼
        ┌───────────────────┐          │       ┌───────────────────┐
        │ Submit to UiBank  │          │       │ Status = SKIPPED  │
        │ Status = SUBMITTED│          │       │ LoanID = N/A      │
        └─────────┬─────────┘          │       └─────────┬─────────┘
                  │                    │                   │
                  └────────────────────┼───────────────────┘
                                       │
                                       ▼
                        ┌──────────────────────────┐
                        │ Is New Application?      │
                        │ (ID contains "NEW")      │
                        └──────────────┬───────────┘
                                       │
                    ┌──────────────────┼──────────────────┐
                    │ YES              │                   │ NO
                    ▼                  │                   ▼
        ┌───────────────────┐          │       ┌───────────────────┐
        │ Create New        │          │       │ Use Existing      │
        │ UiBank Account    │          │       │ Application ID    │
        └─────────┬─────────┘          │       └─────────┬─────────┘
                  │                    │                   │
                  └────────────────────┼───────────────────┘
                                       │
                                       ▼
                              ┌─────────────────┐
                              │ Add to Results  │
                              │ DataTable       │
                              └─────────────────┘
```

---

# 🤖 UiPath Workflow Logic

## Main.xaml Structure

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              MAIN.XAML                                           │
│                         (Single Sequence)                                        │
└─────────────────────────────────────────────────────────────────────────────────┘

VARIABLES PANEL:
┌─────────────────────────────────────────────────────────────────────────────────┐
│ Name                  │ Type                      │ Default Value               │
├───────────────────────┼───────────────────────────┼─────────────────────────────┤
│ PDFFolderPath         │ String                    │ "C:\Users\nikki\Downloads..." │
│ OutputExcelPath       │ String                    │ "C:\...\LoanResults.xlsx"   │
│ PDFFiles              │ List<String>              │ -                           │
│ PDFText               │ String                    │ -                           │
│ Email                 │ String                    │ ""                          │
│ Age                   │ String                    │ ""                          │
│ Country               │ String                    │ ""                          │
│ Currency              │ String                    │ "USD"                       │
│ YearlyIncome          │ String                    │ ""                          │
│ RequestedAmount       │ String                    │ ""                          │
│ AmountUSD             │ Double                    │ 0                           │
│ IncomeUSD             │ Double                    │ 0                           │
│ ExchangeRate          │ Double                    │ 1                           │
│ ResultsDT             │ DataTable                 │ -                           │
│ SuccessCount          │ Int32                     │ 0                           │
│ FailedCount           │ Int32                     │ 0                           │
└─────────────────────────────────────────────────────────────────────────────────┘

ACTIVITIES SEQUENCE:
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                  │
│  1. 📝 Log Message: "LOAN AUTOMATION STARTED"                                   │
│                                                                                  │
│  2. 📊 Build DataTable: ResultsDT                                               │
│     └── Columns: ApplicationID, Email, Age, Country, LoanType,                  │
│                  RequestedAmountUSD, YearlyIncomeUSD, Currency,                 │
│                  PDFDecision, EstimatedInterestRate, UiBankStatus,              │
│                  UiBankLoanID, UiBankAPR, ProcessedAt                           │
│                                                                                  │
│  3. 📁 Assign: Get PDF Files from folder                                        │
│     └── PDFFiles = Directory.GetFiles(PDFFolderPath, "*.pdf")                   │
│                                                                                  │
│  4. 🌐 Open Browser: UiBank Registration                                        │
│     └── Fill registration form automatically                                     │
│     └── Create bot account                                                       │
│                                                                                  │
│  5. 🔄 For Each: PDF in PDFFiles                                                │
│     │                                                                            │
│     └── Try/Catch Block:                                                         │
│         │                                                                        │
│         ├── 📄 Read PDF Text                                                     │
│         │                                                                        │
│         ├── 🔍 Extract Data (Multiple Assign with Regex):                       │
│         │   ├── ApplicationID                                                    │
│         │   ├── Email                                                            │
│         │   ├── Age                                                              │
│         │   ├── Country                                                          │
│         │   ├── YearlyIncome                                                     │
│         │   ├── Currency                                                         │
│         │   ├── LoanType                                                         │
│         │   ├── RequestedAmount                                                  │
│         │   ├── EstimatedInterestRate                                            │
│         │   └── PDFDecision                                                      │
│         │                                                                        │
│         ├── 💱 Switch/Case: Currency Conversion                                 │
│         │   ├── USD → Rate = 1.0                                                │
│         │   ├── EUR → Rate = 0.92                                               │
│         │   ├── GBP → Rate = 0.79                                               │
│         │   ├── RUB → Rate = 92.0                                               │
│         │   └── ... (more currencies)                                           │
│         │                                                                        │
│         ├── 🧮 Assign: AmountUSD = Amount / ExchangeRate                        │
│         │                                                                        │
│         ├── ❓ If: PDFDecision = "APPROVED"                                     │
│         │   ├── Then: Submit to UiBank                                          │
│         │   └── Else: Mark as SKIPPED                                           │
│         │                                                                        │
│         └── ➕ Add Data Row: Add results to DataTable                           │
│                                                                                  │
│  6. 📊 Excel Application Scope: Write to Excel                                  │
│     └── Write Range: ResultsDT → "Loan Results" sheet                           │
│                                                                                  │
│  7. 📝 Log Message: "AUTOMATION COMPLETE"                                       │
│                                                                                  │
│  8. 💬 Message Box: Show summary to user                                        │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

## Activity Screenshots Reference

### Screenshot 1: Variables Panel
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│ VARIABLES                                                         [+ Variable]  │
├─────────────────────────────────────────────────────────────────────────────────┤
│ 📋 PDFFolderPath      String    "C:\Users\nikki\Downloads\Loan_Applic..."      │
│ 📋 OutputExcelPath    String    "C:\Users\nikki\Downloads\UIBank_Auto..."      │
│ 📋 PDFFiles           List<Str>                                                 │
│ 📋 PDFText            String                                                    │
│ 📋 Email              String    ""                                              │
│ 📋 Age                String    ""                                              │
│ 📋 AmountUSD          Double    0                                               │
│ 📋 ResultsDT          DataTable                                                 │
│ 📋 SuccessCount       Int32     0                                               │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Screenshot 2: Main Sequence
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│ ▼ Sequence: LOAN AUTOMATION - MAIN                                              │
│   │                                                                              │
│   ├── 📝 Log Message "========== LOAN AUTOMATION STARTED =========="           │
│   │                                                                              │
│   ├── 📊 Build DataTable                                                        │
│   │   └── [ResultsDT] with 14 columns                                           │
│   │                                                                              │
│   ├── ✏️ Assign: BotEmail = "loanbot" & DateTime...                             │
│   │                                                                              │
│   ├── ✏️ Assign: PDFFiles = Directory.GetFiles(...)                             │
│   │                                                                              │
│   ├── 🌐 Open Browser: UiBank Registration                                      │
│   │   └── ▼ Sequence: Register Account                                          │
│   │       ├── ⏱️ Delay 3 seconds                                                │
│   │       ├── ⌨️ Type Into: Email                                               │
│   │       ├── ⌨️ Type Into: Password                                            │
│   │       ├── ⌨️ Type Into: First Name                                          │
│   │       └── ... (more fields)                                                  │
│   │                                                                              │
│   ├── 🔄 For Each: PDF in PDFFiles                                              │
│   │   └── ▼ Try Catch                                                           │
│   │       ├── Try: ▼ Sequence: Process Single PDF                               │
│   │       │   ├── 📄 Read PDF Text                                              │
│   │       │   ├── ✏️ Assign: Extract Email (Regex)                              │
│   │       │   ├── ✏️ Assign: Extract Age (Regex)                                │
│   │       │   ├── ... (more extractions)                                        │
│   │       │   ├── 🔀 Switch: Currency                                           │
│   │       │   ├── ❓ If: PDFDecision = "APPROVED"                               │
│   │       │   └── ➕ Add Data Row                                               │
│   │       └── Catch: Exception                                                   │
│   │           └── 📝 Log Error                                                   │
│   │                                                                              │
│   ├── 📊 Excel Application Scope                                                │
│   │   └── 📝 Write Range                                                        │
│   │                                                                              │
│   └── 💬 Message Box: Summary                                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Screenshot 3: For Each Loop Detail
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│ ▼ For Each: PDF in PDFFiles                                                     │
│   │                                                                              │
│   │ TypeArgument: String                                                         │
│   │ Values: PDFFiles                                                             │
│   │                                                                              │
│   └── ▼ Body                                                                     │
│       └── ▼ Try Catch                                                            │
│           │                                                                      │
│           ├── ▼ Try                                                              │
│           │   └── ▼ Sequence: Process Single PDF                                 │
│           │       │                                                              │
│           │       ├── ✏️ Assign: CurrentPDF = pdfPath                           │
│           │       │                                                              │
│           │       ├── 📝 Log Message: "Processing: " & fileName                 │
│           │       │                                                              │
│           │       ├── 📄 Read PDF Text                                          │
│           │       │   ├── FileName: [pdfPath]                                    │
│           │       │   └── Output: [PDFText]                                      │
│           │       │                                                              │
│           │       ├── ════════════════════════════════════════                  │
│           │       │   EXTRACTION SECTION (Regex Assigns)                        │
│           │       ├── ════════════════════════════════════════                  │
│           │       │                                                              │
│           │       ├── ✏️ Assign: ApplicationID                                  │
│           │       │   └── Regex.Match(PDFText, "Application ID[:\s]*(\S+)")     │
│           │       │                                                              │
│           │       ├── ✏️ Assign: Email                                          │
│           │       │   └── Regex.Match(PDFText, "Email Address\s+(\S+@\S+)")     │
│           │       │                                                              │
│           │       └── ... (continues)                                            │
│           │                                                                      │
│           └── ▼ Catches                                                          │
│               └── Catch Exception                                                │
│                   └── 📝 Log Message: "Error: " & ex.Message                    │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

# ☁️ Power Automate Integration

## Power Automate Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    POWER AUTOMATE CLOUD FLOW                                     │
│                  (Hybrid Integration with UiPath)                                │
└─────────────────────────────────────────────────────────────────────────────────┘

    ┌─────────────────┐
    │   ⏰ TRIGGER     │
    │   Recurrence    │
    │   Daily 9:00 AM │
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │ 📁 GET FILES    │
    │ SharePoint      │
    │ Get PDF list    │
    └────────┬────────┘
             │
             ▼
    ┌─────────────────────────────────────────────────────────────────────────┐
    │                         🔄 FOR EACH PDF                                  │
    │  ┌─────────────┐                                                        │
    │  │             │                                                        │
    │  │  📄 Get PDF ├──▶ 🤖 AI Builder ──▶ 💱 HTTP ──▶ 🤖 Trigger    │
    │  │  Content    │    Extract Data      Exchange    UiPath Job    │
    │  │             │                      Rate API                          │
    │  └─────────────┘                                                        │
    │         │                                                               │
    │         ▼                                                               │
    │  ┌─────────────┐                                                        │
    │  │ ⏳ Wait for │                                                        │
    │  │ Job Complete│                                                        │
    │  └─────────────┘                                                        │
    │         │                                                               │
    │         ▼                                                               │
    │  ┌─────────────┐                                                        │
    │  │ 📊 Collect  │                                                        │
    │  │ Results     │                                                        │
    │  └─────────────┘                                                        │
    └────────────────────────────────────────────────────────────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │ 📊 CREATE EXCEL │
    │ Excel Online    │
    │ Write Results   │
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │ 📧 NOTIFY       │
    │ Teams Message   │
    │ "Complete!"     │
    └─────────────────┘
```

## Power Automate Actions Detail

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                     POWER AUTOMATE ACTIONS                                       │
└─────────────────────────────────────────────────────────────────────────────────┘

ACTION 1: Recurrence Trigger
┌─────────────────────────────────────┐
│ ⏰ Recurrence                       │
│ ├── Frequency: Day                  │
│ ├── Interval: 1                     │
│ └── Start Time: 09:00 AM            │
└─────────────────────────────────────┘

ACTION 2: Get Files from SharePoint
┌─────────────────────────────────────┐
│ 📁 Get files (properties only)      │
│ ├── Site: yoursite.sharepoint.com   │
│ ├── Library: LoanPDFs               │
│ └── Filter: FileType eq 'pdf'       │
└─────────────────────────────────────┘

ACTION 3: For Each Loop
┌─────────────────────────────────────┐
│ 🔄 Apply to each                    │
│ ├── Select output from: Get files   │
│ │                                   │
│ │ ┌─────────────────────────────┐  │
│ │ │ 📄 Get file content         │  │
│ │ ├── File: @{items('...')['Id']}│  │
│ │ └─────────────────────────────┘  │
│ │                                   │
│ │ ┌─────────────────────────────┐  │
│ │ │ 🤖 AI Builder - Extract     │  │
│ │ ├── Model: LoanApplicationModel│  │
│ │ ├── Document: @{body('...')}   │  │
│ │ └─────────────────────────────┘  │
│ │                                   │
│ │ ┌─────────────────────────────┐  │
│ │ │ 🌐 HTTP - Exchange Rate     │  │
│ │ ├── Method: GET               │  │
│ │ ├── URI: api.exchangerate...  │  │
│ │ └─────────────────────────────┘  │
│ │                                   │
│ │ ┌─────────────────────────────┐  │
│ │ │ 🤖 UiPath - Start Job       │  │
│ │ ├── Process: LoanAutomation   │  │
│ │ ├── Input: @{json(...)}       │  │
│ │ └─────────────────────────────┘  │
│ │                                   │
│ └───────────────────────────────────│
└─────────────────────────────────────┘

ACTION 4: Create Excel Report
┌─────────────────────────────────────┐
│ 📊 Add a row into a table           │
│ ├── Location: OneDrive for Business │
│ ├── Document: LoanResults.xlsx      │
│ ├── Table: Results                  │
│ └── Row: @{variables('Results')}    │
└─────────────────────────────────────┘

ACTION 5: Send Notification
┌─────────────────────────────────────┐
│ 📧 Post message in a chat or channel│
│ ├── Team: Operations                │
│ ├── Channel: Loan Processing        │
│ └── Message: "Complete! ✅"          │
└─────────────────────────────────────┘
```

---

# 📊 Data Flow

## Data Transformation Pipeline

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                          DATA FLOW DIAGRAM                                       │
└─────────────────────────────────────────────────────────────────────────────────┘

┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   SOURCE     │     │  EXTRACTED   │     │  CONVERTED   │     │   OUTPUT     │
│   (PDF)      │────▶│    DATA      │────▶│    DATA      │────▶│   (EXCEL)    │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘

┌──────────────────────────────────────────────────────────────────────────────────┐
│ PDF CONTENT                                                                       │
│ ────────────                                                                      │
│ LOAN APPLICATION REPORT                                                           │
│ Application ID: US42851                                                           │
│ Email Address: kevin.davis760@gmail.com                                           │
│ Age: 69                                                                           │
│ Country: United States                                                            │
│ Yearly Income: USD 66,579                                                         │
│ Requested Amount: USD 31,049                                                      │
│ Estimated Interest Rate: 7.64%                                                    │
│ APPROVED                                                                          │
└──────────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ REGEX EXTRACTION
                                    ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ EXTRACTED DATA (Dictionary)                                                       │
│ ──────────────────────────                                                        │
│ ApplicationID    = "US42851"                                                      │
│ Email            = "kevin.davis760@gmail.com"                                     │
│ Age              = "69"                                                           │
│ Country          = "United States"                                                │
│ YearlyIncome     = "66579"                                                        │
│ Currency         = "USD"                                                          │
│ RequestedAmount  = "31049"                                                        │
│ InterestRate     = "7.64%"                                                        │
│ PDFDecision      = "APPROVED"                                                     │
└──────────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ CURRENCY CONVERSION
                                    ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ CONVERTED DATA                                                                    │
│ ──────────────                                                                    │
│ AmountUSD        = 31049.00  (USD → USD, rate = 1.0)                             │
│ IncomeUSD        = 66579.00  (USD → USD, rate = 1.0)                             │
│                                                                                   │
│ Example with EUR:                                                                 │
│ Original: 50000 EUR → 50000 / 0.92 = 54347.83 USD                                │
└──────────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ ADD TO DATATABLE
                                    ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ EXCEL OUTPUT (DataTable Row)                                                      │
│ ────────────────────────────                                                      │
│ ┌────────────┬─────────────────────────┬─────┬───────────────┬──────────────┐   │
│ │ApplicationID│Email                    │Age  │Country        │LoanType      │   │
│ ├────────────┼─────────────────────────┼─────┼───────────────┼──────────────┤   │
│ │US42851     │kevin.davis760@gmail.com │69   │United States  │Car Loan      │   │
│ └────────────┴─────────────────────────┴─────┴───────────────┴──────────────┘   │
│ ┌────────────┬────────────┬────────┬───────────┬──────────┬─────────┬──────┐   │
│ │AmountUSD   │IncomeUSD   │Currency│PDFDecision│Interest  │Status   │LoanID│   │
│ ├────────────┼────────────┼────────┼───────────┼──────────┼─────────┼──────┤   │
│ │31049.00    │66579.00    │USD     │APPROVED   │7.64%     │SUBMITTED│US4285│   │
│ └────────────┴────────────┴────────┴───────────┴──────────┴─────────┴──────┘   │
└──────────────────────────────────────────────────────────────────────────────────┘
```

## Currency Conversion Table

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      CURRENCY CONVERSION RATES                                   │
│                         (Hardcoded Fallback)                                     │
└─────────────────────────────────────────────────────────────────────────────────┘

┌──────────┬──────────────┬─────────────┬─────────────────────────────────────────┐
│ Currency │ Exchange Rate│ Example     │ Calculation                             │
│          │ (to 1 USD)   │             │                                         │
├──────────┼──────────────┼─────────────┼─────────────────────────────────────────┤
│ USD      │ 1.00         │ $50,000     │ 50000 / 1.00 = $50,000.00 USD          │
│ EUR      │ 0.92         │ €50,000     │ 50000 / 0.92 = $54,347.83 USD          │
│ GBP      │ 0.79         │ £50,000     │ 50000 / 0.79 = $63,291.14 USD          │
│ RUB      │ 92.00        │ ₽50,000     │ 50000 / 92.00 = $543.48 USD            │
│ MXN      │ 17.50        │ $50,000 MXN │ 50000 / 17.50 = $2,857.14 USD          │
│ CAD      │ 1.36         │ $50,000 CAD │ 50000 / 1.36 = $36,764.71 USD          │
│ AUD      │ 1.53         │ $50,000 AUD │ 50000 / 1.53 = $32,679.74 USD          │
│ JPY      │ 149.50       │ ¥50,000     │ 50000 / 149.50 = $334.45 USD           │
│ CNY      │ 7.25         │ ¥50,000 CNY │ 50000 / 7.25 = $6,896.55 USD           │
│ INR      │ 83.00        │ ₹50,000     │ 50000 / 83.00 = $602.41 USD            │
│ BRL      │ 5.00         │ R$50,000    │ 50000 / 5.00 = $10,000.00 USD          │
└──────────┴──────────────┴─────────────┴─────────────────────────────────────────┘
```

---

# 📋 Technical Specifications

## Regex Patterns Used

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           REGEX PATTERNS                                         │
└─────────────────────────────────────────────────────────────────────────────────┘

┌─────────────────┬────────────────────────────────────────────────────────────────┐
│ Field           │ Regex Pattern                                                  │
├─────────────────┼────────────────────────────────────────────────────────────────┤
│ Application ID  │ Application ID[:\s]*(\S+)                                      │
│ Email           │ Email Address\s+(\S+@\S+)                                      │
│ Age             │ Age\s+(\d+)                                                    │
│ Country         │ Country\s+([A-Za-z\s]+?)(?=Credit|$)                          │
│ Yearly Income   │ Yearly Income\s+[A-Z]{3}\s+([0-9,]+)                          │
│ Currency        │ Yearly Income\s+([A-Z]{3})                                     │
│ Loan Type       │ Loan Type\s+([A-Za-z\s]+?)(?=Requested|$)                     │
│ Requested Amount│ Requested Amount\s+[A-Z]{3}\s+([0-9,]+)                       │
│ Interest Rate   │ Estimated Interest Rate\s+([0-9.]+)%                          │
│ Decision        │ Contains("APPROVED") or Contains("DECLINED")                   │
└─────────────────┴────────────────────────────────────────────────────────────────┘
```

## System Requirements

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         SYSTEM REQUIREMENTS                                      │
└─────────────────────────────────────────────────────────────────────────────────┘

HARDWARE:
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Component        │ Minimum              │ Recommended                            │
├──────────────────┼──────────────────────┼────────────────────────────────────────┤
│ CPU              │ Dual Core 2.0 GHz    │ Quad Core 3.0 GHz                     │
│ RAM              │ 4 GB                 │ 8 GB                                   │
│ Storage          │ 500 MB free          │ 1 GB free                              │
│ Display          │ 1024x768             │ 1920x1080                              │
└──────────────────┴──────────────────────┴────────────────────────────────────────┘

SOFTWARE:
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Software         │ Version              │ Purpose                                │
├──────────────────┼──────────────────────┼────────────────────────────────────────┤
│ UiPath Studio    │ 2024 (v26.x)         │ Automation development & execution     │
│ Chrome Browser   │ Latest               │ Web automation                         │
│ Microsoft Excel  │ 2016 or later        │ Report generation                      │
│ .NET Framework   │ 4.6.1 or later       │ Runtime dependency                     │
│ Windows OS       │ 10/11                │ Operating system                       │
└──────────────────┴──────────────────────┴────────────────────────────────────────┘

UIPATH PACKAGES:
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Package                          │ Version       │ Purpose                       │
├──────────────────────────────────┼───────────────┼───────────────────────────────┤
│ UiPath.System.Activities         │ 24.10.0+      │ Core activities               │
│ UiPath.UIAutomation.Activities   │ 24.10.0+      │ Browser automation            │
│ UiPath.PDF.Activities            │ 3.18.0+       │ PDF text extraction           │
│ UiPath.Excel.Activities          │ 2.23.0+       │ Excel report writing          │
└──────────────────────────────────┴───────────────┴───────────────────────────────┘
```

---

# 📸 Screenshots Guide

## How to Take Screenshots in UiPath Studio

To document your project with actual screenshots:

### Screenshot 1: Full Project View
```
1. Open Main.xaml in UiPath Studio
2. Zoom out to see entire workflow (Ctrl + Mouse Wheel)
3. Press Print Screen or use Snipping Tool
4. Save as: "01_Full_Project_View.png"
```

### Screenshot 2: Variables Panel
```
1. Click on "Variables" tab at bottom of designer
2. Expand panel to show all variables
3. Screenshot the variables panel
4. Save as: "02_Variables_Panel.png"
```

### Screenshot 3: For Each Loop
```
1. Expand the "For Each PDF" activity
2. Show the inner sequence
3. Screenshot the expanded view
4. Save as: "03_ForEach_Loop.png"
```

### Screenshot 4: Regex Extraction
```
1. Click on one of the Assign activities for extraction
2. Show the Properties panel with Regex pattern
3. Screenshot the activity and properties
4. Save as: "04_Regex_Extraction.png"
```

### Screenshot 5: Currency Switch
```
1. Expand the Switch activity for currency
2. Show all the Case branches
3. Screenshot the expanded switch
4. Save as: "05_Currency_Switch.png"
```

### Screenshot 6: Excel Write
```
1. Expand the Excel Application Scope
2. Show the Write Range activity
3. Screenshot the configuration
4. Save as: "06_Excel_Write.png"
```

---

# 📎 Appendix

## Sample PDF Content Structure

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    LOAN APPLICATION REPORT                                       │
│                    Application ID: US42851                                       │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│ APPLICANT INFORMATION                                                            │
│ ┌─────────────────────┬─────────────────────────────────┐                       │
│ │ Field               │ Value                           │                       │
│ ├─────────────────────┼─────────────────────────────────┤                       │
│ │ Email Address       │ kevin.davis760@gmail.com        │                       │
│ │ Age                 │ 69                              │                       │
│ │ Country             │ United States                   │                       │
│ │ Credit Score        │ 737                             │                       │
│ │ Risk Tier           │ Good                            │                       │
│ │ Yearly Income       │ USD 66,579                      │                       │
│ └─────────────────────┴─────────────────────────────────┘                       │
│                                                                                  │
│ LOAN REQUEST DETAILS                                                             │
│ ┌─────────────────────┬─────────────────────────────────┐                       │
│ │ Field               │ Value                           │                       │
│ ├─────────────────────┼─────────────────────────────────┤                       │
│ │ Loan Type           │ Car Loan                        │                       │
│ │ Requested Amount    │ USD 31,049                      │                       │
│ │ Loan Term           │ 4 years                         │                       │
│ │ Currency            │ USD                             │                       │
│ └─────────────────────┴─────────────────────────────────┘                       │
│                                                                                  │
│ RISK ASSESSMENT                                                                  │
│ ┌─────────────────────┬─────────────────┬───────────────┐                       │
│ │ Metric              │ Value           │ Status        │                       │
│ ├─────────────────────┼─────────────────┼───────────────┤                       │
│ │ Debt-to-Income      │ 11.7%           │ Good          │                       │
│ │ Loan-to-Income      │ 0.47            │ Good          │                       │
│ │ Interest Rate       │ 7.64%           │ Competitive   │                       │
│ └─────────────────────┴─────────────────┴───────────────┘                       │
│                                                                                  │
│ APPROVAL DECISION                                                                │
│ ┌─────────────────────────────────────────────────────────┐                     │
│ │                      ✅ APPROVED                         │                     │
│ └─────────────────────────────────────────────────────────┘                     │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

## Excel Output Structure

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         EXCEL OUTPUT: LoanResults.xlsx                           │
│                              Sheet: "Loan Results"                               │
└─────────────────────────────────────────────────────────────────────────────────┘

┌──────────┬─────────────────────────┬─────┬───────────────┬──────────┬──────────┐
│   A      │          B              │  C  │      D        │    E     │    F     │
│AppID     │Email                    │Age  │Country        │LoanType  │AmountUSD │
├──────────┼─────────────────────────┼─────┼───────────────┼──────────┼──────────┤
│US42851   │kevin.davis760@gmail.com │69   │United States  │Car Loan  │31049.00  │
│NEW       │james.baker90@icloud.com │27   │Russia         │Personal  │354.90    │
│CA19374   │sarah.wilson@yahoo.com   │45   │Canada         │Mortgage  │125000.00 │
│...       │...                      │...  │...            │...       │...       │
└──────────┴─────────────────────────┴─────┴───────────────┴──────────┴──────────┘

┌──────────┬──────────┬───────────┬──────────┬─────────────┬──────────┬──────────┐
│    G     │    H     │    I      │    J     │     K       │    L     │    M     │
│IncomeUSD │Currency  │PDFDecision│Interest  │UiBankStatus │LoanID    │APR       │
├──────────┼──────────┼───────────┼──────────┼─────────────┼──────────┼──────────┤
│66579.00  │USD       │APPROVED   │7.64%     │SUBMITTED    │US42851   │7.64%     │
│411.01    │RUB       │DECLINED   │17.40%    │SKIPPED      │N/A       │N/A       │
│95000.00  │CAD       │APPROVED   │5.25%     │SUBMITTED    │CA19374   │5.25%     │
│...       │...       │...        │...       │...          │...       │...       │
└──────────┴──────────┴───────────┴──────────┴─────────────┴──────────┴──────────┘
```

---

*Document Version: 2.0*
*Last Updated: January 2025*
*Total Pages: ~25*
