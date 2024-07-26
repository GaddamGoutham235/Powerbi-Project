

# Revenue Insights -Dashboard

## Problem Statement

- Identify the data sources pertaining to revenue management
- Clean and model the data as per requirement for analysis
- Create a revenue dashboard that measures important KPIs 
- Relevant filters need to provided to slice and dice the data
- The dashboard should depict both high level and granular insights


## Solution approach

- There are 5 tables provided for tracking revenue, 3 dimension tables (date, hotel, room) and 2 fact tables (bookings, aggregated bookings)
- Power BI was the tool used for creating the visualization/dashboard
- The data was imported, analysed and transformed as per necessity within Power Query
- The relationships between the tables were created within Power Pivot
-  Calculated column was created in which for week number and day type.

### For creating new column following DAX expression was written

    1) wn = WEEKNUM(dim_date[date])

    "To get the week number from the corresponding date."


    2) day type = 
    Var wkd = WEEKDAY(dim_date[date],1)
    return
    IF(
    wkd>5,""Weekend"",""Weekday"")

    Based on the feedback from stakeholder, we considered Friday and Saturday as weekend and weekdays from Sunday to Thurdsay. 
    In PowerBI, Sunday weekday number is 1, Monday is 2 and so on. So, if weekday number is greater than 5, then weekend or else weekday.

### For creating measures following DAX expression was written

1) Revenue	- To get the total revenue_realized	
    
        Revenue = SUM(fact_bookings[revenue_realized])

2) Total Bookings -	To get the total number of bookings happened	
    
        Total Bookings = COUNT(fact_bookings[booking_id])

3) Total Capacity -	To get the total capacity of rooms present in hotels
	
        Total Capacity = SUM(fact_aggregated_bookings[capacity])

4) Total Succesful Bookings	- To get the total succesful bookings happened for all hotels	

        Total Succesful Bookings = SUM(fact_aggregated_bookings[successful_bookings])

5) Occupancy %  -	"Occupancy means total successful bookings happened to the 
total rooms available(capacity)"	

    Occupancy % = DIVIDE([Total Succesful Bookings],[Total Capacity],0)

6) Average Rating	- Get the average ratings given by the customers	

        Average Rating = AVERAGE(fact_bookings[ratings_given])

7) No of days -	"To get the total number of days present in the data. In our case, we have data from May to July. So 92 days.

        No of days = DATEDIFF(MIN(dim_date[date]),MAX(dim_date[date]),DAY) +1

8) Total cancelled bookings	To get the"Cancelled" bookings out of all Total bookings happened	
    
        Total cancelled bookings = CALCULATE([Total Bookings],fact_bookings[booking_status]="Cancelled")

9) Cancellation %	"calculating the cancellaton percentage.

        Cancellation % = DIVIDE([Total cancelled bookings],[Total Bookings])

10) Total Checked Out -	To get the successful 'Checked out' bookings out of all Total bookings happened	
 
        Total Checked Out = CALCULATE([Total Bookings],fact_bookings[booking_status]="Checked Out")

11) Total no show bookings - "To get the""No Show"" bookings out of all Total bookings happened 

(""No show"" means those customers who neither cancelled nor attend to their booked rooms)"	

    Total no show bookings = CALCULATE([Total Bookings],fact_bookings[booking_status]="No Show")


12) No Show rate %	- calculating the no show percentage.

        No Show rate % = DIVIDE([Total no show bookings],[Total Bookings])

13) ADR - Calculate the ADR(Average Daily rate)

    It is the ratio of revenue to the total rooms booked/sold. 
It is the measure of the average paid for rooms sold in a given time period	

        ADR = DIVIDE( [Revenue], [Total Bookings],0)

14) Realisation %	- calculate  the realisation percentage.

    It is nothing but the succesful ""checked out"" percentage over all bookings happened.

        Realisation % = 1- ([Cancellation %]+[No Show rate %])      

15) RevPAR -	"Calculate the RevPAR(Revenue Per Available Room)

    RevPAR represents the revenue generated per available room, whether or not they are occupied. 
RevPAR helps hotels measure their revenue generating performance to accurately price rooms. 

RevPAR can help hotels measure themselves against other properties or brands."	

        RevPAR = DIVIDE([Revenue],[Total Capacity])
16) DBRN - calculate DBRN(Daily Booked Room Nights)

    This metrics tells on average how many rooms are booked for a day considering a time period

        DBRN = DIVIDE([Total Bookings], [No of days])

17) DSRN -	calculate DSRN(Daily Sellable Room Nights)

    This metrics tells on average how many rooms are ready to sell for a day considering a time period

        DSRN = DIVIDE([Total Capacity], [No of days])

18) DURN - calculate DURN(Daily Utilized Room Nights)

    This metric tells on average how many rooms are succesfully utilized by customers   
    for a day considering a time period

        DURN = DIVIDE([Total Checked Out],[No of days])

### Business outcomes

The following are some important business insights derived from the revenue dashboard:

- Mumbai generates highest revenue and Delhi the least revenue during May to Jul 2022.  Company need to focus on increasing the revenue in Delhi.

- The occupancy rate is higher during weekends across all cities, months and booking platforms. Leverage this insight to increase revenue generated during weekends.

- 70% of the bookings are checked out while 5% of booking don’t show up across all cities and booking platforms which means 75% of bookings generate revenue for AtliQ hotels. Identify and analyze the reasons for cancellations and try to reduce them

- Avg rating varies between 3.4 to 3.8 across cities and avg stay duration is 2.4 for each booking. Compare it with the industry benchmark across cities and evaluate the performance.

- Occupancy rate is highest at Delhi with 60+ % for all months though generates least revenue compared to other cities. Identify the reason for higher occupancy and use that to drive the revenue growth.

### Conclusion

- A revenue dashboard was built for AtliQ hotels depicting its various KPIs visually

- Relevant filters along with tooltips and interactions was provided in the dashboard

- This dashboard can be used for both high-level and in-depth analysis of KPIs across various dimensions









      



















