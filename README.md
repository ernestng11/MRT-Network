# MRT-Network

## Data Exploration and Cleaning

### Problems
- For station names with consecutive capital letters behind, they **might** exist on mutiple train lines E.g Bugis DTL, Bugis NSEW
- For one origin station, the dataset shows that many destination stations can be reached. To plot a network graph, we want to focus on stations that can be reached from one origin in 1 station travel.
- To determine reachability, we want to retrieve the journeys with the shortest possible travel time to get stations on the left and right of the origin station.
- There will definitely be stations that exist on the extreme end of the train line. **How do we identify these stations?**
- We need to get the last 1 (if station is an endpoint on train line eg Tuas), 2 (if station is in the middle of train line eg Holland Village), 4 (if origin station exists on 2 train lines eg Bugis) or 6 (if origin station exists on 3 train lines eg Dhoby Ghaut). Possible maximum reachable nodes = 2n where n is the number of train lines associated with the origin station.

### Solution
  - Treat each station on different lines as different stations. Eg treat Bugis DTL and Bugis NSEW as different stations. Then connect with a dotted line to indicate that these stations are together
  - For all trips associated with origin X, get mean/median for each possible destinations. Sort the trips with origin X in increasing order and take the shortest 2. If destinations for those shortest 2 are reachable from each other, then we can conclude that X is an endpoint, else X is in the middle of its train line. If there is only 1 possible shortest path from origin X after sorting, then we can also say that X is an endpoint.

### Work Plan
1. Check data types for each column
2. Check for any self-loops where origin and destination nodes are the same. Eg Kent Ridge - Kent Ridge
3. Parse string time into **time in seconds** and get the travel time
4. Check if any travel time is negative and remove if any
5. Group each travel Node i -> Node j .
6. For the groups defined in step 7, plot distribution of travel times and decide on whether to use **mean or median** (Check for skewness of distribution)
7. Group by each origin node and sort the medians in increasing order then take the shortest 2 paths if possible. (Use pivot table)
8. Do the endpoint check
9. Add the paths in edge_list
10. Plot graph with NetworkX

Edits:
2. Remove station codes attached to station names if any. Eg Cleaning *Bugis DTL* into *Bugis*
3. Check for any duplicated rows and remove them - same destination, origin, destination_tm, origin_tm
5. Group all rows where origin and destination are the same -  Eg. Create new column where value = "Node i, Node j" for trips from i to j and j to i.
