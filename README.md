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

## Question 3:

### Question: Does the ESRB rating of a game affect the sales in each region provided in the dataset NA, Japan, Europe, Other, Global)? 

#### Method:
To answer this question, I first had to come up with a Null and Alternate hypothesis.  

Ho: The ESRB rating does not affect the unit sales of the game
Ha: The ESRB rating does affect the unit sales of the game

With that done, I needed to then start setting up my data frames and see if I need to clean the columns before proceeding.  When I ran a count on a groupby of the `Rating` column, I found that there were 4 Ratings that had less than 10 games contained from the data set.  I removed these by creating a new data frame `rating_df` which contained only the ESRB ratings with more than 10 games each.  The remaining were ESRB ratings of E, E10+, T, and M.  After creating a dataframe of only the ratings I needed, I then created a data frame for each ESRB rating for an ANOVA test at each stage.

For each region, I followed the same steps:
1.	Create a factorplot using seaborn ploting that regions sales on the y-axis and the rating on the x-axis.  
2.	I then ran an ANOVA test using the f_oneway() function from scipy’s stats package.
    * a.	For each of the calculations, I dropped the `na` values before calculating the f_oneway statistic.
![image](https://user-images.githubusercontent.com/20977403/42423002-4d9b65c0-82c0-11e8-8404-60f5cf4ef114.png)

3.	Repeated the above steps for each region.

#### Results:
Unsurprisingly, I found that most of the regions rejected the Ho stating that the ESRB rating does not have an effect on unit sales of the game in the region.  Each of the p-values when running the ANOVA test were well below the alpha selected.  Japan, however, did not show those same results.  I found that with an alpha = 0.01, we would fail to reject the null hypothesis that the ESRB rating does affect the unit sales of a game in Japan.  When running the ANOVA test on the different Rating specific data frames, we receive a p-value of 0.014270083344292479 which is higher than the set alpha.

**Japan:**
![image](https://user-images.githubusercontent.com/20977403/42423008-8fff2118-82c0-11e8-91c8-03cc34538b20.png)

## Question 4:

### Question: What is the most popular gaming platform for critically acclaimed games? User rated games?

#### Method:
For this question, I had to create two data frames with a groupby() and agg() function.  The first was using the `master` data frame and using a groupby() on the Platform with as_index = False to keep the index column.  After, I used an agg(“mean”) function to get the mean and dropped all `na` values.  Finally, I sorted this data frame by the critic score in descending order.  I used a similar method as illustrated above for the second data frame containing information on the user scores.

To construct the complete visualization at the end, I had to do a bit of additional manipulation of the data frame.  First, I had to merge the two created above on the `Platform`.  After, I had to figure out a way to make it so that the source of the score became a categorical variable that I could separate the data by when plotting the result.  The solution I came up with was using a pd.melt() using the `Platform` as the ID, the var_name became `ScoreSource` and the ‘value_name’ became the values.  This resulting data frame gave me two entries for each platform, a categorical variable for the kind of score, and what the value of the score was.  This enabled me to utilize seaborn’s barplot and set the `hue` to the ScoreSource giving me a side-by-side bar chart for each of the Platforms and how their average critic score and user score differ.

#### Results:
When using a .head() on both data frames for user score and critic score (grouped by the Platform), We see that the platform that has the highest average critic score for its games is the PC and the platform with the highest average user score for its games is the PS or PlayStation.  Below are the top 5 from each sorted data frame.  When diving deeper in to these platforms I found that the top game for PC from a critic’s stand point is Grand Theft Auto V and the top game from a user’s stand point is Castlevania: Symphony of the Night.

![image](https://user-images.githubusercontent.com/20977403/42423038-45b98750-82c1-11e8-8ea7-d43e67a19a7a.png)
![image](https://user-images.githubusercontent.com/20977403/42423040-4ce55108-82c1-11e8-86aa-3cab687beb0f.png)

The plot below shows the average critic score and user score for each platform.  It is sorted in descending order based on the critic score to give some structure.  The User score is encoded as BLUE and the Critic Score is encoded as ORANGE. It’s interesting to see that the Xbox One (XOne) has one of the highest critic scores, however the average user score of their games is the lowest values out of all of the platforms. The Game Boy Advance (GBA) is another interesting find that has the second highest average user rating for all games yet has a much lower average critic rating.

![image](https://user-images.githubusercontent.com/20977403/42423045-5a621708-82c1-11e8-89f1-1e9a3ba31184.png)

## Question 5:

### Question: What is the most popular video game genre based off number of copies sold in each nation available in the dataset (NA, Japan, Europe, Other, Global)?

#### Method:
For this I created a data frame from the cleaned `master` data frame grouped by the “Genre” and aggregated off the sum over the years in each region’s sales.  I dropped all `na` values in this step.  I then used a pd.merge on each of the five individual data frames to build one overall data frame.  I then used a simple .plot(kind=”bar”, x=”genre”) to visualize as this question was just out of curiosity from the data set.

#### Results:
![image](https://user-images.githubusercontent.com/20977403/42423057-97586856-82c1-11e8-9a63-27cafb064aa8.png)

**NA:** Action
**Japan:** Role-Playing
**EU:** Action
**Other:** Action
**Global:** Action

Japan was not only the only region that saw the most sold genre of games total, but also was the only country that beat North American unit sales by genre both of which were Role-Playing games.
