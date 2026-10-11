# Analytics Setup

The workflow logs each conversation with:

```text
Date | Session ID | Customer Message | AI Response | Escalated | Topic
```

Create a second tab in the same spreadsheet named:

`Analytics`

Assuming the conversation-log tab is named `Sheet1`, use the following.

## Main metrics

| Metric | Formula |
|---|---|
| Total Conversations | `=COUNTA(Sheet1!C2:C)` |
| Total Escalations | `=COUNTIF(Sheet1!E2:E,"Yes")` |
| Escalation Rate | `=IFERROR(COUNTIF(Sheet1!E2:E,"Yes")/COUNTA(Sheet1!C2:C),0)` |
| Normal Conversations | `=COUNTIF(Sheet1!E2:E,"No")` |
| Unique Chat Sessions | `=COUNTA(UNIQUE(FILTER(Sheet1!B2:B,Sheet1!B2:B<>"")))` |

Format the Escalation Rate cell as a percentage.

## Topic analytics

Example section:

```text
Topic                  | Count
Services               |
Consultation           |
Payments / Refunds     |
Business Info          |
Support / Complaints   |
Other                  |
```

Formulas:

```excel
=COUNTIF(Sheet1!F:F,"Services")
=COUNTIF(Sheet1!F:F,"Consultation")
=COUNTIF(Sheet1!F:F,"Payments / Refunds")
=COUNTIF(Sheet1!F:F,"Business Info")
=COUNTIF(Sheet1!F:F,"Support / Complaints")
=COUNTIF(Sheet1!F:F,"Other")
```

## Most common topic

If topic names are in `A10:A15` and counts are in `B10:B15`:

```excel
=INDEX(A10:A15,MATCH(MAX(B10:B15),B10:B15,0))
```

## Suggested charts

- **Customer Support Topics** — column chart using the topic/count table
- **Escalation Overview** — pie chart with:
  - Escalated: `=COUNTIF(Sheet1!E:E,"Yes")`
  - Not Escalated: `=COUNTIF(Sheet1!E:E,"No")`
