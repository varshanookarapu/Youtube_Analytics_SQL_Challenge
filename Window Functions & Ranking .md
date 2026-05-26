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
SELECT video_id,views, SUM(views) OVER(PARTITION BY video_id ORDER BY view_date) as running_total_of_views ,view_date
FROM Youtube.daily_views 
ORDER BY video_id,view_date
```
<img width="1890" height="657" alt="image" src="https://github.com/user-attachments/assets/3c4c8ffd-6cd6-472b-bf60-80c86b343ec2" />

---
**Question 33:** Monthly growth rate of views for each video (LAG).
```sql

WITH daily_views_cte AS
(
SELECT video_id,EXTRACT(MONTH FROM view_date) as month ,views, LAG(views) OVER(PARTITION BY video_id ORDER BY EXTRACT(MONTH FROM view_date)) as previous_month_views
FROM Youtube.daily_views 
ORDER BY video_id
) 

SELECT * ,
 100*(views-previous_month_views)/previous_month_views as growth_rate_of_views
FROM daily_views_cte

```
<img width="1324" height="350" alt="image" src="https://github.com/user-attachments/assets/374e7e3c-c25f-4c5b-8b92-11763f477a43" />

---
**Question 34:** Top 3 videos per month (partition + ranking).
```sql
WITH cte AS 
(
SELECT  EXTRACT(MONTH FROM view_date) as month , v.video_id, views 
FROM Youtube.videos v
INNER JOIN  Youtube.daily_views dv ON
v.video_id = dv.video_id
ORDER BY month, video_id
),

ranked_videos AS
(
  
SELECT  * , RANK() OVER(PARTITION BY month ORDER BY views DESC) as rank
FROM cte
)

SELECT * FROM ranked_videos WHERE rank <=3
```
<img width="1895" height="658" alt="image" src="https://github.com/user-attachments/assets/b3df5875-3a2d-419b-b05a-30b3541eea57" />

---
**Question 35:** Percentile of views for each video (NTILE).
```sql
--NTILE(n) splits ordered rows into n buckets (groups) as evenly as possible.
--NTILE(10) OVER(PARTITION BY video_id ORDER BY views)

--means:

--For each video_id
--Sort rows by views
--Divide the rows into 10 groups
--Assign bucket numbers: 1 → 10
--Mathematically, it behaves approximately like:
--bucket=⌈row number×n/total rows⌉
--Where:
--n = number of buckets
--row number = position after sorting
--total rows = rows in the partition

SELECT video_id, views,
ntile(10) OVER(PARTITION BY video_id ORDER BY views) as percentile
FROM daily_views 
ORDER BY video_id


```
<img width="1539" height="356" alt="image" src="https://github.com/user-attachments/assets/a4118076-de8d-4f93-9734-9a23338fe3d6" />

---
**Question 36:** Lead/Lag to calculate day-over-day % change.
```sql

--day-over-day-percentage-growth = [ current_views - pervious_day_views / previous_day_views ] * 100

with views AS
(
SELECT video_id, view_date,views,
LAG(views) OVER(PARTITION BY video_id ORDER BY view_date) as previous_day_views,
LEAD(views) OVER(PARTITION BY video_id ORDER BY view_date) as next_day_views
FROM daily_views
ORDER BY video_id,view_date
)


SELECT * , 100*(views-previous_day_views)/previous_day_views as day_over_day_percentage_change
FROM views

```
<img width="1891" height="784" alt="image" src="https://github.com/user-attachments/assets/81a7126e-e264-4a70-886a-cd4d76f182c1" />

---
**Question 37:** Cumulative watch time per creator.
```sql
```
---
**Question 38:** Rank creators by average watch time per video.
```sql
WITH watchtime AS
(
SELECT  creator_id,v.video_id, SUM(avg_view_duration_seconds) as total_avg_view_duration_seconds
FROM
Youtube.videos v 
LEFT JOIN  Youtube.daily_views dv ON v.video_id = dv.video_id
GROUP BY creator_id,v.video_id
ORDER BY creator_id,v.video_id
)

SELECT *, RANK() OVER(PARTITION BY creator_id ORDER BY total_avg_view_duration_seconds DESC) as rank 
FROM watchtime
```
<img width="1887" height="740" alt="image" src="https://github.com/user-attachments/assets/f41d08bf-1514-4b3e-a3d8-2734cf2487b0" />

---
**Question 39:** Use ROW_NUMBER to deduplicate and get latest daily stats.
```sql
```
---
