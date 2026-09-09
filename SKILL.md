---
name: performance-review
description: Analyzes a Facebook post insights screenshot. Extracts metrics, calculates rates, velocity, and plateau status. Projects final view count. Auto-saves snapshot to benchmarks file. Add "log this post" to finalize a finished post.
allowed-tools: Read, Edit
argument-hint: [attach screenshot. Add "log this post" to mark it as final.]
---

# Facebook Page Performance Review

Read the screenshot, load benchmark history, calculate all metrics, save the snapshot, and output the report.

---

## Step 1 — Extract from Screenshot

Pull every visible number. Note N/A if not shown.

| Field | Where to find it |
|---|---|
| Views | Overview or Views section |
| Viewers | Overview |
| Reactions | Interactions breakdown |
| Comments | Interactions breakdown |
| Shares | Interactions breakdown |
| Saves | Interactions breakdown (not always visible) |
| Follows | Overview or Follows widget |
| Non-followers % | Followers vs. Non-followers chart |
| Post age | "X hours/days ago" or Published timestamp — convert to hours |
| Curve shape | Views over time graph — describe the shape |

---

## Step 2 — Calculate Rates

- **Share rate** = shares ÷ views × 100 (%)
- **Save rate** = saves ÷ views × 100 (%) — only if saves visible
- **Follow rate** = follows ÷ views × 100 (%)
- **Engagement rate** = (reactions + comments + shares) ÷ views × 100 (%)

---

## Step 3 — Load Benchmarks and Snapshot History

Read `./data/content/post-benchmarks.json`.

1. Find the post with `"status": "active"` — this is the post being tracked. If multiple active posts, match by topic visible in the screenshot.
2. Load its `snapshots` array — the history of all prior check-ins for this post.
3. Load posts with `"status": "final"` for benchmark comparison.
4. Load `thresholds` for rate interpretation.

Key thresholds:
- Share rate 0.10% = minimum for continued distribution
- Share rate 0.15% = outperforming average
- Share rate 0.20%+ = exceptional — algorithm pushes to strangers at scale
- Follow rate 0.10% = good; 0.15%+ = strong
- Non-followers % climbing toward 95%+ = algorithm actively expanding reach to strangers

---

## Step 4 — Calculate Velocity

Using the current screenshot data and the most recent entry in the `snapshots` array:

```
velocity_current = (current_views − last_snapshot_views) ÷ (current_hours − last_snapshot_hours)
velocity_peak = highest velocity_views_per_hr across all snapshots
```

Classify velocity trend:
- **Accelerating** — current > 120% of prior check
- **Holding** — within ±20% of prior check
- **Decelerating** — current < 80% of prior check
- **No prior snapshots** — calculate avg velocity as current_views ÷ current_hours and note it's a baseline reading

---

## Step 5 — Project Final View Count

Estimate final views based on share rate and current trajectory:

| Share rate | Curve status | Projected range |
|---|---|---|
| ≥ 0.20% | Climbing (< 24h) | 150K–300K+ |
| ≥ 0.20% | Tapering | 130K–200K |
| 0.15–0.20% | Climbing | 100K–180K |
| 0.15–0.20% | Tapering | 80K–150K |
| 0.10–0.15% | Any | 50K–120K |
| < 0.10% | Any | 20K–60K |

Adjust the range upward if the second geographic wave has not yet arrived (post < 9h old and audience spans two time zones).
Adjust the range downward if velocity is sharply decelerating and non-follower % growth has stalled.

---

## Step 6 — Plateau Detection

| Growth status | Criteria |
|---|---|
| Still distributing | Velocity > 30% of peak OR non-follower % still growing OR post < 12h old |
| Tapering | Velocity 10–30% of peak AND non-follower % flat AND post > 24h |
| Plateauing | Velocity < 10% of peak AND post > 36h — ready to log as final |
| Mature | Flat for 12h+ — log as final immediately |

---

## Step 7 — Geographic Wave Context

Posts published 9–11 AM in the primary audience's timezone often follow a two-peak pattern:
- Peak 1 (hours 1–9): first audience timezone, daytime scroll
- Trough (~hours 6–8 from publish): velocity drops — NOT the post dying
- Peak 2 (hours 9–21): second audience timezone waking up — often stronger than Peak 1

Do not classify as Tapering during the trough. Check again at 21h.

**Author reply signal:** A substantive author reply in comments during hours 6–9 can trigger a velocity spike (~3x observed). Note if an author reply is visible in the screenshot.

**Recommended check-in cadence:** 3h · 6h · 9h · 11h · 21h · 25h · 36h

---

## Step 8 — Auto-Save Snapshot

After every review, append the current reading to the active post's `snapshots` array in `post-benchmarks.json` using the Edit tool.

Before saving, check the last entry in `snapshots`: if its `hours` value is within 1 hour of the current post age, skip the save to avoid duplicates.

Append this structure:

```json
{
  "hours": [post age in hours — integer],
  "views": [current views],
  "shares": [current shares],
  "follows": [current follows or null if not visible],
  "non_follower_pct": [current % or null],
  "velocity_views_per_hr": [calculated velocity — round to nearest integer]
}
```

---

## Step 9 — Log-Post Mode (triggered by user)

Triggered when the user says: "log this post", "finalize", "bài này xong rồi", or similar phrasing.

Update the active post in `post-benchmarks.json` using Edit:
1. Set `"status"` to `"final"`
2. Set `"final_at_hours"` to current post age
3. Fill in all final stat fields (views, shares, saves, follows, non_follower_pct) and all rate fields
4. Append final snapshot to snapshots array
5. Confirm in output: "✓ [topic] logged as final in post-benchmarks.json"

---

## Step 10 — Output

Toàn bộ kết quả trả về bằng tiếng Việt. Dùng cấu trúc sau, không thêm bớt:

---

### BÀI VIẾT
[Tên bài viết và ngày đăng]

### THỐNG KÊ — [số giờ]h
| Chỉ số | Giá trị |
|---|---|
| Lượt xem | |
| Lượt chia sẻ | |
| Lượt lưu | |
| Lượt theo dõi mới | |
| Tỷ lệ người chưa theo dõi | |
| Đường cong | |

### TỐC ĐỘ
| | Giá trị |
|---|---|
| Hiện tại | X.XXX lượt xem/giờ |
| Lần kiểm tra trước | X.XXX lượt xem/giờ (giờ thứ X) |
| Trạng thái | Đang tăng tốc / Ổn định / Đang giảm tốc |

### TỶ LỆ
| Chỉ số | Bài này | Bài hoàn chỉnh gần nhất | Xu hướng |
|---|---|---|---|
| Tỷ lệ chia sẻ | | | |
| Tỷ lệ lưu | | | |
| Tỷ lệ theo dõi mới | | | |

### DỰ ĐOÁN
[Ước tính phạm vi lượt xem cuối cùng và lý do ngắn gọn]

### TRẠNG THÁI PHÂN PHỐI
[Đang phân phối / Đang giảm dần / Đạt ngưỡng / Hoàn thành — một câu giải thích]

### SO VỚI BÀI CŨ
Một câu so sánh bài này với bài hoàn chỉnh gần nhất.

### NHẬN ĐỊNH
🟢 Tốt / 🟡 Theo dõi / 🔴 Cần chú ý — một câu giải thích lý do.

### HÀNH ĐỘNG TIẾP THEO
Một việc cụ thể cần làm ngay — hoặc xác nhận đã tự động lưu thống kê.

---

## Tiêu chí nhận định

🟢 **Tốt** — đường cong vẫn tăng HOẶC tỷ lệ chia sẻ trên 0.15% HOẶC lượt theo dõi vượt các bài trước
🟡 **Theo dõi** — bài đang ở vùng lặng giữa hai đợt; hoặc tỷ lệ chia sẻ 0.10–0.15% — kiểm tra lại ở giờ thứ 21
🔴 **Cần chú ý** — giảm trước giờ thứ 12, tỷ lệ chia sẻ dưới 0.05%, dưới 30 lượt theo dõi sau 24 giờ
