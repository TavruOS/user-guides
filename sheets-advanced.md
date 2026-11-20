# Clean sales data in Tavru Sheets and chart it in Analytics

[All guides](README.md) · [Analytics dashboard guide](analytics.md)

Use [sales-cleanup.csv](samples/sales-cleanup.csv) to practice standardizing labels. The script trims spaces and makes capitalization consistent so a region appears as one group in Analytics.

![Tavru Sheets workbook and editing ribbon](screens/sheets.png)

The Python example requires a Tavru Sheets build with the `workbook` scripting API. If your preview build predates that API, use the [formula workflow](#use-a-formula-on-an-earlier-build) below.

1. Open the CSV in Tavru Sheets and confirm **Region** is column A, **Product** is B, and **Revenue** is C. A pasted table in a blank workbook also works if the values land in separate columns. Save a working copy as XLSX.
2. Open **Developer → Code Editor**, select **Python**, and paste the script below. It reads six cells on the first worksheet and proposes updates only to those same cells. Python 3 must be available. Run scripts you trust because they execute with your account’s permissions.
3. Acknowledge the trust prompt and choose **Run preview**. Verify that it proposes edits in **A2:A7** only and reports six normalized labels. Choose **Apply script changes** when satisfied. The edits are grouped so you can undo them together.
4. Save the XLSX. In Tavru Analytics, choose **Connect**, open the workbook, and select the worksheet. Build a chart with **Region** on **Columns** and **Sum of Revenue** on **Rows**. Expect **East 2,800**, **North 2,600**, and **West 3,000**.

```python
rows = workbook.read("A2:A7")
cleaned = [[value.strip().title()] for (value,) in rows]
workbook.write("A2:A7", cleaned)
print(f"Normalized {len(cleaned)} region labels.")
```

The script uses the workbook bridge provided by Tavru Sheets. It assumes every cell in the sample range contains text. For a different workbook, inspect its first worksheet and range before running the script. Run a new preview if the workbook changes after the first one.


## Use a formula on an earlier build

1. Enter `Clean Region` in **D1** and `=PROPER(TRIM(A2))` in **D2**.
2. Fill the formula down through **D7**. Verify the cleaned labels, then save the workbook.
3. In Analytics, use **Clean Region** on **Columns** and **Sum of Revenue** on **Rows**. The totals are the same as in the Python workflow.
