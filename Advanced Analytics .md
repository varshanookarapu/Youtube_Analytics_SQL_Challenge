## 🟢 Advanced Analytics (40–50)

**Question 40:** Compute engagement score = (likes + comments + clicks) / impressions.

```sql
--engagement score = likes + comments + clicks / impressions

WITH video_engagement AS
(
SELECT dv.video_id ,  
SUM(CASE WHEN likes IS NULL THEN 0 ELSE likes END) as likes,
SUM(clicks) as clicks  , SUM(impressions) as impressions,COUNT(comment_id) as comments  
FROM Youtube.daily_views dv 
LEFT JOIN Youtube.comments c ON dv.video_id = c.video_id
LEFT JOIN  Youtube.likes_dislikes  ld ON dv.video_id = ld.video_id
GROUP BY dv.video_id 
ORDER BY dv.video_id
)


SELECT * , ROUND((likes+comments+clicks)::NUMERIC/impressions,2) AS engagement_score FROM video_engagement
```
<img width="1654" height="577" alt="image" src="https://github.com/user-attachments/assets/552de32d-7f5b-4966-bb17-8274edf30363" />

---

**Question 41:** Detect anomaly days using z-score on daily views.
```sql
--A z-score tells you how far away something is from the average, measured in standard deviations
WITH views_stats AS
(
SELECT video_id, AVG(views) as average_views, STDDEV(views) as standard_deviation_views
FROM daily_views
GROUP BY video_id
ORDER BY video_id
)


SELECT dv.video_id,view_date,views , CASE WHEN standard_deviation_views  IS NULL THEN 0 ELSE 
ROUND((views - average_views)/standard_deviation_views,2) END as z_score

FROM daily_views dv JOIN views_stats vs ON
dv.video_id = vs.video_id
ORDER BY video_id,view_date
```
<img width="1891" height="617" alt="image" src="https://github.com/user-attachments/assets/f63dc19a-b70a-4ef3-9975-bdce8f2f5db0" />

---
**Question 42:** Creator retention: % of videos still getting views after 90 days.
```sql
```
---
**Question 43:** Find videos causing highest drop-off (low avg_view_duration vs duration).
```sql
```
---
**Question 44:** Creator lifetime value = total revenue / number of videos.
```sql
```
---
**Question 45:** Segment videos by length and analyze CTR per segment.
```sql
```
---
**Question 46:** Identify top comment contributors.
```sql
```
---
**Question 47:** Videos with >20k impressions but no revenue.
```sql
```
---
**Question 48:** Monthly revenue pivot by category.
```sql
```
---
**Question 49:** Funnel analysis: impressions → clicks → views → watch_time.
```sql
```
---
**Question 50:** Detect inconsistent data across daily metrics tables.
```sql
```
---
