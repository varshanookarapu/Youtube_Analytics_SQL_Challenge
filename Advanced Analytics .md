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
WITH video_duration AS
(
SELECT dv.video_id, title,duration_seconds, AVG(avg_view_duration_seconds) as avg_watch_time
FROM daily_views dv LEFT JOIN videos v ON
dv.video_id = v.video_id
GROUP BY dv.video_id ,title ,duration_seconds
ORDER BY dv.video_id
) 

SELECT *, (avg_watch_time / duration_seconds)*100 as drop_off_ratio 
 FROM video_duration
```

<img width="1786" height="605" alt="image" src="https://github.com/user-attachments/assets/3a57ace7-bdb7-48db-8789-c9d97b959131" />

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
-- we calcualted avg ctr for every segment 
WITH duration_bins  AS 
(

  SELECT video_id, 
  CASE WHEN duration_seconds < 300 THEN  'Short_video_<5min'
  WHEN duration_seconds < 1200 THEN 'Medium_video_5_to_20_min'
  WHEN duration_seconds > 1200 THEN 'Long_video'
  END as video_duration_bin
  FROM videos
),

ctr AS (
SELECT dv.video_id, SUM(impressions) as total_impressions ,SUM(clicks) as total_clicks
FROM daily_views dv 
GROUP BY dv.video_id 
ORDER BY dv.video_id  
)  



SELECT video_duration_bin, 
AVG(CASE WHEN total_impressions=0 THEN 0 ELSE total_clicks/total_impressions :: NUMERIC  END ) AS Avg_click_through_rate  
FROM duration_bins db
LEFT JOIN ctr ON db.video_id = ctr.video_id
GROUP BY video_duration_bin
ORDER BY Avg_click_through_rate DESC
```
<img width="1195" height="235" alt="image" src="https://github.com/user-attachments/assets/39b07dd8-2cd8-49f9-9dd3-d49ea0f0ad44" />

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
SELECT 
category ,

SUM( ad_revenue+subscription_revenue+other_revenue) 
FILTER (WHERE EXTRACT( YEAR FROM revenue_date) || '-' || EXTRACT( MONTH FROM revenue_date) = '2023-1' ) as Jan_Revenue_2023,
SUM( ad_revenue+subscription_revenue+other_revenue) 
FILTER (WHERE EXTRACT( YEAR FROM revenue_date) || '-' || EXTRACT( MONTH FROM revenue_date) = '2023-2' ) as Feb_Revenue_2023,
SUM( ad_revenue+subscription_revenue+other_revenue) 
FILTER (WHERE EXTRACT( YEAR FROM revenue_date) || '-' || EXTRACT( MONTH FROM revenue_date) = '2023-3' ) as Mar_Revenue_2023,
SUM( ad_revenue+subscription_revenue+other_revenue) 
FILTER (WHERE EXTRACT( YEAR FROM revenue_date) || '-' || EXTRACT( MONTH FROM revenue_date) = '2023-4' ) as Apr_Revenue_2023


FROM videos v 
LEFT JOIN revenue r 
ON v.video_id = r.video_id
WHERE revenue_date IS NOT NULL
GROUP BY category  
ORDER BY category
 
```
<img width="1825" height="386" alt="image" src="https://github.com/user-attachments/assets/bbd78fdb-3054-4ebe-938f-2699f7aef477" />


---
**Question 49:** Funnel analysis: impressions → clicks → views → watch_time.
```sql
SELECT video_id, SUM(views) as total_views, SUM(clicks) as total_clicks, SUM(impressions) as total_impressions,SUM(watch_time_seconds) as total_watch_time_seconds, 
CASE WHEN  SUM(impressions) = 0 THEN 0 ELSE ( SUM(clicks)*1.0 /SUM(impressions) ) END as ctr ,
CASE WHEN SUM(clicks)= 0 THEN 0 ELSE SUM(views)*1.0/SUM(clicks)  END as click_to_view
FROM daily_views
GROUP BY video_id
ORDER BY video_id
LIMIT 10

-- Interpretation 

High CTR + High Click-to-View → Attractive and delivers what users expect.
High CTR + Low Click-to-View → Clickbait risk or mismatch between expectation and content.
Low CTR + High Click-to-View → Content is valuable, but not attracting enough clicks.
Low CTR + Low Click-to-View → Problems with both discovery and engagement.

-- For instance "Video 1001 had a CTR of 6.83%, which means that out of 54,240 times the video thumbnail or link was shown to users, about 6.83% resulted in a click. In absolute terms, 3,702 clicks were generated from 54,240 impressions. This indicates the thumbnail, title, or placement was reasonably effective at attracting user interest."
```
<img width="1875" height="534" alt="image" src="https://github.com/user-attachments/assets/1f720d3a-609d-4639-a94b-458cb953a157" />

---
**Question 50:** Detect inconsistent data across daily metrics tables.
```sql
```
---
