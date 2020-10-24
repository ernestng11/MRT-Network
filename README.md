# MRT-Network

## Data Exploration and Cleaning

### Trips Dataset
1. Check data types for each column
2. Remove station codes attached to station names if any. Eg Cleaning *Bugis DTL* into *Bugis*
3. Check for any duplicated rows and remove them - same destination, origin, destination_tm, origin_tm
4. Check for any self-loops where origin and destination nodes are the same. Eg Bugis to Bugis
5. Parse string time into **time in seconds** and get the travel time
6. Check if any travel time is negative and remove if any
7. Group all rows where origin and destination are the same -  Eg. Create new column where value = "Node i, Node j" for trips from i to j and j to i.
8. For the groups defined in step 7, get the **mean travel time**

