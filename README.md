# Video_Game_Analysis

## Overview

The motivation for this project came from opportunity and curiosity.  I have played video games for most of my life and, between my friends and I, have played hours on every system that has been available in the US.  When I found this dataset on Kaggle, it seemed interesting to me since I could answer some questions I’ve had while playing video games.  The four (plus one out of general curiosity after exploring the data some more) questions I set out to answer were the following:


1.	Who is the most acclaimed game developer by critics/users?
2.	What was the most profitable sales year for video games globally? Japan? North America? Europe? Other?
3.	Does the ESRB rating of a game affect the sales in each region provided in the dataset NA, Japan, Europe, Other, Global)? 
4.	What is the most popular gaming platform for critically acclaimed games? User rated games?
5.	Curiosity
a.	What is the most popular video game genre based off number of copies sold in each region available in the dataset (NA, Japan, European union, Other, Global)?

The dataset I worked with was downloaded from Kaggle at https://www.kaggle.com/rush4ratio/video-game-sales-with-ratings.  The data is compiled from an existing dataset (found at https://www.kaggle.com/gregorut/videogamesales which included all the video game sales data) and a web scrape from Metacritic that the author of the dataset added.  The updated version of this dataset was pulled on Dec 22, 2016 and contains information on roughly 16,719 video games with complete sets of roughly ~7,500 games with critic and user scores. The reason for the loss of critic and user score data is that some gaming platforms are not supported on Metacritic.

## Metadata
![image](https://user-images.githubusercontent.com/20977403/42422890-79d7a2fa-82bd-11e8-8164-e9c03ea10a6d.png)

To get the cleaned “master” data frame, I followed the following steps:

1.	Convert the type of `Year_of_Release` into an `np.int64` type.  
    * a.	This was to remove the trailing 0.0 from all values for cleaner visualizations in the future.
    * b.	I also filled in missing values with a `0` so I could remove them easier in later visualizations

![image](https://user-images.githubusercontent.com/20977403/42422848-8e2a2b5c-82bc-11e8-8cbe-4940174655d6.png)

2.	Removed Platforms with a total game count of less than 10.  
    * a.	This was to remove platforms from analysis with little to no games contained in them.

![image](https://user-images.githubusercontent.com/20977403/42422900-a7656e00-82bd-11e8-864b-9c24c8e589be.png)

3.	Converted the `User_Score` column to numeric type and multiplied each value by 10 to mimic the values in `Critic_Score`.
    * a.	Critic scores are measured out of 100 and User score was originally measured out of 10.  I had to convert the objects to numeric since there was apparently an issue with loading the type of this column into Pandas.  According to the Metadata, this step should not have been needed.

![image](https://user-images.githubusercontent.com/20977403/42422903-bbd798ea-82bd-11e8-9f69-6efc4bf95e1c.png)

## Question 1:

### Question: Who is the most acclaimed game developer by critics/users?

#### Method:
The method I used to answer this question was to create 2 new data frames using the groupby() method on the developer.  One was for the `Critic_Score` which aggregated the mean score of each Developer, dropped null values, and sorted the values in descending order.  The other was for the `User_Score` which followed the same steps as the Critic score data frame.  Last, I merged the two data frames together to get a side-by-side comparison of how the developer scored on both a critic score and user score.  The final two data frames are sorted based of one of the two values giving us a view of the top 15 developers based on user score and critic score.

#### Results:
I found that when sorting the average rating for developer's based off the critic score, we see that the top 5 are Irrational Games, 2K Marin with a score of 96.0; Digital Extremes, 2K Marin with a score of 94.0; Kojima Productions, Moby Dick Studio with a score of 94.0; Bungie Software with a score of 93.66; and DMA Design, Rockstar North with a score of 93.0.

When looking at the average rating for developer's based off the user score, we see that the top 5 are Infinite Dreams, Paragon 5 with a score of 95.0; Inferno Games with a score of 95.0; Tecmo, Graphic Research with a score of 94.0; Pax Softonica with a score of 93.0; and Telenet with a score of 93.0.

![image](https://user-images.githubusercontent.com/20977403/42422913-f3aa7b98-82bd-11e8-915b-4ff0e503f959.png)

![image](https://user-images.githubusercontent.com/20977403/42422915-f914633c-82bd-11e8-99fc-d8ec55ba032f.png)

## Question 2:

### Question: What was the most profitable sales year for video games globally? Japan? North America? European Union? Other?

#### Method:
To start setting up this question, I had to create a new data frame from the ‘master’ cleaned data frame with the `Year_of_Release` not including years == 0 and years that were > 2016.  I was able to do this cleaning because of the second step when cleaning the `master` data frame that filled all the `na` values with ‘0’.  Also, since this data set was current as of Dec 22, 2016, any data with a release year that was greater than 2016 could not be used.  

To answer each part of the overall question, I had to break the cleaned data frame above into 5 subsets to calculate the average unit sales for each region.  
1.	I did this by using a groupby function on the cleaned data frame above on the `Year_of_Release` and setting as_index=False.  
2.	I then used .agg(“mean) to get the mean sales for each year and sorted the values by the region’s sales in descending order.  
3.	The data frame was then visualized using seaborn barplot.  Manipulation of the x-axis labels to rotate 40 degrees was done for readability.  
4.	Above the print out of the bar plot, I have the top 5 years sorted by sales for that data frame to get the exact year and value using `.head()`.

I repeated the steps above 4 more times on each regions data to get the answer for each region.

#### Results:

![image](https://user-images.githubusercontent.com/20977403/42422926-56ce02bc-82be-11e8-8cb0-a00f1504d1b8.png)

![image](https://user-images.githubusercontent.com/20977403/42422931-67e95254-82be-11e8-881b-4851369ce0db.png)

![image](https://user-images.githubusercontent.com/20977403/42422938-7c9a19ae-82be-11e8-8621-4078c4d24686.png)

![image](https://user-images.githubusercontent.com/20977403/42422940-8db102ac-82be-11e8-8840-c5bfe97d83ec.png)

![image](https://user-images.githubusercontent.com/20977403/42422945-af69918e-82be-11e8-924a-eca89f1f1ee6.png)

In all of the graphs above, it would appear that around 1989 video games were at their peak.  Generally, most of the regions aw a decline in the volume of video game sales except for Other Countries and the European Union.  Both of those regions seem to still be rising as time does on signifying that video games are still growing in popularity around those regions. Globally, video game sales have remained fairly constant since the 2000’s.
