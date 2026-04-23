## 🔵 Joins & Multi-table (21–30)

**Question 21:** For each creator, total revenue (ad + subscription + other).

```sql
SELECT v.creator_id, creator_name, SUM(ad_revenue+subscription_revenue+other_revenue) AS total_revenue 
FROM videos v LEFT JOIN
revenue r ON
v.video_id = r.video_id
LEFT JOIN creators c ON
v.creator_id = c.creator_id
GROUP BY creator_name,v.creator_id
ORDER BY v.creator_id
```
<img width="1370" height="351" alt="image" src="https://github.com/user-attachments/assets/993290ab-e153-4f24-9501-d59473110812" />

---

**Question 22:** For each video, last 7-day rolling average views.
```sql
```
---
**Question 23:** Top performing video per creator by revenue.
```sql
WITH top_revenue AS
(

SELECT v.creator_id, creator_name,v.video_id, SUM( CASE WHEN ad_revenue + subscription_revenue + other_revenue IS NULL THEN 0 ELSE (ad_revenue + subscription_revenue + other_revenue) END ) AS total_revenue 
FROM videos v LEFT JOIN
revenue r ON
v.video_id = r.video_id
LEFT JOIN creators c ON
v.creator_id = c.creator_id
GROUP BY creator_name,v.creator_id,v.video_id
ORDER BY v.creator_id,video_id

),

top_performers AS
(

SELECT * ,RANK() OVER(PARTITION BY creator_id ORDER BY total_revenue DESC) as rank 
FROM top_revenue
WHERE total_revenue !=0 
)

SELECT * FROM top_performers WHERE rank =1

```
<img width="1737" height="744" alt="image" src="https://github.com/user-attachments/assets/5709203a-5b3f-4cca-9107-c61584de59d2" />

---
**Question 24:** Video comment sentiment breakdown (positive / neutral / negative).
```sql
SELECT v.video_id, COUNT(CASE WHEN sentiment ='positive' THEN v.video_id END ) As sentiment_postivie,
COUNT(CASE WHEN sentiment ='negative' THEN v.video_id END ) As sentiment_negative,
COUNT(CASE WHEN sentiment ='neutral' THEN v.video_id END ) As sentiment_neutral
FROM videos v
INNER JOIN comments c ON
v.video_id = c.video_id
GROUP BY v.video_id
ORDER BY v.video_id

```
<img width="1575" height="326" alt="image" src="https://github.com/user-attachments/assets/6af7b759-ad14-4420-aa4d-dbfad074cce3" />

---
**Question 25:** Videos with impressions but 0 clicks (possible data issue).
```sql
SELECT v.video_id, impressions, clicks 
FROM videos v
INNER JOIN daily_views dv ON
v.video_id = dv.video_id
WHERE impressions >0 AND clicks = 0
ORDER BY v.video_id
-- Total video count
WITH CTE AS(
  
SELECT v.video_id, impressions, clicks 
FROM videos v
INNER JOIN daily_views dv ON
v.video_id = dv.video_id
WHERE impressions >0 AND clicks = 0
ORDER BY v.video_id
)

SELECT COUNT(*)  as videos_with_impressions_no_clicks FROM CTE

```
<img width="492" height="169" alt="image" src="https://github.com/user-attachments/assets/cab07fd5-8d3b-48de-b1af-cdefa75faaef" />

<img width="1728" height="901" alt="image" src="https://github.com/user-attachments/assets/4878d2a9-8ca0-4146-b9de-649cee64fe6d" />

---
**Question 26:** List videos and their peak daily views date.
```sql
```
---
**Question 27:** Show creators who published more than 10 videos.
```sql
```
---
**Question 28:** Videos with multiple high-spike days.
```sql
```
---
**Question 29:** Find videos with payments revenue but zero views (data mismatch).
```sql
```
---
**Question 30:** Creator-wise average CTR.
```sql
```
---

