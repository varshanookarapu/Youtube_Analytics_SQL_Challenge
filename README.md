
# 50 SQL Questions – YouTube Analytics Project

Practice-focused SQL questions using a single YouTube Analytics dataset. Designed for interviews, real-world data
analysis, and SQL mastery.

## Basic Queries (1–10)

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

## Filters & Aggregates (11–20)

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


## Joins & Multi-table (21–30)

21. For each creator, total revenue (ad + subscription + other).

22. For each video, last 7-day rolling average views.

23. Top performing video per creator by revenue.

24. Video comment sentiment breakdown (pos/neutral/neg).

25. Videos with impressions but 0 clicks (possible data issue).

26. List videos and their peak daily views date.

27. Show creators who published > 10 videos.

28. Videos with multiple high-spike days.

29. Find videos with payments revenue but zero views (data mismatch).

30. Creator-wise average CTR.

---

## Window Functions & Ranking (31–39)

31. Rank videos by total views within each category (RANK/DENSE_RANK).

32. Running total of views per video (window).

33. Monthly growth rate of views for each video (LAG).

34. Top 3 videos per month (window + partition).

35. Percentile of views for each video (NTILE).

36. Lead/Lag to calculate day-over-day % change.

37. Cumulative watch time per creator.

38. Rank creators by average watch time per video.

39. Use ROW_NUMBER to deduplicate and get latest daily_stats.

---

## Advanced / Analytics (41–50)

40. Compute engagement score = (likes + comments + clicks) / impressions.

41. Detect anomaly days using z-score on daily views.

42. Calculate creator retention: percent of videos still getting views after 90 days.

43. Find videos that cause highest drop-off (avg_view_duration << duration).

44. Customer lifetime value style: total revenue per creator normalized by number of videos.

45. Segment videos into bins by length and analyze average CTR per bin.

46. Identify top comments contributors (who comments most).

47. Find videos which had more than 20k impressions but no revenue.

48. Build a pivot: monthly revenue by category.

49. Multi-day conversion funnel: impressions → clicks → views → watch_time (aggregate ratios).

50. Mark videos with inconsistent data (daily_views exists but no likes_dislikes for that day).
