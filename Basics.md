## 🟢 Basics (1–10)

1. List all videos and their creator names.
```sql
SELECT  c.creator_id, creator_name,video_id,title FROM Youtube.creators c
LEFT JOIN Youtube.videos v ON
c.creator_id = v.creator_id
ORDER BY c.creator_id, video_id

```
<img width="1383" height="349" alt="image" src="https://github.com/user-attachments/assets/ac001fa8-bd2d-43fe-847e-d826f4996a2a" />

---
   
2. Count total videos per creator.
```sql
SELECT  c.creator_id, COUNT(*) as video_count FROM Youtube.creators c
LEFT JOIN Youtube.videos v ON
c.creator_id = v.creator_id
GROUP BY c.creator_id
ORDER BY creator_id
```
<img width="1185" height="746" alt="image" src="https://github.com/user-attachments/assets/cd817fac-67b8-4f88-a266-053789c542b7" />

---
3. Get total comments for a given video.  
```sql
SELECT v.video_id , COUNT(comment_id) as comments_count FROM Youtube.videos v
LEFT JOIN Youtube.comments c  ON
v.video_id = c.video_id
GROUP BY v.video_id
ORDER BY v.video_id
LIMIT 5
```
<img width="946" height="292" alt="image" src="https://github.com/user-attachments/assets/4fdcc2a1-ca0f-466e-9c0f-938b552c4eb8" />

---
4. List videos published in the last 18 months.  
```sql
SELECT video_id,title FROM Youtube.videos 
WHERE 
publish_date 
BETWEEN (SELECT (MAX(publish_date) -  INTERVAL '18 months') :: DATE FROM Youtube.videos )
AND (SELECT MAX(publish_date) FROM Youtube.videos )
ORDER BY video_id;

```
1000 - 1219 videos
<img width="1334" height="754" alt="image" src="https://github.com/user-attachments/assets/f87479af-eb2c-4dd5-a8d5-b11ed5b4618c" />

---
5. Find videos longer than 20 minutes.
```sql
SELECT video_id , title, (duration_seconds/60) :: NUMERIC as duration_minutes FROM Youtube.videos 
WHERE duration_seconds > 20*60
ORDER BY video_id
```
<img width="1320" height="353" alt="image" src="https://github.com/user-attachments/assets/f50ff746-33bc-4ae9-bc21-a2894e212c5e" />

---
6. Show top 10 videos by total views (aggregate daily_views).  
```sql
WITH tvc AS
(
SELECT video_id , SUM(CASE WHEN views IS NOT NULL THEN views ELSE 0 END ) as total_views 
FROM 
Youtube.daily_views 
GROUP BY video_id 


)

SELECT video_id, total_views,RANK() OVER(ORDER BY total_views DESC) as rank
FROM tvc
LIMIT 10

-- NOTE : I used partition by video_id in the window function 
-- Then i got all ones in the rank column, the reason is that in our CTE we already partitioned ( grouped by ) by video id , so  there is exactly one row for each video id , hence when a partition has one row , its rank is always one that is the reason why i was seeing all ones before.
--RANK() OVER(PARTITION BY video_id ORDER BY total_views ) as rank

```
<img width="1770" height="533" alt="image" src="https://github.com/user-attachments/assets/5b63c1fd-65d2-424a-99b8-1f2085032282" />

---
7. Show unique categories.  
```sql
SELECT DISTINCT category FROM Youtube.videos
ORDER BY category;
```
<img width="425" height="498" alt="image" src="https://github.com/user-attachments/assets/6dfdd270-b9a1-4cb0-823b-a208cba41aff" />

---
8. Count creators per country.  
```sql
SELECT country, COUNT(creator_id) as creators_count FROM Youtube.creators 
GROUP BY country
ORDER BY country;
```
<img width="1281" height="562" alt="image" src="https://github.com/user-attachments/assets/2da2cef4-ce7e-4489-8ba7-337037f74f51" />

---
9. Get average views per video per creator.  
```sql
WITH av AS
(
SELECT creator_id,v.video_id,views as view_count_per_video_id 
FROM Youtube.videos v 
LEFT JOIN Youtube.daily_views dv
ON v.video_id = dv.video_id 
ORDER BY creator_id,v.video_id
)

SELECT creator_id, video_id,  SUM(view_count_per_video_id ) total_views_per_video_id ,
ROUND((SUM(view_count_per_video_id ) / COUNT(*) ) ::NUMERIC) as average_views_per_creator_per_video
FROM av
WHERE creator_id = 1 
GROUP BY creator_id,video_id
```
<img width="1396" height="288" alt="image" src="https://github.com/user-attachments/assets/35e85f37-bf3d-4dc9-aa99-a3bcb5996a84" />

---
10. Find videos with zero comments.  
```sql
WITH zero_video_comments AS
(
SELECT v.video_id, COUNT(comment_id) as total_comments 
FROM Youtube.videos v 
LEFT JOIN Youtube.comments c 
ON v.video_id = c.video_id 
GROUP BY v.video_id
)
-- This question was a bit ambigious so i listed all the video ids as well as total videos with zero comments 
-- SELECt video_id FROM zero_video_comments WHERE total_comments =0 
-- ORDER BY video_id ;

SELECT COUNT(video_id) as videos_with_zero_comments FROM zero_video_comments WHERE total_comments =0 
```
<img width="309" height="91" alt="image" src="https://github.com/user-attachments/assets/bf26e053-62ab-4cf0-8229-85bd09afc1f3" />
<img width="182" height="747" alt="image" src="https://github.com/user-attachments/assets/075accca-7b6a-4cf8-803b-1bdbec9e6f96" />

---
