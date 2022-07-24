
# Cyclistic Case Study
### Google Data Analytics Certificate Capstone Project #1

### Introduction and Company Profile
Hello, my name is Kevin, and for the purposes of this case study I’m a Junior Data Analyst in the marketing department for the bike sharing program called Cyclistic.  The venture was launched in 2016 and the current fleet consists of 5824 geo-tracked bicycles that are locked into a network of 692 stations located in and around the city of Chicago.


### A Clear Statement of the Business Task
Lily Moreno, who is the director of marketing as well as my manager, has tasked the marketing department with the goal to “design marketing strategies aimed at converting casual riders into annual members.”
Three questions are being given to guide the future marketing program:
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

The first question has been assigned to me, although some of my final recommendations touch on the second question as well.

The flexible pricing plans (single-ride passes, full-day passes, and annual memberships) have aided considerably to building general awareness of the company and the program, as well as appealing to a diverse customer base that includes both leisure riders and commuters.
The analysts over in the finance department have concluded that there is more profit to be generated from riders with annual memberships as compared to casual riders utilizing day passes.  The problem that we are facing is how to motivate the casual rider to purchase an annual membership based on their riding habits and trends.
As previously stated, the question that has been posed to me is, “How do annual members and casual riders use Cyclistic bikes differently?” to which I would also add, “and how can those differences be leveraged to promote the changes that we would like to see in our customer base and ridership?”
The stakeholders of this project are Lily Moreno and the Cyclistic executive team.

### Description of Data Sources Used for Analysis
The original data that is being utilized for this task was generated in house and is comprehensive for the purpose of this project with well over 5 million records generated during the selected time period.  The data that has been considered is from the previous [12 calendar months](https://divvy-tripdata.s3.amazonaws.com/index.html) beginning with FEB 2021 and has been made available in the form of .csv files under this [license](https://ride.divvybikes.com/data-license-agreement).  The data does not include any personal or financial information of any rider.  It does, however, record the types of riders and their corresponding habits, such as which type of bike they rode and their start and end times.  This data can help to answer the question because knowing the different habits of the riders will give insight into how to design a marketing campaign that will maximize the annual rider membership subscriptions.
The data was downloaded as individual monthly .zip files into a specified folder on my local drive with folder/location specific password protected encryption through the Windows 11 Advanced Folder Options.  The .zip files were left intact after unpacking to serve as a controlled backup.
Using a high-level overview of the data to determine the level of its reliability, it’s readily noticeable that there is a sizable percentage of NULL, or missing, values.  However, since they’re all related to location data (verified through the cleaning process and discussed in the next section), which was not being used at this time for the purpose of this analysis, it was determined that they provided the required/desired values and therefore would be included in the cleaned dataset and final analysis so as not to limit the breadth of the overall data set and the corresponding results.  

### Documentation of Cleaning/Manipulation of Data
Microsoft SQL Server Management Studio was used for data cleaning and wrangling due to the size of the dataset and familiarity with the SQL database language.  
I began by creating a Cyclistic database and importing the data using the 64-bit SQL Server 2019 Import and Export Data Wizard.  Once all the data was properly imported, I created a new table that would be populated by all the data contained in the 12 monthly tables.  For this I used the Union function.
The first task I performed on the raw aggregate table was to do an overall record count.  The total came to over 5.7 million.  I also wanted to double check that there weren’t any duplicate rows, so I repeated the count, but added a qualifying criterion to the function to only count the records with distinct ride ids.  Since the count results came back identical, I was left to assume that there are no duplicate rows in the table.
My next task was to check for NULL values in each of the table columns and determine the potential impact to the project results based on what information is missing.  What I found was that there was a sizable percentage of NULLs in various location-based columns.  Specifically, there were NULLs found in columns containing data for station name, station ID, station latitude and longitude, for both starting and ending locations.  I tried to discern and insert the missing data by matching existing data from other columns.  However, since the bikes are geo-tracked to their specific and individual locations and not the locations of the individual stations, this proved impossible without adjusting either the manner in which the geo-tracking data is collected or by significantly increasing the accuracy of the GPS data.  The final determination was that the location data was not deemed necessary for the scope of this project at this time.  The location data could be cleaned and utilized later to examine traffic patterns, which could prove to be valuable for an additional study.  
The data referencing the start and end of ride times is comprehensive and was used to populate a newly created column called ‘ride_duration_seconds’ by utilizing the DATEDIFF() function.  As the column name implies, the results of the new column show the time duration in seconds that passed from when the ride began to when the ride ended.  Further time units were found with division/multiplication to change from seconds to minutes, hours, or days (as applicable).  There was a small sample of records (657) in the table that were deleted because the “started at” time was chronologically greater than or equal to the “ended at” time, and therefore created either a negative or zero seconds ride time duration.
There was a larger group of records (roughly 400,000) that were eliminated from the results due to their ambiguity and the high likelihood that they would skew the results and diminish accuracy.  Of that group, a relatively small portion was eliminated because the calculated ride durations were too short to represent actual rides.  The timeframe of 1 minute was chosen to be the cutoff, and at first glance may seem either arbitrary or rather short.  However, during analysis it was observed that there are records around the 1-minute timeframe that show bikes being checked out of one station and checked back in at another which would demonstrate that the ride details are valid.
The larger portion of the eliminated group constitutes rides using “docked bikes”, which were only logged under casual riders.  The definition of docked bike was unclear, and an explanation was not available.  Also, several of the records showed ride durations that were clearly atypical (the longest being 38 days), and not indicative of an “average” ride.  Given the ambiguity of the docked bike classification, the potential for skew, and the fact that there were no records of members using them, it was determined that the best course of action was to eliminate those records from the analysis.
Excel was used to create the visualizations because of the desire for a simple and clear presentation so that the details of the project could really be the star of the show.

### Summary of Analysis and Discussion
I began my analysis by counting different aspects of the dataset and taking note of the different population values based on the two types of members and their riding habits as they relate to the type of bike ridden.  Further into my analysis I was counting aspects of rides based on the ride duration and the day of the week that the ride took place.  I wanted to get a snapshot of what constituted an average ride based on the varying criteria for the different rider types and the different types of bicycles that are available.  
I used varying functions to calculate different ride duration averages based on the member type, the bike type, time of day, day of the week, and month of the year.
I utilized several COUNT(CASE) functions to create distributions for different timeframes of ride durations.  I started with a high level look over a 24-hour period, then zoomed in to 6 hours, then zoomed to 3 hours, and finished with a close-up view of rides that lasted less than 1 hour and grouped into blocks of 5 minutes.  Once charted, this allowed me to see and better understand what constituted a more typical, or average ride for each type of rider.
I used a combination of the DATENAME() and DATEPART() functions to populate a column I created to show the day of the week the ride took place.  With this information available I was able to see the rider trends and how members and casual riders utilized the bikes differently and for different purposes.
Finally, I created a distribution that showed a count of the rides as they took place in different months of the year and grouped by the type of rider.  This distribution shows the seasonality of the ride totals and provides clues to the motives of the different rider types.

### Supporting Visualizations and Key Findings
The first thing to note is the comparison of the total ride count and ride duration averages.  Members log more rides, but casual riders log more ride time.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/ratio-rides-by-member-type.png)

This chart shows the ratio of the two types of riders that logged rides during the study period.  This is not to say that there is a higher population of annual members than casual riders (more data on individual rider habits would need to be collected), but that the members used the bikes at a higher frequency.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/average-ride-duration-in-minutes-by-rider-type.png)

The overall average ride duration of a casual rider is almost double that of the average for an annual member.
One possible explanation for this is that the members appear to use the bikes more as a replacement mode of transportation for everyday activities such as running errands or commuting to work and that the likelihood of them living within the city limits is high.  Since the members don’t have to worry about paying per short duration ride, they would be more likely to utilize the service for a higher frequency of shorter trips.
The demographics of casual riders has more potential for usage variability.  Tourism produces a temporary influx of potential riders who might not necessarily benefit from a traditional annual membership.  More than likely, the typical casual rider is a short-term visitor who does not live within the city limits and is using the bikes to move around the city’s points of interest.  It’s plausible that they would tend to ride for longer durations because they want to maximize the usage of the bike for the time duration that they paid for.  

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/total-monthly-rides-by-month-and-rider-type.png)

Looking at a monthly view of total rides, July is the only month where the total ride count for members is eclipsed by the casual riders with August being a relatively close second place.
Casual ridership notably falls off during the colder months.  This is most likely due to the winter off-season of tourism.  Annual member ride volumes also dropped significantly in the colder months from a high of over 385,000 in August to a low of just under 84,000 in January, but not with the same variance rate as the casual riders who dropped almost as low as 17,000 rides in January from a high of almost 379,000 in July.  This is additional evidence that there is likely a dedicated base of membership that use the bikes for regular commuting around the city and as a vehicle replacement to traditional cars.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/ride-count-by-rideable-bike-types.png)

The chart above shows the popularity of the classic bike over the electric by both types of riders.  The fleet bike type population numbers are not readily available from the dataset, so it’s not clear why there is a difference in usage between the classic and electric bikes.  What is clear is that the classic bike is being ridden more often than the electric by both types of riders.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/total-daily-rides-by-member-type-and-bike-type.png)

As can be seen in the chart above, casual ridership dramatically peaks on the weekends with a specific focus on classic bike usage when compared to their electric counterparts.  
Member’s average use of both classic and electric bikes doesn’t show a variance of much significance throughout the course of the week, which can be another example of their use being more utilitarian in nature.  Their usage-peak being mid-week aids to the idea that their purpose is mainly for commuting.
Although the member ride duration averages for both classic and electric bikes are almost identical, this chart shows that members are riding the classic bikes by an almost 2 to 1 ratio.  It is unclear using this dataset whether they are making a conscious choice to ride the classic bike or if there are other factors at play, such as bike type availability or a lack of education or knowledge regarding electric bike use or benefits.
Regardless of whether it is a choice, members are appearing to stick with the same bike type with some consistency for their daily commutes.  Casual riders seem to prefer the classic bikes with a higher frequency on the weekends than the electric bikes.  As previously discussed, perhaps it is due to variable factors such as bike availability or an aversion to try an electric bike because it is “different” than what they may be used to.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/average-ride-duration-in-minutes-by-rider-and-bike-type.png)

A couple of important points from the chart above are how consistent members are with average ride duration regardless of bike type, how the casual riders have a sizable variance regarding bike type, and also the comparison of ride duration between the casual riders and the members.
There is a sizable difference between average ride durations of casual riders and annual members.  Keeping in mind that total ride counts for members dwarf those of casual riders, it’s noteworthy that the causal riders use the bikes for much longer average durations that the annual members.  This can potentially be explained by the casual riders’ tendency to be a tourist and wanting to get the most value for their purchase.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/avg-weekly-ride-duration-in-minutes-by-member-type-and-bike-type.png)

As seen in the above chart as well as the previous one, members are fairly consistent with average ride duration throughout the week regardless of bike type.  This lends to the assumption that annual members tend to use the bikes more for utilitarian purposes such as commuting.
Casual riders have a sizable weekly ride duration variance regarding bike type as well as average ride duration when compared to the members.  Having a higher potential for variability, the casual rider demographic-use will be more susceptible to calendar-related trends both in the short and longer term.  This would be why their usage peaks on the weekends and in the warmer months.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/total-daily-rides-by-day-of-the-week-and-rider-type.png)

As previously noted, overall, members logged a higher number of rides than casual riders during the study period.  However, on average, the members’ usage is overtaken on the weekends by the casual riders by ride volume.  
Another thing to take note on this chart is which days shows the maximum and minimum total rides for each of the rider types.  The members’ busiest day of the week is Wednesday, and Sunday is their slowest.  Again, these results are indicative of a mostly commuting, or utilitarian demographic.
Where casual riders held the top spot for volatility regarding average ride duration when comparing midweek to the weekends, the same is true for the total logged rides.  However, their Monday through Thursday volume is quite consistent.  This could suggest that, for whatever reason, there are a group of casual riders using the bikes for commuting that have not decided to pull the lever on an annual membership and that have potential to be tapped as a source of new annual subscribers.
Casual riders use the bikes more than members on the weekends with a larger variance (230K) when compared with mid-week use.  Member’s variance from weekend to mid-week (105K) is less pronounced and the daily use frequency is more consistent.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/avg-ride-duration-in-minutes-by-rider-type-and-day-of-the-week.png)

Although members logged more rides, casual riders rode for longer average durations throughout the week.  Members ride the longest on the weekends with a 25% variance between an average of 15 minutes on Sundays to 12 minutes on Monday through Wednesday.  Casual riders also rode for the longest average duration on the weekends with a 33% variance in average between Sunday and Wednesday/Thursday.
Members mid-week use is more uniform with less variation.  They rode for an average duration of 12 minutes Monday through Thursday with a slight uptick on Friday and into the weekend.  This is most likely due to the members using the bikes primarily to commute to work around the city during the week and perhaps the addition of leisure rides or running errands on the weekends.
Casual riders’ use of the bikes has more variance throughout the week.  Their use peaks on the weekends which would likely have less to do with commuting and more to do with tourism.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/count-of-total-ride-duration-by-hours.png)

This chart demonstrates that the majority of all rides are 2 hours or less.  This makes sense because the pricing structure has been designed in such a way to favor short duration rides by specifying a fixed base ride duration with a per-minute additional charge beyond that timeframe.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/grouped-ride-duration-by-rider-type.png)

If we zoom in a little closer, we can see that many of the rides are less than 30 minutes in length.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/count-of-grouped-ride-duration-by-rider-type-in-minutes.png)

This chart shows that the largest percentage of rides are 10 minutes or less in duration for both members and casual riders.  It also shows the level to which the members far outpace casual riders in volume.  This makes sense with the casual riders’ higher average ride duration.  However, a portion of these shorter-ride-duration casual riders are most likely the ones that are using the service for commuting purposes and should be subscribing to an annual membership.  These are the riders that the new marketing campaign should be paying specific attention to.
Due to the pricing structure, it can be assumed that the annual members are more comfortable using their unlimited short rides than the casual riders who must pay for each individual trip.  Making some adjustments to the pricing structure could favorably tip the scales to a larger annual membership.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/count-of-rides-per-hour-of-the-day-by-member-type.png)

The final analysis that this report will discuss is based on the time of day that rides are typically taking place.  As can be seen in the chart above, members still command most of the ride volume throughout a typical day with spikes at 08:00, 12:00, and 17:00.  This shows their highest traffic times during rush-hour commuting and lunch.  One thing that is interesting is that more member rides are taking place in the evening than in the morning.  It could be that members may be utilizing one type of public transportation going to work and then riding bikes home, or it could be that there are members that use the bikes after work as a means of exercise or running errands.
A potentially fair assumption of people on vacation is reinforced by the fact that casual riders don’t appear to be morning people as can be seen in this chart.  Their day doesn’t really seem to ramp up until about 09:00 with a steady rise in volume through lunch and leading up to dinner.  They do utilize the bikes more for nightlife than the members do, but the numbers are rather insignificant for the purpose of this project.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/count-of-rides-per-hour-of-the-day-by-member-type-and-bike-type.png)

The results of sorting the ride volume by rider type and bike type is in line with previously reported results regarding bike type popularity.  For both types of riders, the overall dominance of the classic bike continues to reign.  There is a 2-hour window of time very early in the morning when the causal rider use of electric bikes holds the top spot. However, similarly to the casual rider nightlife volume, the numbers are insignificant and of little value, if any, to incorporate into a marketing strategy.

![](https://github.com/Kevrock01/cyclistic_case_study/blob/main/images/avg-ride-duration-per-hour-of-the-day.png)

As has been the case in previous portions of the analysis, the causal riders are consistently riding for longer average durations than the members.  One thing that is interesting is that during the overnight hours when the ride volume is nearing its lowest point in the day, the casual riders are riding for their longest average durations.  Why would this be?  A possible explanation could come from Chicago’s robust late-night bar and restaurant scene.  There are a number of establishments that have liquor licenses that allow them to pour drinks until 04:00 most of the week and 05:00 on Saturdays.
The other observation of note is how consistent the members’ ride durations are throughout the day.  Interestingly, they are taking their longest average rides in the overnight hours just as the casual riders are.  However, for the purposes of this analysis and the proposed marketing strategy, the focus should be placed on the times with the highest ride volumes.

### Top Recommendations Based on Analysis

1. Possibly add additional pricing tier(s) for rider type conversion
•	Discounted member teaser rate for short period with an automatic subscription renewal at the member price

•	Additional price savings for subscribing to an auto-renewing membership vs manual renewal

•	Create a new package for weekend riders (not primarily commuters)

•	Create a seasonal-based pass to capture more of the tourism that takes place in the warmer months of the year

•	Offer a student discount

2. Raise the prices for casual riders to lower the “payback threshold” of membership
3. Education outreach about efficiency (Costs, convenience, health, ecological, etc.) of bike usage
4. Add verbiage to marketing campaign that demonstrates savings thresholds for conversion after a set number of casual rides
5. Create a “point” system for annual member rides with prizes

The data paints a picture of two distinct types of riders with different apparent goals for their riding habits.  One is a commuter and uses the bikes as an alternate mode of transportation to cars or other types of vehicles.  Those riders appear dedicated and educated to the benefits of holding an annual membership to the Cyclistic bike sharing service.  One could assume that the chance of these riders living inside the city limits to be high.
The other type of rider appears more eclectic in their use of a bicycle.  They could be a commuter or a regular user, but most likely is a short-term visitor to the city as a tourist.  One could assume that the chance of these riders living inside the city limits is low.
To get people to change you need to incentivize them to take some action.  In this case that could take the form of increasing the perceived value they would receive by switching their rider type from casual to member.  This could be achieved by either increasing the convenience of the service or by altering the pricing structure to better accommodate membership.
There are going to be casual riders that you just can’t convince for one reason or another, or for whom it just doesn’t make financial sense due to cost or individual logistics.  Someone who is visiting the city for a short time is not going to be able to justify the cost of an annual membership because their riding requirement would be better served by either the per-ride pass or the daily pass.   Thankfully, those options are there for them to take advantage of.  However, someone who is going to utilize the bikes for a week or more during any calendar year using the daily pass option would exceed the cost of an annual membership.  In this scenario, it would be better for that rider to purchase the annual membership, or a newly created seasonal version, simply to save money regardless of whether they would ever use it again.  
A potential concern that a moderately-periodic rider might have regarding the purchase of an annual membership could be account security.  This could also include the fear of a recuring charge or an automatically renewing membership.  In the case of a rider that might visit the city long enough to justify the cost of an annual membership, but not be in a position to benefit beyond the initial membership period, it would be advisable that the option be made available for a rider to opt-out of auto-renewing memberships.

### Possible Further Actions
1.	Analyzing location data for traffic patterns
Having a better understanding of the traffic habits and trends of riders could prove to be valuable for potential future action and decision making.  It would better answer the question of whether casual riders actually are using the bikes more for tourism and less for commuting.  It would also be useful to determine the efficiency of bike depot locations.  This could benefit plans for future expansion of the station network into different areas of the city.

2.	Do the electric bikes get people where they want to go faster and/or more efficiently than classic bikes?
This could be a good question to answer and might incorporate the use of the location data.  Investing in more electric bikes as the fleet needs to be updated could lead to shorter ride durations in the future.  This in turn could lead to better turnover of bike usage by having less bikes in transit at any given time.  Of course, this would also raise other questions relating to cost accounting analysis, such as ratios of maintenance costs for classic vs electric bikes.
