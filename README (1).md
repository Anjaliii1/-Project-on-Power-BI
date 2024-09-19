
# Project Title

A brief description of what this project does and who it's for

# Domain: Hospitality                                     
### Function: Revenue

### Dashboard Link : 

### Dashboard clip : 

## Problem Statement

AtliQ Grands owns multiple five-star hotels across India. They have been in the hospitality industry for the past 20 years. Due to strategic moves from other competitors and ineffective decision-making in management, AtliQ Grands are losing its market share and revenue in the luxury/business hotels category. As a strategic move, the managing director of AtliQ Grands wanted to incorporate “Business and Data Intelligence” to regain their market share and revenue. However, they do not have an in-house data analytics team to provide them with these insights.

Their revenue management team had decided to hire a 3rd party service provider to provide them with insights from their historical data.

## Task:  

You are a data analyst who has been provided with sample data and a mock-up dashboard to work on the following task. You can download all relevant documents from the download section.

  * Create the metrics according to the metric list.
  * Create a dashboard according to the mock-up provided by stakeholders.
  * Create relevant insights that are not provided in the metric list/mock-up dashboard.


### Steps followed 



-----------------------------> Data Loading and Power Query Documentation -------------------------------------->


1. Create a folder in Desktop and store all the csv files related to hospitality challenge.

2. Open a Power BI file, and In "Get Data", select the option as folder and browse through the folder containing csv files.

3. Then go to Tranform data to expand each and every dataset.

4. Now, duplicate the data source 4 times and in each one, expand one dataset by clicking on "Binary" option. also, rename 
   the tables accordingly.


*****************  Power Query steps for tables:  *******************
1. dim_date:
	- remove the column 'day_type'
	- we are deleting this because, we got a feedback from the mock dashboard review that Friday and Saturday are           
	  considered as weekends in the industry and not sunday. But In our datasets, saturday and sunday are considered           
	  as weekends. So we delete this column and re-create day_type using calculated columns.

2. dim_rooms
	- The column names are not captured here. We need to select "Use First Row as Headers" option .

3. dim_hotels,fact_aggregated_booking,fact_booking there is not such changes we do because data is correct format and clean.

4. From Now , we calculate the key measures for this we create the new colume for all the DAX measures we calculate for dashboard.
        
        DAX measures (Key measure):

            1. Revenue = SUM(fact_bookings[revenue_realized])
            2. Total Bookings = COUNT(fact_bookings[booking_id])
            3. Total Capacity = SUM(fact_aggregated_bookings[capacity])
            4. Total Succesful Bookings = SUM(fact_aggregated_bookings[successful_bookings])
            5. Occupancy % = DIVIDE([Total Succesful Bookings],[Total Capacity],0)
            6. Average Rating = AVERAGE(fact_bookings[ratings_given])
            7. No of days  = DATEDIFF(MIN(dim_date[date]),MAX(dim_date[date]),DAY) +1
            8. Total cancelled bookings  CALCULATE([Total Bookings],fact_bookings[booking_status]="Cancelled")
            9. Cancellation % = DIVIDE([Total cancelled bookings],[Total Bookings])
            10. Total Checked Out = CALCULATE([Total Bookings],fact_bookings[booking_status]="Checked Out")
            11. Total no show bookings = CALCULATE([Total Bookings],fact_bookings[booking_status]="No Show")
            12. No Show rate % = DIVIDE([Total no show bookings],[Total Bookings])
            13. "Booking % by Platform = DIVIDE([Total Bookings],CALCULATE([Total Bookings], ALL(fact_bookings[booking_platform])))*100"
            14. "Booking % by Room class = DIVIDE([Total Bookings],CALCULATE([Total Bookings], ALL(dim_rooms[room_class])))*100"
            15. ADR = DIVIDE( [Revenue], [Total Bookings],0)
            16. Realisation % = 1- ([Cancellation %]+[No Show rate %])
            17. RevPAR = DIVIDE([Revenue],[Total Capacity])
            18. DBRN = DIVIDE([Total Bookings], [No of days])
            19. DSRN = DIVIDE([Total Capacity], [No of days])
            20. DURN = DIVIDE([Total Checked Out],[No of days])
            21. "Revenue WoW change % = 
                 Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
                 var revcw = CALCULATE([Revenue],dim_date[wn]= selv)
                 var revpw =  CALCULATE([Revenue],FILTER(ALL(dim_date),dim_date[wn]= selv-1)) return  DIVIDE(revcw,revpw,0)-1"
            22. "Occupancy WoW change % = 
                Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
                var revcw = CALCULATE([Occupancy %],dim_date[wn]= selv)
                var revpw =  CALCULATE([Occupancy %],FILTER(ALL(dim_date),dim_date[wn]= selv-1))

                return  DIVIDE(revcw,revpw,0)-1"
            23. "ADR WoW change % = 
               Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
               var revcw = CALCULATE([ADR],dim_date[wn]= selv)
               var revpw =  CALCULATE([ADR],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
               return  DIVIDE(revcw,revpw,0)-1"
            24. "Revpar WoW change % = 
               Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
               var revcw = CALCULATE([RevPAR],dim_date[wn]= selv)
               var revpw =  CALCULATE([RevPAR],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
               return DIVIDE(revcw,revpw,0)-1"
           25. "Realisation WoW change % = 
               Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
               var revcw = CALCULATE([Realisation %],dim_date[wn]= selv)
               var revpw =  CALCULATE([Realisation %],FILTER(ALL(dim_date),dim_date[wn]=              selv-1)) 
               return DIVIDE(revcw,revpw,0)-1"
          26. "DSRN WoW change % = 
               Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
               var revcw = CALCULATE([DSRN],dim_date[wn]= selv)
               var revpw =  CALCULATE([DSRN],FILTER(ALL(dim_date),dim_date[wn]= selv-1))              return DIVIDE(revcw,revpw,0)-1"

----------- >>After calculating all key measures  Required in Dashboard like Revenue, total booking, total capacity,like so on

* Week-on-Week (WoW) is a type of business metric that measures changes in a specific variable 

over a period of one week compared to the previous week. It is a common way of tracking 
business performance over time and is particularly useful for analyzing trends and identifying 
areas where improvements can be made.
Here are the metrics for which we found the WoW change% in this video:

        1. Revenue WoW change %: To get the revenue change percentage week over week.
        2. Occupancy WoW change %: To get the occupancy change percentage week over week.
        3. ADR WoW change %: To get the ADR (Average Daily rate) change percentage 
           week over week.
        4. RevPAR WoW change %: To get the RevPAR (Revenue Per Available Room) change 
           percentage week over week.
        5. Realisation WoW change %: To get the Realisation change percentage week       
           over week.
        6. DSRN WoW change %: To get the DSRN (Daily Sellable Room Nights) change  
           percentage week over week

---------- >>Following Charts on the dashboard;

        * Percentage of Revenue by Category
        * Trend by Key Metrics - ADR, RevPAR, Occupancy %
        * Realization % and ADR by Booking Platforms
        * Property by Key Metrics
        * Week over week changes of Key Metrics


---------- >>Key Insights

Atliq Bay has the highest occupancy rate of 66%
Atliq Exotica performs better compared to all other properties with 320M revenue, rating 3.62, occupancy % 57 and cancellation rate as 24.4%
Week 24 recorded the highest revenue among all which is 139.6M
Delhi city has the highest occupancy rate and rating followed by the cities Hyderabad, Mumbai, Bangalore
The management has lost around 298M due to booking cancellations
Elite type rooms have the most booking and as well higher cancellation rate.

snap of DashBoard :
![DashBoard](https://github.com/user-attachments/assets/66282f8f-2e7a-4ea6-a58c-a8d4feb07c2c)