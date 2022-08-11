# A Udacity data gathering and wrangling project

For this project three data set are been used which are;
* tweet_json.txt ( scraped from twitter)
* image-predictions.tsv
* twitter_archive_enhanced.csv

## The objectives of the project

The aim and objective of this project is to wrangle the data of dog ratings, the data source is from
the twitter user @WeRateDogs. Gathering, assessing and cleaning would be done to prepare the
data for analysis. The process would be discussed as follows

## data gathering

The following code was used to scrape the tweet_json.txt file from twitter while the others were provided by the Udacity instructors
```
api = tweepy.API(auth, wait_on_rate_limit=True)
tweet_ids = df_A.tweet_id.values
len(tweet_ids)
#Query Twitter's API for JSON data for each tweet ID in the Twitter archive
count = 0
fails_dict = {}
start = timer()
#Save each tweet's returned JSON as a new line in a .txt file
with open('tweet_json.txt', 'w') as outfile:
     #This loop will likely take 20-30 minutes to run because of Twitter's rate limit
    for tweet_id in tweet_ids:
        count += 1
        print(str(count) + ": " + str(tweet_id))
        try:
            tweet = api.get_status(tweet_id, tweet_mode='extended')
            print("Success")
            json.dump(tweet._json, outfile)
            outfile.write('\n')
        except tweepy.TweepError as e:
            print("Fail")
            fails_dict[tweet_id] = e
            pass
end = timer()
print(end - start)
print(fails_dict) 

```
## data wrangling

I used two method to assess the data;
Visual assessment: i printed out all the data by calling their variable names used to store them to see hoh they look like  
Programmmatic assessment: i used the pandas function such as .info(), .describe(), .isnull(), .head() and .value_counts() method to assess the data

### data cleaning

At this point made a copy of all the data and created an new variable name by adding _clean to their previous variable names that is df_A as df_A_clean, image_prediction as image_prediction_clean and tweetcount as tweetcount_clean.
i then define the problem each data has, code the problem in order to solve it and test to see 
1. replaced the ’None’, ’a’, ’an’ and ’the’ etc. values with NaN using the replace method
2. replaced the ’None’ values with NAN using the replace method and dropping the NaN values
3. masked the df_A_clean to remove the retweeted data using the isnull() method
4. used the pandas drop method to drop the columns ’in_reply_to_status_id’, ’in_reply_to_user_id’, ’retweeted_status_id’,   ’retweeted_status_user_id’, ’retweeted_status_timestamp all have more than 50% of its data missing
5. changed the datatype of the ’timestamp’ column using the pandas to_datatime funtion
6. used the regular expression method to extract the name of the source
7. used the pandas rename method to rename the colums ’img_num’ and ’jpg_url’
8. used the pandas rename method to rename the ’id’ column
9. used the pandas DatetimeIndex function to split the ’timestamp’ into ’year’, ’month’ and ’day’
10. used the pandas string(split) and indexing method to remove dulipcate links
11. i merged the three dataframes together

### data storing

After the gathering assessing and cleaning processes were concluded i saved the merged data in a csv file named twitter_archive_master.csv

# conclusion

The data wrangling process was quite hectic and tidious but i was able to apply the methods that i learnt from the course which was exciting.
