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
