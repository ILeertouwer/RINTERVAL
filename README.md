# RINTERVAL
Function for creating (roughly) equal intervals for timeseries data in R.

Arguments:
* data = A data frame.
* id = An ID (participant) variable.
* datetime = The datetime variable in datetime format. If your datetime variable is not in this format, you can use lubridate::as_datetime(datetime variable) first.
* min_int = The minimum interval in minutes. If the minimum interval in the supplied data is shorter than that minimum interval, it is not a problem.
* no_night = If set to TRUE does not add missing observations between the last observation of the one day and the first observation of the next. 

General steps:
1. Calculate Time Differences: Compute the time difference between consecutive measurement occasions.
2. Determine Interval Counts: For each computed time difference, determine how many times the specified minimum time interval can fit into it (rounded appropriately).
3. Insert Missing Values: Divide the computed time difference by the number determined in step 2. Insert missing values at these calculated intervals to create evenly spaced time points between the original measurements.
4. Create Day Variable: Create a new variable that records the day on which each measurement (including the added missing values) occurred.
5. Compute Differences: Create a variable that expresses the difference in time between all measurement occasions, including the added missing values.
6. Cumulative Differences: Create a variable that calculates the cumulative difference in time from the start of the measurement period, incorporating all original and newly added time points.

Resulting intervals:
The minimum time interval for added missing values is .75 times the specified minimum interval. Suppose that the minimum interval is one hour (01:00:00): 
T1 = 00:00:01, T2 = 01:30:01. True interval: 01:30, rounded to: 02:00. One missing value added; observed interval: 00:45.	

The maximum time interval for added missing values is 1.5 times the specified minimum interval. Suppose that the minimum interval is one hour (01:00:00): 
T1 = 00:00:01, T2 = 01:29:59. True interval: 01:30, rounded to: 01:00.	No missing value added; observed interval: 01:30.

