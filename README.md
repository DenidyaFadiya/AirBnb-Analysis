# Data_Exploratory-AirBnbAnalysis
This analysis is used to see customers preference when booking AirBnb in NYC

<p align="center">
  <img width="550" height="400" src="https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/8dc5b8c6-600c-4914-a63f-949c5c5a86d2">
</p>


In this Project I will be exploring customer's preference when they are booking their AirBnb in New York City. I will be seeing customer's preference based on the Area and type of room they are choosing.

## Full Query for Data Exploratory Project
**Check the Full Query for Data Exploratory Project [here](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/blob/main/Full%20Query%20-%20AirBnb%20New%20York).**

**Tableau Interactive Dashboard [here]
(https://public.tableau.com/app/profile/denidya.fadiya/viz/AirBnbAnalysis_16914000059400/Dashboard1)**

In this Data Exploratory project I will be cleaning data that I took from [Kaggle Data Source]([https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot](https://www.kaggle.com/datasets/arianazmoudeh/airbnbopendata)).

## Exploratory Data Project


### How many AirBnb are there in New York City?
Manhattam has  27.649, Brooklyn	has 25.997, Queens 8.555, Bronx	1,765. Staten Island 596
 ```
select 
	neighbourhood_group,
	COUNT(neighbourhood_group) as CountofNG
from opendata
group by neighbourhood_group
order by CountofNG desc
```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/2c1bb6c9-5fd8-44c8-ac01-91cb5f351c8f)

![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/bb346a2f-d4a0-4c0e-8203-14dc97b52de5)


### Average Price and Average Fee per Area / Neighbourhood
Bronx price average is the highest among the area stated with average $632/night and Hotel Room price is the highest in Room Type with average $663/night.
```
--Average Price and Average Fee per Area / Neighbourhood
select 
	neighbourhood_group,
	count(neighbourhood_group) as CountOfArea,
	round(avg(price),1) as AvgPrice,
	round(avg(service_fee),1) as AvgFee
from opendata
group by neighbourhood_group
order by AvgPrice desc

--Average Price and Average Fee per Room Type
select 
	room_type,
	count(room_type) as CountOfType,
	round(avg(price),1) as AvgPrice,
	round(avg(service_fee),1) as AvgFee
from opendata
group by room_type
order by AvgPrice desc
```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/fc2a2a89-eb0b-4177-9188-ffb43f5afea0)

![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/f4c3f161-34b0-48f6-a54d-6cf4f1de873e)


### how about the spreading of room type in each neighbourhood?
The result shows that each of the Room type ; Home/Apartment, Hotel Room,
Shared room are mostly rented in Manhattan, but customer prefer Private room 
when they are in Brooklyn Area
```
--- how about the spreading of room type 
--in each neighbourhood?
select
	room_type,
	neighbourhood_group,
	COUNT(neighbourhood_group) as CountPerType
from opendata
group by room_type, neighbourhood_group
order by room_type, CountPerType desc
```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/a786bebc-668f-4a2b-8b08-4e7acf8aba91)

![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/0c811984-056d-4d5e-ba40-6b48e8da645b)


### which of the policy does the customers prefer?
The customers are avoiding the strict cancellation policy, which it does make
a lot of sense, since it is part of the service if the customers wants 
to cancel their AirBnb booking easily. 
```
---which of the policy does the customers prefer?
select cancellation_policy,
		count(cancellation_policy) as CancelPref,
		round(count(*) * 100.0 / sum(count(*)) over(),1) as PctCancel
from OpenData
group by cancellation_policy 
order by CancelPref desc

--the elaboration for cancellation policy
select cancellation_policy,
		neighbourhood_group,
		count(cancellation_policy) as CancelPref
from OpenData
group by cancellation_policy, neighbourhood_group
order by cancellation_policy, CancelPref desc

```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/c5ed7036-8667-4738-ae7f-e4222680787a)

![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/c2fc983e-462f-4c2c-9fc5-08ababd960b9)

 

### which of the bookable regulation does the customers prefer?
The customers have no problem with the booking regulation, they are fine with
the non-instant booking policy. 
```
---which of the bookable regulation
--does the customers prefer?
select instant_bookable,
		count(instant_bookable) as BookPref,
		round(count(*) * 100.0 / sum(count(*)) over(),1) as PctBook
from OpenData
group by instant_bookable 
order by  BookPref desc

--the elaboration for bookable regulation
select instant_bookable,
		neighbourhood_group,
		count(instant_bookable) as CancelPref
from OpenData
group by instant_bookable, neighbourhood_group
order by instant_bookable, CancelPref desc
```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/c0cc208b-b9fd-43c5-9efd-1ed5d509dbbd)

![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/e1f5d862-9c5b-414b-be73-f9f8e1357517)


### how long does the customers like to stay?
98.5% or customers are renting for less than a month.
```
----how long does the customers like to stay?
select 
		stay_category,
		count(stay_category) as Nights,
		round(count(*) * 100.0 / sum(count(*)) over(),1) as  NightsPct
from OpenData
where minimum_nights > 0
group by stay_category
order by Nights desc

```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/4121beb2-9eed-4e84-9693-ae1cb4196fcf)

![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/51526a03-76ba-4c10-b65c-7da4cec6588d)


### How satisfied are the customer with AirBnb? 
Ratings are not that quite good, the spreading of rating are almost equal for all stars.
and the average rating is only 3 star. 
```
select rating,
		count(rating) as AreaRate,
		round(count(*) * 100.0 / sum(count(*)) over(),1) as  RatePct
from opendata
group by rating
order by AreaRate desc
```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/af444aeb-64af-4e93-94d1-ecba9b179ff3)
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/c7691678-05b1-4f59-bdcd-fef7d7dd19b9)


Weighted Average Rating
```
--average rate for AirBnb
with cte_rate
as (
select rating,
		count(rating) as CountofRating
from opendata
group by rating
)
select 
    sum(Rating * CountofRating) / sum(CountofRating) as AvgRating
from cte_rate;
```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/d4966d57-bc45-40dd-bd36-9ad2f2acea57)
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/761f2fbf-f3b9-400b-87b7-147d596d0c89)


### which neighbourhood_group has the best Airbnb ratings?
Most Bronx customers are not happy with their AirBnb which their highest rate is only star 3.
```
---which neighbourhood_group has the best Airbnb ratings?
select neighbourhood_group,
		rating,
		count(rating) as AreaRate
from opendata
group by neighbourhood_group, rating
order by neighbourhood_group, AreaRate desc
```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/3916cf49-3ec8-4dd9-8aba-7e1fe567758b)

![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/9c1f5b8c-bb2c-46df-90f0-ae5bddd25904)


### which room_type has the best Airbnb ratings?
Customers are satisfied with the room type. Almost all room type are rated with 5 star but shared room are the only one who gets rated 4 star out of them.
```
---which room_type has the best Airbnb ratings?
select room_type,
		rating,
		count(rating) as AreaRate
from opendata
group by room_type, rating
order by room_type, AreaRate desc
```
![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/b1f80023-9afc-4158-9243-758a28e556b5)

![image](https://github.com/DenidyaFadiya/Data_Exploratory-AirBnbAnalysis/assets/129844542/8e2ae9f7-05ed-4344-8e74-6d4b7f9bb0f9)

