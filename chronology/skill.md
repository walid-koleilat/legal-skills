---
name: chronology
description: Build a comprehensive, date-based chronology of key facts and events from legal documents with page-level source citations, output as Word document
---

# Legal Chronology Skill

## What This Skill Produces

A structured legal chronology **Word document (.docx)** containing:

| Section | Description |
|---------|-------------|
| **Matter Information** | Case name, client, file number, jurisdiction, date prepared |
| **Short Summary** | 2-4 sentence case overview with citations |
| **Chronology of Events** | All dated events in chronological order |
| **Mitigation Journal** | Job search activities, applications, interviews, outcomes |
| **Description of Parties** | All individuals and entities with roles |
| **Key Legal Issues** | Nature of dispute and legal questions involved |

**Output file**: `output/chronology.docx`

---

## Instructions

You are a legal assistant building a comprehensive case chronology. Analyze the provided documents and extract all relevant information with page-level citations.

### Input

Legal documents in any format (PDF, Word, text):
- Contracts and employment agreements
- Correspondence and letters
- Pleadings and court filings
- Emails and communications
- Employee handbooks and policies
- Pay stubs and financial records
- Job search records and mitigation journals
- Records of Employment (ROE)

---

## Extraction Requirements

### 1. Matter Information

Extract from documents:
- **Matter Name**: Case or file name
- **Client Name**: Primary client/plaintiff
- **File Number**: Court file or internal reference (e.g., CV-21-00661737-0000)
- **Jurisdiction**: Court and location
- **Date Prepared**: Today's date

If any field cannot be determined, mark as "Not specified in documents".

### 2. Parties

Identify ALL parties mentioned:

| Category | Examples |
|----------|----------|
| Primary parties | Plaintiff, defendant, appellant, respondent |
| Individuals | Names with titles/positions |
| Entities | Company names (legal and operating names) |
| Legal counsel | Lawyers for each side |
| Witnesses | Named witnesses |
| HR/Management | HR personnel, managers, executives |
| Third parties | Service providers, insurers, agencies |
| Government | Service Canada, Ministry of Labour |

For each party, capture the **source document and page** where first mentioned.

### 3. Events

Extract every relevant event with:

| Field | Description | Example |
|-------|-------------|---------|
| **Date** | YYYY-MM-DD format | 2020-06-08 |
| **Event** | Concise description | Employment commenced |
| **Parties** | Who was involved | Barbara Wilds, Gibson Building Supplies |
| **Source** | Document and page | Employment Agreement.pdf, [2] |
| **Notes** | Legal significance | Triggers probation period |

**Critical Event Categories:**

1. **Employment/Contract Events**
   - Offer, acceptance, start date
   - Probation periods and end dates
   - Salary, position changes, promotions
   - Benefit eligibility dates

2. **Termination Events**
   - Notice of termination
   - Effective termination date
   - Last day worked vs. last day paid
   - Severance offered
   - ROE issuance (note if late)

3. **Correspondence**
   - Each letter/email as separate entry
   - Demands and deadlines
   - Settlement discussions

4. **Litigation Milestones**
   - Statement of Claim filed/served
   - Statement of Defence due/filed
   - Court filings with dates
   - Default notices

5. **Mitigation Efforts** (CRITICAL - Be Detailed)
   - Each job application (company, position, date)
   - Interview dates
   - Offers received/rejected
   - New employment (start date, position, salary)

6. **Financial Events**
   - Pay stubs and periods
   - Expenses and reimbursements
   - Bonus entitlements
   - Unpaid amounts claimed

7. **Policy References**
   - Handbook effective dates
   - Relevant policy provisions

### 4. Key Legal Issues

Identify:
- Nature of the dispute
- Legal issues involved (wrongful dismissal, ESA violations, etc.)
- Status of proceedings
- Notable context

---

## Citation Format (MANDATORY)

**Every fact MUST include the source document and page number.**

### Correct Format
```
Document Name.pdf, [page]
```

### Examples

CORRECT:
- `Employment Agreement.pdf, [2]`
- `Intake Meeting Notes.docx, [1]`
- `July 12, 2021 Letter to.pdf, [1-3]`
- `Termination Letter.pdf, [1]; Employment Agreement.pdf, [10]`

WRONG (never use):
- `Employment Agreement.pdf, page 2`
- `Employment Agreement.pdf, p. 2`
- `Employment Agreement.pdf, Page 2`
- `[2]` (missing document name)
- `Employment Agreement.pdf` (missing page)

---

## Output Format: Word Document (.docx)

Generate a professional Word document saved to `output/chronology.docx`.

### Document Structure

Use the following structure with proper Word formatting:

```
LEGAL CHRONOLOGY
================
[Title - Heading 1, Bold, Centered]

MATTER INFORMATION
------------------
[Heading 2]

| Field          | Value                    |
|----------------|--------------------------|
| Matter Name    | [Matter Name]            |
| Client Name    | [Client Name]            |
| File Number    | [File Number]            |
| Jurisdiction   | [Court/Location]         |
| Date Prepared  | [Today's Date]           |

[Use Word table formatting with borders]

---

SHORT SUMMARY OF CASE
---------------------
[Heading 2]

[2-4 sentences summarizing the case. Each sentence must include at least one citation.]

Example:
"Barbara Wilds was terminated without cause by Gibson Building Supplies on
October 29, 2020 (Termination Letter.pdf, [1]). The dispute centers on unpaid
statutory entitlements including vacation pay ($726.92), bonus ($450), and
expense reimbursements ($46.93) (Intake Meeting Notes.docx, [1]). Legal
proceedings commenced with a Statement of Claim filed May 5, 2021, Court
File CV-21-00661737-0000 (Dec. 12, 2021 Letter to R. Lexi.pdf, [1])."

---

CHRONOLOGY OF EVENTS
--------------------
[Heading 2]

[Word table with the following columns:]
| Date | Event / Description | Source Document(s) | Involved Parties | Notes / Legal Significance |

[Table formatting: Header row bold with gray background, alternating row colors optional]

---

MITIGATION JOURNAL
------------------
[Heading 2]

[Word table with the following columns:]
| Date | Activity | Details | Outcome | Source |

---

DESCRIPTION OF PARTIES
----------------------
[Heading 2]

[Word table with the following columns:]
| Name / Entity | Description / Relationship | First Referenced |

---

KEY LEGAL ISSUES
----------------
[Heading 2]

1. **Wrongful Dismissal** - Whether termination was lawful and appropriate notice/pay provided (Termination Letter.pdf, [1])
2. **ESA Violations** - Alleged failure to pay statutory minimums (Intake Meeting Notes.docx, [1])
3. [Additional issues as numbered list with bold headings]
```

### Word Document Formatting Requirements

1. **Font**: Use a professional font (e.g., Times New Roman 12pt or Calibri 11pt)
2. **Headings**: Use Word heading styles (Heading 1, Heading 2) for proper structure
3. **Tables**:
   - Use Word table formatting with visible borders
   - Header rows should be bold with light gray background
   - Auto-fit to content width
4. **Page Layout**:
   - Standard margins (1 inch)
   - Include page numbers in footer
   - Include document title in header (optional)
5. **Citations**: Format citations in italics within the text

### Creating the Word Document

Use Python with the `python-docx` library to generate the document:

```python
from docx import Document
from docx.shared import Inches, Pt
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.enum.table import WD_TABLE_ALIGNMENT

# Create document
doc = Document()

# Add title
title = doc.add_heading('LEGAL CHRONOLOGY', 0)
title.alignment = WD_ALIGN_PARAGRAPH.CENTER

# Add Matter Information section
doc.add_heading('Matter Information', level=1)
table = doc.add_table(rows=5, cols=2)
table.style = 'Table Grid'
# ... populate table cells

# Add other sections similarly

# Save document
doc.save('output/chronology.docx')
```

Alternatively, use the Bash tool with `pandoc` to convert markdown to docx:

```bash
pandoc output/chronology.md -o output/chronology.docx \
  --reference-doc=template.docx \
  -f markdown -t docx
```

---

## Rules

1. **Be exhaustive** - Capture every dated event, no matter how minor
2. **Granularity** - Each distinct event gets its own row; do not combine events
3. **Always cite** - Every fact must have document name and page number
4. **Chronological order** - Sort events from earliest to latest
5. **Uncertain dates** - Note as "circa January 2024" or "early 2024"
6. **Unknown info** - State "Not specified in documents" rather than guessing
7. **Mitigation is critical** - Thoroughly document all job search activities
8. **Cross-reference** - If documents reference the same event, verify and note both sources
9. **Professional language** - Use plain, concise legal terminology
10. **Word formatting** - Ensure proper heading styles, table formatting, and professional appearance

---

## Completion Summary

After generating the chronology, report:
- Number of documents analyzed
- Number of events captured
- Date range covered (earliest to latest event)
- Number of parties identified
- Location of output file: `output/chronology.docx`
