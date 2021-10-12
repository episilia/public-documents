# **Release notes for Episilia 0.12**

# Features and enhancements:

- Search enable Multi threaded index warming. Significantly improves index warming by 5X.

- Search now cleans-up old indexes as per the sliding window to.hours - from.hours.

- **Optimizer new version released:** Now folder wise run enabled.

# Bug fixes:
NA

# Config changes:

- **Deleted search properties:** Daystoinclude and indexrefresh.interval.hours\
  Added from.hours, to.hours for "live" mode.\
  Aded from.yyyymmddhh, to.yyyymmddhh for "fixed" mode.

- **Index.warmer.thread.count:** Number of index warming threads defaults to 5.

- Optimize.request.topic
  
   
