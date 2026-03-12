# LOAN AUTOMATION - READY TO RUN

## Quick Start

1. **Extract your PDFs** to: `C:\Users\nikki\Downloads\Loan_Applications_Complete_Package`
2. **Open UiPath Studio**
3. **File → Open Project** → Select `project.json`
4. **Install packages if prompted** (PDF, Excel activities)
5. **Click RUN (F5)**

## What It Does

1. ✅ Reads all 100 PDF loan applications
2. ✅ Extracts: Email, Age, Country, Income, Amount, Interest Rate, Decision
3. ✅ Converts all currencies to USD
4. ✅ Creates UiBank account automatically
5. ✅ Generates Excel report with all results
6. ⛔ Does NOT send any emails

## Output

Excel file saved to: `C:\Users\nikki\Downloads\UIBank_Automation-main\UIBank_Automation-main\LoanResults.xlsx`

## Paths (Update if different)

In Main.xaml, change these variables if your paths are different:
- `PDFFolderPath` - Where your PDFs are
- `OutputExcelPath` - Where to save Excel results

## Requirements

- UiPath Studio 2024 (Community Edition)
- Chrome browser
- PDF files extracted from ZIP
