
# 📊 SQL Practice Questions – Video Analytics Dataset

This repository contains SQL practice questions covering **basic queries, aggregations, joins, window functions, and advanced analytics** using a video analytics dataset.

I put the dataset code in this file [Youtube Analytics Dataset](Youtube_Analytics_SQL_Challenge/Youtube_Analytics_Dataset_Code.md) which you can copy paste in DB fiddle (https://www.db-fiddle.com/) and work on these challenges .  You can intially test the data set by using the following queries 

```sql
SELECT * FROM Youtube.creators LIMIT 2;
SELECT * FROM Youtube.videos LIMIT 2;
SELECT * FROM Youtube.daily_views LIMIT 2;
SELECT * FROM Youtube.comments LIMIT 2;
SELECT * FROM Youtube.likes_dislikes LIMIT 2;
SELECT * FROM Youtube.revenue LIMIT 2
```
---

## 🟢 Basics (1–10)

1. List all videos and their creator names.  
2. Count total videos per creator.  
3. Get total comments for a given video.  
4. List videos published in the last 18 months.  
5. Find videos longer than 20 minutes.  
6. Show top 10 videos by total views (aggregate daily_views).  
7. Show unique categories.  
8. Count creators per country.  
9. Get average views per video per creator.  
10. Find videos with zero comments.  

---

## 🟡 Filters & Aggregates (11–20)

11. Total impressions and clicks per video.  
12. Compute CTR = clicks / impressions per day.  
13. Average watch time per view (watch_time_seconds / views).  
14. Daily views trend for a single video.  
15. Views per category.  
16. Top 5 videos by watch_time_seconds.  
17. Average likes/dislikes per video.  
18. List videos with more dislikes than likes.  
19. Videos where avg_view_duration < 20% of duration.  
20. Videos that gained more than 1k views in a day (spikes).  

---

## 🔵 Joins & Multi-table (21–30)

21. For each creator, total revenue (ad + subscription + other).  
22. For each video, last 7-day rolling average views.  
23. Top performing video per creator by revenue.  
24. Video comment sentiment breakdown (positive / neutral / negative).  
25. Videos with impressions but 0 clicks (possible data issue).  
26. List videos and their peak daily views date.  
27. Show creators who published more than 10 videos.  
28. Videos with multiple high-spike days.  
29. Find videos with payments revenue but zero views (data mismatch).  
30. Creator-wise average CTR.  

---

## 🟣 Window Functions & Ranking (31–39)

31. Rank videos by total views within each category (RANK / DENSE_RANK).  
32. Running total of views per video (window function).  
33. Monthly growth rate of views for each video (LAG).  
34. Top 3 videos per month (partition + ranking).  
35. Percentile of views for each video (NTILE).  
36. Lead/Lag to calculate day-over-day % change.  
37. Cumulative watch time per creator.  
38. Rank creators by average watch time per video.  
39. Use ROW_NUMBER to deduplicate and get latest daily stats.  

---

## 🔴 Advanced / Analytics (40–50)

40. Compute engagement score = (likes + comments + clicks) / impressions.  
41. Detect anomaly days using z-score on daily views.  
42. Creator retention: % of videos still getting views after 90 days.  
43. Find videos causing highest drop-off (low avg_view_duration vs duration).  
44. Creator lifetime value = total revenue / number of videos.  
45. Segment videos by length and analyze CTR per segment.  
46. Identify top comment contributors.  
47. Videos with >20k impressions but no revenue.  
48. Monthly revenue pivot by category.  
49. Funnel analysis: impressions → clicks → views → watch_time.  
50. Detect inconsistent data across daily metrics tables.  

---

## 🚀 Purpose

This dataset is designed to practice:
- SQL Joins
- Aggregations
- Window Functions
- Real-world analytics scenarios
- Data quality checks
