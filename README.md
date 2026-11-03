# Mark Six Checker

Static pages for checking Hong Kong Mark Six results. Draw history lives in [results.json](results.json): newest first, each row is a date, six main numbers, and one special number. Open [estimation.html](estimation.html) directly, or host the folder on GitHub Pages. No build step is required.

The estimation page automatically fetches the default GitHub results URL on startup. Loading controls start collapsed; open **JSON 載入設定** to reload with **從網址讀取**, change the URL, choose a local `.json` file, or paste JSON. Load errors appear inside these settings. The file and pasted text stay in the browser. The special number is counted together with the six main numbers unless a section says otherwise. Counts, gaps, and highlights are descriptions of past draws, not a next-draw probability.

## Estimation page

[estimation.html](estimation.html) has seven sections.

### 每個號碼的 5 期走勢

Uses the latest 30 draws, split into non-overlapping windows 1–5, 6–10, 11–15, 16–20, 21–25, and 26–30.

- **30期冷熱:** 0–3 冷, 4–5 正常, 6+ 熱. Each five-draw cell is 0–1 冷, 2–3 正常, 4–5 熱.
- **活躍度** weights the windows 6, 5, 4, 3, 2, 1 from newest to oldest.
- **趨勢分** uses +3, +2, +1, −1, −2, −3. A positive score only means more of the appearances were recent.
- **距今** is how many draws since the number last appeared.
- Attention tags: 近5熱, 潛力上升, 熱但近10未出, 冷但回升, and 轉冷.
- Sort by number, activity, trend, last 5 draws, or 30-draw count. Filter by temperature, or show only non-negative trend scores.
- Click a number to highlight its row.

### 區間 × 5 期窗口

Counts numbers in 1–10, 11–20, 21–30, 31–40, and 41–49 for each five-draw window. Shows the expected count, the 30-draw total, and how far the latest window is from that expectation. 41–49 has a lower expectation because it contains 9 numbers, not 10.

### 每期區間分佈

One row per draw: the six main numbers, the special number, and the five band counts. **只顯示估算用的最新 30 期** hides older draws. Draws outside that window are dimmed when shown.

### 所有歷史結果

The full history table. Click a number to highlight every row that contains it and open its statistics. Click outside the number or popover, or press Escape, to clear both. Search 1–49 to keep only dates that contain that number, in the main numbers or as the special number.

Above the table:

- **臨近平均間距:** numbers whose current gap is within 1 draw of their own average gap. Yellow means still up to 1 draw short. Red means it has reached the average, or passed it by up to 1 draw.
- **最久未出:** the 10 numbers with the longest current gap, with the last date.
- **跟上期重覆:** how many balls from the latest draw also appeared in the previous draw, compared with the historical average overlap. It also shows how often each of the latest 7 numbers returned within 1 draw and within 2 draws. A green border means that return rate is at least the average of all 49 numbers.

The popover shows the all-history count, main versus special split, last appearance, average / median / longest completed gap, where the current gap sits among those gaps, the shortfall or overrun versus the average, and the 30-draw temperature, trend, activity, and verdict.

### 每月號碼走勢

Uses all loaded draws, including special numbers, grouped by calendar month. Click a number in the table to open its Chart.js line chart in a popup, oldest month first. Close with the close button, Escape, or a click on the backdrop. Months with no loaded draws appear as gaps, not zero appearances. These are raw counts; different months may contain different numbers of draws.

The table lists all 49 numbers with oldest-month-first counts from left to right (January through October for the current data) and overall totals. Month headers show draw counts, and the number column stays visible during horizontal scrolling. Both views refresh with the existing URL, local-file, or pasted-JSON loader. Chart.js loads from a CDN; the table remains available if it fails to load.

**偏多時段** groups all loaded draws into month-start (days 1–10), middle (11–20), and end (21–31, including the 31st). The row shows the segment with the most raw appearances and its share of that number's appearances. More than half is labelled **為主**, otherwise **較多**; equal highest counts are shown as ties. Hover or expand the popup evidence for all three counts, draw totals, and appearances per draw, since the segments may contain different numbers of draws. Missing records and partial months affect the distribution; it is not evidence that a number prefers a calendar period.

**常同出（前3）** shows each number's three most frequent companions across all loaded history, including special numbers, with joint-appearance counts. Ties are ordered by number. Hover for the count and share of the selected number's appearances; click a companion to open its monthly chart. The popup evidence lists every companion with a positive count. These are historical co-occurrences, not numbers guaranteed to appear together or next-draw probabilities. The existing **號碼組合** section remains limited to the latest 30 draws.

**近期走勢** describes the latest two consecutive loaded calendar months, independently of the future-facing **走勢參考**. Equal raw appearance counts are treated as flat despite different draw counts: **近期持平**, **低位徘徊** when both months are low, or **近2月未出** for two zeros. Otherwise it compares appearances per draw, with low defined as a rounded per-12-draw level of 0–1. Other labels include **近期上升**, **低位回升**, **由低位回升**, **下降至低位**, and **近期下降**. Rising/rebounding text is green, falling text red, low/absent text blue, and flat/insufficient data neutral. Hover for the source monthly counts and draw counts. Missing months are not bridged; partial or missing data can distort the description. This describes observed activity, not what must happen next.

**下月外推** adds a historical projection for each number, rounded to one decimal: appearances in the latest three calendar months of loaded data / draws in those months × planned next-month draws. The projected month follows the latest loaded month, not today's date. Set **預計下月期數**, or leave it blank to use the rounded average draw count of the loaded months within that window. Missing months are not treated as zero-draw months. The popup also shows the number's estimate. Invalid draw counts disable estimates without hiding the history table.

This is a continuation of a recent historical rate, not a validated forecast or next-draw probability. Partial months and missing records affect both the observed rate and the automatic draw-count assumption; supply a planned draw count when known. Fractional estimates describe an average, not a guaranteed integer result.

**走勢參考** shows one plain-language sentence, such as **其後有可能會上升**, **其後有可能會下降**, or **其後有可能維持平穩**. The summary checks historical references in order: matching two-month levels, three-month shape, two-month direction, then same raw count. It uses the first category with a strict majority of following-month rate outcomes; a tied or inconclusive category falls back to the next. If none resolves, it shows unclear direction or insufficient data. This is a descriptive heuristic, not a probability or agreement between all methods; even one example can produce a tentative sentence.

The popup repeats the sentence and keeps the evidence collapsed under **查看歷史依據**, including the chosen category, sample count, matching dates, and warnings. Two-month level patterns use appearances per 12 draws, rounded to an integer; zero is reserved for no appearances, and nonzero rates have a minimum level of 1. Missing calendar months are never joined into a pattern. Reference counts use pooled following-month appearances / draws × planned next-month draws. Fewer than three matches are marked **資料不足** in the evidence. Overlapping examples are not independent samples. No number is "due" because it was absent; partial or missing records can distort these references.

Click a monthly row to highlight it across all columns; click it again to clear, or another row to move the highlight. A focused row also supports Enter or Space. Clicking its number highlights the row and opens the chart without toggling the highlight off. Highlighting preserves horizontal scroll and survives a projection draw-count change.

Use **Add to watchlist** in the number popup to save a number; the same button changes to **Remove from watchlist** when it is saved. Alternatively, click **+** at the end of a monthly row to add its number, or **−** to remove it. Table buttons, popup controls, and the saved list stay synchronized without changing row highlights or horizontal scroll. Above the monthly table, saved numbers appear in ascending order with individual remove buttons, **Clear watchlist**, and **Copy watchlist**. Copying produces only unique numbers separated by commas, for example `1,8,11`, with no spaces or labels. A read-only text field remains available for manual copying if clipboard access fails. The watchlist is separate from row highlighting, survives result/projection reloads, and is saved in this browser using local storage when available. It is not synced between devices; blocked storage still allows an in-memory list and copying.

The expandable popup evidence includes **同次數**, **近2月方向**, and **近3月形態**. Same-count references find earlier months with the latest month's raw appearance count, not necessarily true peaks or equivalent exposure. Two-month direction references match the latest rise, fall, or flat movement; three-month shape references match both directions in the latest three-month run. Directions use unrounded appearances per draw, without requiring identical counts, so equal raw counts in months with different draw counts may have different directions. All show match counts, following-month rate rises / flats / falls, and exposure-adjusted reference counts using the planned next-month draws.

Inside **查看歷史依據** below the popup chart, each reference lists the actual source months and the next one or two recorded monthly outcomes with draw counts. Gaps are never bridged, unavailable outcomes are not inferred, and only earlier references with a known next month are counted. Fewer than three examples are marked **資料不足**. These broader comparisons can disagree; they are not independent evidence or an ensemble forecast, and no historical rise or decline guarantees a repeat.

### 號碼組合

Two-number and three-number combinations in the latest 30 draws, including the special number. Filter by one or more required numbers and a minimum number of times they appeared together. Results are paged 100 rows at a time. Click a number for the same statistics popover.

### 隨機分組

Load at least 30 draws, then choose **2組**, **3組**, or **4組** and click **隨機生成**. With the full 1–49 pool, sizes are 24/25, 16/16/17, or 12/12/12/13 respectively.

- Leave **號碼範圍** blank for 1–49, or enter a shared pool such as `{1-20,31-40}` (30 numbers). Braces are optional; individual numbers and overlapping ranges are accepted, with duplicates removed. Numbers outside the ranges are omitted.
- Every allowed number appears exactly once across the groups, with no duplicates within or between groups. Group sizes differ by at most one: 30 numbers produce 15/15 for two groups or 10/10/10 for three groups.
- Each group contains cold, normal, and hot numbers using the latest 30 draws, including special numbers: 0–3 cold, 4–5 normal, and 6+ hot. Category counts are spread as evenly as group capacity allows.
- Generation stops with an explanation if any category has fewer numbers than the chosen number of groups. The category requirements are not silently relaxed.
- Each group shows its size, category counts, and colour-coded numbers in ascending order. Tap or click a number for the same statistics popover, including its trend score, recent counts, activity, and verdict. Viewing statistics does not change the generated groups.
- **複製第N組** copies only that group's numbers, separated by commas, without a heading. A read-only text field also contains the comma-separated list; if automatic copying fails, it is selected for manual copying.
- Generate again to reshuffle. Changing the group count, editing the ranges, or loading new results clears the generated lists and any open number popover. Invalid ranges or a pool too small to meet the category requirements produce an error rather than partial groups.

Random grouping is a way to organise the numbers, not a prediction of future draws.
