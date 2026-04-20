## 🟢 Basics (1–10)

1. List all videos and their creator names.
```sql
SELECT  DISTINCT video_id,title,creator_name FROM Youtube.creators c
LEFT JOIN Youtube.videos v ON
c.creator_id = v.creator_id
ORDER BY video_id

```
<img width="1633" height="744" alt="image" src="https://github.com/user-attachments/assets/a13cd7c5-5077-4ea7-8ab0-f44c2980f4ed" />

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
```
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
```
---
10. Find videos with zero comments.  
```sql
```
---
