# Booking.com-challenge

Data preprocessing

Raw dataset was grouped by ‘utrip_id’ so, cities are ordered and grouped within the
same trip. Then the ordered cities are paired like (departure city, arrival city), for instance, if a
man goes through A, B, anc C, they are paired like (A,B), and (B,C). The same paired rows
were counted. So, final transformed data have three columns ‘departure_city’, ‘arrvial_city’, and
‘count’. In this dataset, the total number of unique paired cities are more than three hundred
thousand rows.
For lift based bayesian theorem, another dataset was generated. From the raw data,
after grouping by ‘utrip_id’, the first city and last city in the same trip were extracted to make a
dataframe. And then the same paired cities are counted and the data frame has three columns,
‘first_city’, ‘fourth_city’, and ‘count’.


Modelling

The overview of this model is that a city_id from the transformed dataset passes through
three gaussian distributions and returns a fourth city list. Through another dataset and bayesian
theorem, four cities are selected for prediction out of the list. In the gaussian distribution part, if
a city is given, the function returns the rows given city is departure_city. And then the function
calculates the mean and standard deviation of the ‘count’. For example, given city id is 33408,
and it returns many rows. The mean and std of the rows of column ‘count’ is 4,78 and 11.97
which means the values are distributed widely. So, only the rows having values greater than 1.5
sigma, about 6.7%, were selected. However, if there are no values greater than 1.5 sigma, the
biggest values are selected and go to the second list. However, after first processing, five cities
were generated, which means there were 5 cities greater than 1.5 sigma. This processing was
repeated to each element and then a third list having 25 cities was generated. After one more
repeat, a fourth city list was generated having 114 cities. However, for prediction, only four cities
are needed, so, lift based bayesian theorem was used to extract four cities.
Formula for lift is seen in (1),
Lift=P(A and B)/P(A)*P(B) (1)
For this calculation, a second transformed dataset was used. This data frame has three
columns, ‘first_city’, ‘fourth_city’, and ‘count’. So, the given city_id, 33408, goes to the ‘first_city’
column to return other values.
P(A and B)= the number of (first_city, fourth_city) in the second dataframe/sum of corresponding
‘count’ in the second dataframe (2)
P(A) =the ’count’ of each fourth city element from the ‘fourth_cites’/sum of corresponding ‘count’
in the second
(3)
P(B) =the ‘count’ of first city in new dataframe /sum of corresponding ‘count’ in the second
dataframe (4)
P(A|B)= Lift*P(A) (5)
In fourth list, there are many duplicate cities, so to get final value, P(A|B) and column the
‘count’ of fourth cities are multiplied and then the list ordered by descending order. Finally, the
first four rows were extracted from the dataframe for prediction.
