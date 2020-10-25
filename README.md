# MRT-Network

## Data Exploration and Cleaning

### Problems
- For station names with consecutive capital letters behind, they **might** exist on mutiple train lines E.g Bugis DTL, Bugis NSEW
- For one origin station, the dataset shows that many destination stations can be reached. To plot a network graph, we want to focus on stations that can be reached from one origin in 1 station travel.
- To determine reachability, we want to retrieve the journeys with the shortest possible travel time to get stations on the left and right of the origin station.
- There will definitely be stations that exist on the extreme end of the train line. **How do we identify these stations?**
- We need to get the last 1 (if station is an endpoint on train line eg Tuas), 2 (if station is in the middle of train line eg Holland Village), 4 (if origin station exists on 2 train lines eg Bugis) or 6 (if origin station exists on 3 train lines eg Dhoby Ghaut). Possible maximum reachable nodes = 2n where n is the number of train lines associated with the origin station.
- Taking the mean/median and finding the 2 shortest path relative to the origin, might not always give us the adjacent stations to the origin. This approach will not give us a confident result that all stations will be connected in some way.


### Assumptions
1. Given a train line (m - x - y - z), time taken to travel from x to m will be shorter than that from x to z for all given origin stations.
2. Taking the mean/median and finding the 2 shortest path relative to the origin, will give us the adjacent stations to the origin.

### Solution
  - Treat each station on different lines as different stations. Eg treat Bugis DTL and Bugis NSEW as different stations. Then connect with a dotted line to indicate that these stations are together
  - For all trips associated with origin X, get mean/median for each possible destinations. Sort the trips with origin X in increasing order and take the shortest 2. If destinations for those shortest 2 are reachable from each other, then we can conclude that X is an endpoint, else X is in the middle of its train line. If there is only 1 possible shortest path from origin X after sorting, then we can also say that X is an endpoint.

### Work Plan
1. Check data types for each column
2. Get list of all possible stations. 
Total of 154 possible stations
2. Check for any self-loops where origin and destination nodes are the same. Eg Kent Ridge - Kent Ridge
Removed 2031 self-loops from dataset
3. Parse string time into **time in seconds** and get the travel time
4. Check if any travel time is negative and remove if any
5. Group each travel Node i -> Node j .
6. For the groups defined in step 7, plot distribution of travel times and decide on whether to use **mean or median** (Check for skewness of distribution)
7. Group by each origin node and sort the medians in increasing order then take the shortest 2 paths if possible. (Use pivot table) (I realised mean is more reliable)
8. Do the endpoint check
9. Add the paths in edge_list
10. Plot graph with NetworkX

