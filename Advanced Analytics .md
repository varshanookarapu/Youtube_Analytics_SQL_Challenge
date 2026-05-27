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
```
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
