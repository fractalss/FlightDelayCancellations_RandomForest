###Predicting Cancellation and Delay of Flight originating from aiports in Texas, 2015
Results
https://public.tableau.com/profile/fractalss#!/vizhome/Predict_Delay_Cancel_Texas_Flights/Cancel_Flights_Exploration

##Data Source
The U.S. Department of Transportation's (DOT) Bureau of Transportation Statistics tracks the on-time performance of domestic flights operated by large air carriers. Summary information on the number of on-time, delayed, canceled, and diverted flights is published in DOT's monthly Air Travel Consumer Report and in this dataset of 2015 flight delays and cancellations. https://www.kaggle.com/usdot/flight-delays

The orginal dataset was huge, so only flights originating from airports in Texas were considered. This is discussed in the Data Cleaning Section and in the notebook.

##Data Cleaning
Kaggle dataset had three CSV formatted files containing an airline list, an airport list and flight records in the year 2015, all over US. The flight file contained information around 6 million records, which consumed more than 1 GB of RAM. This was difficult to deal with, and there were Tableau, github issues. So, only the records that had fligths originating from airports in Texas were considered. Used the smallest integer data types possible, converted to many categorial data to their respective numeric values (already sounding like label encoding).

There were other issues with the data, mentioning only some :

For the entire month of October, the origin and destination airport fields were recorded incorrectly with 5- digit airport IDs instead of IATA 3-letter codes. Using the airport data, a mapping was built, and inconsistencies in October flight records were rectified with corresponding IATA codes.

Various timestamp and time interval fields were empty. Timestamps were set to -1 to indicate “unavailable” and time intervals were set to 0.

Using all of these, the RAM usage came down to around ~400 MB. This was still problematic for the time in hand.

So, only the airports that had flights originating from the Texas airports were considered. This brought down the RAM usage to around manageable 50 MB.

##Classifying flights that were Cancelled and not Cancelled
The flights had only 1% records that were cancelled. This was really challenging to deal with. Even though with grid-search and other ML models, the accuracy was around 99% or more, but all of them got a 0 in F1 score for the "cancelled flights". This is an imbalanced dataset and it was dealt with upsampling the minority class (cancelled flights) of data. By using Random Forest and upsampling reasonably good F1 scores were obtained for the cancelled flights.

##Classifying flights that were not Delayed, had a short delay (<15 min) and a longer delay (>15 min)
In reality, flights that have a shorter departure delay are not well documented in the airlines industry. This is simply because, flights with shorter departure delay can arrive on time at the destination airport. Therefore there would always be lack of defining features that can predict shorter delay with some degree of confidence. Even though the Random forest had good score for non-delayed and longer-delayed flights, it had okay score for shorter delay flights, which is understandable. A grid-search to tune hyperparameters were done to get the best scores in this instance.:1

