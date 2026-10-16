# Mark Six Checker

Static pages for checking Hong Kong Mark Six results. Draw history lives in [results.json](results.json): newest first, each row is a date, six main numbers, and one special number. Open [estimation.html](estimation.html) directly, or host the folder on GitHub Pages. No build step is required.

Load results with **從網址讀取**, a local `.json` file, or pasted JSON. The file and pasted text stay in the browser. The special number is counted together with the six main numbers unless a section says otherwise. Counts, gaps, and highlights are descriptions of past draws, not a next-draw probability.

## Estimation page

[estimation.html](estimation.html) has six sections.

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

### 號碼組合

Two-number and three-number combinations in the latest 30 draws, including the special number. Filter by one or more required numbers and a minimum number of times they appeared together. Results are paged 100 rows at a time. Click a number for the same statistics popover.

### 隨機分組

Load at least 30 draws, then choose **3組（16、16、17）** or **4組（12、12、12、13）** and click **隨機生成**.

- Every number from 1 to 49 appears exactly once across the groups, with no duplicates within or between groups.
- Each group contains cold, normal, and hot numbers using the latest 30 draws, including special numbers: 0–3 cold, 4–5 normal, and 6+ hot. Category counts are spread as evenly as group capacity allows.
- Generation stops with an explanation if any category has fewer numbers than the chosen number of groups. The category requirements are not silently relaxed.
- Each group shows its size, category counts, and colour-coded numbers in ascending order. Tap or click a number for the same statistics popover, including its trend score, recent counts, activity, and verdict. Viewing statistics does not change the generated groups.
- **複製第N組** copies only that group's numbers, separated by commas, without a heading. A read-only text field also contains the comma-separated list; if automatic copying fails, it is selected for manual copying.
- Generate again to reshuffle. Changing the group count or loading new results clears the generated lists and any open number popover.

Random grouping is a way to organise the numbers, not a prediction of future draws.
