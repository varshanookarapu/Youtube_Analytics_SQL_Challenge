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
 SELECT view_date,SUM(clicks) as total_clicks_per_day ,SUM(impressions) as total_impressions_per_day ,
(CASE WHEN SUM(impressions)=0 THEN 0 ELSE (SUM(clicks)*100/SUM(impressions)) ::NUMERIC  END ) AS CTR_click_through_rate  
FROM videos v 
LEFT JOIN daily_views dv 
ON v.video_id = dv.video_id
WHERE view_date IS NOT NULL
GROUP BY view_date
ORDER BY view_date

 ```
 <img width="1770" height="745" alt="image" src="https://github.com/user-attachments/assets/928c32b4-ef97-489a-a084-5e8de60be327" />

---
13. Average watch time per view (watch_time_seconds / views).
```sql
SELECT  v.video_id , ROUND(SUM(watch_time_seconds)/SUM(views)) :: NUMERIC as watch_time_per_view FROM  videos v LEFT JOIN daily_views dv ON
v.video_id = dv.video_id
GROUP BY v.video_id
ORDER BY v.video_id
 ```
 <img width="1002" height="882" alt="image" src="https://github.com/user-attachments/assets/8577d9bc-58bf-435e-92e6-1132107791a6" />

---
14. Daily views trend for a single video.

```sql
SELECT  v.video_id , view_date,views , RANK() OVER(PARTITION BY v.video_id ORDER BY views DESC) as rank

FROM  videos v LEFT JOIN daily_views dv ON
v.video_id = dv.video_id
ORDER BY v.video_id ,view_date
 ```
 <img width="1780" height="808" alt="image" src="https://github.com/user-attachments/assets/9768d0a0-e194-4bf4-8047-6f60ba21ff7a" />

---
15. Views per category.  
```sql
-- per video
SELECT  v.video_id , category,SUM(views) as total_views_per_category
FROM  videos v LEFT JOIN daily_views dv ON
v.video_id = dv.video_id
GROUP BY v.video_id
ORDER BY v.video_id

-- per category
SELECT   category,SUM(views) as total_views_per_category
FROM  videos v LEFT JOIN daily_views dv ON
v.video_id = dv.video_id
GROUP BY category
ORDER BY category 
 ```
 <img width="1563" height="830" alt="image" src="https://github.com/user-attachments/assets/9733ec33-4694-439a-8216-ed63f858826d" />
<img width="1119" height="428" alt="image" src="https://github.com/user-attachments/assets/90ef6a1a-7766-4474-b0eb-b5233a6618e9" />

---

16. Top 5 videos by watch_time_seconds.  
```sql
SELECT  v.video_id , title, SUM(CASE WHEN  watch_time_seconds IS NULL THEN 0 ELSE watch_time_seconds END) as total_watch_time_seconds
FROM  videos v LEFT JOIN daily_views dv ON
v.video_id = dv.video_id
GROUP BY v.video_id ,title 
ORDER BY total_watch_time_seconds DESC
LIMIT 5
 ```
 <img width="1306" height="376" alt="image" src="https://github.com/user-attachments/assets/baf45c0c-f381-4589-bf67-e8378b096169" />

---

17. Average likes/dislikes per video.  
```sql
 SELECT  v.video_id , SUM(CASE WHEN likes IS NULL THEN 0 ELSE likes END)/COUNT(*)  as average_likes,SUM(CASE WHEN dislikes IS NULL THEN 0 ELSE dislikes END )/COUNT(*) as average_dislikes
      FROM  videos v LEFT JOIN likes_dislikes ld ON
      v.video_id = ld.video_id 
      GROUP BY v.video_id
      ORDER BY v.video_id
      
 ```
 <img width="1756" height="722" alt="image" src="https://github.com/user-attachments/assets/9416b677-ceb3-4694-a46f-5b8c7786d314" />

---

18. List videos with more dislikes than likes.  
```sql
   SELECT  v.video_id ,likes,dislikes 
      FROM  videos v LEFT JOIN likes_dislikes ld ON
      v.video_id = ld.video_id 
      WHERE dislikes>likes
      ORDER BY v.video_id
 ```
 <img width="1618" height="160" alt="image" src="https://github.com/user-attachments/assets/870a5105-49a8-4e38-8ffb-6c4519771e3a" />

---

19. Videos where avg_view_duration < 20% of duration.

```sql


SELECT video_id, SUM(avg_view_duration_seconds) avg_view_duration_seconds FROM daily_views
GROUP BY video_id
HAVING SUM(avg_view_duration_seconds) < 0.20*SUM(watch_time_seconds)
ORDER BY video_id
 ```
 <img width="826" height="378" alt="image" src="https://github.com/user-attachments/assets/5816ae42-b9c5-4204-b11d-2451797974d1" />

---
20. Videos that gained more than 1k views in a day (spikes).

```sql
 SELECT  v.video_id ,view_date, SUM(views) as total_views_per_day
      FROM  videos v LEFT JOIN daily_views dv ON
      v.video_id = dv.video_id 
      GROUP BY v.video_id,view_date
      HAVING SUM(views) > 1000
      ORDER BY v.video_id
 ```
 <img width="1420" height="830" alt="image" src="https://github.com/user-attachments/assets/c86e1c0c-09db-410e-ade0-75f6e5993745" />

---
