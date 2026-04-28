## Window Functions & Ranking (31–39)

**Question 31:** Rank videos by total views within each category (RANK / DENSE_RANK).
```sql
WITH cte As
(
SELECT  category,v.video_id, SUM(views) as total_views FROM videos v 
LEFT JOIN daily_views dv ON
v.video_id = dv.video_id
GROUP BY category,v.video_id
ORDER BY category,v.video_id
)
-- Rank -when there are ties it assigns the same numbers but skips the next rank .
SELECT * , RANK() OVER(PARTITION BY category ORDER BY total_views DESC) as rank FROM cte 
WHERE total_views IS NOT NULL
-- Dense Rank - when there are ties it assigns the same numbers but does not skip rank 
SELECT * , DENSE_RANK() OVER(PARTITION BY category ORDER BY total_views DESC) as rank FROM cte 
WHERE total_views IS NOT NULL
```

<img width="1896" height="748" alt="image" src="https://github.com/user-attachments/assets/c04fb4f8-2566-42e5-b2e1-c818777f69ab" />

---
**Question 32:** Running total of views per video (window function).
```sql
```
---
**Question 33:** Monthly growth rate of views for each video (LAG).
```sql
```
---
**Question 34:** Top 3 videos per month (partition + ranking).
```sql
```
---
**Question 35:** Percentile of views for each video (NTILE).
```sql
```
---
**Question 36:** Lead/Lag to calculate day-over-day % change.
```sql
```
---
**Question 37:** Cumulative watch time per creator.
```sql
```
---
**Question 38:** Rank creators by average watch time per video.
```sql
```
---
**Question 39:** Use ROW_NUMBER to deduplicate and get latest daily stats.
```sql
```
---
