# Mark Six Checker

Static pages for checking Hong Kong Mark Six results. Draw history lives in [results.json](results.json): newest first, each row is a date, six main numbers, and one special number. Open [estimation.html](estimation.html) directly, or host the folder on GitHub Pages. No build step is required.

The estimation page automatically fetches the default GitHub results URL on startup. Loading controls start collapsed; open **JSON 載入設定** to reload with **從網址讀取**, change the URL, choose a local `.json` file, or paste JSON. Load errors appear inside these settings. The file and pasted text stay in the browser. The special number is counted together with the six main numbers unless a section says otherwise. Counts, gaps, and highlights are descriptions of past draws, not a next-draw probability.

Every import is validated before replacing the loaded history: each record needs a real `YYYY-MM-DD` date and exactly six main numbers plus one special number, all distinct integers from 1–49. Imports are sorted newest-first without modifying the source file. Identical results for the same date are counted once (main-number order does not matter), with the skipped count shown in the loading status. Conflicting results on the same date reject the entire import, as do malformed records; the previous history remains loaded. Errors automatically expand the loading settings and identify the offending record. Empty arrays remain supported for clearing the loaded data. Validation checks structure and consistency, not authenticity or completeness against official draw records.

## Estimation page

[estimation.html](estimation.html) has ten sections.

### 每個號碼的 5 期走勢

Uses the latest 30 draws, split into non-overlapping windows 1–5, 6–10, 11–15, 16–20, 21–25, and 26–30.

- **30期冷熱:** 0–3 冷, 4–5 正常, 6+ 熱. Each five-draw cell is 0–1 冷, 2–3 正常, 4–5 熱.
- **活躍度** weights the windows 6, 5, 4, 3, 2, 1 from newest to oldest.
- **趨勢分** uses +3, +2, +1, −1, −2, −3. A positive score only means more of the appearances were recent.
- **距今** is how many draws since the number last appeared.
- Attention tags: 近5熱, 潛力上升, 熱但近10未出, 冷但回升, and 轉冷.
- Sort by number, activity, trend, last 5 draws, or 30-draw count. Filter by temperature, or show only non-negative trend scores.
- Click a number to highlight its row.
- **COPY ALL** copies every number currently shown in the table, after the temperature and attention filters, as a comma-separated list sorted by number. It does not require SELECT.

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

The table lists all 49 numbers and overall totals, with monthly count columns collapsed by default. **月份明細** can show the latest 6 loaded months, latest 12, or all months, oldest first. This only changes visible columns: totals, forecasts, historical references, and popup charts still use the full loaded history. Month headers show draw counts, and the number column stays visible during horizontal scrolling. Both views refresh with the existing URL, local-file, or pasted-JSON loader. Chart.js loads from a CDN; the table remains available if it fails to load.

**偏多時段** groups all loaded draws into month-start (days 1–10), middle (11–20), and end (21–31, including the 31st). The row shows the segment with the most raw appearances and its share of that number's appearances. More than half is labelled **為主**, otherwise **較多**; equal highest counts are shown as ties. Hover or expand the popup evidence for all three counts, draw totals, and appearances per draw, since the segments may contain different numbers of draws. Missing records and partial months affect the distribution; it is not evidence that a number prefers a calendar period.

**常同出（前3）** shows each number's three most frequent companions across all loaded history, including special numbers, with joint-appearance counts. Ties are ordered by number. Hover for the count and share of the selected number's appearances; click a companion to open its monthly chart. The popup evidence lists every companion with a positive count. These are historical co-occurrences, not numbers guaranteed to appear together or next-draw probabilities. The existing **號碼組合** section remains limited to the latest 30 draws.

**近期走勢** describes the latest two consecutive loaded calendar months, independently of the future-facing **未來N期** column. Equal raw appearance counts are treated as flat despite different draw counts: **近期持平**, **低位徘徊** when both months are low, or **近2月未出** for two zeros. Otherwise it compares appearances per draw, with low defined as a rate at or below the 7/49 baseline, so the label no longer depends on a fixed 12-draw scale. Other labels include **近期上升**, **低位稍回升**, **由低位回升**, **下降至低位**, and **近期下降**. Rising/rebounding text is green, falling text red, low/absent text blue, and flat/insufficient data neutral. Hover for the source monthly counts, draw counts, and the low threshold. Missing months are not bridged; partial or missing data can distort the description. This describes observed activity, not what must happen next.

**下月外推** adds a historical projection for each number, rounded to one decimal: appearances in the latest three calendar months of loaded data / draws in those months × planned next-month draws. The projected month follows the latest loaded month, not today's date. Set **預計下月期數**, or leave it blank to use the rounded average draw count of the loaded months within that window. Missing months are not treated as zero-draw months. The popup also shows the number's estimate. Invalid draw counts disable estimates without hiding the history table.

This is a continuation of a recent historical rate, not a validated forecast or next-draw probability. Partial months and missing records affect both the observed rate and the automatic draw-count assumption; supply a planned draw count when known. Fractional estimates describe an average, not a guaranteed integer result.

**未來N期** replaces the old single direction sentence with per-number figures. Choose N = 1–4 with **短期預測期數**. Each cell shows the expected appearances in the next N draws with the random baseline (7/49 × N) in parentheses, then the chance of appearing at least once with its baseline, and a **偏多** / **偏少** / **未見差異** label. The estimate starts from the observed rate in the latest 30 draws, turns it into a z-score against 7/49 capped at ±2, tilts it by the current gap relative to that number's average completed gap (capped at ±1.5), applies fixed log-odds weights of 0.25 and 0.10, and clamps the per-draw rate to 8–26%. The label appears only when the 90% Wilson interval of the raw 30-draw rate excludes 7/49; because 49 numbers are compared at once, about five labels are expected from randomness alone. Hover a cell for the sample size, interval, gaps, and adjusted rate. Treat the column as a description of recent activity; it is not a probability for the next draw.

The popup shows the same forecast for the selected number above the collapsed **查看歷史依據**, and no longer prints a direction sentence. The two-month level pattern and the three historical references stay as descriptive evidence, each stating its own sample count next to the fair baseline. Two-month level patterns bucket each month by a scale-free Poisson z-score of appearances against draws × 7/49: **低** for z ≤ −0.7, **高** for z ≥ 0.7, and **中** otherwise, so months with different draw counts are comparable and zero appearances is no longer a separate case. Missing calendar months are never joined into a pattern. Reference counts use pooled following-month appearances / draws × planned next-month draws. Fewer than three matches are marked **樣本少**. Overlapping examples are not independent samples. No number is "due" because it was absent; partial or missing records can distort these references.

**未來期數回測與校準** is collapsed under the monthly table and reports a walk-forward backtest of the same model. After 40 warmup draws, anchors advance by N draws, so their next-N-draw test windows do not overlap. Each forecast uses only information available at its anchor, selects the ten highest estimates, and counts how many selected numbers appear at least once in the test window. At least ten complete windows are required (`40 + 10 × N` loaded draws). It also reports the Brier score versus a constant baseline (46.0% for N = 4), predicted-versus-observed rates in three probability buckets, and lag 1–4 re-hit rates with pair counts. These are descriptive comparisons, not significance tests: training histories still overlap and numbers within a draw are dependent, so sigma/z-based advantage claims are not shown. With 250 draws, N = 4 provides 52 test windows. More history adds evaluation samples and completed-gap information, but does not expand the forecast's fixed latest-30-draw frequency window or establish predictive accuracy. The backtest only uses loaded draws and follows the horizon selector.

**未來N期預估 ≥** in the monthly toolbar filters the table by the **未來N期** estimate, so the number you type matches the figure shown in that column. Leave it blank (placeholder **全部**) to show all 49 numbers; blank, cleared, or unreadable input falls back to the full table. The value is an absolute appearance count and stays fixed when **短期預測期數** changes, so re-check it after switching horizon; **基準** fills in the random baseline 7/49 × N rounded to two decimals. The status line under the toolbar reports the visible row count, the threshold, and the baseline, and the footer keeps total appearances for all 49 numbers. Filtering only re-renders the table, so the backtest and charts are not recomputed. A threshold above the highest estimate leaves one explanatory row instead of an empty table.

**加入篩選至 watchlist** adds exactly the numbers currently on screen and skips ones already saved, reporting how many were added and skipped. It stays disabled while the filter is blank or matches nothing, so a single click cannot add all 49 numbers. The filter only reads back the model's own ordering; it does not demonstrate that a filtered set is a better bet than the random baseline.

Click a monthly row to highlight it across all columns; click it again to clear, or another row to move the highlight. A focused row also supports Enter or Space. Clicking its number highlights the row and opens the chart without toggling the highlight off. Highlighting preserves horizontal scroll and survives a projection draw-count change.

Use **Add to watchlist** in the number popup to save a number; the same button changes to **Remove from watchlist** when it is saved. Alternatively, click **+** at the end of a monthly row to add its number, or **−** to remove it. Table buttons, popup controls, and the saved list stay synchronized without changing row highlights or horizontal scroll. Above the monthly table, saved numbers appear in ascending order with individual remove buttons, **Clear watchlist**, and **Copy watchlist**. Copying produces only unique numbers separated by commas, for example `1,8,11`, with no spaces or labels. A read-only text field remains available for manual copying if clipboard access fails. The watchlist is separate from row highlighting, survives result/projection reloads, and is saved in this browser using local storage when available. It is not synced between devices; blocked storage still allows an in-memory list and copying.

The expandable popup evidence includes **同次數**, **近2月方向**, and **近3月形態**. Same-count references find earlier months with the latest month's raw appearance count, not necessarily true peaks or equivalent exposure. Two-month direction references match the latest rise, fall, or flat movement; three-month shape references match both directions in the latest three-month run. Directions use unrounded appearances per draw, without requiring identical counts, so equal raw counts in months with different draw counts may have different directions. All show match counts, following-month rate rises / flats / falls, and exposure-adjusted reference counts using the planned next-month draws. Each line also prints its own sample count and the near-half rise/fall baseline, so one matching example is not read as a direction.

Inside **查看歷史依據** below the popup chart, each reference lists the actual source months and the next one or two recorded monthly outcomes with draw counts. Gaps are never bridged, unavailable outcomes are not inferred, and only earlier references with a known next month are counted. Fewer than three examples are marked **樣本少**. These broader comparisons can disagree; they are not independent evidence or an ensemble forecast, and no historical rise or decline guarantees a repeat. No reference line feeds a summary sentence any more; each one exists only to show what the loaded history contains.

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

### 縮水注項

Enter a pool as comma-separated numbers from 1–49. Chinese commas and extra spaces are accepted. Duplicates are removed. At least six distinct numbers are required; the pool is not limited to 12. Leave **最多注數** blank to cover every possible 6-number draw inside that pool, or set a cap to stop earlier.

Each generated line is six of the entered numbers. A line covers a possible draw when they share at least five numbers. The status shows the number of possible draws, C(n, 6), the counting lower bound ceil(C(n, 6) / (1 + 6(n − 6))), the line count, and the exact coverage. At 100% coverage, if all six main numbers fall inside the pool, at least one line matches five or more of them. The special number is not part of this guarantee. Partial coverage does not make that promise for the uncovered draws.

Pools of up to 22 numbers use the greedy maximum-coverage search. Larger pools use a direct covering scan, which can need several times the lower bound. Neither search certifies a minimum wheel. A blank cap on a large pool can produce tens of thousands of lines; set **最多注數** to cover only part of the draws. The on-screen list shows the first 80 lines once the result exceeds 4,000 lines. **複製全部注項** and **下載 txt** use the full list, one comma-separated line per bet. Editing the pool or the cap clears the result. This is a conditional cover, not a next-draw probability and not an increase in the expected value of a ticket.

### 17注包49

**隨機生成** builds 17 bets of six numbers from 1–49. Every number appears at least once. Click again to reshuffle.

- Lines 1–8 are eight disjoint bets from 48 numbers. One number is left out.
- Line 9 is that leftover plus five numbers drawn at random from the 48.
- Lines 10–16 do not use those five. They are seven disjoint bets from 42 of the remaining 44, so two numbers are left out.
- Line 17 is those two leftovers plus four numbers drawn at random from the other 47. Those four may include the five used on line 9.

Lines 1–8 do not repeat a number. Lines 10–16 do not repeat a number and do not use line 9's five. Lines 9 and 17 may reuse numbers from earlier lines. The status names the leftover and the random fillers. Each line is also listed with its role, and the numbers can be clicked for the same statistics popover. **複製全部注項** and **下載 txt** contain only the 17 comma-separated lines, with no labels. This is not a prize guarantee and not a next-draw probability.

### 範圍包號

A separate generator. It does not change **17注包49**. Enter a range such as `1-30,41-49`, or leave it blank for 1–49. Chinese commas are accepted. Overlapping ranges are merged. Leave **注數** blank to use about `round(numbers / 2.6)`, or type another count from 1 to 60.

Each line is six different numbers from the pool. Every pool number appears at least once, and a number may appear on more than one line.

- Each round makes `floor(|W| / 6)` disjoint lines from the current working set and leaves `|W| % 6` numbers out. The first working set is the full pool.
- Each leftover then forms its own line with five distinct numbers from the full pool minus those leftovers. The next working set is the full pool minus the leftovers and those fillers.
- If nothing is left over, the next round starts again from the full pool. For 30 numbers and 15 bets, that is five disjoint lines, then another pass over all 30. A ticket is not repeated.
- If there is a remainder, later rounds use only the shrunk set. For 20 numbers, three disjoint lines leave 2, two seed lines use 10 fillers, and 8 numbers remain. Those 8 make one line and leave 2, then two more seed lines use 10 fillers from the other 18. For 34 numbers, five disjoint lines leave 4, four seed lines use 20 fillers, and 10 numbers remain. Those 10 make one line and leave 4, then four more seed lines. Repeat until the requested count.
- If the next set has fewer than six numbers, generation restarts from the full pool. If no new ticket can be made, generation stops with an error.

The hint shows the pool size, the minimum count for this pattern, and the suggested count. Generation rejects a pool smaller than six numbers, a leftover step that cannot find enough distinct fillers, fewer slots than numbers, and a count below the first cycle. For example, 39 numbers and 5 draws fail because 30 slots cannot hold 39 numbers, and 10 numbers fail because four leftovers would need 20 distinct fillers from only six numbers. **複製全部注項** and **下載 txt** contain only the comma-separated lines, with no labels. This is not a prize guarantee and not a next-draw probability.
