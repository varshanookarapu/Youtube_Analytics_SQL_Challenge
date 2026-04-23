## 🔵 Joins & Multi-table (21–30)

**Question 21:** For each creator, total revenue (ad + subscription + other).

```sql
SELECT v.creator_id, creator_name, SUM(ad_revenue+subscription_revenue+other_revenue) AS total_revenue 
FROM videos v LEFT JOIN
revenue r ON
v.video_id = r.video_id
LEFT JOIN creators c ON
v.creator_id = c.creator_id
GROUP BY creator_name,v.creator_id
ORDER BY v.creator_id
```
<img width="1370" height="351" alt="image" src="https://github.com/user-attachments/assets/993290ab-e153-4f24-9501-d59473110812" />

---

**Question 22:** For each video, last 7-day rolling average views.
```sql
```
---
**Question 23:** Top performing video per creator by revenue.
```sql
```
---
**Question 24:** Video comment sentiment breakdown (positive / neutral / negative).
```sql
```
---
**Question 25:** Videos with impressions but 0 clicks (possible data issue).
```sql
```
---
**Question 26:** List videos and their peak daily views date.
```sql
```
---
**Question 27:** Show creators who published more than 10 videos.
```sql
```
---
**Question 28:** Videos with multiple high-spike days.
```sql
```
---
**Question 29:** Find videos with payments revenue but zero views (data mismatch).
```sql
```
---
**Question 30:** Creator-wise average CTR.
```sql
```
---

