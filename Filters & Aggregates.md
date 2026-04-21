## 🟡 Filters & Aggregates (11–20)
---
11. Total impressions and clicks per video.
 ```sql
 SELECT v.video_id, SUM(impressions) as total_impressions , SUM(clicks) as total_clicks  FROM videos v 
LEFT JOIN daily_views dv 
ON v.video_id = dv.video_id
GROUP BY v.video_id
ORDER BY v.video_id
 ```
<img width="1578" height="775" alt="image" src="https://github.com/user-attachments/assets/312ee835-bbaf-47a5-970e-4ded1a4aef32" />

---
12. Compute CTR = clicks / impressions per day.
 ```sql
 ```
---
13. Average watch time per view (watch_time_seconds / views).
```sql
 ```
---
14. Daily views trend for a single video.

```sql
 ```
---
15. Views per category.  
```sql
 ```
---

16. Top 5 videos by watch_time_seconds.  
```sql
 ```
---

17. Average likes/dislikes per video.  
```sql
 ```
---

18. List videos with more dislikes than likes.  
```sql
 ```
---

19. Videos where avg_view_duration < 20% of duration.

```sql
 ```
---
20. Videos that gained more than 1k views in a day (spikes).

```sql
 ```
---
