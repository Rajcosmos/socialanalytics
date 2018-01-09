

```python
### Hi, this is a sentiment analysis of the Twitter activity for 5 News Media - BBC, CBS, CNN, Fox and New York times
### We are charting 2 plots - scatter using matplotlib and Bar graph using Seaborn libraries
###  Observation, 
### Ran the script 3 days consecutive just to collect samples and understand the variance in the sentiments of the 5 different media news
### 1. By looking at scatter plot (relation between tweets polarity and tweets time sorted, the tweet polarity is ranging from neutral (0) to -ve sentiments for most of the nytimes and foxnews. Only BBC,CBS and CNN were ranging between +ve and neutral
#### Continue #1-> conclusion, nytimes and Foxnews focus on somewhat -ve news.
####2. By looking at bar chart, CBS has overall highest positive tweets. 
### 3. Also plotted -ve average sentiments to check on which media is most overall sentiment -ve, mostly Fox and NY times were slightly have -ve sentiments comparitive to other 3 media sources
```


```python
## Defining dependencies 
import json
import tweepy
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import time 
from datetime import datetime 
import numpy as np
## Using Vader Sentiment to analyse Sentiment
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer= SentimentIntensityAnalyzer()
```


```python
#seaborn settings
sns.set_palette("deep",desat=.6)
sns.set_context(rc={"figure:figsize":(8,4)})
```


```python
## Twitter authentication details
consumer_key = "akczLSsb7D39rCQITi6HKKTbS"
consumer_secret = "ZzlplKxUh1agPMHRjouqC1E3T6gdssATmU2e1kiFupNodEwmrH"
access_token = "41056206-bpASiEYIYW7gti7fvZjm1ckaQfskqwMfUIDyfcGRQ"
access_token_secret = "AYjW9Maj2nOELJ7LLEj6YrfXmVqjYWN0lBpX4ariTXED2"
filename = 'apikey'
def get_file_contents(filename):
    """ Given a filename,
        return the contents of that file
    """
    try:
        with open(filename, 'r') as f:
            for row in f:
                consumer_key = row[0]
            return f.read().strip()
    except FileNotFoundError:
        print("'%s' file not found" % filename)

#api_key = get_file_contents(filename)
#print("Our API key is: %s" % (api_key))

```


```python
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token,access_token_secret)
api=tweepy.API(auth, parser=tweepy.parsers.JSONParser())
```


```python
target_user = ("@BBC","@CBS","@CNN","@FoxNews","@nytimes")
```


```python
compound_list = []
positive_list = []
negative_list = []
neutral_list = []
user_list = []
tweet_times = []
tweet_count = []
tweet_text = []
tweet_compound_mean = []
tweet_negative_mean = []
```


```python
## Function to capture compound mean that we can use in Seaborn plotting
def tweet_mean_call():
        tweet_compound_mean.append(np.mean(compound_list))
        tweet_positive_mean.append(np.mean(positive_list))
        tweet_negative_mean.append(np.mean(negative_list))

for user in target_user:
   
    counter=1
    
    for x in range(5):
        public_tweets = api.user_timeline(user,page=20)
        for tweet in public_tweets:
            #print("Tweet from %s #%s:%s" %(user,counter,tweet["text"]))
            compound = analyzer.polarity_scores(tweet["text"])["compound"]
            pos = analyzer.polarity_scores(tweet["text"])["pos"]
            neu = analyzer.polarity_scores(tweet["text"])["neu"]
            neg = analyzer.polarity_scores(tweet["text"])["neg"]
            tweet_datetime = datetime.strptime(tweet["created_at"],"%a %b %d %H:%M:%S %z %Y")
            print("Tweet from %s #%s created on %s:%s" %(user,tweet_datetime,counter,tweet["text"]))
        
            compound_list.append(compound)
            positive_list.append(pos)
            negative_list.append(neg)
            neutral_list.append(neu)
            user_list.append(user)
            tweet_times.append(tweet_datetime)
            tweet_count.append(counter)
            tweet_text.append(tweet["text"])
            counter = counter +1

    #print("Total number of tweets %s" %str(counter -1))
    print(" ---------------TWEET COMPLETED FOR %s-------------" %user)
    
    # Calculating 
    tweet_mean_call()
    
            
```

    Tweet from @BBC #2017-12-29 13:41:30+00:00 created on 1:RT @BBCCiN: Best Day Ever? You weren't kidding!
    
    Special thanks to @NiallOfficial​ for topping off a wonderful day for some of the young ca…
    Tweet from @BBC #2017-12-29 13:32:04+00:00 created on 2:Ever wondered what makes a wet dog smell? 🤔🐶 10 mysteries you’d like to get to the bottom of https://t.co/MthZREY7Ui https://t.co/o0LwuuXTlk
    Tweet from @BBC #2017-12-29 13:00:06+00:00 created on 3:How do we control our body temperature? ❄️🌡
    
    Dr Chris and Dr Xand investigate... https://t.co/kXW1DnCDax https://t.co/F1cyigzOfG
    Tweet from @BBC #2017-12-29 12:32:03+00:00 created on 4:From Trump's presidency to the FIFA corruption scandal, The Simpsons has a habit of predicting the future. 
    
    What p… https://t.co/VN0aP5WzSQ
    Tweet from @BBC #2017-12-29 12:25:38+00:00 created on 5:RT @bbc5live: "One of the worst films I can remember seeing for many a year... I hope to never have to mention it or think about it ever ag…
    Tweet from @BBC #2017-12-29 12:00:02+00:00 created on 6:2016 was Rio's deadliest year on record. Now people are using apps to help keep themselves safe. 📱
    
    Via @BBCClick https://t.co/JCPqSWIjir
    Tweet from @BBC #2017-12-29 11:40:03+00:00 created on 7:From a short story collection to a century-spanning novel about a Korean family - are these the 10 best books of 20… https://t.co/HZ77Ygy3QW
    Tweet from @BBC #2017-12-29 11:29:09+00:00 created on 8:RT @BBCRadio2: Rumour has it, they've since healed the rift... 👏
    Enjoy the sounds of @liamgallagher (and his pencil sharpener) In Concert f…
    Tweet from @BBC #2017-12-29 11:15:04+00:00 created on 9:🏒 Ice Hockey’s Transgender Star: Harrison Browne is a professional ice-hockey player, also known for being the firs… https://t.co/AC7objhq4W
    Tweet from @BBC #2017-12-29 10:49:03+00:00 created on 10:Do you have the gumption to become a fully-fledged massive snob? Take this @BBCRadio4 quiz to find out!… https://t.co/WZ2tC5FXeZ
    Tweet from @BBC #2017-12-29 10:29:32+00:00 created on 11:RT @BBCNewsbeat: What it's like to grow up in war zones around the world: @UNICEF is highlighting the dangers faced by young people https:/…
    Tweet from @BBC #2017-12-29 10:14:04+00:00 created on 12:No regrets? Research suggests that regret 'improves children's decision-making'. 🤔 https://t.co/kdcQjzkIZV https://t.co/AYJDPrtvBv
    Tweet from @BBC #2017-12-29 09:56:19+00:00 created on 13:RT @BBCWomansHour: At 10am ⏰: Over 3600 pregnant women and children live in temporary accommodation in Birmingham alone. Why are the number…
    Tweet from @BBC #2017-12-29 09:45:27+00:00 created on 14:RT @bbcwritersroom: The #writer behind the success of Morecambe &amp; Wise, Eddie Braben, and the immense pressure to create shows watched by u…
    Tweet from @BBC #2017-12-29 09:30:06+00:00 created on 15:Born deaf, Jamie never thought it would be possible to be an actor. He's proved himself wrong. 🎭 (via @BBCStories) https://t.co/jDMzxqLIuM
    Tweet from @BBC #2017-12-29 09:03:05+00:00 created on 16:Knocking on a door? A Formula 1 car? Nope. This is the sound two icebergs make when they rub together. ❄️ 🔊… https://t.co/AsKOsZHOoF
    Tweet from @BBC #2017-12-28 21:31:04+00:00 created on 17:🎶 @BrunoMars: Live in Harlem. 10:25pm on @BBCOne. 💜 https://t.co/WoGSJ3JvdV https://t.co/jKLXAp3cTC
    Tweet from @BBC #2017-12-28 20:29:04+00:00 created on 18:Fidget spinners and more: the top Google searches of 2017 📱🤳https://t.co/qRwxjAH1DF https://t.co/hVjxB8GvNL
    Tweet from @BBC #2017-12-28 20:05:03+00:00 created on 19:The theatre shows you won’t want to miss in 2018 🎭🎟 https://t.co/mpFBZ0wYcU https://t.co/9wNat5aZGB
    Tweet from @BBC #2017-12-28 19:30:03+00:00 created on 20:This first aid phone app is saving lives.📱🏥
    
    Via BBC World Hacks https://t.co/wISQ2tt4si
    Tweet from @BBC #2017-12-29 13:41:30+00:00 created on 21:RT @BBCCiN: Best Day Ever? You weren't kidding!
    
    Special thanks to @NiallOfficial​ for topping off a wonderful day for some of the young ca…
    Tweet from @BBC #2017-12-29 13:32:04+00:00 created on 22:Ever wondered what makes a wet dog smell? 🤔🐶 10 mysteries you’d like to get to the bottom of https://t.co/MthZREY7Ui https://t.co/o0LwuuXTlk
    Tweet from @BBC #2017-12-29 13:00:06+00:00 created on 23:How do we control our body temperature? ❄️🌡
    
    Dr Chris and Dr Xand investigate... https://t.co/kXW1DnCDax https://t.co/F1cyigzOfG
    Tweet from @BBC #2017-12-29 12:32:03+00:00 created on 24:From Trump's presidency to the FIFA corruption scandal, The Simpsons has a habit of predicting the future. 
    
    What p… https://t.co/VN0aP5WzSQ
    Tweet from @BBC #2017-12-29 12:25:38+00:00 created on 25:RT @bbc5live: "One of the worst films I can remember seeing for many a year... I hope to never have to mention it or think about it ever ag…
    Tweet from @BBC #2017-12-29 12:00:02+00:00 created on 26:2016 was Rio's deadliest year on record. Now people are using apps to help keep themselves safe. 📱
    
    Via @BBCClick https://t.co/JCPqSWIjir
    Tweet from @BBC #2017-12-29 11:40:03+00:00 created on 27:From a short story collection to a century-spanning novel about a Korean family - are these the 10 best books of 20… https://t.co/HZ77Ygy3QW
    Tweet from @BBC #2017-12-29 11:29:09+00:00 created on 28:RT @BBCRadio2: Rumour has it, they've since healed the rift... 👏
    Enjoy the sounds of @liamgallagher (and his pencil sharpener) In Concert f…
    Tweet from @BBC #2017-12-29 11:15:04+00:00 created on 29:🏒 Ice Hockey’s Transgender Star: Harrison Browne is a professional ice-hockey player, also known for being the firs… https://t.co/AC7objhq4W
    Tweet from @BBC #2017-12-29 10:49:03+00:00 created on 30:Do you have the gumption to become a fully-fledged massive snob? Take this @BBCRadio4 quiz to find out!… https://t.co/WZ2tC5FXeZ
    Tweet from @BBC #2017-12-29 10:29:32+00:00 created on 31:RT @BBCNewsbeat: What it's like to grow up in war zones around the world: @UNICEF is highlighting the dangers faced by young people https:/…
    Tweet from @BBC #2017-12-29 10:14:04+00:00 created on 32:No regrets? Research suggests that regret 'improves children's decision-making'. 🤔 https://t.co/kdcQjzkIZV https://t.co/AYJDPrtvBv
    Tweet from @BBC #2017-12-29 09:56:19+00:00 created on 33:RT @BBCWomansHour: At 10am ⏰: Over 3600 pregnant women and children live in temporary accommodation in Birmingham alone. Why are the number…
    Tweet from @BBC #2017-12-29 09:45:27+00:00 created on 34:RT @bbcwritersroom: The #writer behind the success of Morecambe &amp; Wise, Eddie Braben, and the immense pressure to create shows watched by u…
    Tweet from @BBC #2017-12-29 09:30:06+00:00 created on 35:Born deaf, Jamie never thought it would be possible to be an actor. He's proved himself wrong. 🎭 (via @BBCStories) https://t.co/jDMzxqLIuM
    Tweet from @BBC #2017-12-29 09:03:05+00:00 created on 36:Knocking on a door? A Formula 1 car? Nope. This is the sound two icebergs make when they rub together. ❄️ 🔊… https://t.co/AsKOsZHOoF
    Tweet from @BBC #2017-12-28 21:31:04+00:00 created on 37:🎶 @BrunoMars: Live in Harlem. 10:25pm on @BBCOne. 💜 https://t.co/WoGSJ3JvdV https://t.co/jKLXAp3cTC
    Tweet from @BBC #2017-12-28 20:29:04+00:00 created on 38:Fidget spinners and more: the top Google searches of 2017 📱🤳https://t.co/qRwxjAH1DF https://t.co/hVjxB8GvNL
    Tweet from @BBC #2017-12-28 20:05:03+00:00 created on 39:The theatre shows you won’t want to miss in 2018 🎭🎟 https://t.co/mpFBZ0wYcU https://t.co/9wNat5aZGB
    Tweet from @BBC #2017-12-28 19:30:03+00:00 created on 40:This first aid phone app is saving lives.📱🏥
    
    Via BBC World Hacks https://t.co/wISQ2tt4si
    Tweet from @BBC #2017-12-29 13:41:30+00:00 created on 41:RT @BBCCiN: Best Day Ever? You weren't kidding!
    
    Special thanks to @NiallOfficial​ for topping off a wonderful day for some of the young ca…
    Tweet from @BBC #2017-12-29 13:32:04+00:00 created on 42:Ever wondered what makes a wet dog smell? 🤔🐶 10 mysteries you’d like to get to the bottom of https://t.co/MthZREY7Ui https://t.co/o0LwuuXTlk
    Tweet from @BBC #2017-12-29 13:00:06+00:00 created on 43:How do we control our body temperature? ❄️🌡
    
    Dr Chris and Dr Xand investigate... https://t.co/kXW1DnCDax https://t.co/F1cyigzOfG
    Tweet from @BBC #2017-12-29 12:32:03+00:00 created on 44:From Trump's presidency to the FIFA corruption scandal, The Simpsons has a habit of predicting the future. 
    
    What p… https://t.co/VN0aP5WzSQ
    Tweet from @BBC #2017-12-29 12:25:38+00:00 created on 45:RT @bbc5live: "One of the worst films I can remember seeing for many a year... I hope to never have to mention it or think about it ever ag…
    Tweet from @BBC #2017-12-29 12:00:02+00:00 created on 46:2016 was Rio's deadliest year on record. Now people are using apps to help keep themselves safe. 📱
    
    Via @BBCClick https://t.co/JCPqSWIjir
    Tweet from @BBC #2017-12-29 11:40:03+00:00 created on 47:From a short story collection to a century-spanning novel about a Korean family - are these the 10 best books of 20… https://t.co/HZ77Ygy3QW
    Tweet from @BBC #2017-12-29 11:29:09+00:00 created on 48:RT @BBCRadio2: Rumour has it, they've since healed the rift... 👏
    Enjoy the sounds of @liamgallagher (and his pencil sharpener) In Concert f…
    Tweet from @BBC #2017-12-29 11:15:04+00:00 created on 49:🏒 Ice Hockey’s Transgender Star: Harrison Browne is a professional ice-hockey player, also known for being the firs… https://t.co/AC7objhq4W
    Tweet from @BBC #2017-12-29 10:49:03+00:00 created on 50:Do you have the gumption to become a fully-fledged massive snob? Take this @BBCRadio4 quiz to find out!… https://t.co/WZ2tC5FXeZ
    Tweet from @BBC #2017-12-29 10:29:32+00:00 created on 51:RT @BBCNewsbeat: What it's like to grow up in war zones around the world: @UNICEF is highlighting the dangers faced by young people https:/…
    Tweet from @BBC #2017-12-29 10:14:04+00:00 created on 52:No regrets? Research suggests that regret 'improves children's decision-making'. 🤔 https://t.co/kdcQjzkIZV https://t.co/AYJDPrtvBv
    Tweet from @BBC #2017-12-29 09:56:19+00:00 created on 53:RT @BBCWomansHour: At 10am ⏰: Over 3600 pregnant women and children live in temporary accommodation in Birmingham alone. Why are the number…
    Tweet from @BBC #2017-12-29 09:45:27+00:00 created on 54:RT @bbcwritersroom: The #writer behind the success of Morecambe &amp; Wise, Eddie Braben, and the immense pressure to create shows watched by u…
    Tweet from @BBC #2017-12-29 09:30:06+00:00 created on 55:Born deaf, Jamie never thought it would be possible to be an actor. He's proved himself wrong. 🎭 (via @BBCStories) https://t.co/jDMzxqLIuM
    Tweet from @BBC #2017-12-29 09:03:05+00:00 created on 56:Knocking on a door? A Formula 1 car? Nope. This is the sound two icebergs make when they rub together. ❄️ 🔊… https://t.co/AsKOsZHOoF
    Tweet from @BBC #2017-12-28 21:31:04+00:00 created on 57:🎶 @BrunoMars: Live in Harlem. 10:25pm on @BBCOne. 💜 https://t.co/WoGSJ3JvdV https://t.co/jKLXAp3cTC
    Tweet from @BBC #2017-12-28 20:29:04+00:00 created on 58:Fidget spinners and more: the top Google searches of 2017 📱🤳https://t.co/qRwxjAH1DF https://t.co/hVjxB8GvNL
    Tweet from @BBC #2017-12-28 20:05:03+00:00 created on 59:The theatre shows you won’t want to miss in 2018 🎭🎟 https://t.co/mpFBZ0wYcU https://t.co/9wNat5aZGB
    Tweet from @BBC #2017-12-28 19:30:03+00:00 created on 60:This first aid phone app is saving lives.📱🏥
    
    Via BBC World Hacks https://t.co/wISQ2tt4si
    Tweet from @BBC #2017-12-29 13:41:30+00:00 created on 61:RT @BBCCiN: Best Day Ever? You weren't kidding!
    
    Special thanks to @NiallOfficial​ for topping off a wonderful day for some of the young ca…
    Tweet from @BBC #2017-12-29 13:32:04+00:00 created on 62:Ever wondered what makes a wet dog smell? 🤔🐶 10 mysteries you’d like to get to the bottom of https://t.co/MthZREY7Ui https://t.co/o0LwuuXTlk
    Tweet from @BBC #2017-12-29 13:00:06+00:00 created on 63:How do we control our body temperature? ❄️🌡
    
    Dr Chris and Dr Xand investigate... https://t.co/kXW1DnCDax https://t.co/F1cyigzOfG
    Tweet from @BBC #2017-12-29 12:32:03+00:00 created on 64:From Trump's presidency to the FIFA corruption scandal, The Simpsons has a habit of predicting the future. 
    
    What p… https://t.co/VN0aP5WzSQ
    Tweet from @BBC #2017-12-29 12:25:38+00:00 created on 65:RT @bbc5live: "One of the worst films I can remember seeing for many a year... I hope to never have to mention it or think about it ever ag…
    Tweet from @BBC #2017-12-29 12:00:02+00:00 created on 66:2016 was Rio's deadliest year on record. Now people are using apps to help keep themselves safe. 📱
    
    Via @BBCClick https://t.co/JCPqSWIjir
    Tweet from @BBC #2017-12-29 11:40:03+00:00 created on 67:From a short story collection to a century-spanning novel about a Korean family - are these the 10 best books of 20… https://t.co/HZ77Ygy3QW
    Tweet from @BBC #2017-12-29 11:29:09+00:00 created on 68:RT @BBCRadio2: Rumour has it, they've since healed the rift... 👏
    Enjoy the sounds of @liamgallagher (and his pencil sharpener) In Concert f…
    Tweet from @BBC #2017-12-29 11:15:04+00:00 created on 69:🏒 Ice Hockey’s Transgender Star: Harrison Browne is a professional ice-hockey player, also known for being the firs… https://t.co/AC7objhq4W
    Tweet from @BBC #2017-12-29 10:49:03+00:00 created on 70:Do you have the gumption to become a fully-fledged massive snob? Take this @BBCRadio4 quiz to find out!… https://t.co/WZ2tC5FXeZ
    Tweet from @BBC #2017-12-29 10:29:32+00:00 created on 71:RT @BBCNewsbeat: What it's like to grow up in war zones around the world: @UNICEF is highlighting the dangers faced by young people https:/…
    Tweet from @BBC #2017-12-29 10:14:04+00:00 created on 72:No regrets? Research suggests that regret 'improves children's decision-making'. 🤔 https://t.co/kdcQjzkIZV https://t.co/AYJDPrtvBv
    Tweet from @BBC #2017-12-29 09:56:19+00:00 created on 73:RT @BBCWomansHour: At 10am ⏰: Over 3600 pregnant women and children live in temporary accommodation in Birmingham alone. Why are the number…
    Tweet from @BBC #2017-12-29 09:45:27+00:00 created on 74:RT @bbcwritersroom: The #writer behind the success of Morecambe &amp; Wise, Eddie Braben, and the immense pressure to create shows watched by u…
    Tweet from @BBC #2017-12-29 09:30:06+00:00 created on 75:Born deaf, Jamie never thought it would be possible to be an actor. He's proved himself wrong. 🎭 (via @BBCStories) https://t.co/jDMzxqLIuM
    Tweet from @BBC #2017-12-29 09:03:05+00:00 created on 76:Knocking on a door? A Formula 1 car? Nope. This is the sound two icebergs make when they rub together. ❄️ 🔊… https://t.co/AsKOsZHOoF
    Tweet from @BBC #2017-12-28 21:31:04+00:00 created on 77:🎶 @BrunoMars: Live in Harlem. 10:25pm on @BBCOne. 💜 https://t.co/WoGSJ3JvdV https://t.co/jKLXAp3cTC
    Tweet from @BBC #2017-12-28 20:29:04+00:00 created on 78:Fidget spinners and more: the top Google searches of 2017 📱🤳https://t.co/qRwxjAH1DF https://t.co/hVjxB8GvNL
    Tweet from @BBC #2017-12-28 20:05:03+00:00 created on 79:The theatre shows you won’t want to miss in 2018 🎭🎟 https://t.co/mpFBZ0wYcU https://t.co/9wNat5aZGB
    Tweet from @BBC #2017-12-28 19:30:03+00:00 created on 80:This first aid phone app is saving lives.📱🏥
    
    Via BBC World Hacks https://t.co/wISQ2tt4si
    Tweet from @BBC #2017-12-29 13:41:30+00:00 created on 81:RT @BBCCiN: Best Day Ever? You weren't kidding!
    
    Special thanks to @NiallOfficial​ for topping off a wonderful day for some of the young ca…
    Tweet from @BBC #2017-12-29 13:32:04+00:00 created on 82:Ever wondered what makes a wet dog smell? 🤔🐶 10 mysteries you’d like to get to the bottom of https://t.co/MthZREY7Ui https://t.co/o0LwuuXTlk
    Tweet from @BBC #2017-12-29 13:00:06+00:00 created on 83:How do we control our body temperature? ❄️🌡
    
    Dr Chris and Dr Xand investigate... https://t.co/kXW1DnCDax https://t.co/F1cyigzOfG
    Tweet from @BBC #2017-12-29 12:32:03+00:00 created on 84:From Trump's presidency to the FIFA corruption scandal, The Simpsons has a habit of predicting the future. 
    
    What p… https://t.co/VN0aP5WzSQ
    Tweet from @BBC #2017-12-29 12:25:38+00:00 created on 85:RT @bbc5live: "One of the worst films I can remember seeing for many a year... I hope to never have to mention it or think about it ever ag…
    Tweet from @BBC #2017-12-29 12:00:02+00:00 created on 86:2016 was Rio's deadliest year on record. Now people are using apps to help keep themselves safe. 📱
    
    Via @BBCClick https://t.co/JCPqSWIjir
    Tweet from @BBC #2017-12-29 11:40:03+00:00 created on 87:From a short story collection to a century-spanning novel about a Korean family - are these the 10 best books of 20… https://t.co/HZ77Ygy3QW
    Tweet from @BBC #2017-12-29 11:29:09+00:00 created on 88:RT @BBCRadio2: Rumour has it, they've since healed the rift... 👏
    Enjoy the sounds of @liamgallagher (and his pencil sharpener) In Concert f…
    Tweet from @BBC #2017-12-29 11:15:04+00:00 created on 89:🏒 Ice Hockey’s Transgender Star: Harrison Browne is a professional ice-hockey player, also known for being the firs… https://t.co/AC7objhq4W
    Tweet from @BBC #2017-12-29 10:49:03+00:00 created on 90:Do you have the gumption to become a fully-fledged massive snob? Take this @BBCRadio4 quiz to find out!… https://t.co/WZ2tC5FXeZ
    Tweet from @BBC #2017-12-29 10:29:32+00:00 created on 91:RT @BBCNewsbeat: What it's like to grow up in war zones around the world: @UNICEF is highlighting the dangers faced by young people https:/…
    Tweet from @BBC #2017-12-29 10:14:04+00:00 created on 92:No regrets? Research suggests that regret 'improves children's decision-making'. 🤔 https://t.co/kdcQjzkIZV https://t.co/AYJDPrtvBv
    Tweet from @BBC #2017-12-29 09:56:19+00:00 created on 93:RT @BBCWomansHour: At 10am ⏰: Over 3600 pregnant women and children live in temporary accommodation in Birmingham alone. Why are the number…
    Tweet from @BBC #2017-12-29 09:45:27+00:00 created on 94:RT @bbcwritersroom: The #writer behind the success of Morecambe &amp; Wise, Eddie Braben, and the immense pressure to create shows watched by u…
    Tweet from @BBC #2017-12-29 09:30:06+00:00 created on 95:Born deaf, Jamie never thought it would be possible to be an actor. He's proved himself wrong. 🎭 (via @BBCStories) https://t.co/jDMzxqLIuM
    Tweet from @BBC #2017-12-29 09:03:05+00:00 created on 96:Knocking on a door? A Formula 1 car? Nope. This is the sound two icebergs make when they rub together. ❄️ 🔊… https://t.co/AsKOsZHOoF
    Tweet from @BBC #2017-12-28 21:31:04+00:00 created on 97:🎶 @BrunoMars: Live in Harlem. 10:25pm on @BBCOne. 💜 https://t.co/WoGSJ3JvdV https://t.co/jKLXAp3cTC
    Tweet from @BBC #2017-12-28 20:29:04+00:00 created on 98:Fidget spinners and more: the top Google searches of 2017 📱🤳https://t.co/qRwxjAH1DF https://t.co/hVjxB8GvNL
    Tweet from @BBC #2017-12-28 20:05:03+00:00 created on 99:The theatre shows you won’t want to miss in 2018 🎭🎟 https://t.co/mpFBZ0wYcU https://t.co/9wNat5aZGB
    Tweet from @BBC #2017-12-28 19:30:03+00:00 created on 100:This first aid phone app is saving lives.📱🏥
    
    Via BBC World Hacks https://t.co/wISQ2tt4si
     ---------------TWEET COMPLETED FOR @BBC-------------
    


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-11-050271f2bcfc> in <module>()
         34 
         35     # Calculating
    ---> 36     tweet_mean_call()
         37 
         38 
    

    <ipython-input-11-050271f2bcfc> in tweet_mean_call()
          2 def tweet_mean_call():
          3         tweet_compound_mean.append(np.mean(compound_list))
    ----> 4         tweet_positive_mean.append(np.mean(positive_list))
          5         tweet_negative_mean.append(np.mean(negative_list))
          6 
    

    NameError: name 'tweet_positive_mean' is not defined



```python
#print compound mean of sentiments for each media news
print(tweet_compound_mean)
```

    [0.11086599999999999, 0.2134385, 0.16866233333333333, 0.13759725, 0.10342100000000001]
    


```python
#print negative mean of sentiments for each media news
print(tweet_negative_mean)
```

    [0.031979999999999995, 0.027539999999999995, 0.033116666666666662, 0.045269999999999991, 0.049906000000000006]
    


```python
## Defining a dataframe
sentiment=pd.DataFrame({
            "User":user_list,
            "Tweet_count":tweet_count,
            "Tweet_date":tweet_times,
            "Tweet_text":tweet_text,
           "Compound":compound_list,
           "Positive":positive_list,
           "Neutral":neutral_list,
            "Negative":negative_list})
# Organize Dataframe
sentiment = sentiment[["User","Tweet_count","Tweet_date","Tweet_text","Compound","Positive","Neutral","Negative"]]
```


```python
sentiment
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>User</th>
      <th>Tweet_count</th>
      <th>Tweet_date</th>
      <th>Tweet_text</th>
      <th>Compound</th>
      <th>Positive</th>
      <th>Neutral</th>
      <th>Negative</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>@BBC</td>
      <td>1</td>
      <td>2018-01-08 21:29:44+00:00</td>
      <td>RT @bbc5live: Looking for some exercise and nu...</td>
      <td>0.2960</td>
      <td>0.095</td>
      <td>0.905</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@BBC</td>
      <td>2</td>
      <td>2018-01-08 21:29:01+00:00</td>
      <td>RT @bbcasiannetwork: 📡 The Asian Network Futur...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@BBC</td>
      <td>3</td>
      <td>2018-01-08 21:28:07+00:00</td>
      <td>RT @bbcthesocial: Jordan thought nobody would ...</td>
      <td>-0.1655</td>
      <td>0.069</td>
      <td>0.831</td>
      <td>0.099</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@BBC</td>
      <td>4</td>
      <td>2018-01-08 21:27:12+00:00</td>
      <td>RT @bbcworldservice: "Our job as journalists i...</td>
      <td>0.3182</td>
      <td>0.095</td>
      <td>0.905</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@BBC</td>
      <td>5</td>
      <td>2018-01-08 21:23:06+00:00</td>
      <td>RT @BBCiPlayer: 🎁 #HardSun is the new #boxset ...</td>
      <td>-0.2500</td>
      <td>0.000</td>
      <td>0.882</td>
      <td>0.118</td>
    </tr>
    <tr>
      <th>5</th>
      <td>@BBC</td>
      <td>6</td>
      <td>2018-01-08 21:22:32+00:00</td>
      <td>RT @BBCWthrWatchers: Some cracking sunsets 🌇 i...</td>
      <td>0.0772</td>
      <td>0.061</td>
      <td>0.939</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>@BBC</td>
      <td>7</td>
      <td>2018-01-08 20:30:06+00:00</td>
      <td>The #SilentWitness team are back. \n\n9pm | @B...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>@BBC</td>
      <td>8</td>
      <td>2018-01-08 20:04:03+00:00</td>
      <td>Recy Taylor: who was the woman Oprah mentioned...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>@BBC</td>
      <td>9</td>
      <td>2018-01-08 19:37:04+00:00</td>
      <td>Tonight, go beyond the theatre doors and disco...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>@BBC</td>
      <td>10</td>
      <td>2018-01-08 19:02:01+00:00</td>
      <td>🦌❄ The moment a moose, which got stuck up to i...</td>
      <td>0.1779</td>
      <td>0.119</td>
      <td>0.793</td>
      <td>0.088</td>
    </tr>
    <tr>
      <th>10</th>
      <td>@BBC</td>
      <td>11</td>
      <td>2018-01-08 18:32:04+00:00</td>
      <td>🏝😁 This year you can get 24 days off in a row ...</td>
      <td>-0.1260</td>
      <td>0.000</td>
      <td>0.923</td>
      <td>0.077</td>
    </tr>
    <tr>
      <th>11</th>
      <td>@BBC</td>
      <td>12</td>
      <td>2018-01-08 18:02:04+00:00</td>
      <td>🎒📚 It was a big day for Princess Charlotte tod...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>@BBC</td>
      <td>13</td>
      <td>2018-01-08 17:30:06+00:00</td>
      <td>♿✈This mum is taking on the global airline ind...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>@BBC</td>
      <td>14</td>
      <td>2018-01-08 17:09:03+00:00</td>
      <td>This #MeatFreeMonday, why not give one of thes...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>@BBC</td>
      <td>15</td>
      <td>2018-01-08 16:48:04+00:00</td>
      <td>'I still have that karaoke machine!' 🎤❤️️\n\n@...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>@BBC</td>
      <td>16</td>
      <td>2018-01-08 16:33:07+00:00</td>
      <td>These are some of the most striking photograph...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>@BBC</td>
      <td>17</td>
      <td>2018-01-08 16:00:05+00:00</td>
      <td>Judi Dench and Maggie Smith star as two sister...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>@BBC</td>
      <td>18</td>
      <td>2018-01-08 15:27:21+00:00</td>
      <td>📚Eight books that will make you want to run aw...</td>
      <td>0.0772</td>
      <td>0.075</td>
      <td>0.925</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>@BBC</td>
      <td>19</td>
      <td>2018-01-08 15:00:07+00:00</td>
      <td>💔💍 If you're married then it might be best to ...</td>
      <td>0.6908</td>
      <td>0.231</td>
      <td>0.769</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>@BBC</td>
      <td>20</td>
      <td>2018-01-08 14:30:06+00:00</td>
      <td>Repeat bouts of warmer seawater are posing a s...</td>
      <td>0.5106</td>
      <td>0.290</td>
      <td>0.710</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>@BBC</td>
      <td>21</td>
      <td>2018-01-08 21:29:44+00:00</td>
      <td>RT @bbc5live: Looking for some exercise and nu...</td>
      <td>0.2960</td>
      <td>0.095</td>
      <td>0.905</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>@BBC</td>
      <td>22</td>
      <td>2018-01-08 21:29:01+00:00</td>
      <td>RT @bbcasiannetwork: 📡 The Asian Network Futur...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>@BBC</td>
      <td>23</td>
      <td>2018-01-08 21:28:07+00:00</td>
      <td>RT @bbcthesocial: Jordan thought nobody would ...</td>
      <td>-0.1655</td>
      <td>0.069</td>
      <td>0.831</td>
      <td>0.099</td>
    </tr>
    <tr>
      <th>23</th>
      <td>@BBC</td>
      <td>24</td>
      <td>2018-01-08 21:27:12+00:00</td>
      <td>RT @bbcworldservice: "Our job as journalists i...</td>
      <td>0.3182</td>
      <td>0.095</td>
      <td>0.905</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>@BBC</td>
      <td>25</td>
      <td>2018-01-08 21:23:06+00:00</td>
      <td>RT @BBCiPlayer: 🎁 #HardSun is the new #boxset ...</td>
      <td>-0.2500</td>
      <td>0.000</td>
      <td>0.882</td>
      <td>0.118</td>
    </tr>
    <tr>
      <th>25</th>
      <td>@BBC</td>
      <td>26</td>
      <td>2018-01-08 21:22:32+00:00</td>
      <td>RT @BBCWthrWatchers: Some cracking sunsets 🌇 i...</td>
      <td>0.0772</td>
      <td>0.061</td>
      <td>0.939</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>@BBC</td>
      <td>27</td>
      <td>2018-01-08 20:30:06+00:00</td>
      <td>The #SilentWitness team are back. \n\n9pm | @B...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>@BBC</td>
      <td>28</td>
      <td>2018-01-08 20:04:03+00:00</td>
      <td>Recy Taylor: who was the woman Oprah mentioned...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>@BBC</td>
      <td>29</td>
      <td>2018-01-08 19:37:04+00:00</td>
      <td>Tonight, go beyond the theatre doors and disco...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>29</th>
      <td>@BBC</td>
      <td>30</td>
      <td>2018-01-08 19:02:01+00:00</td>
      <td>🦌❄ The moment a moose, which got stuck up to i...</td>
      <td>0.1779</td>
      <td>0.119</td>
      <td>0.793</td>
      <td>0.088</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>470</th>
      <td>@nytimes</td>
      <td>71</td>
      <td>2018-01-08 14:41:02+00:00</td>
      <td>Last May, Jared Kushner accompanied President ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>471</th>
      <td>@nytimes</td>
      <td>72</td>
      <td>2018-01-08 14:36:33+00:00</td>
      <td>RT @UpshotNYT: The jobless rate is low; so are...</td>
      <td>-0.2732</td>
      <td>0.118</td>
      <td>0.684</td>
      <td>0.198</td>
    </tr>
    <tr>
      <th>472</th>
      <td>@nytimes</td>
      <td>73</td>
      <td>2018-01-08 14:31:09+00:00</td>
      <td>A fire broke out near the top of Trump Tower i...</td>
      <td>-0.8126</td>
      <td>0.064</td>
      <td>0.571</td>
      <td>0.364</td>
    </tr>
    <tr>
      <th>473</th>
      <td>@nytimes</td>
      <td>74</td>
      <td>2018-01-08 14:15:14+00:00</td>
      <td>Eight pairs of actresses and activists attende...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>474</th>
      <td>@nytimes</td>
      <td>75</td>
      <td>2018-01-08 14:00:22+00:00</td>
      <td>Bannon needs Breitbart. Does Breitbart need Ba...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>475</th>
      <td>@nytimes</td>
      <td>76</td>
      <td>2018-01-08 13:46:07+00:00</td>
      <td>Natalie Portman delivered a line that instantl...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>476</th>
      <td>@nytimes</td>
      <td>77</td>
      <td>2018-01-08 13:32:06+00:00</td>
      <td>"The Globes still celebrated the power players...</td>
      <td>0.0258</td>
      <td>0.159</td>
      <td>0.687</td>
      <td>0.155</td>
    </tr>
    <tr>
      <th>477</th>
      <td>@nytimes</td>
      <td>78</td>
      <td>2018-01-08 13:00:28+00:00</td>
      <td>Morning Briefing: Here's what you need to know...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>478</th>
      <td>@nytimes</td>
      <td>79</td>
      <td>2018-01-08 12:45:15+00:00</td>
      <td>Iran has banned the teaching of English in pri...</td>
      <td>-0.7096</td>
      <td>0.000</td>
      <td>0.704</td>
      <td>0.296</td>
    </tr>
    <tr>
      <th>479</th>
      <td>@nytimes</td>
      <td>80</td>
      <td>2018-01-08 12:31:05+00:00</td>
      <td>A senior BBC News editor accused the network o...</td>
      <td>-0.7003</td>
      <td>0.000</td>
      <td>0.746</td>
      <td>0.254</td>
    </tr>
    <tr>
      <th>480</th>
      <td>@nytimes</td>
      <td>81</td>
      <td>2018-01-08 12:15:09+00:00</td>
      <td>A transcript of Seth Meyers's Golden Globes mo...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>481</th>
      <td>@nytimes</td>
      <td>82</td>
      <td>2018-01-08 12:00:21+00:00</td>
      <td>President Trump defended his fitness for offic...</td>
      <td>0.2732</td>
      <td>0.110</td>
      <td>0.890</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>482</th>
      <td>@nytimes</td>
      <td>83</td>
      <td>2018-01-08 11:50:08+00:00</td>
      <td>Stephen Bannon tried backing away from his exp...</td>
      <td>0.0258</td>
      <td>0.058</td>
      <td>0.942</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>483</th>
      <td>@nytimes</td>
      <td>84</td>
      <td>2018-01-08 11:40:13+00:00</td>
      <td>Read speeches from Elisabeth Moss, Laura Dern,...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>484</th>
      <td>@nytimes</td>
      <td>85</td>
      <td>2018-01-08 11:30:06+00:00</td>
      <td>Morning Briefing: Here's what you need to know...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>485</th>
      <td>@nytimes</td>
      <td>86</td>
      <td>2018-01-08 11:21:02+00:00</td>
      <td>An Iranian oil tanker that collided with anoth...</td>
      <td>-0.2732</td>
      <td>0.000</td>
      <td>0.909</td>
      <td>0.091</td>
    </tr>
    <tr>
      <th>486</th>
      <td>@nytimes</td>
      <td>87</td>
      <td>2018-01-08 11:11:02+00:00</td>
      <td>RT @nytimesworld: Remember the “new Coke” blun...</td>
      <td>0.4215</td>
      <td>0.123</td>
      <td>0.877</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>487</th>
      <td>@nytimes</td>
      <td>88</td>
      <td>2018-01-08 11:00:23+00:00</td>
      <td>Israel published a blacklist of 20 organizatio...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>488</th>
      <td>@nytimes</td>
      <td>89</td>
      <td>2018-01-08 10:46:03+00:00</td>
      <td>The strongest challenger to Egypt’s leader wit...</td>
      <td>0.5267</td>
      <td>0.216</td>
      <td>0.784</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>489</th>
      <td>@nytimes</td>
      <td>90</td>
      <td>2018-01-08 10:29:02+00:00</td>
      <td>Read Oprah Winfrey's Golden Globes speech http...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>490</th>
      <td>@nytimes</td>
      <td>91</td>
      <td>2018-01-08 10:15:12+00:00</td>
      <td>The police in Belfast said they were investiga...</td>
      <td>-0.4215</td>
      <td>0.000</td>
      <td>0.872</td>
      <td>0.128</td>
    </tr>
    <tr>
      <th>491</th>
      <td>@nytimes</td>
      <td>92</td>
      <td>2018-01-08 09:59:03+00:00</td>
      <td>The complete list of winners at the 2018 Golde...</td>
      <td>0.4767</td>
      <td>0.237</td>
      <td>0.763</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>492</th>
      <td>@nytimes</td>
      <td>93</td>
      <td>2018-01-08 09:44:04+00:00</td>
      <td>RT @nytimesworld: How do you maintain a fence ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>493</th>
      <td>@nytimes</td>
      <td>94</td>
      <td>2018-01-08 09:31:01+00:00</td>
      <td>Opinion: "America is upside down and inside ou...</td>
      <td>-0.4404</td>
      <td>0.000</td>
      <td>0.854</td>
      <td>0.146</td>
    </tr>
    <tr>
      <th>494</th>
      <td>@nytimes</td>
      <td>95</td>
      <td>2018-01-08 09:16:05+00:00</td>
      <td>Countries are unsure whether to take President...</td>
      <td>-0.2500</td>
      <td>0.000</td>
      <td>0.900</td>
      <td>0.100</td>
    </tr>
    <tr>
      <th>495</th>
      <td>@nytimes</td>
      <td>96</td>
      <td>2018-01-08 08:52:06+00:00</td>
      <td>Iran has banned the teaching of English in pri...</td>
      <td>-0.7096</td>
      <td>0.000</td>
      <td>0.704</td>
      <td>0.296</td>
    </tr>
    <tr>
      <th>496</th>
      <td>@nytimes</td>
      <td>97</td>
      <td>2018-01-08 08:37:05+00:00</td>
      <td>RT @FrankBruni: In this anxious time of absent...</td>
      <td>0.6187</td>
      <td>0.229</td>
      <td>0.690</td>
      <td>0.082</td>
    </tr>
    <tr>
      <th>497</th>
      <td>@nytimes</td>
      <td>98</td>
      <td>2018-01-08 08:22:05+00:00</td>
      <td>Heavy snowfall. Face-freezing temperatures. Ar...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>498</th>
      <td>@nytimes</td>
      <td>99</td>
      <td>2018-01-08 08:07:11+00:00</td>
      <td>Will Steve Bannon be able to hang on to Breitb...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>499</th>
      <td>@nytimes</td>
      <td>100</td>
      <td>2018-01-08 07:52:01+00:00</td>
      <td>RT @nytimesarts: The Golden Globes was a half-...</td>
      <td>0.4019</td>
      <td>0.109</td>
      <td>0.891</td>
      <td>0.000</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 8 columns</p>
</div>




```python
sentiment['User'].unique()
```




    array(['@BBC', '@CBS', '@CNN', '@FoxNews', '@nytimes'], dtype=object)




```python
# Push the sentiment DataFrame to a new CSV file
sentiment.to_csv("sentiment.csv", encoding="utf-8", index=False, header=True)
```


```python
### Sorting by time of tweets and assigning to each dataframe
BBC_df=sentiment[sentiment['User']=="@BBC"]
BBC_df=BBC_df.sort_values(by='Tweet_date', ascending = False)
CBS_df=sentiment[sentiment['User']=="@CBS"]
CBS_df=CBS_df.sort_values(by='Tweet_date', ascending = False)
CNN_df=sentiment[sentiment['User']=="@CNN"]
CNN_df=BBC_df.sort_values(by='Tweet_date', ascending = False)
FoxNews_df=sentiment[sentiment['User']=="@FoxNews"]
FoxNews_df=FoxNews_df.sort_values(by='Tweet_date', ascending = False)
nytimes_df=sentiment[sentiment['User']=="@nytimes"]
nytimes_df=nytimes_df.sort_values(by='Tweet_date', ascending = False)
```


```python
## Incorporate the scatter plot
ax1=BBC_df.plot(kind='scatter',x = "Tweet_count", y = "Compound",c="blue",s=50, alpha=0.5, label='@BBC')
ax2=CBS_df.plot(kind='scatter',x = "Tweet_count", y = "Compound",c="darkgreen", s=50, alpha=0.5, label='@CBS',ax=ax1)
ax3=CNN_df.plot(kind='scatter',x = "Tweet_count", y = "Compound",c="red", s=50, alpha=0.5, label='@CNN',ax=ax2)
ax4=FoxNews_df.plot(kind='scatter',x = "Tweet_count", y = "Compound",c="darkblue", s=50, alpha=0.5, label='@FoxNews',ax=ax3)
ax5=nytimes_df.plot(kind='scatter',x = "Tweet_count", y = "Compound",c="yellow",s=50, alpha=0.5, label='@nytimes',ax=ax4)

plt.title("Sentiment Analysis of Tweets (%s)" % time.strftime("%x"))
plt.ylabel("Tweet Polarity")
plt.xlabel("Tweets Ago")
plt.ylim([-1.05,1.05])
plt.xlim([105,0])
plt.legend(loc='center left', bbox_to_anchor=(1.0, 0.75))
plt.show()
```


![png](README_files/README_15_0.png)



```python
public_tweets = api.home_timeline()

# Loop through all tweets
for tweet in public_tweets:
    # Utilize JSON dumps to generate a pretty-printed json
    print(json.dumps(tweet, sort_keys=True, indent=4, separators=(',', ': ')))
    #print(tweet['counter'])
```

    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 21:56:42 +0000 2018",
        "entities": {
            "hashtags": [],
            "media": [
                {
                    "display_url": "pic.twitter.com/R5OnxW2thW",
                    "expanded_url": "https://twitter.com/SrBachchan/status/950486447097724928/photo/1",
                    "id": 950486353879289856,
                    "id_str": "950486353879289856",
                    "indices": [
                        37,
                        60
                    ],
                    "media_url": "http://pbs.twimg.com/media/DTDOTY7U0AAq27B.jpg",
                    "media_url_https": "https://pbs.twimg.com/media/DTDOTY7U0AAq27B.jpg",
                    "sizes": {
                        "large": {
                            "h": 2048,
                            "resize": "fit",
                            "w": 1502
                        },
                        "medium": {
                            "h": 1200,
                            "resize": "fit",
                            "w": 880
                        },
                        "small": {
                            "h": 680,
                            "resize": "fit",
                            "w": 499
                        },
                        "thumb": {
                            "h": 150,
                            "resize": "crop",
                            "w": 150
                        }
                    },
                    "type": "photo",
                    "url": "https://t.co/R5OnxW2thW"
                }
            ],
            "symbols": [],
            "urls": [],
            "user_mentions": []
        },
        "extended_entities": {
            "media": [
                {
                    "display_url": "pic.twitter.com/R5OnxW2thW",
                    "expanded_url": "https://twitter.com/SrBachchan/status/950486447097724928/photo/1",
                    "id": 950486353879289856,
                    "id_str": "950486353879289856",
                    "indices": [
                        37,
                        60
                    ],
                    "media_url": "http://pbs.twimg.com/media/DTDOTY7U0AAq27B.jpg",
                    "media_url_https": "https://pbs.twimg.com/media/DTDOTY7U0AAq27B.jpg",
                    "sizes": {
                        "large": {
                            "h": 2048,
                            "resize": "fit",
                            "w": 1502
                        },
                        "medium": {
                            "h": 1200,
                            "resize": "fit",
                            "w": 880
                        },
                        "small": {
                            "h": 680,
                            "resize": "fit",
                            "w": 499
                        },
                        "thumb": {
                            "h": 150,
                            "resize": "crop",
                            "w": 150
                        }
                    },
                    "type": "photo",
                    "url": "https://t.co/R5OnxW2thW"
                },
                {
                    "display_url": "pic.twitter.com/R5OnxW2thW",
                    "expanded_url": "https://twitter.com/SrBachchan/status/950486447097724928/photo/1",
                    "id": 950486353883545600,
                    "id_str": "950486353883545600",
                    "indices": [
                        37,
                        60
                    ],
                    "media_url": "http://pbs.twimg.com/media/DTDOTY8VwAAjkjy.jpg",
                    "media_url_https": "https://pbs.twimg.com/media/DTDOTY8VwAAjkjy.jpg",
                    "sizes": {
                        "large": {
                            "h": 2048,
                            "resize": "fit",
                            "w": 1450
                        },
                        "medium": {
                            "h": 1200,
                            "resize": "fit",
                            "w": 850
                        },
                        "small": {
                            "h": 680,
                            "resize": "fit",
                            "w": 482
                        },
                        "thumb": {
                            "h": 150,
                            "resize": "crop",
                            "w": 150
                        }
                    },
                    "type": "photo",
                    "url": "https://t.co/R5OnxW2thW"
                },
                {
                    "display_url": "pic.twitter.com/R5OnxW2thW",
                    "expanded_url": "https://twitter.com/SrBachchan/status/950486447097724928/photo/1",
                    "id": 950486404101885952,
                    "id_str": "950486404101885952",
                    "indices": [
                        37,
                        60
                    ],
                    "media_url": "http://pbs.twimg.com/media/DTDOWUBU0AA__hs.jpg",
                    "media_url_https": "https://pbs.twimg.com/media/DTDOWUBU0AA__hs.jpg",
                    "sizes": {
                        "large": {
                            "h": 2048,
                            "resize": "fit",
                            "w": 1499
                        },
                        "medium": {
                            "h": 1200,
                            "resize": "fit",
                            "w": 879
                        },
                        "small": {
                            "h": 680,
                            "resize": "fit",
                            "w": 498
                        },
                        "thumb": {
                            "h": 150,
                            "resize": "crop",
                            "w": 150
                        }
                    },
                    "type": "photo",
                    "url": "https://t.co/R5OnxW2thW"
                }
            ]
        },
        "favorite_count": 567,
        "favorited": false,
        "geo": null,
        "id": 950486447097724928,
        "id_str": "950486447097724928",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "retweet_count": 51,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
        "text": "T 2526 - Daughters are the best .. ! https://t.co/R5OnxW2thW",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 18 05:16:47 +0000 2010",
            "default_profile": false,
            "default_profile_image": false,
            "description": "Actor ... well at least some are STILL saying so !!",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "srbachchan.tumblr.com",
                            "expanded_url": "http://srbachchan.tumblr.com",
                            "indices": [
                                0,
                                22
                            ],
                            "url": "http://t.co/Vkp8AE24J1"
                        }
                    ]
                }
            },
            "favourites_count": 62,
            "follow_request_sent": false,
            "followers_count": 32660825,
            "following": true,
            "friends_count": 1169,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 145125358,
            "id_str": "145125358",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 30488,
            "location": "Mumbai, India",
            "name": "Amitabh Bachchan",
            "notifications": false,
            "profile_background_color": "EDECE9",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/145125358/1500209752",
            "profile_image_url": "http://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_link_color": "DD2E44",
            "profile_sidebar_border_color": "000000",
            "profile_sidebar_fill_color": "000000",
            "profile_text_color": "000000",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "SrBachchan",
            "statuses_count": 60702,
            "time_zone": "Mumbai",
            "translator_type": "none",
            "url": "http://t.co/Vkp8AE24J1",
            "utc_offset": 19800,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 19:06:40 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/i/web/status/9\u2026",
                    "expanded_url": "https://twitter.com/i/web/status/950443653666713601",
                    "indices": [
                        117,
                        140
                    ],
                    "url": "https://t.co/UxBD2DBv2G"
                }
            ],
            "user_mentions": [
                {
                    "id": 20957191,
                    "id_str": "20957191",
                    "indices": [
                        85,
                        96
                    ],
                    "name": "American Farm Bureau",
                    "screen_name": "FarmBureau"
                }
            ]
        },
        "favorite_count": 26556,
        "favorited": false,
        "geo": null,
        "id": 950443653666713601,
        "id_str": "950443653666713601",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "retweet_count": 5811,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
        "text": "Can\u2019t wait to be back in the amazing state of Tennessee to address the 99th American @FarmBureau Federation\u2019s Annua\u2026 https://t.co/UxBD2DBv2G",
        "truncated": true,
        "user": {
            "contributors_enabled": false,
            "created_at": "Wed Mar 18 13:46:38 +0000 2009",
            "default_profile": false,
            "default_profile_image": false,
            "description": "45th President of the United States of America\ud83c\uddfa\ud83c\uddf8",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "Instagram.com/realDonaldTrump",
                            "expanded_url": "http://www.Instagram.com/realDonaldTrump",
                            "indices": [
                                0,
                                23
                            ],
                            "url": "https://t.co/OMxB0x7xC5"
                        }
                    ]
                }
            },
            "favourites_count": 24,
            "follow_request_sent": false,
            "followers_count": 46288008,
            "following": true,
            "friends_count": 45,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 25073877,
            "id_str": "25073877",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 82407,
            "location": "Washington, DC",
            "name": "Donald J. Trump",
            "notifications": false,
            "profile_background_color": "6D5C18",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/530021613/trump_scotland__43_of_70_cc.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/530021613/trump_scotland__43_of_70_cc.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/25073877/1514347856",
            "profile_image_url": "http://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_normal.jpg",
            "profile_link_color": "1B95E0",
            "profile_sidebar_border_color": "BDDCAD",
            "profile_sidebar_fill_color": "C5CEC0",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "realDonaldTrump",
            "statuses_count": 36721,
            "time_zone": "Eastern Time (US & Canada)",
            "translator_type": "regular",
            "url": "https://t.co/OMxB0x7xC5",
            "utc_offset": -18000,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 17:17:45 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "instagram.com/p/BdrZtc5AhB3/",
                    "expanded_url": "https://www.instagram.com/p/BdrZtc5AhB3/",
                    "indices": [
                        95,
                        118
                    ],
                    "url": "https://t.co/X24k3han8M"
                }
            ],
            "user_mentions": [
                {
                    "id": 3072160164,
                    "id_str": "3072160164",
                    "indices": [
                        77,
                        93
                    ],
                    "name": "John Kraus",
                    "screen_name": "johnkrausphotos"
                }
            ]
        },
        "favorite_count": 12642,
        "favorited": false,
        "geo": null,
        "id": 950416244997488640,
        "id_str": "950416244997488640",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "retweet_count": 2202,
        "retweeted": false,
        "source": "<a href=\"http://instagram.com\" rel=\"nofollow\">Instagram</a>",
        "text": "Long exposure of rocket ascent, reentry from space and landing burn. (Credit @johnkrausphotos) https://t.co/X24k3han8M",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue Jun 02 20:12:29 +0000 2009",
            "default_profile": false,
            "default_profile_image": false,
            "description": "",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 706,
            "follow_request_sent": false,
            "followers_count": 17856124,
            "following": true,
            "friends_count": 47,
            "geo_enabled": false,
            "has_extended_profile": false,
            "id": 44196397,
            "id_str": "44196397",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 40456,
            "location": "Boring",
            "name": "Elon Musk",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/399721902/fusion.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/399721902/fusion.jpg",
            "profile_background_tile": false,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/44196397/1354486475",
            "profile_image_url": "http://pbs.twimg.com/profile_images/782474226020200448/zDo-gAo0_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/782474226020200448/zDo-gAo0_normal.jpg",
            "profile_link_color": "0084B4",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "elonmusk",
            "statuses_count": 3769,
            "time_zone": "Pacific Time (US & Canada)",
            "translator_type": "none",
            "url": null,
            "utc_offset": -28800,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 14:20:25 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/i/web/status/9\u2026",
                    "expanded_url": "https://twitter.com/i/web/status/950371619247153154",
                    "indices": [
                        116,
                        139
                    ],
                    "url": "https://t.co/1SJ1vHcqfI"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 87291,
        "favorited": false,
        "geo": null,
        "id": 950371619247153154,
        "id_str": "950371619247153154",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "retweet_count": 23833,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
        "text": "African American unemployment is the lowest ever recorded in our country. The Hispanic unemployment rate dropped a\u2026 https://t.co/1SJ1vHcqfI",
        "truncated": true,
        "user": {
            "contributors_enabled": false,
            "created_at": "Wed Mar 18 13:46:38 +0000 2009",
            "default_profile": false,
            "default_profile_image": false,
            "description": "45th President of the United States of America\ud83c\uddfa\ud83c\uddf8",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "Instagram.com/realDonaldTrump",
                            "expanded_url": "http://www.Instagram.com/realDonaldTrump",
                            "indices": [
                                0,
                                23
                            ],
                            "url": "https://t.co/OMxB0x7xC5"
                        }
                    ]
                }
            },
            "favourites_count": 24,
            "follow_request_sent": false,
            "followers_count": 46288008,
            "following": true,
            "friends_count": 45,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 25073877,
            "id_str": "25073877",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 82407,
            "location": "Washington, DC",
            "name": "Donald J. Trump",
            "notifications": false,
            "profile_background_color": "6D5C18",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/530021613/trump_scotland__43_of_70_cc.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/530021613/trump_scotland__43_of_70_cc.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/25073877/1514347856",
            "profile_image_url": "http://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_normal.jpg",
            "profile_link_color": "1B95E0",
            "profile_sidebar_border_color": "BDDCAD",
            "profile_sidebar_fill_color": "C5CEC0",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "realDonaldTrump",
            "statuses_count": 36721,
            "time_zone": "Eastern Time (US & Canada)",
            "translator_type": "regular",
            "url": "https://t.co/OMxB0x7xC5",
            "utc_offset": -18000,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 07:29:09 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/MosesSapir/sta\u2026",
                    "expanded_url": "https://twitter.com/MosesSapir/status/950231050386632707",
                    "indices": [
                        105,
                        128
                    ],
                    "url": "https://t.co/BcteAQ1dSI"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 3032,
        "favorited": false,
        "geo": null,
        "id": 950268117652881409,
        "id_str": "950268117652881409",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": true,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "quoted_status": {
            "contributors": null,
            "coordinates": null,
            "created_at": "Mon Jan 08 05:01:51 +0000 2018",
            "entities": {
                "hashtags": [],
                "media": [
                    {
                        "display_url": "pic.twitter.com/42WjsCMCfI",
                        "expanded_url": "https://twitter.com/MosesSapir/status/950231050386632707/photo/1",
                        "id": 950231040970383362,
                        "id_str": "950231040970383362",
                        "indices": [
                            50,
                            73
                        ],
                        "media_url": "http://pbs.twimg.com/media/DS_mGOOW4AIQ8w_.jpg",
                        "media_url_https": "https://pbs.twimg.com/media/DS_mGOOW4AIQ8w_.jpg",
                        "sizes": {
                            "large": {
                                "h": 593,
                                "resize": "fit",
                                "w": 686
                            },
                            "medium": {
                                "h": 593,
                                "resize": "fit",
                                "w": 686
                            },
                            "small": {
                                "h": 588,
                                "resize": "fit",
                                "w": 680
                            },
                            "thumb": {
                                "h": 150,
                                "resize": "crop",
                                "w": 150
                            }
                        },
                        "type": "photo",
                        "url": "https://t.co/42WjsCMCfI"
                    }
                ],
                "symbols": [],
                "urls": [],
                "user_mentions": [
                    {
                        "id": 145125358,
                        "id_str": "145125358",
                        "indices": [
                            13,
                            24
                        ],
                        "name": "Amitabh Bachchan",
                        "screen_name": "SrBachchan"
                    }
                ]
            },
            "extended_entities": {
                "media": [
                    {
                        "display_url": "pic.twitter.com/42WjsCMCfI",
                        "expanded_url": "https://twitter.com/MosesSapir/status/950231050386632707/photo/1",
                        "id": 950231040970383362,
                        "id_str": "950231040970383362",
                        "indices": [
                            50,
                            73
                        ],
                        "media_url": "http://pbs.twimg.com/media/DS_mGOOW4AIQ8w_.jpg",
                        "media_url_https": "https://pbs.twimg.com/media/DS_mGOOW4AIQ8w_.jpg",
                        "sizes": {
                            "large": {
                                "h": 593,
                                "resize": "fit",
                                "w": 686
                            },
                            "medium": {
                                "h": 593,
                                "resize": "fit",
                                "w": 686
                            },
                            "small": {
                                "h": 588,
                                "resize": "fit",
                                "w": 680
                            },
                            "thumb": {
                                "h": 150,
                                "resize": "crop",
                                "w": 150
                            }
                        },
                        "type": "photo",
                        "url": "https://t.co/42WjsCMCfI"
                    }
                ]
            },
            "favorite_count": 174,
            "favorited": false,
            "geo": null,
            "id": 950231050386632707,
            "id_str": "950231050386632707",
            "in_reply_to_screen_name": null,
            "in_reply_to_status_id": null,
            "in_reply_to_status_id_str": null,
            "in_reply_to_user_id": null,
            "in_reply_to_user_id_str": null,
            "is_quote_status": false,
            "lang": "en",
            "place": null,
            "possibly_sensitive": false,
            "possibly_sensitive_appealable": false,
            "retweet_count": 13,
            "retweeted": false,
            "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
            "text": "On the stage @SrBachchan Amitabh Bachchan concert https://t.co/42WjsCMCfI",
            "truncated": false,
            "user": {
                "contributors_enabled": false,
                "created_at": "Sun Dec 13 19:54:50 +0000 2009",
                "default_profile": false,
                "default_profile_image": false,
                "description": "ISG Media Ltd -TV Channel's-Management-Israel I Love Amitabh Bachchan",
                "entities": {
                    "description": {
                        "urls": []
                    },
                    "url": {
                        "urls": [
                            {
                                "display_url": "twitter.com/mosessapir",
                                "expanded_url": "https://twitter.com/mosessapir",
                                "indices": [
                                    0,
                                    23
                                ],
                                "url": "https://t.co/WWtQJCiPRg"
                            }
                        ]
                    }
                },
                "favourites_count": 27291,
                "follow_request_sent": false,
                "followers_count": 27139,
                "following": false,
                "friends_count": 2585,
                "geo_enabled": true,
                "has_extended_profile": true,
                "id": 96613617,
                "id_str": "96613617",
                "is_translation_enabled": false,
                "is_translator": false,
                "lang": "en",
                "listed_count": 79,
                "location": "Israel",
                "name": "Moses Sapir",
                "notifications": false,
                "profile_background_color": "FFF04D",
                "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/378800000056539541/3446a8469e434127a7cc8ede2f353197.jpeg",
                "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/378800000056539541/3446a8469e434127a7cc8ede2f353197.jpeg",
                "profile_background_tile": false,
                "profile_banner_url": "https://pbs.twimg.com/profile_banners/96613617/1514544292",
                "profile_image_url": "http://pbs.twimg.com/profile_images/939502811519307776/r5IGBt8r_normal.jpg",
                "profile_image_url_https": "https://pbs.twimg.com/profile_images/939502811519307776/r5IGBt8r_normal.jpg",
                "profile_link_color": "0099CC",
                "profile_sidebar_border_color": "FFFFFF",
                "profile_sidebar_fill_color": "DDFFCC",
                "profile_text_color": "333333",
                "profile_use_background_image": true,
                "protected": false,
                "screen_name": "MosesSapir",
                "statuses_count": 211660,
                "time_zone": "Greenland",
                "translator_type": "none",
                "url": "https://t.co/WWtQJCiPRg",
                "utc_offset": -10800,
                "verified": false
            }
        },
        "quoted_status_id": 950231050386632707,
        "quoted_status_id_str": "950231050386632707",
        "retweet_count": 211,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
        "text": "1983 .. world tour .. 12 cites .. 3 continents .. in 14 days .. 50 crew .. not a single baggage lost !!\ud83d\ude00 https://t.co/BcteAQ1dSI",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 18 05:16:47 +0000 2010",
            "default_profile": false,
            "default_profile_image": false,
            "description": "Actor ... well at least some are STILL saying so !!",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "srbachchan.tumblr.com",
                            "expanded_url": "http://srbachchan.tumblr.com",
                            "indices": [
                                0,
                                22
                            ],
                            "url": "http://t.co/Vkp8AE24J1"
                        }
                    ]
                }
            },
            "favourites_count": 62,
            "follow_request_sent": false,
            "followers_count": 32660825,
            "following": true,
            "friends_count": 1169,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 145125358,
            "id_str": "145125358",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 30488,
            "location": "Mumbai, India",
            "name": "Amitabh Bachchan",
            "notifications": false,
            "profile_background_color": "EDECE9",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/145125358/1500209752",
            "profile_image_url": "http://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_link_color": "DD2E44",
            "profile_sidebar_border_color": "000000",
            "profile_sidebar_fill_color": "000000",
            "profile_text_color": "000000",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "SrBachchan",
            "statuses_count": 60702,
            "time_zone": "Mumbai",
            "translator_type": "none",
            "url": "http://t.co/Vkp8AE24J1",
            "utc_offset": 19800,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 07:27:08 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/MosesSapir/sta\u2026",
                    "expanded_url": "https://twitter.com/MosesSapir/status/950231864056406016",
                    "indices": [
                        5,
                        28
                    ],
                    "url": "https://t.co/ceozq3pOCl"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 1494,
        "favorited": false,
        "geo": null,
        "id": 950267611601780736,
        "id_str": "950267611601780736",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": true,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "quoted_status": {
            "contributors": null,
            "coordinates": null,
            "created_at": "Mon Jan 08 05:05:05 +0000 2018",
            "entities": {
                "hashtags": [],
                "symbols": [],
                "urls": [
                    {
                        "display_url": "twitter.com/i/web/status/9\u2026",
                        "expanded_url": "https://twitter.com/i/web/status/950231864056406016",
                        "indices": [
                            117,
                            140
                        ],
                        "url": "https://t.co/WNKEwY8VkH"
                    }
                ],
                "user_mentions": [
                    {
                        "id": 145125358,
                        "id_str": "145125358",
                        "indices": [
                            19,
                            30
                        ],
                        "name": "Amitabh Bachchan",
                        "screen_name": "SrBachchan"
                    }
                ]
            },
            "favorite_count": 102,
            "favorited": false,
            "geo": null,
            "id": 950231864056406016,
            "id_str": "950231864056406016",
            "in_reply_to_screen_name": null,
            "in_reply_to_status_id": null,
            "in_reply_to_status_id_str": null,
            "in_reply_to_user_id": null,
            "in_reply_to_user_id_str": null,
            "is_quote_status": false,
            "lang": "en",
            "place": null,
            "possibly_sensitive": false,
            "possibly_sensitive_appealable": false,
            "retweet_count": 23,
            "retweeted": false,
            "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
            "text": "'Guddi' on the set @SrBachchan Amitabh ji shot for it for almost 10 days. But after 'Anand' became a huge hit,AB re\u2026 https://t.co/WNKEwY8VkH",
            "truncated": true,
            "user": {
                "contributors_enabled": false,
                "created_at": "Sun Dec 13 19:54:50 +0000 2009",
                "default_profile": false,
                "default_profile_image": false,
                "description": "ISG Media Ltd -TV Channel's-Management-Israel I Love Amitabh Bachchan",
                "entities": {
                    "description": {
                        "urls": []
                    },
                    "url": {
                        "urls": [
                            {
                                "display_url": "twitter.com/mosessapir",
                                "expanded_url": "https://twitter.com/mosessapir",
                                "indices": [
                                    0,
                                    23
                                ],
                                "url": "https://t.co/WWtQJCiPRg"
                            }
                        ]
                    }
                },
                "favourites_count": 27291,
                "follow_request_sent": false,
                "followers_count": 27139,
                "following": false,
                "friends_count": 2585,
                "geo_enabled": true,
                "has_extended_profile": true,
                "id": 96613617,
                "id_str": "96613617",
                "is_translation_enabled": false,
                "is_translator": false,
                "lang": "en",
                "listed_count": 79,
                "location": "Israel",
                "name": "Moses Sapir",
                "notifications": false,
                "profile_background_color": "FFF04D",
                "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/378800000056539541/3446a8469e434127a7cc8ede2f353197.jpeg",
                "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/378800000056539541/3446a8469e434127a7cc8ede2f353197.jpeg",
                "profile_background_tile": false,
                "profile_banner_url": "https://pbs.twimg.com/profile_banners/96613617/1514544292",
                "profile_image_url": "http://pbs.twimg.com/profile_images/939502811519307776/r5IGBt8r_normal.jpg",
                "profile_image_url_https": "https://pbs.twimg.com/profile_images/939502811519307776/r5IGBt8r_normal.jpg",
                "profile_link_color": "0099CC",
                "profile_sidebar_border_color": "FFFFFF",
                "profile_sidebar_fill_color": "DDFFCC",
                "profile_text_color": "333333",
                "profile_use_background_image": true,
                "protected": false,
                "screen_name": "MosesSapir",
                "statuses_count": 211660,
                "time_zone": "Greenland",
                "translator_type": "none",
                "url": "https://t.co/WWtQJCiPRg",
                "utc_offset": -10800,
                "verified": false
            }
        },
        "quoted_status_id": 950231864056406016,
        "quoted_status_id_str": "950231864056406016",
        "retweet_count": 97,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
        "text": "true https://t.co/ceozq3pOCl",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 18 05:16:47 +0000 2010",
            "default_profile": false,
            "default_profile_image": false,
            "description": "Actor ... well at least some are STILL saying so !!",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "srbachchan.tumblr.com",
                            "expanded_url": "http://srbachchan.tumblr.com",
                            "indices": [
                                0,
                                22
                            ],
                            "url": "http://t.co/Vkp8AE24J1"
                        }
                    ]
                }
            },
            "favourites_count": 62,
            "follow_request_sent": false,
            "followers_count": 32660825,
            "following": true,
            "friends_count": 1169,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 145125358,
            "id_str": "145125358",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 30488,
            "location": "Mumbai, India",
            "name": "Amitabh Bachchan",
            "notifications": false,
            "profile_background_color": "EDECE9",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/145125358/1500209752",
            "profile_image_url": "http://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_link_color": "DD2E44",
            "profile_sidebar_border_color": "000000",
            "profile_sidebar_fill_color": "000000",
            "profile_text_color": "000000",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "SrBachchan",
            "statuses_count": 60702,
            "time_zone": "Mumbai",
            "translator_type": "none",
            "url": "http://t.co/Vkp8AE24J1",
            "utc_offset": 19800,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 07:11:17 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/i/web/status/9\u2026",
                    "expanded_url": "https://twitter.com/i/web/status/950263623452012545",
                    "indices": [
                        117,
                        140
                    ],
                    "url": "https://t.co/FJqIPDd6YB"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 9466,
        "favorited": false,
        "geo": null,
        "id": 950263623452012545,
        "id_str": "950263623452012545",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "hi",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "retweet_count": 994,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
        "text": "T 2526 - \n\" \u0935\u093e\u0923\u0940 \" ~ \" \u0935\u0940\u0923\u093e \" \u092c\u0928\u0947 \n\" \u0935\u093e\u0923\u0940 \" ~ \" \u092c\u093e\u0923 \" \u0928 \u092c\u0928\u0947  !\n\u0915\u094d\u092f\u094b\u0915\u093f \n\" \u0935\u0940\u0923\u093e \" \u092c\u0928\u0940  \u0924\u094b,\n\u091c\u0940\u0935\u0928 \" \u0938\u0902\u0917\u0940\u0924 \"\u0939\u094b\u0917\u093e... !\n \"\u2026 https://t.co/FJqIPDd6YB",
        "truncated": true,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 18 05:16:47 +0000 2010",
            "default_profile": false,
            "default_profile_image": false,
            "description": "Actor ... well at least some are STILL saying so !!",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "srbachchan.tumblr.com",
                            "expanded_url": "http://srbachchan.tumblr.com",
                            "indices": [
                                0,
                                22
                            ],
                            "url": "http://t.co/Vkp8AE24J1"
                        }
                    ]
                }
            },
            "favourites_count": 62,
            "follow_request_sent": false,
            "followers_count": 32660825,
            "following": true,
            "friends_count": 1169,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 145125358,
            "id_str": "145125358",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 30488,
            "location": "Mumbai, India",
            "name": "Amitabh Bachchan",
            "notifications": false,
            "profile_background_color": "EDECE9",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/145125358/1500209752",
            "profile_image_url": "http://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_link_color": "DD2E44",
            "profile_sidebar_border_color": "000000",
            "profile_sidebar_fill_color": "000000",
            "profile_text_color": "000000",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "SrBachchan",
            "statuses_count": 60702,
            "time_zone": "Mumbai",
            "translator_type": "none",
            "url": "http://t.co/Vkp8AE24J1",
            "utc_offset": 19800,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 04:40:31 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/girlposts/stat\u2026",
                    "expanded_url": "https://twitter.com/girlposts/status/950224295527149568",
                    "indices": [
                        27,
                        50
                    ],
                    "url": "https://t.co/u57IGpsrcg"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 2444,
        "favorited": false,
        "geo": null,
        "id": 950225683321520130,
        "id_str": "950225683321520130",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": true,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "quoted_status": {
            "contributors": null,
            "coordinates": null,
            "created_at": "Mon Jan 08 04:35:01 +0000 2018",
            "entities": {
                "hashtags": [],
                "media": [
                    {
                        "display_url": "pic.twitter.com/tIeB57lLkK",
                        "expanded_url": "https://twitter.com/BabyAnimalPics/status/949818226736852993/video/1",
                        "id": 949818130322329600,
                        "id_str": "949818130322329600",
                        "indices": [
                            14,
                            37
                        ],
                        "media_url": "http://pbs.twimg.com/ext_tw_video_thumb/949818130322329600/pu/img/DZJ4KAVfzLrF_UYW.jpg",
                        "media_url_https": "https://pbs.twimg.com/ext_tw_video_thumb/949818130322329600/pu/img/DZJ4KAVfzLrF_UYW.jpg",
                        "sizes": {
                            "large": {
                                "h": 320,
                                "resize": "fit",
                                "w": 256
                            },
                            "medium": {
                                "h": 320,
                                "resize": "fit",
                                "w": 256
                            },
                            "small": {
                                "h": 320,
                                "resize": "fit",
                                "w": 256
                            },
                            "thumb": {
                                "h": 150,
                                "resize": "crop",
                                "w": 150
                            }
                        },
                        "source_status_id": 949818226736852993,
                        "source_status_id_str": "949818226736852993",
                        "source_user_id": 1372975219,
                        "source_user_id_str": "1372975219",
                        "type": "photo",
                        "url": "https://t.co/tIeB57lLkK"
                    }
                ],
                "symbols": [],
                "urls": [],
                "user_mentions": []
            },
            "extended_entities": {
                "media": [
                    {
                        "additional_media_info": {
                            "monetizable": false,
                            "source_user": {
                                "contributors_enabled": false,
                                "created_at": "Mon Apr 22 19:48:40 +0000 2013",
                                "default_profile": false,
                                "default_profile_image": false,
                                "description": "The cutest tweets imaginable. Follow us for instant happiness! \ud83d\udc95 *We own no content posted* \u2709\ufe0f: r@babyanimalvids.com",
                                "entities": {
                                    "description": {
                                        "urls": []
                                    },
                                    "url": {
                                        "urls": [
                                            {
                                                "display_url": "Instagram.com/babyanimalvids",
                                                "expanded_url": "http://Instagram.com/babyanimalvids",
                                                "indices": [
                                                    0,
                                                    23
                                                ],
                                                "url": "https://t.co/fGsZDMC6X1"
                                            }
                                        ]
                                    }
                                },
                                "favourites_count": 34784,
                                "follow_request_sent": false,
                                "followers_count": 2095954,
                                "following": false,
                                "friends_count": 55,
                                "geo_enabled": false,
                                "has_extended_profile": false,
                                "id": 1372975219,
                                "id_str": "1372975219",
                                "is_translation_enabled": false,
                                "is_translator": false,
                                "lang": "en",
                                "listed_count": 5831,
                                "location": "",
                                "name": "Baby Animals",
                                "notifications": false,
                                "profile_background_color": "000000",
                                "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
                                "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
                                "profile_background_tile": false,
                                "profile_banner_url": "https://pbs.twimg.com/profile_banners/1372975219/1512105214",
                                "profile_image_url": "http://pbs.twimg.com/profile_images/674018647833075712/4AnZjeut_normal.jpg",
                                "profile_image_url_https": "https://pbs.twimg.com/profile_images/674018647833075712/4AnZjeut_normal.jpg",
                                "profile_link_color": "FA2828",
                                "profile_sidebar_border_color": "FFFFFF",
                                "profile_sidebar_fill_color": "DDEEF6",
                                "profile_text_color": "333333",
                                "profile_use_background_image": false,
                                "protected": false,
                                "screen_name": "BabyAnimalPics",
                                "statuses_count": 32720,
                                "time_zone": "Atlantic Time (Canada)",
                                "translator_type": "regular",
                                "url": "https://t.co/fGsZDMC6X1",
                                "utc_offset": -14400,
                                "verified": false
                            }
                        },
                        "display_url": "pic.twitter.com/tIeB57lLkK",
                        "expanded_url": "https://twitter.com/BabyAnimalPics/status/949818226736852993/video/1",
                        "id": 949818130322329600,
                        "id_str": "949818130322329600",
                        "indices": [
                            14,
                            37
                        ],
                        "media_url": "http://pbs.twimg.com/ext_tw_video_thumb/949818130322329600/pu/img/DZJ4KAVfzLrF_UYW.jpg",
                        "media_url_https": "https://pbs.twimg.com/ext_tw_video_thumb/949818130322329600/pu/img/DZJ4KAVfzLrF_UYW.jpg",
                        "sizes": {
                            "large": {
                                "h": 320,
                                "resize": "fit",
                                "w": 256
                            },
                            "medium": {
                                "h": 320,
                                "resize": "fit",
                                "w": 256
                            },
                            "small": {
                                "h": 320,
                                "resize": "fit",
                                "w": 256
                            },
                            "thumb": {
                                "h": 150,
                                "resize": "crop",
                                "w": 150
                            }
                        },
                        "source_status_id": 949818226736852993,
                        "source_status_id_str": "949818226736852993",
                        "source_user_id": 1372975219,
                        "source_user_id_str": "1372975219",
                        "type": "video",
                        "url": "https://t.co/tIeB57lLkK",
                        "video_info": {
                            "aspect_ratio": [
                                4,
                                5
                            ],
                            "duration_millis": 58567,
                            "variants": [
                                {
                                    "content_type": "application/x-mpegURL",
                                    "url": "https://video.twimg.com/ext_tw_video/949818130322329600/pu/pl/wYbgPN-1-xpK_lSY.m3u8"
                                },
                                {
                                    "bitrate": 320000,
                                    "content_type": "video/mp4",
                                    "url": "https://video.twimg.com/ext_tw_video/949818130322329600/pu/vid/256x320/xAoX4e6wWvIYMUGm.mp4"
                                }
                            ]
                        }
                    }
                ]
            },
            "favorite_count": 7814,
            "favorited": false,
            "geo": null,
            "id": 950224295527149568,
            "id_str": "950224295527149568",
            "in_reply_to_screen_name": null,
            "in_reply_to_status_id": null,
            "in_reply_to_status_id_str": null,
            "in_reply_to_user_id": null,
            "in_reply_to_user_id_str": null,
            "is_quote_status": false,
            "lang": "en",
            "place": null,
            "possibly_sensitive": false,
            "possibly_sensitive_appealable": false,
            "retweet_count": 1611,
            "retweeted": false,
            "source": "<a href=\"http://bufferapp.com\" rel=\"nofollow\">Buffer</a>",
            "text": "So fluffy \ud83d\ude0d\ud83d\ude0d  https://t.co/tIeB57lLkK",
            "truncated": false,
            "user": {
                "contributors_enabled": false,
                "created_at": "Wed Apr 14 04:20:26 +0000 2010",
                "default_profile": false,
                "default_profile_image": false,
                "description": "I'm a Victoria's Secret model but it's such a secret not even Victoria knows.",
                "entities": {
                    "description": {
                        "urls": []
                    }
                },
                "favourites_count": 72139,
                "follow_request_sent": false,
                "followers_count": 9493063,
                "following": false,
                "friends_count": 208696,
                "geo_enabled": false,
                "has_extended_profile": false,
                "id": 132774626,
                "id_str": "132774626",
                "is_translation_enabled": false,
                "is_translator": false,
                "lang": "en",
                "listed_count": 17427,
                "location": "",
                "name": "Common White Girl",
                "notifications": false,
                "profile_background_color": "FFFFFF",
                "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/378800000170694213/oBOPesu1.png",
                "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/378800000170694213/oBOPesu1.png",
                "profile_background_tile": false,
                "profile_banner_url": "https://pbs.twimg.com/profile_banners/132774626/1513390021",
                "profile_image_url": "http://pbs.twimg.com/profile_images/859863737146253317/ClunlkwL_normal.jpg",
                "profile_image_url_https": "https://pbs.twimg.com/profile_images/859863737146253317/ClunlkwL_normal.jpg",
                "profile_link_color": "FF33CC",
                "profile_sidebar_border_color": "FFFFFF",
                "profile_sidebar_fill_color": "DDEEF6",
                "profile_text_color": "333333",
                "profile_use_background_image": true,
                "protected": false,
                "screen_name": "girlposts",
                "statuses_count": 3359,
                "time_zone": "Eastern Time (US & Canada)",
                "translator_type": "regular",
                "url": null,
                "utc_offset": -18000,
                "verified": false
            }
        },
        "quoted_status_id": 950224295527149568,
        "quoted_status_id_str": "950224295527149568",
        "retweet_count": 217,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
        "text": "soooooo cuutteeeeee .. !!! https://t.co/u57IGpsrcg",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 18 05:16:47 +0000 2010",
            "default_profile": false,
            "default_profile_image": false,
            "description": "Actor ... well at least some are STILL saying so !!",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "srbachchan.tumblr.com",
                            "expanded_url": "http://srbachchan.tumblr.com",
                            "indices": [
                                0,
                                22
                            ],
                            "url": "http://t.co/Vkp8AE24J1"
                        }
                    ]
                }
            },
            "favourites_count": 62,
            "follow_request_sent": false,
            "followers_count": 32660825,
            "following": true,
            "friends_count": 1169,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 145125358,
            "id_str": "145125358",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 30488,
            "location": "Mumbai, India",
            "name": "Amitabh Bachchan",
            "notifications": false,
            "profile_background_color": "EDECE9",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/145125358/1500209752",
            "profile_image_url": "http://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_link_color": "DD2E44",
            "profile_sidebar_border_color": "000000",
            "profile_sidebar_fill_color": "000000",
            "profile_text_color": "000000",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "SrBachchan",
            "statuses_count": 60702,
            "time_zone": "Mumbai",
            "translator_type": "none",
            "url": "http://t.co/Vkp8AE24J1",
            "utc_offset": 19800,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 04:38:10 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/i/web/status/9\u2026",
                    "expanded_url": "https://twitter.com/i/web/status/950225091232546818",
                    "indices": [
                        117,
                        140
                    ],
                    "url": "https://t.co/OvrtmfTxoo"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 3329,
        "favorited": false,
        "geo": null,
        "id": 950225091232546818,
        "id_str": "950225091232546818",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "retweet_count": 245,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
        "text": "T 2586 - This kid is NOT 2 yrs old .. she's 202 years .. !!! hahaha .. too cute and funny .. watch .. \ud83d\ude00\ud83d\ude00\ud83d\ude00\ud83d\ude00\ud83d\ude00\ud83d\ude02\ud83d\ude02\ud83d\ude02\ud83d\ude02\ud83d\ude02\ud83d\ude02\ud83d\ude02\ud83d\ude02\u2026 https://t.co/OvrtmfTxoo",
        "truncated": true,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 18 05:16:47 +0000 2010",
            "default_profile": false,
            "default_profile_image": false,
            "description": "Actor ... well at least some are STILL saying so !!",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "srbachchan.tumblr.com",
                            "expanded_url": "http://srbachchan.tumblr.com",
                            "indices": [
                                0,
                                22
                            ],
                            "url": "http://t.co/Vkp8AE24J1"
                        }
                    ]
                }
            },
            "favourites_count": 62,
            "follow_request_sent": false,
            "followers_count": 32660825,
            "following": true,
            "friends_count": 1169,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 145125358,
            "id_str": "145125358",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 30488,
            "location": "Mumbai, India",
            "name": "Amitabh Bachchan",
            "notifications": false,
            "profile_background_color": "EDECE9",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/145125358/1500209752",
            "profile_image_url": "http://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_link_color": "DD2E44",
            "profile_sidebar_border_color": "000000",
            "profile_sidebar_fill_color": "000000",
            "profile_text_color": "000000",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "SrBachchan",
            "statuses_count": 60702,
            "time_zone": "Mumbai",
            "translator_type": "none",
            "url": "http://t.co/Vkp8AE24J1",
            "utc_offset": 19800,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:58:13 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/prashantkawadi\u2026",
                    "expanded_url": "https://twitter.com/prashantkawadia/status/950038441374359553",
                    "indices": [
                        53,
                        76
                    ],
                    "url": "https://t.co/9duV3T7bTF"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 1602,
        "favorited": false,
        "geo": null,
        "id": 950215038286970880,
        "id_str": "950215038286970880",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": true,
        "lang": "und",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "quoted_status": {
            "contributors": null,
            "coordinates": null,
            "created_at": "Sun Jan 07 16:16:29 +0000 2018",
            "entities": {
                "hashtags": [],
                "media": [
                    {
                        "display_url": "pic.twitter.com/wOK2IL7HsE",
                        "expanded_url": "https://twitter.com/prashantkawadia/status/950038441374359553/photo/1",
                        "id": 950038415340269568,
                        "id_str": "950038415340269568",
                        "indices": [
                            68,
                            91
                        ],
                        "media_url": "http://pbs.twimg.com/media/DS82571U8AAzrIo.jpg",
                        "media_url_https": "https://pbs.twimg.com/media/DS82571U8AAzrIo.jpg",
                        "sizes": {
                            "large": {
                                "h": 1773,
                                "resize": "fit",
                                "w": 1110
                            },
                            "medium": {
                                "h": 1200,
                                "resize": "fit",
                                "w": 751
                            },
                            "small": {
                                "h": 680,
                                "resize": "fit",
                                "w": 426
                            },
                            "thumb": {
                                "h": 150,
                                "resize": "crop",
                                "w": 150
                            }
                        },
                        "type": "photo",
                        "url": "https://t.co/wOK2IL7HsE"
                    }
                ],
                "symbols": [],
                "urls": [],
                "user_mentions": [
                    {
                        "id": 145125358,
                        "id_str": "145125358",
                        "indices": [
                            8,
                            19
                        ],
                        "name": "Amitabh Bachchan",
                        "screen_name": "SrBachchan"
                    }
                ]
            },
            "extended_entities": {
                "media": [
                    {
                        "display_url": "pic.twitter.com/wOK2IL7HsE",
                        "expanded_url": "https://twitter.com/prashantkawadia/status/950038441374359553/photo/1",
                        "id": 950038415340269568,
                        "id_str": "950038415340269568",
                        "indices": [
                            68,
                            91
                        ],
                        "media_url": "http://pbs.twimg.com/media/DS82571U8AAzrIo.jpg",
                        "media_url_https": "https://pbs.twimg.com/media/DS82571U8AAzrIo.jpg",
                        "sizes": {
                            "large": {
                                "h": 1773,
                                "resize": "fit",
                                "w": 1110
                            },
                            "medium": {
                                "h": 1200,
                                "resize": "fit",
                                "w": 751
                            },
                            "small": {
                                "h": 680,
                                "resize": "fit",
                                "w": 426
                            },
                            "thumb": {
                                "h": 150,
                                "resize": "crop",
                                "w": 150
                            }
                        },
                        "type": "photo",
                        "url": "https://t.co/wOK2IL7HsE"
                    }
                ]
            },
            "favorite_count": 95,
            "favorited": false,
            "geo": null,
            "id": 950038441374359553,
            "id_str": "950038441374359553",
            "in_reply_to_screen_name": null,
            "in_reply_to_status_id": null,
            "in_reply_to_status_id_str": null,
            "in_reply_to_user_id": null,
            "in_reply_to_user_id_str": null,
            "is_quote_status": false,
            "lang": "hi",
            "place": null,
            "possibly_sensitive": false,
            "possibly_sensitive_appealable": false,
            "retweet_count": 19,
            "retweeted": false,
            "source": "<a href=\"http://twitter.com/download/android\" rel=\"nofollow\">Twitter for Android</a>",
            "text": "Gurudev @SrBachchan Sir in &amp; as \nAMI - - - TABH \nAMIT - - - ABH https://t.co/wOK2IL7HsE",
            "truncated": false,
            "user": {
                "contributors_enabled": false,
                "created_at": "Sat Aug 27 17:38:16 +0000 2011",
                "default_profile": false,
                "default_profile_image": false,
                "description": "Blessed with Follow back by... My Inspiration...The Legend on the Planet @SrBachchan Sir.\n\u092e\u0928 \u0915\u093e \u0939\u094b \u0924\u094b \u0905\u091a\u094d\u091b\u093e, \u0928\u0939\u0940 \u0924\u094b \u091c\u094d\u092f\u093e\u0926\u093e \u0905\u091a\u094d\u091b\u093e...\nDie - Hard fan of AB Sir",
                "entities": {
                    "description": {
                        "urls": []
                    }
                },
                "favourites_count": 32522,
                "follow_request_sent": false,
                "followers_count": 4314,
                "following": false,
                "friends_count": 494,
                "geo_enabled": false,
                "has_extended_profile": true,
                "id": 363188892,
                "id_str": "363188892",
                "is_translation_enabled": false,
                "is_translator": false,
                "lang": "en",
                "listed_count": 9,
                "location": "",
                "name": "\u092a\u094d\u0930\u0936\u093e\u0902\u0924 \u0915\u093e\u0935\u095c\u093f\u092f\u093e\ud83d\udc97\ud83c\udd8e",
                "notifications": false,
                "profile_background_color": "000000",
                "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
                "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
                "profile_background_tile": false,
                "profile_banner_url": "https://pbs.twimg.com/profile_banners/363188892/1457868743",
                "profile_image_url": "http://pbs.twimg.com/profile_images/889897191359250433/0wuV_6vy_normal.jpg",
                "profile_image_url_https": "https://pbs.twimg.com/profile_images/889897191359250433/0wuV_6vy_normal.jpg",
                "profile_link_color": "3B94D9",
                "profile_sidebar_border_color": "000000",
                "profile_sidebar_fill_color": "000000",
                "profile_text_color": "000000",
                "profile_use_background_image": false,
                "protected": false,
                "screen_name": "prashantkawadia",
                "statuses_count": 24962,
                "time_zone": null,
                "translator_type": "none",
                "url": null,
                "utc_offset": null,
                "verified": false
            }
        },
        "quoted_status_id": 950038441374359553,
        "quoted_status_id_str": "950038441374359553",
        "retweet_count": 133,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
        "text": "yes .. Ami - \u0924\u092c ; and Amit - \u0905\u092c  ... truly ingenious https://t.co/9duV3T7bTF",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 18 05:16:47 +0000 2010",
            "default_profile": false,
            "default_profile_image": false,
            "description": "Actor ... well at least some are STILL saying so !!",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "srbachchan.tumblr.com",
                            "expanded_url": "http://srbachchan.tumblr.com",
                            "indices": [
                                0,
                                22
                            ],
                            "url": "http://t.co/Vkp8AE24J1"
                        }
                    ]
                }
            },
            "favourites_count": 62,
            "follow_request_sent": false,
            "followers_count": 32660825,
            "following": true,
            "friends_count": 1169,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 145125358,
            "id_str": "145125358",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 30488,
            "location": "Mumbai, India",
            "name": "Amitabh Bachchan",
            "notifications": false,
            "profile_background_color": "EDECE9",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/625668922574962688/bUhhoKyd.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/145125358/1500209752",
            "profile_image_url": "http://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/768780297437450240/FJBOm5n8_normal.jpg",
            "profile_link_color": "DD2E44",
            "profile_sidebar_border_color": "000000",
            "profile_sidebar_fill_color": "000000",
            "profile_text_color": "000000",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "SrBachchan",
            "statuses_count": 60702,
            "time_zone": "Mumbai",
            "translator_type": "none",
            "url": "http://t.co/Vkp8AE24J1",
            "utc_offset": 19800,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:48:25 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [],
            "user_mentions": [
                {
                    "id": 14285735,
                    "id_str": "14285735",
                    "indices": [
                        3,
                        17
                    ],
                    "name": "andreasdotorg",
                    "screen_name": "andreasdotorg"
                }
            ]
        },
        "favorite_count": 0,
        "favorited": false,
        "geo": null,
        "id": 950212572304719872,
        "id_str": "950212572304719872",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": true,
        "lang": "en",
        "place": null,
        "quoted_status_id": 949457156071288833,
        "quoted_status_id_str": "949457156071288833",
        "retweet_count": 358,
        "retweeted": false,
        "retweeted_status": {
            "contributors": null,
            "coordinates": null,
            "created_at": "Sat Jan 06 12:16:22 +0000 2018",
            "entities": {
                "hashtags": [],
                "symbols": [],
                "urls": [
                    {
                        "display_url": "twitter.com/chanian/status\u2026",
                        "expanded_url": "https://twitter.com/chanian/status/949457156071288833",
                        "indices": [
                            102,
                            125
                        ],
                        "url": "https://t.co/GlSFqok8PX"
                    }
                ],
                "user_mentions": []
            },
            "favorite_count": 446,
            "favorited": false,
            "geo": null,
            "id": 949615624250187777,
            "id_str": "949615624250187777",
            "in_reply_to_screen_name": null,
            "in_reply_to_status_id": null,
            "in_reply_to_status_id_str": null,
            "in_reply_to_user_id": null,
            "in_reply_to_user_id_str": null,
            "is_quote_status": true,
            "lang": "en",
            "place": null,
            "possibly_sensitive": false,
            "possibly_sensitive_appealable": false,
            "quoted_status": {
                "contributors": null,
                "coordinates": null,
                "created_at": "Sat Jan 06 01:46:40 +0000 2018",
                "entities": {
                    "hashtags": [
                        {
                            "indices": [
                                4,
                                13
                            ],
                            "text": "Meltdown"
                        }
                    ],
                    "symbols": [],
                    "urls": [
                        {
                            "display_url": "twitter.com/i/web/status/9\u2026",
                            "expanded_url": "https://twitter.com/i/web/status/949457156071288833",
                            "indices": [
                                117,
                                140
                            ],
                            "url": "https://t.co/2FuYXlhbzS"
                        }
                    ],
                    "user_mentions": []
                },
                "favorite_count": 1279,
                "favorited": false,
                "geo": null,
                "id": 949457156071288833,
                "id_str": "949457156071288833",
                "in_reply_to_screen_name": null,
                "in_reply_to_status_id": null,
                "in_reply_to_status_id_str": null,
                "in_reply_to_user_id": null,
                "in_reply_to_user_id_str": null,
                "is_quote_status": false,
                "lang": "en",
                "place": null,
                "possibly_sensitive": false,
                "possibly_sensitive_appealable": false,
                "retweet_count": 1264,
                "retweeted": false,
                "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
                "text": "The #Meltdown patch (presumably) being applied to the underlying AWS EC2 hypervisor on some of our production Kafka\u2026 https://t.co/2FuYXlhbzS",
                "truncated": true,
                "user": {
                    "contributors_enabled": false,
                    "created_at": "Thu Mar 05 06:42:32 +0000 2009",
                    "default_profile": false,
                    "default_profile_image": false,
                    "description": "Director of Engineering @BranchMetrics. Platforms, Deep links and Analytics. Previously Engineering @Twitter, (\u00ef\u0101\u00f1)",
                    "entities": {
                        "description": {
                            "urls": []
                        },
                        "url": {
                            "urls": [
                                {
                                    "display_url": "chanian.com",
                                    "expanded_url": "http://chanian.com",
                                    "indices": [
                                        0,
                                        23
                                    ],
                                    "url": "https://t.co/cZFF8e88lO"
                                }
                            ]
                        }
                    },
                    "favourites_count": 10506,
                    "follow_request_sent": false,
                    "followers_count": 14895,
                    "following": false,
                    "friends_count": 1001,
                    "geo_enabled": true,
                    "has_extended_profile": false,
                    "id": 22891211,
                    "id_str": "22891211",
                    "is_translation_enabled": false,
                    "is_translator": false,
                    "lang": "en",
                    "listed_count": 346,
                    "location": "Toronto \u2708 San Francisco",
                    "name": "Ian Chan",
                    "notifications": false,
                    "profile_background_color": "022330",
                    "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/663303208/kpzmxbnwvxk9cfr7hpdw.png",
                    "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/663303208/kpzmxbnwvxk9cfr7hpdw.png",
                    "profile_background_tile": true,
                    "profile_banner_url": "https://pbs.twimg.com/profile_banners/22891211/1514616309",
                    "profile_image_url": "http://pbs.twimg.com/profile_images/755591515565494272/ugfxjY2J_normal.jpg",
                    "profile_image_url_https": "https://pbs.twimg.com/profile_images/755591515565494272/ugfxjY2J_normal.jpg",
                    "profile_link_color": "0084B4",
                    "profile_sidebar_border_color": "FFFFFF",
                    "profile_sidebar_fill_color": "C0DFEC",
                    "profile_text_color": "333333",
                    "profile_use_background_image": true,
                    "protected": false,
                    "screen_name": "chanian",
                    "statuses_count": 12216,
                    "time_zone": "Pacific Time (US & Canada)",
                    "translator_type": "regular",
                    "url": "https://t.co/cZFF8e88lO",
                    "utc_offset": -28800,
                    "verified": false
                }
            },
            "quoted_status_id": 949457156071288833,
            "quoted_status_id_str": "949457156071288833",
            "retweet_count": 358,
            "retweeted": false,
            "source": "<a href=\"http://twitter.com/download/android\" rel=\"nofollow\">Twitter for Android</a>",
            "text": "Now who will end up paying for this? In our shop, this translates into seven figures in our AWS bill. https://t.co/GlSFqok8PX",
            "truncated": false,
            "user": {
                "contributors_enabled": false,
                "created_at": "Wed Apr 02 17:53:05 +0000 2008",
                "default_profile": true,
                "default_profile_image": false,
                "description": "I'm a hacker, pretty much in the old school sense of the word. But I do know IT security too.",
                "entities": {
                    "description": {
                        "urls": []
                    }
                },
                "favourites_count": 12790,
                "follow_request_sent": false,
                "followers_count": 10343,
                "following": false,
                "friends_count": 2821,
                "geo_enabled": true,
                "has_extended_profile": false,
                "id": 14285735,
                "id_str": "14285735",
                "is_translation_enabled": false,
                "is_translator": false,
                "lang": "de",
                "listed_count": 392,
                "location": "",
                "name": "andreasdotorg",
                "notifications": false,
                "profile_background_color": "C0DEED",
                "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
                "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
                "profile_background_tile": false,
                "profile_image_url": "http://pbs.twimg.com/profile_images/2857263613/87df849b9667509c53079d7bb7fd0b31_normal.jpeg",
                "profile_image_url_https": "https://pbs.twimg.com/profile_images/2857263613/87df849b9667509c53079d7bb7fd0b31_normal.jpeg",
                "profile_link_color": "1DA1F2",
                "profile_sidebar_border_color": "C0DEED",
                "profile_sidebar_fill_color": "DDEEF6",
                "profile_text_color": "333333",
                "profile_use_background_image": true,
                "protected": false,
                "screen_name": "andreasdotorg",
                "statuses_count": 50743,
                "time_zone": "Greenland",
                "translator_type": "none",
                "url": null,
                "utc_offset": -10800,
                "verified": false
            }
        },
        "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
        "text": "RT @andreasdotorg: Now who will end up paying for this? In our shop, this translates into seven figures in our AWS bill. https://t.co/GlSFq\u2026",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Thu Aug 21 21:00:09 +0000 2008",
            "default_profile": true,
            "default_profile_image": false,
            "description": "NetApp Public Cloud Architect, SF Giants Fan, SJ Sharks fan. Ardent defender of the Oxford comma. Opinions my own.",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 3116,
            "follow_request_sent": false,
            "followers_count": 336,
            "following": true,
            "friends_count": 509,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 15936833,
            "id_str": "15936833",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 61,
            "location": "Los Gatos, CA",
            "name": "Mark Beaupre",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_tile": false,
            "profile_image_url": "http://pbs.twimg.com/profile_images/686795481914028032/H-yiPjKN_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/686795481914028032/H-yiPjKN_normal.jpg",
            "profile_link_color": "1DA1F2",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "mark_beaupre",
            "statuses_count": 4153,
            "time_zone": "Pacific Time (US & Canada)",
            "translator_type": "none",
            "url": null,
            "utc_offset": -28800,
            "verified": false
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:38:23 +0000 2018",
        "entities": {
            "hashtags": [
                {
                    "indices": [
                        19,
                        28
                    ],
                    "text": "prodmgmt"
                }
            ],
            "symbols": [],
            "urls": [
                {
                    "display_url": "medium.com/@johnpcutler/a\u2026",
                    "expanded_url": "https://medium.com/@johnpcutler/answer-these-16-questions-about-your-roadmap-items-717bb9e7978f?source=linkShare-4c3f4fe11e6b-1515295164",
                    "indices": [
                        80,
                        103
                    ],
                    "url": "https://t.co/aZfq0eqAYw"
                }
            ],
            "user_mentions": [
                {
                    "id": 533409964,
                    "id_str": "533409964",
                    "indices": [
                        3,
                        17
                    ],
                    "name": "John Cutler",
                    "screen_name": "johncutlefish"
                }
            ]
        },
        "favorite_count": 0,
        "favorited": false,
        "geo": null,
        "id": 950210044775165953,
        "id_str": "950210044775165953",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "retweet_count": 24,
        "retweeted": false,
        "retweeted_status": {
            "contributors": null,
            "coordinates": null,
            "created_at": "Sun Jan 07 03:20:49 +0000 2018",
            "entities": {
                "hashtags": [
                    {
                        "indices": [
                            0,
                            9
                        ],
                        "text": "prodmgmt"
                    }
                ],
                "symbols": [],
                "urls": [
                    {
                        "display_url": "medium.com/@johnpcutler/a\u2026",
                        "expanded_url": "https://medium.com/@johnpcutler/answer-these-16-questions-about-your-roadmap-items-717bb9e7978f?source=linkShare-4c3f4fe11e6b-1515295164",
                        "indices": [
                            61,
                            84
                        ],
                        "url": "https://t.co/aZfq0eqAYw"
                    }
                ],
                "user_mentions": []
            },
            "favorite_count": 62,
            "favorited": false,
            "geo": null,
            "id": 949843236532600832,
            "id_str": "949843236532600832",
            "in_reply_to_screen_name": null,
            "in_reply_to_status_id": null,
            "in_reply_to_status_id_str": null,
            "in_reply_to_user_id": null,
            "in_reply_to_user_id_str": null,
            "is_quote_status": false,
            "lang": "en",
            "place": null,
            "possibly_sensitive": false,
            "possibly_sensitive_appealable": false,
            "retweet_count": 24,
            "retweeted": false,
            "source": "<a href=\"https://about.twitter.com/products/tweetdeck\" rel=\"nofollow\">TweetDeck</a>",
            "text": "#prodmgmt should be prepared to answer these questions ... \n\nhttps://t.co/aZfq0eqAYw",
            "truncated": false,
            "user": {
                "contributors_enabled": false,
                "created_at": "Thu Mar 22 19:22:32 +0000 2012",
                "default_profile": true,
                "default_profile_image": false,
                "description": "Product development nut. I love wrangling complex problems and answering the why with qual/quant data. I post writing at https://t.co/r1JgWT0NOs",
                "entities": {
                    "description": {
                        "urls": [
                            {
                                "display_url": "medium.com/@johnpcutler",
                                "expanded_url": "https://medium.com/@johnpcutler",
                                "indices": [
                                    121,
                                    144
                                ],
                                "url": "https://t.co/r1JgWT0NOs"
                            }
                        ]
                    }
                },
                "favourites_count": 37944,
                "follow_request_sent": false,
                "followers_count": 14149,
                "following": false,
                "friends_count": 10431,
                "geo_enabled": true,
                "has_extended_profile": false,
                "id": 533409964,
                "id_str": "533409964",
                "is_translation_enabled": false,
                "is_translator": false,
                "lang": "en",
                "listed_count": 1527,
                "location": "",
                "name": "John Cutler",
                "notifications": false,
                "profile_background_color": "C0DEED",
                "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
                "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
                "profile_background_tile": false,
                "profile_banner_url": "https://pbs.twimg.com/profile_banners/533409964/1454244608",
                "profile_image_url": "http://pbs.twimg.com/profile_images/870169811812106241/z9fdNNjW_normal.jpg",
                "profile_image_url_https": "https://pbs.twimg.com/profile_images/870169811812106241/z9fdNNjW_normal.jpg",
                "profile_link_color": "1DA1F2",
                "profile_sidebar_border_color": "C0DEED",
                "profile_sidebar_fill_color": "DDEEF6",
                "profile_text_color": "333333",
                "profile_use_background_image": true,
                "protected": false,
                "screen_name": "johncutlefish",
                "statuses_count": 23761,
                "time_zone": null,
                "translator_type": "none",
                "url": null,
                "utc_offset": null,
                "verified": false
            }
        },
        "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
        "text": "RT @johncutlefish: #prodmgmt should be prepared to answer these questions ... \n\nhttps://t.co/aZfq0eqAYw",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Thu Aug 21 21:00:09 +0000 2008",
            "default_profile": true,
            "default_profile_image": false,
            "description": "NetApp Public Cloud Architect, SF Giants Fan, SJ Sharks fan. Ardent defender of the Oxford comma. Opinions my own.",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 3116,
            "follow_request_sent": false,
            "followers_count": 336,
            "following": true,
            "friends_count": 509,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 15936833,
            "id_str": "15936833",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 61,
            "location": "Los Gatos, CA",
            "name": "Mark Beaupre",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_tile": false,
            "profile_image_url": "http://pbs.twimg.com/profile_images/686795481914028032/H-yiPjKN_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/686795481914028032/H-yiPjKN_normal.jpg",
            "profile_link_color": "1DA1F2",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "mark_beaupre",
            "statuses_count": 4153,
            "time_zone": "Pacific Time (US & Canada)",
            "translator_type": "none",
            "url": null,
            "utc_offset": -28800,
            "verified": false
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:24:23 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/i/web/status/9\u2026",
                    "expanded_url": "https://twitter.com/i/web/status/950206523690639360",
                    "indices": [
                        117,
                        140
                    ],
                    "url": "https://t.co/e3TjsBYrtb"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 67884,
        "favorited": false,
        "geo": null,
        "id": 950206523690639360,
        "id_str": "950206523690639360",
        "in_reply_to_screen_name": "realDonaldTrump",
        "in_reply_to_status_id": 950206338411499520,
        "in_reply_to_status_id_str": "950206338411499520",
        "in_reply_to_user_id": 25073877,
        "in_reply_to_user_id_str": "25073877",
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "retweet_count": 15667,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
        "text": "...Clinton in the WH, doubling down on Barack Obama\u2019s failed policies, washes away any doubts that America made the\u2026 https://t.co/e3TjsBYrtb",
        "truncated": true,
        "user": {
            "contributors_enabled": false,
            "created_at": "Wed Mar 18 13:46:38 +0000 2009",
            "default_profile": false,
            "default_profile_image": false,
            "description": "45th President of the United States of America\ud83c\uddfa\ud83c\uddf8",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "Instagram.com/realDonaldTrump",
                            "expanded_url": "http://www.Instagram.com/realDonaldTrump",
                            "indices": [
                                0,
                                23
                            ],
                            "url": "https://t.co/OMxB0x7xC5"
                        }
                    ]
                }
            },
            "favourites_count": 24,
            "follow_request_sent": false,
            "followers_count": 46288008,
            "following": true,
            "friends_count": 45,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 25073877,
            "id_str": "25073877",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 82407,
            "location": "Washington, DC",
            "name": "Donald J. Trump",
            "notifications": false,
            "profile_background_color": "6D5C18",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/530021613/trump_scotland__43_of_70_cc.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/530021613/trump_scotland__43_of_70_cc.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/25073877/1514347856",
            "profile_image_url": "http://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_normal.jpg",
            "profile_link_color": "1B95E0",
            "profile_sidebar_border_color": "BDDCAD",
            "profile_sidebar_fill_color": "C5CEC0",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "realDonaldTrump",
            "statuses_count": 36721,
            "time_zone": "Eastern Time (US & Canada)",
            "translator_type": "regular",
            "url": "https://t.co/OMxB0x7xC5",
            "utc_offset": -18000,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:23:39 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [
                {
                    "display_url": "twitter.com/i/web/status/9\u2026",
                    "expanded_url": "https://twitter.com/i/web/status/950206338411499520",
                    "indices": [
                        116,
                        139
                    ],
                    "url": "https://t.co/u5Zn5BjFu1"
                }
            ],
            "user_mentions": []
        },
        "favorite_count": 66671,
        "favorited": false,
        "geo": null,
        "id": 950206338411499520,
        "id_str": "950206338411499520",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "retweet_count": 14204,
        "retweeted": false,
        "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
        "text": "\u201cHis is turning out to be an enormously consequential presidency. So much so that, despite my own frustration over\u2026 https://t.co/u5Zn5BjFu1",
        "truncated": true,
        "user": {
            "contributors_enabled": false,
            "created_at": "Wed Mar 18 13:46:38 +0000 2009",
            "default_profile": false,
            "default_profile_image": false,
            "description": "45th President of the United States of America\ud83c\uddfa\ud83c\uddf8",
            "entities": {
                "description": {
                    "urls": []
                },
                "url": {
                    "urls": [
                        {
                            "display_url": "Instagram.com/realDonaldTrump",
                            "expanded_url": "http://www.Instagram.com/realDonaldTrump",
                            "indices": [
                                0,
                                23
                            ],
                            "url": "https://t.co/OMxB0x7xC5"
                        }
                    ]
                }
            },
            "favourites_count": 24,
            "follow_request_sent": false,
            "followers_count": 46288008,
            "following": true,
            "friends_count": 45,
            "geo_enabled": true,
            "has_extended_profile": false,
            "id": 25073877,
            "id_str": "25073877",
            "is_translation_enabled": true,
            "is_translator": false,
            "lang": "en",
            "listed_count": 82407,
            "location": "Washington, DC",
            "name": "Donald J. Trump",
            "notifications": false,
            "profile_background_color": "6D5C18",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/530021613/trump_scotland__43_of_70_cc.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/530021613/trump_scotland__43_of_70_cc.jpg",
            "profile_background_tile": true,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/25073877/1514347856",
            "profile_image_url": "http://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_normal.jpg",
            "profile_link_color": "1B95E0",
            "profile_sidebar_border_color": "BDDCAD",
            "profile_sidebar_fill_color": "C5CEC0",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "realDonaldTrump",
            "statuses_count": 36721,
            "time_zone": "Eastern Time (US & Canada)",
            "translator_type": "regular",
            "url": "https://t.co/OMxB0x7xC5",
            "utc_offset": -18000,
            "verified": true
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:22:58 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [],
            "user_mentions": []
        },
        "favorite_count": 0,
        "favorited": false,
        "geo": null,
        "id": 950206165543141376,
        "id_str": "950206165543141376",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "retweet_count": 0,
        "retweeted": false,
        "source": "<a href=\"http://test.com\" rel=\"nofollow\">rajat_test</a>",
        "text": "Tweeting with timer test python script #4",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 19 04:40:04 +0000 2009",
            "default_profile": true,
            "default_profile_image": true,
            "description": "",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 0,
            "follow_request_sent": false,
            "followers_count": 28,
            "following": false,
            "friends_count": 17,
            "geo_enabled": false,
            "has_extended_profile": false,
            "id": 41056206,
            "id_str": "41056206",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 0,
            "location": "",
            "name": "RAJAT KAURA",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_tile": false,
            "profile_image_url": "http://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_image_url_https": "https://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_link_color": "1DA1F2",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": true,
            "screen_name": "RKauras",
            "statuses_count": 50,
            "time_zone": null,
            "translator_type": "none",
            "url": null,
            "utc_offset": null,
            "verified": false
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:22:53 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [],
            "user_mentions": []
        },
        "favorite_count": 0,
        "favorited": false,
        "geo": null,
        "id": 950206143355224064,
        "id_str": "950206143355224064",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "retweet_count": 0,
        "retweeted": false,
        "source": "<a href=\"http://test.com\" rel=\"nofollow\">rajat_test</a>",
        "text": "Tweeting with timer test python script #3",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 19 04:40:04 +0000 2009",
            "default_profile": true,
            "default_profile_image": true,
            "description": "",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 0,
            "follow_request_sent": false,
            "followers_count": 28,
            "following": false,
            "friends_count": 17,
            "geo_enabled": false,
            "has_extended_profile": false,
            "id": 41056206,
            "id_str": "41056206",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 0,
            "location": "",
            "name": "RAJAT KAURA",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_tile": false,
            "profile_image_url": "http://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_image_url_https": "https://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_link_color": "1DA1F2",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": true,
            "screen_name": "RKauras",
            "statuses_count": 50,
            "time_zone": null,
            "translator_type": "none",
            "url": null,
            "utc_offset": null,
            "verified": false
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:22:47 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [],
            "user_mentions": []
        },
        "favorite_count": 0,
        "favorited": false,
        "geo": null,
        "id": 950206121167302656,
        "id_str": "950206121167302656",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "retweet_count": 0,
        "retweeted": false,
        "source": "<a href=\"http://test.com\" rel=\"nofollow\">rajat_test</a>",
        "text": "Tweeting with timer test python script #2",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 19 04:40:04 +0000 2009",
            "default_profile": true,
            "default_profile_image": true,
            "description": "",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 0,
            "follow_request_sent": false,
            "followers_count": 28,
            "following": false,
            "friends_count": 17,
            "geo_enabled": false,
            "has_extended_profile": false,
            "id": 41056206,
            "id_str": "41056206",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 0,
            "location": "",
            "name": "RAJAT KAURA",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_tile": false,
            "profile_image_url": "http://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_image_url_https": "https://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_link_color": "1DA1F2",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": true,
            "screen_name": "RKauras",
            "statuses_count": 50,
            "time_zone": null,
            "translator_type": "none",
            "url": null,
            "utc_offset": null,
            "verified": false
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:22:42 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [],
            "user_mentions": []
        },
        "favorite_count": 0,
        "favorited": false,
        "geo": null,
        "id": 950206099143065600,
        "id_str": "950206099143065600",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "retweet_count": 0,
        "retweeted": false,
        "source": "<a href=\"http://test.com\" rel=\"nofollow\">rajat_test</a>",
        "text": "Tweeting with timer test python script #1",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 19 04:40:04 +0000 2009",
            "default_profile": true,
            "default_profile_image": true,
            "description": "",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 0,
            "follow_request_sent": false,
            "followers_count": 28,
            "following": false,
            "friends_count": 17,
            "geo_enabled": false,
            "has_extended_profile": false,
            "id": 41056206,
            "id_str": "41056206",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 0,
            "location": "",
            "name": "RAJAT KAURA",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_tile": false,
            "profile_image_url": "http://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_image_url_https": "https://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_link_color": "1DA1F2",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": true,
            "screen_name": "RKauras",
            "statuses_count": 50,
            "time_zone": null,
            "translator_type": "none",
            "url": null,
            "utc_offset": null,
            "verified": false
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 03:22:37 +0000 2018",
        "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [],
            "user_mentions": []
        },
        "favorite_count": 0,
        "favorited": false,
        "geo": null,
        "id": 950206076846186496,
        "id_str": "950206076846186496",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "retweet_count": 0,
        "retweeted": false,
        "source": "<a href=\"http://test.com\" rel=\"nofollow\">rajat_test</a>",
        "text": "Tweeting with timer test python script #0",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue May 19 04:40:04 +0000 2009",
            "default_profile": true,
            "default_profile_image": true,
            "description": "",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 0,
            "follow_request_sent": false,
            "followers_count": 28,
            "following": false,
            "friends_count": 17,
            "geo_enabled": false,
            "has_extended_profile": false,
            "id": 41056206,
            "id_str": "41056206",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 0,
            "location": "",
            "name": "RAJAT KAURA",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
            "profile_background_tile": false,
            "profile_image_url": "http://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_image_url_https": "https://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
            "profile_link_color": "1DA1F2",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": true,
            "screen_name": "RKauras",
            "statuses_count": 50,
            "time_zone": null,
            "translator_type": "none",
            "url": null,
            "utc_offset": null,
            "verified": false
        }
    }
    {
        "contributors": null,
        "coordinates": null,
        "created_at": "Mon Jan 08 02:24:03 +0000 2018",
        "entities": {
            "hashtags": [],
            "media": [
                {
                    "display_url": "pic.twitter.com/679wN4F8kX",
                    "expanded_url": "https://twitter.com/SpaceX/status/950172552642482176/photo/1",
                    "id": 950172526553964544,
                    "id_str": "950172526553964544",
                    "indices": [
                        63,
                        86
                    ],
                    "media_url": "http://pbs.twimg.com/media/DS-w4PCVoAA5Auc.jpg",
                    "media_url_https": "https://pbs.twimg.com/media/DS-w4PCVoAA5Auc.jpg",
                    "sizes": {
                        "large": {
                            "h": 535,
                            "resize": "fit",
                            "w": 957
                        },
                        "medium": {
                            "h": 535,
                            "resize": "fit",
                            "w": 957
                        },
                        "small": {
                            "h": 380,
                            "resize": "fit",
                            "w": 680
                        },
                        "thumb": {
                            "h": 150,
                            "resize": "crop",
                            "w": 150
                        }
                    },
                    "source_status_id": 950172552642482176,
                    "source_status_id_str": "950172552642482176",
                    "source_user_id": 34743251,
                    "source_user_id_str": "34743251",
                    "type": "photo",
                    "url": "https://t.co/679wN4F8kX"
                }
            ],
            "symbols": [],
            "urls": [],
            "user_mentions": [
                {
                    "id": 34743251,
                    "id_str": "34743251",
                    "indices": [
                        3,
                        10
                    ],
                    "name": "SpaceX",
                    "screen_name": "SpaceX"
                }
            ]
        },
        "extended_entities": {
            "media": [
                {
                    "display_url": "pic.twitter.com/679wN4F8kX",
                    "expanded_url": "https://twitter.com/SpaceX/status/950172552642482176/photo/1",
                    "id": 950172526553964544,
                    "id_str": "950172526553964544",
                    "indices": [
                        63,
                        86
                    ],
                    "media_url": "http://pbs.twimg.com/media/DS-w4PCVoAA5Auc.jpg",
                    "media_url_https": "https://pbs.twimg.com/media/DS-w4PCVoAA5Auc.jpg",
                    "sizes": {
                        "large": {
                            "h": 535,
                            "resize": "fit",
                            "w": 957
                        },
                        "medium": {
                            "h": 535,
                            "resize": "fit",
                            "w": 957
                        },
                        "small": {
                            "h": 380,
                            "resize": "fit",
                            "w": 680
                        },
                        "thumb": {
                            "h": 150,
                            "resize": "crop",
                            "w": 150
                        }
                    },
                    "source_status_id": 950172552642482176,
                    "source_status_id_str": "950172552642482176",
                    "source_user_id": 34743251,
                    "source_user_id_str": "34743251",
                    "type": "photo",
                    "url": "https://t.co/679wN4F8kX"
                }
            ]
        },
        "favorite_count": 0,
        "favorited": false,
        "geo": null,
        "id": 950191339349528576,
        "id_str": "950191339349528576",
        "in_reply_to_screen_name": null,
        "in_reply_to_status_id": null,
        "in_reply_to_status_id_str": null,
        "in_reply_to_user_id": null,
        "in_reply_to_user_id_str": null,
        "is_quote_status": false,
        "lang": "en",
        "place": null,
        "possibly_sensitive": false,
        "possibly_sensitive_appealable": false,
        "retweet_count": 4923,
        "retweeted": false,
        "retweeted_status": {
            "contributors": null,
            "coordinates": null,
            "created_at": "Mon Jan 08 01:09:24 +0000 2018",
            "entities": {
                "hashtags": [],
                "media": [
                    {
                        "display_url": "pic.twitter.com/679wN4F8kX",
                        "expanded_url": "https://twitter.com/SpaceX/status/950172552642482176/photo/1",
                        "id": 950172526553964544,
                        "id_str": "950172526553964544",
                        "indices": [
                            51,
                            74
                        ],
                        "media_url": "http://pbs.twimg.com/media/DS-w4PCVoAA5Auc.jpg",
                        "media_url_https": "https://pbs.twimg.com/media/DS-w4PCVoAA5Auc.jpg",
                        "sizes": {
                            "large": {
                                "h": 535,
                                "resize": "fit",
                                "w": 957
                            },
                            "medium": {
                                "h": 535,
                                "resize": "fit",
                                "w": 957
                            },
                            "small": {
                                "h": 380,
                                "resize": "fit",
                                "w": 680
                            },
                            "thumb": {
                                "h": 150,
                                "resize": "crop",
                                "w": 150
                            }
                        },
                        "type": "photo",
                        "url": "https://t.co/679wN4F8kX"
                    }
                ],
                "symbols": [],
                "urls": [],
                "user_mentions": []
            },
            "extended_entities": {
                "media": [
                    {
                        "display_url": "pic.twitter.com/679wN4F8kX",
                        "expanded_url": "https://twitter.com/SpaceX/status/950172552642482176/photo/1",
                        "id": 950172526553964544,
                        "id_str": "950172526553964544",
                        "indices": [
                            51,
                            74
                        ],
                        "media_url": "http://pbs.twimg.com/media/DS-w4PCVoAA5Auc.jpg",
                        "media_url_https": "https://pbs.twimg.com/media/DS-w4PCVoAA5Auc.jpg",
                        "sizes": {
                            "large": {
                                "h": 535,
                                "resize": "fit",
                                "w": 957
                            },
                            "medium": {
                                "h": 535,
                                "resize": "fit",
                                "w": 957
                            },
                            "small": {
                                "h": 380,
                                "resize": "fit",
                                "w": 680
                            },
                            "thumb": {
                                "h": 150,
                                "resize": "crop",
                                "w": 150
                            }
                        },
                        "type": "photo",
                        "url": "https://t.co/679wN4F8kX"
                    }
                ]
            },
            "favorite_count": 32058,
            "favorited": false,
            "geo": null,
            "id": 950172552642482176,
            "id_str": "950172552642482176",
            "in_reply_to_screen_name": null,
            "in_reply_to_status_id": null,
            "in_reply_to_status_id_str": null,
            "in_reply_to_user_id": null,
            "in_reply_to_user_id_str": null,
            "is_quote_status": false,
            "lang": "en",
            "place": null,
            "possibly_sensitive": false,
            "possibly_sensitive_appealable": false,
            "retweet_count": 4923,
            "retweeted": false,
            "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>",
            "text": "Falcon 9 first stage has landed at Landing Zone 1. https://t.co/679wN4F8kX",
            "truncated": false,
            "user": {
                "contributors_enabled": false,
                "created_at": "Thu Apr 23 21:53:30 +0000 2009",
                "default_profile": false,
                "default_profile_image": false,
                "description": "Official Twitter for SpaceX, the future of space travel. SpaceX designs, manufactures and launches the world\u2019s most advanced rockets and spacecraft.",
                "entities": {
                    "description": {
                        "urls": []
                    },
                    "url": {
                        "urls": [
                            {
                                "display_url": "spacex.com",
                                "expanded_url": "http://www.spacex.com",
                                "indices": [
                                    0,
                                    22
                                ],
                                "url": "http://t.co/FIyHerP6TF"
                            }
                        ]
                    }
                },
                "favourites_count": 58,
                "follow_request_sent": false,
                "followers_count": 5695465,
                "following": false,
                "friends_count": 533,
                "geo_enabled": false,
                "has_extended_profile": false,
                "id": 34743251,
                "id_str": "34743251",
                "is_translation_enabled": false,
                "is_translator": false,
                "lang": "en",
                "listed_count": 19158,
                "location": "Hawthorne, CA",
                "name": "SpaceX",
                "notifications": false,
                "profile_background_color": "000000",
                "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/734766064/c3da2fe7108f89eec596e077c330a3ba.jpeg",
                "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/734766064/c3da2fe7108f89eec596e077c330a3ba.jpeg",
                "profile_background_tile": false,
                "profile_banner_url": "https://pbs.twimg.com/profile_banners/34743251/1413492463",
                "profile_image_url": "http://pbs.twimg.com/profile_images/671865418701606912/HECw8AzK_normal.jpg",
                "profile_image_url_https": "https://pbs.twimg.com/profile_images/671865418701606912/HECw8AzK_normal.jpg",
                "profile_link_color": "62616B",
                "profile_sidebar_border_color": "FFFFFF",
                "profile_sidebar_fill_color": "EFEFEF",
                "profile_text_color": "333333",
                "profile_use_background_image": true,
                "protected": false,
                "screen_name": "SpaceX",
                "statuses_count": 3593,
                "time_zone": "Pacific Time (US & Canada)",
                "translator_type": "none",
                "url": "http://t.co/FIyHerP6TF",
                "utc_offset": -28800,
                "verified": true
            }
        },
        "source": "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
        "text": "RT @SpaceX: Falcon 9 first stage has landed at Landing Zone 1. https://t.co/679wN4F8kX",
        "truncated": false,
        "user": {
            "contributors_enabled": false,
            "created_at": "Tue Jun 02 20:12:29 +0000 2009",
            "default_profile": false,
            "default_profile_image": false,
            "description": "",
            "entities": {
                "description": {
                    "urls": []
                }
            },
            "favourites_count": 706,
            "follow_request_sent": false,
            "followers_count": 17856124,
            "following": true,
            "friends_count": 47,
            "geo_enabled": false,
            "has_extended_profile": false,
            "id": 44196397,
            "id_str": "44196397",
            "is_translation_enabled": false,
            "is_translator": false,
            "lang": "en",
            "listed_count": 40456,
            "location": "Boring",
            "name": "Elon Musk",
            "notifications": false,
            "profile_background_color": "C0DEED",
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/399721902/fusion.jpg",
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/399721902/fusion.jpg",
            "profile_background_tile": false,
            "profile_banner_url": "https://pbs.twimg.com/profile_banners/44196397/1354486475",
            "profile_image_url": "http://pbs.twimg.com/profile_images/782474226020200448/zDo-gAo0_normal.jpg",
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/782474226020200448/zDo-gAo0_normal.jpg",
            "profile_link_color": "0084B4",
            "profile_sidebar_border_color": "C0DEED",
            "profile_sidebar_fill_color": "DDEEF6",
            "profile_text_color": "333333",
            "profile_use_background_image": true,
            "protected": false,
            "screen_name": "elonmusk",
            "statuses_count": 3769,
            "time_zone": "Pacific Time (US & Canada)",
            "translator_type": "none",
            "url": null,
            "utc_offset": -28800,
            "verified": true
        }
    }
    


```python
#### Seaborn
```


```python
## Defining overall Sentiment dataframe
overall_sentiment = pd.DataFrame ({"User":target_user,
                                "tweet_compound_mean":tweet_compound_mean,
                                  "tweet_negative_mean":tweet_negative_mean})
overall_sentiment
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>User</th>
      <th>tweet_compound_mean</th>
      <th>tweet_negative_mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>@BBC</td>
      <td>0.110866</td>
      <td>0.031980</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@CBS</td>
      <td>0.213439</td>
      <td>0.027540</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@CNN</td>
      <td>0.171734</td>
      <td>0.029537</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@FoxNews</td>
      <td>0.128808</td>
      <td>0.046867</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@nytimes</td>
      <td>0.098242</td>
      <td>0.051730</td>
    </tr>
  </tbody>
</table>
</div>




```python
#colors={"@BBC":'lightblue',"@CBS":'green',"@CNN":'red',"@Fox":'blue',"@nytimes":'yellow'}
colors=["lightblue", "green", "red", "blue", "yellow"]
```


```python
sns.barplot(x='User',y='tweet_compound_mean',data=overall_sentiment,palette=colors)
# Sets the y limits of Sentiments
plt.ylim(-0.8,0.8)
plt.ylabel("Tweet Polarity")
plt.title("Overall Media Sentiment(compound data) based on Twitter (%s)" % time.strftime("%x"),loc = "center")
```




    Text(0.5,1,'Overall Media Sentiment(compound data) based on Twitter (01/08/18)')




```python
plt.show()
```


![png](README_files/README_21_0.png)



```python
sns.barplot(x='User',y='tweet_negative_mean',data=overall_sentiment,palette=colors)
# Sets the y limits of Sentiments
plt.ylim(-0.8,0.8)
plt.ylabel("Tweet Polarity")
plt.title("Overall Media negative Sentiment based on Twitter (%s)" % time.strftime("%x"),loc = "center")
```




    Text(0.5,1,'Overall Media negative Sentiment based on Twitter (01/08/18)')




```python
plt.show()
```


![png](README_files/README_23_0.png)

