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
WITH creator_stats AS
(
SELECT creator_id,COUNT(DISTINCT v.video_id) as number_of_videos,SUM(ad_revenue + subscription_revenue + other_revenue) as total_revenue  FROM Youtube.videos v 
LEFT JOIN revenue r ON
v.video_id =r.video_id
GROUP BY creator_id
ORDER BY creator_id
)

SELECT creator_id ,number_of_videos,total_revenue, ROUND(total_revenue/number_of_videos,2) as revenue_per_video FROM creator_stats
```
<img width="1875" height="524" alt="image" src="https://github.com/user-attachments/assets/ee91d633-8b9e-4b04-9516-5abf2e30c0c1" />

---
**Question 45:** Segment videos by length and analyze CTR per segment.
```sql
```
---
**Question 46:** Identify top comment contributors.
```sql
SELECT commenter_name,COUNT(DISTINCT dv.video_id ) as total_videos_commented 
FROM  daily_views dv 
LEFT JOIN comments c ON dv.video_id = c.video_id
WHERE commenter_name IS NOT NULL
GROUP BY commenter_name
ORDER BY total_videos_commented DESC
```
<img width="1157" height="624" alt="image" src="https://github.com/user-attachments/assets/6f5cec19-bc44-4652-9f80-86daca4d5421" />

---
**Question 47:** Videos with >20k impressions but no revenue.
```sql
SELECT dv.video_id, SUM(impressions) as total_impressions ,  SUM( ad_revenue + subscription_revenue+other_revenue) as total_revenue  FROM Youtube.daily_views dv 
LEFT JOIN revenue r ON dv.video_id = r.video_id 
GROUP BY dv.video_id 
HAVING SUM(impressions) > 20000 AND SUM( ad_revenue + subscription_revenue+other_revenue) IS NULL 
ORDER BY dv.video_id

```
<img width="1899" height="746" alt="image" src="https://github.com/user-attachments/assets/d29eccb4-1754-4ad2-971b-66890dad4a31" />

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
