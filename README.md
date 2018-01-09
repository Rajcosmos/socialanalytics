

```python
### Hi, this is a sentiment analysis of the Twitter activity for 5 News Media - BBC, CBS, CNN, Fox and New York times
### We are charting 2 plots - scatter using matplotlib and Bar graph using Seaborn libraries
###  Observation, 
### Ran the script 3 days consecutive just to collect samples and understand the variance in the sentiments of the 5 different media news
### 1. By looking at scatter plot (relation between tweets polarity and tweets ago), the tweet polarity is ranging from neutral (0) to -ve sentiments for most of the nytimes and foxnews. Only BBC,CBS and CNN were ranging between +ve and neutral
#### Continue #1-> conclusion, nytimes and Foxnews focus on somewhat -ve news.
####2. By looking at bar chart, CBS has overall highest positive tweets. 
### 3. Also plotted -ve average sentiments to check on which media is most overall sentiment -ve, mostly Fox and NY times were slightly have slighlt less +ve sentiments comparitive to other 3 media sources. 
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
       return tweet_compound_mean.append(np.mean(compound_list))
def tweet_mean_neg_call():
        return tweet_negative_mean.append(np.mean(negative_list))

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
    tweet_mean_neg_call()
    
            
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
    Tweet from @CBS #2017-10-19 20:57:53+00:00 created on 1:RT @ScorpionCBS: No one should be bullied or called names simply for being who they are. Today, #TeamScorpion is going purple in honor of #…
    Tweet from @CBS #2017-10-19 19:50:57+00:00 created on 2:Get ready to go on an unexpected, magical adventure in the animated special Michael Jackson's Halloween on Friday,… https://t.co/CQxmLmDvTy
    Tweet from @CBS #2017-10-19 18:59:01+00:00 created on 3:An advantage changed the course of last night's tribal council. Watch the latest episode of #Survivor now:… https://t.co/zNBz4t39i6
    Tweet from @CBS #2017-10-19 18:26:42+00:00 created on 4:RT @swatcbs: #SWAT is proud to wear purple &amp; stand together against bullying on #SpiritDay! https://t.co/ch5h6DHLLE
    Tweet from @CBS #2017-10-19 18:20:01+00:00 created on 5:RT @LifeInPiecesCBS: Laughter is coming your way! #LifeInPieces returns in TWO WEEKS! https://t.co/kvHTQA3Vlk
    Tweet from @CBS #2017-10-19 18:19:51+00:00 created on 6:RT @MomCBS: Do a little dance... #Mom returns in TWO WEEKS! https://t.co/qshl2F8ChX
    Tweet from @CBS #2017-10-19 17:33:18+00:00 created on 7:.@MacgyverCBS's George Eads shares why you'll be moved by the ending of the new special Michael Jackson's Halloween… https://t.co/MDpRaWKGUI
    Tweet from @CBS #2017-10-19 17:31:23+00:00 created on 8:RT @walliscw: Today is @glaad #SpiritDay, a day reminding us that EVERY day is a good day to speak out against LGBTQ bullying. @MadamSecret…
    Tweet from @CBS #2017-10-19 17:31:09+00:00 created on 9:RT @marythechief: Thnx 2 my beautiful friend &amp; advocate @wcruz73 I just took @glaad ‘s #SpiritDay pledge against bullying! Join me at https…
    Tweet from @CBS #2017-10-19 17:31:07+00:00 created on 10:RT @wcruz73: It’s #SpiritDay! Show your support for LGBTQ youth today by standing against bullying. @glaad @glsenofficial ✊🏽❤️🏳️‍🌈 https://…
    Tweet from @CBS #2017-10-19 14:36:38+00:00 created on 11:RT @MomCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #Mom https://t.co/3xuyKIXpPS
    Tweet from @CBS #2017-10-19 14:35:32+00:00 created on 12:RT @CodeBlackCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #CodeBlack https://t.co/JuAQvXFaVa
    Tweet from @CBS #2017-10-19 14:35:16+00:00 created on 13:RT @PriceIsRight: No one should be bullied or called names simply for who they are. #SpiritDay https://t.co/P1X3VUveOL
    Tweet from @CBS #2017-10-19 14:33:12+00:00 created on 14:RT @9JKLCBS: No one should be bullied or called names simply for being who they are. Today, the cast of #9JKL is wearing purple for #Spirit…
    Tweet from @CBS #2017-10-19 00:17:00+00:00 created on 15:RT @SalvationCBS: #Salvation is renewed for Season 2! Congrats to the cast and crew. More thrills coming Summer 2018. ☄️#SalvationCBS https…
    Tweet from @CBS #2017-10-18 20:56:39+00:00 created on 16:RT @SuperiorDonuts: What's life without a few sprinkles and a lot of love? 🍩 #SuperiorDonuts Season 2 premieres on Mon, Oct 30 at 9:30/8:30…
    Tweet from @CBS #2017-10-18 20:55:01+00:00 created on 17:Check out some of the hottest hook ups in #Survivor history before tonight's all-new episode:… https://t.co/qpM4bcJZj1
    Tweet from @CBS #2017-10-18 16:12:32+00:00 created on 18:RT @ManWithAPlan: The cast of #ManWithAPlan is on set and gearing up for Season 2! See what they've been up to: https://t.co/j6J2tUCIlj htt…
    Tweet from @CBS #2017-10-17 21:50:42+00:00 created on 19:It was a wedding to remember on last night's #KevinCanWait. Catch up now: https://t.co/gbLTSN8UEa https://t.co/Ex0bFLgXGT
    Tweet from @CBS #2017-10-17 18:14:21+00:00 created on 20:RT @CrimMinds_CBS: 17 quotes that made us fall in love with Dr. Spencer Reid: https://t.co/HveiMl7u3T #CriminalMinds #Throwback https://t.c…
    Tweet from @CBS #2017-10-19 20:57:53+00:00 created on 21:RT @ScorpionCBS: No one should be bullied or called names simply for being who they are. Today, #TeamScorpion is going purple in honor of #…
    Tweet from @CBS #2017-10-19 19:50:57+00:00 created on 22:Get ready to go on an unexpected, magical adventure in the animated special Michael Jackson's Halloween on Friday,… https://t.co/CQxmLmDvTy
    Tweet from @CBS #2017-10-19 18:59:01+00:00 created on 23:An advantage changed the course of last night's tribal council. Watch the latest episode of #Survivor now:… https://t.co/zNBz4t39i6
    Tweet from @CBS #2017-10-19 18:26:42+00:00 created on 24:RT @swatcbs: #SWAT is proud to wear purple &amp; stand together against bullying on #SpiritDay! https://t.co/ch5h6DHLLE
    Tweet from @CBS #2017-10-19 18:20:01+00:00 created on 25:RT @LifeInPiecesCBS: Laughter is coming your way! #LifeInPieces returns in TWO WEEKS! https://t.co/kvHTQA3Vlk
    Tweet from @CBS #2017-10-19 18:19:51+00:00 created on 26:RT @MomCBS: Do a little dance... #Mom returns in TWO WEEKS! https://t.co/qshl2F8ChX
    Tweet from @CBS #2017-10-19 17:33:18+00:00 created on 27:.@MacgyverCBS's George Eads shares why you'll be moved by the ending of the new special Michael Jackson's Halloween… https://t.co/MDpRaWKGUI
    Tweet from @CBS #2017-10-19 17:31:23+00:00 created on 28:RT @walliscw: Today is @glaad #SpiritDay, a day reminding us that EVERY day is a good day to speak out against LGBTQ bullying. @MadamSecret…
    Tweet from @CBS #2017-10-19 17:31:09+00:00 created on 29:RT @marythechief: Thnx 2 my beautiful friend &amp; advocate @wcruz73 I just took @glaad ‘s #SpiritDay pledge against bullying! Join me at https…
    Tweet from @CBS #2017-10-19 17:31:07+00:00 created on 30:RT @wcruz73: It’s #SpiritDay! Show your support for LGBTQ youth today by standing against bullying. @glaad @glsenofficial ✊🏽❤️🏳️‍🌈 https://…
    Tweet from @CBS #2017-10-19 14:36:38+00:00 created on 31:RT @MomCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #Mom https://t.co/3xuyKIXpPS
    Tweet from @CBS #2017-10-19 14:35:32+00:00 created on 32:RT @CodeBlackCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #CodeBlack https://t.co/JuAQvXFaVa
    Tweet from @CBS #2017-10-19 14:35:16+00:00 created on 33:RT @PriceIsRight: No one should be bullied or called names simply for who they are. #SpiritDay https://t.co/P1X3VUveOL
    Tweet from @CBS #2017-10-19 14:33:12+00:00 created on 34:RT @9JKLCBS: No one should be bullied or called names simply for being who they are. Today, the cast of #9JKL is wearing purple for #Spirit…
    Tweet from @CBS #2017-10-19 00:17:00+00:00 created on 35:RT @SalvationCBS: #Salvation is renewed for Season 2! Congrats to the cast and crew. More thrills coming Summer 2018. ☄️#SalvationCBS https…
    Tweet from @CBS #2017-10-18 20:56:39+00:00 created on 36:RT @SuperiorDonuts: What's life without a few sprinkles and a lot of love? 🍩 #SuperiorDonuts Season 2 premieres on Mon, Oct 30 at 9:30/8:30…
    Tweet from @CBS #2017-10-18 20:55:01+00:00 created on 37:Check out some of the hottest hook ups in #Survivor history before tonight's all-new episode:… https://t.co/qpM4bcJZj1
    Tweet from @CBS #2017-10-18 16:12:32+00:00 created on 38:RT @ManWithAPlan: The cast of #ManWithAPlan is on set and gearing up for Season 2! See what they've been up to: https://t.co/j6J2tUCIlj htt…
    Tweet from @CBS #2017-10-17 21:50:42+00:00 created on 39:It was a wedding to remember on last night's #KevinCanWait. Catch up now: https://t.co/gbLTSN8UEa https://t.co/Ex0bFLgXGT
    Tweet from @CBS #2017-10-17 18:14:21+00:00 created on 40:RT @CrimMinds_CBS: 17 quotes that made us fall in love with Dr. Spencer Reid: https://t.co/HveiMl7u3T #CriminalMinds #Throwback https://t.c…
    Tweet from @CBS #2017-10-19 20:57:53+00:00 created on 41:RT @ScorpionCBS: No one should be bullied or called names simply for being who they are. Today, #TeamScorpion is going purple in honor of #…
    Tweet from @CBS #2017-10-19 19:50:57+00:00 created on 42:Get ready to go on an unexpected, magical adventure in the animated special Michael Jackson's Halloween on Friday,… https://t.co/CQxmLmDvTy
    Tweet from @CBS #2017-10-19 18:59:01+00:00 created on 43:An advantage changed the course of last night's tribal council. Watch the latest episode of #Survivor now:… https://t.co/zNBz4t39i6
    Tweet from @CBS #2017-10-19 18:26:42+00:00 created on 44:RT @swatcbs: #SWAT is proud to wear purple &amp; stand together against bullying on #SpiritDay! https://t.co/ch5h6DHLLE
    Tweet from @CBS #2017-10-19 18:20:01+00:00 created on 45:RT @LifeInPiecesCBS: Laughter is coming your way! #LifeInPieces returns in TWO WEEKS! https://t.co/kvHTQA3Vlk
    Tweet from @CBS #2017-10-19 18:19:51+00:00 created on 46:RT @MomCBS: Do a little dance... #Mom returns in TWO WEEKS! https://t.co/qshl2F8ChX
    Tweet from @CBS #2017-10-19 17:33:18+00:00 created on 47:.@MacgyverCBS's George Eads shares why you'll be moved by the ending of the new special Michael Jackson's Halloween… https://t.co/MDpRaWKGUI
    Tweet from @CBS #2017-10-19 17:31:23+00:00 created on 48:RT @walliscw: Today is @glaad #SpiritDay, a day reminding us that EVERY day is a good day to speak out against LGBTQ bullying. @MadamSecret…
    Tweet from @CBS #2017-10-19 17:31:09+00:00 created on 49:RT @marythechief: Thnx 2 my beautiful friend &amp; advocate @wcruz73 I just took @glaad ‘s #SpiritDay pledge against bullying! Join me at https…
    Tweet from @CBS #2017-10-19 17:31:07+00:00 created on 50:RT @wcruz73: It’s #SpiritDay! Show your support for LGBTQ youth today by standing against bullying. @glaad @glsenofficial ✊🏽❤️🏳️‍🌈 https://…
    Tweet from @CBS #2017-10-19 14:36:38+00:00 created on 51:RT @MomCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #Mom https://t.co/3xuyKIXpPS
    Tweet from @CBS #2017-10-19 14:35:32+00:00 created on 52:RT @CodeBlackCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #CodeBlack https://t.co/JuAQvXFaVa
    Tweet from @CBS #2017-10-19 14:35:16+00:00 created on 53:RT @PriceIsRight: No one should be bullied or called names simply for who they are. #SpiritDay https://t.co/P1X3VUveOL
    Tweet from @CBS #2017-10-19 14:33:12+00:00 created on 54:RT @9JKLCBS: No one should be bullied or called names simply for being who they are. Today, the cast of #9JKL is wearing purple for #Spirit…
    Tweet from @CBS #2017-10-19 00:17:00+00:00 created on 55:RT @SalvationCBS: #Salvation is renewed for Season 2! Congrats to the cast and crew. More thrills coming Summer 2018. ☄️#SalvationCBS https…
    Tweet from @CBS #2017-10-18 20:56:39+00:00 created on 56:RT @SuperiorDonuts: What's life without a few sprinkles and a lot of love? 🍩 #SuperiorDonuts Season 2 premieres on Mon, Oct 30 at 9:30/8:30…
    Tweet from @CBS #2017-10-18 20:55:01+00:00 created on 57:Check out some of the hottest hook ups in #Survivor history before tonight's all-new episode:… https://t.co/qpM4bcJZj1
    Tweet from @CBS #2017-10-18 16:12:32+00:00 created on 58:RT @ManWithAPlan: The cast of #ManWithAPlan is on set and gearing up for Season 2! See what they've been up to: https://t.co/j6J2tUCIlj htt…
    Tweet from @CBS #2017-10-17 21:50:42+00:00 created on 59:It was a wedding to remember on last night's #KevinCanWait. Catch up now: https://t.co/gbLTSN8UEa https://t.co/Ex0bFLgXGT
    Tweet from @CBS #2017-10-17 18:14:21+00:00 created on 60:RT @CrimMinds_CBS: 17 quotes that made us fall in love with Dr. Spencer Reid: https://t.co/HveiMl7u3T #CriminalMinds #Throwback https://t.c…
    Tweet from @CBS #2017-10-19 20:57:53+00:00 created on 61:RT @ScorpionCBS: No one should be bullied or called names simply for being who they are. Today, #TeamScorpion is going purple in honor of #…
    Tweet from @CBS #2017-10-19 19:50:57+00:00 created on 62:Get ready to go on an unexpected, magical adventure in the animated special Michael Jackson's Halloween on Friday,… https://t.co/CQxmLmDvTy
    Tweet from @CBS #2017-10-19 18:59:01+00:00 created on 63:An advantage changed the course of last night's tribal council. Watch the latest episode of #Survivor now:… https://t.co/zNBz4t39i6
    Tweet from @CBS #2017-10-19 18:26:42+00:00 created on 64:RT @swatcbs: #SWAT is proud to wear purple &amp; stand together against bullying on #SpiritDay! https://t.co/ch5h6DHLLE
    Tweet from @CBS #2017-10-19 18:20:01+00:00 created on 65:RT @LifeInPiecesCBS: Laughter is coming your way! #LifeInPieces returns in TWO WEEKS! https://t.co/kvHTQA3Vlk
    Tweet from @CBS #2017-10-19 18:19:51+00:00 created on 66:RT @MomCBS: Do a little dance... #Mom returns in TWO WEEKS! https://t.co/qshl2F8ChX
    Tweet from @CBS #2017-10-19 17:33:18+00:00 created on 67:.@MacgyverCBS's George Eads shares why you'll be moved by the ending of the new special Michael Jackson's Halloween… https://t.co/MDpRaWKGUI
    Tweet from @CBS #2017-10-19 17:31:23+00:00 created on 68:RT @walliscw: Today is @glaad #SpiritDay, a day reminding us that EVERY day is a good day to speak out against LGBTQ bullying. @MadamSecret…
    Tweet from @CBS #2017-10-19 17:31:09+00:00 created on 69:RT @marythechief: Thnx 2 my beautiful friend &amp; advocate @wcruz73 I just took @glaad ‘s #SpiritDay pledge against bullying! Join me at https…
    Tweet from @CBS #2017-10-19 17:31:07+00:00 created on 70:RT @wcruz73: It’s #SpiritDay! Show your support for LGBTQ youth today by standing against bullying. @glaad @glsenofficial ✊🏽❤️🏳️‍🌈 https://…
    Tweet from @CBS #2017-10-19 14:36:38+00:00 created on 71:RT @MomCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #Mom https://t.co/3xuyKIXpPS
    Tweet from @CBS #2017-10-19 14:35:32+00:00 created on 72:RT @CodeBlackCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #CodeBlack https://t.co/JuAQvXFaVa
    Tweet from @CBS #2017-10-19 14:35:16+00:00 created on 73:RT @PriceIsRight: No one should be bullied or called names simply for who they are. #SpiritDay https://t.co/P1X3VUveOL
    Tweet from @CBS #2017-10-19 14:33:12+00:00 created on 74:RT @9JKLCBS: No one should be bullied or called names simply for being who they are. Today, the cast of #9JKL is wearing purple for #Spirit…
    Tweet from @CBS #2017-10-19 00:17:00+00:00 created on 75:RT @SalvationCBS: #Salvation is renewed for Season 2! Congrats to the cast and crew. More thrills coming Summer 2018. ☄️#SalvationCBS https…
    Tweet from @CBS #2017-10-18 20:56:39+00:00 created on 76:RT @SuperiorDonuts: What's life without a few sprinkles and a lot of love? 🍩 #SuperiorDonuts Season 2 premieres on Mon, Oct 30 at 9:30/8:30…
    Tweet from @CBS #2017-10-18 20:55:01+00:00 created on 77:Check out some of the hottest hook ups in #Survivor history before tonight's all-new episode:… https://t.co/qpM4bcJZj1
    Tweet from @CBS #2017-10-18 16:12:32+00:00 created on 78:RT @ManWithAPlan: The cast of #ManWithAPlan is on set and gearing up for Season 2! See what they've been up to: https://t.co/j6J2tUCIlj htt…
    Tweet from @CBS #2017-10-17 21:50:42+00:00 created on 79:It was a wedding to remember on last night's #KevinCanWait. Catch up now: https://t.co/gbLTSN8UEa https://t.co/Ex0bFLgXGT
    Tweet from @CBS #2017-10-17 18:14:21+00:00 created on 80:RT @CrimMinds_CBS: 17 quotes that made us fall in love with Dr. Spencer Reid: https://t.co/HveiMl7u3T #CriminalMinds #Throwback https://t.c…
    Tweet from @CBS #2017-10-19 20:57:53+00:00 created on 81:RT @ScorpionCBS: No one should be bullied or called names simply for being who they are. Today, #TeamScorpion is going purple in honor of #…
    Tweet from @CBS #2017-10-19 19:50:57+00:00 created on 82:Get ready to go on an unexpected, magical adventure in the animated special Michael Jackson's Halloween on Friday,… https://t.co/CQxmLmDvTy
    Tweet from @CBS #2017-10-19 18:59:01+00:00 created on 83:An advantage changed the course of last night's tribal council. Watch the latest episode of #Survivor now:… https://t.co/zNBz4t39i6
    Tweet from @CBS #2017-10-19 18:26:42+00:00 created on 84:RT @swatcbs: #SWAT is proud to wear purple &amp; stand together against bullying on #SpiritDay! https://t.co/ch5h6DHLLE
    Tweet from @CBS #2017-10-19 18:20:01+00:00 created on 85:RT @LifeInPiecesCBS: Laughter is coming your way! #LifeInPieces returns in TWO WEEKS! https://t.co/kvHTQA3Vlk
    Tweet from @CBS #2017-10-19 18:19:51+00:00 created on 86:RT @MomCBS: Do a little dance... #Mom returns in TWO WEEKS! https://t.co/qshl2F8ChX
    Tweet from @CBS #2017-10-19 17:33:18+00:00 created on 87:.@MacgyverCBS's George Eads shares why you'll be moved by the ending of the new special Michael Jackson's Halloween… https://t.co/MDpRaWKGUI
    Tweet from @CBS #2017-10-19 17:31:23+00:00 created on 88:RT @walliscw: Today is @glaad #SpiritDay, a day reminding us that EVERY day is a good day to speak out against LGBTQ bullying. @MadamSecret…
    Tweet from @CBS #2017-10-19 17:31:09+00:00 created on 89:RT @marythechief: Thnx 2 my beautiful friend &amp; advocate @wcruz73 I just took @glaad ‘s #SpiritDay pledge against bullying! Join me at https…
    Tweet from @CBS #2017-10-19 17:31:07+00:00 created on 90:RT @wcruz73: It’s #SpiritDay! Show your support for LGBTQ youth today by standing against bullying. @glaad @glsenofficial ✊🏽❤️🏳️‍🌈 https://…
    Tweet from @CBS #2017-10-19 14:36:38+00:00 created on 91:RT @MomCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #Mom https://t.co/3xuyKIXpPS
    Tweet from @CBS #2017-10-19 14:35:32+00:00 created on 92:RT @CodeBlackCBS: No one should be bullied or called names simply for being who they are. #SpiritDay #CodeBlack https://t.co/JuAQvXFaVa
    Tweet from @CBS #2017-10-19 14:35:16+00:00 created on 93:RT @PriceIsRight: No one should be bullied or called names simply for who they are. #SpiritDay https://t.co/P1X3VUveOL
    Tweet from @CBS #2017-10-19 14:33:12+00:00 created on 94:RT @9JKLCBS: No one should be bullied or called names simply for being who they are. Today, the cast of #9JKL is wearing purple for #Spirit…
    Tweet from @CBS #2017-10-19 00:17:00+00:00 created on 95:RT @SalvationCBS: #Salvation is renewed for Season 2! Congrats to the cast and crew. More thrills coming Summer 2018. ☄️#SalvationCBS https…
    Tweet from @CBS #2017-10-18 20:56:39+00:00 created on 96:RT @SuperiorDonuts: What's life without a few sprinkles and a lot of love? 🍩 #SuperiorDonuts Season 2 premieres on Mon, Oct 30 at 9:30/8:30…
    Tweet from @CBS #2017-10-18 20:55:01+00:00 created on 97:Check out some of the hottest hook ups in #Survivor history before tonight's all-new episode:… https://t.co/qpM4bcJZj1
    Tweet from @CBS #2017-10-18 16:12:32+00:00 created on 98:RT @ManWithAPlan: The cast of #ManWithAPlan is on set and gearing up for Season 2! See what they've been up to: https://t.co/j6J2tUCIlj htt…
    Tweet from @CBS #2017-10-17 21:50:42+00:00 created on 99:It was a wedding to remember on last night's #KevinCanWait. Catch up now: https://t.co/gbLTSN8UEa https://t.co/Ex0bFLgXGT
    Tweet from @CBS #2017-10-17 18:14:21+00:00 created on 100:RT @CrimMinds_CBS: 17 quotes that made us fall in love with Dr. Spencer Reid: https://t.co/HveiMl7u3T #CriminalMinds #Throwback https://t.c…
     ---------------TWEET COMPLETED FOR @CBS-------------
    Tweet from @CNN #2018-01-05 22:20:08+00:00 created on 1:Vermont moves to legalize pot as feds signal possible crackdown https://t.co/SNx8qXKWzQ https://t.co/vrTvPbaMxN
    Tweet from @CNN #2018-01-05 22:10:03+00:00 created on 2:Secretary of State Rex Tillerson says he's never questioned President Trump's mental fitness https://t.co/w4qiCnrCek https://t.co/sBCygN5Hqh
    Tweet from @CNN #2018-01-05 22:00:05+00:00 created on 3:She was roasting marshmallows on New Year's Eve. Then a gas can exploded. https://t.co/dIPVpSksRO https://t.co/iXmHTB1bqz
    Tweet from @CNN #2018-01-05 21:50:08+00:00 created on 4:Rep. Elijah Cummings was admitted to the hospital for a bacterial infection in his knee, his office has announced… https://t.co/RGKgL39aen
    Tweet from @CNN #2018-01-05 21:40:47+00:00 created on 5:"Jeopardy!" host Alex Trebek is taking a medical leave after he experienced complications from a head injury known… https://t.co/KOhQ3vMuqI
    Tweet from @CNN #2018-01-05 21:30:13+00:00 created on 6:Delta Air Lines flew its last Boeing 747 to an aircraft boneyard in Arizona this week, ending operations for 747s b… https://t.co/arSAFoTMg6
    Tweet from @CNN #2018-01-05 21:20:06+00:00 created on 7:What the heck does Steve Bannon do now? | Analysis by @CillizzaCNN https://t.co/XCkYPhDcKi https://t.co/d8bjpcBE6o
    Tweet from @CNN #2018-01-05 21:10:08+00:00 created on 8:GOP Rep. Chris Stewart, a member of the House Intelligence Committee, joined the growing list of Republicans callin… https://t.co/u47xZ7DX4W
    Tweet from @CNN #2018-01-05 21:00:13+00:00 created on 9:Why billions of dollars in lottery prizes go unclaimed https://t.co/jWkE30IsxI https://t.co/p6LRarCGQ5
    Tweet from @CNN #2018-01-05 20:50:04+00:00 created on 10:"Lyin’ Ted"
    "Low Energy Jeb"
    "Little Marco"
    
    Where does "Sloppy Steve" rank among President Trump's other nicknames… https://t.co/bBf534HMy3
    Tweet from @CNN #2018-01-05 20:40:10+00:00 created on 11:The future of how the internet is regulated could be decided by a showdown between states and the federal governmen… https://t.co/OKHMA1zFBM
    Tweet from @CNN #2018-01-05 20:31:20+00:00 created on 12:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/w7UMkqmcHI
    Tweet from @CNN #2018-01-05 20:31:15+00:00 created on 13:"One week. Welcome to 2018." - @BrookeBCNN 
    
    We're only five days in and these are all the things that have already… https://t.co/hKIWFRmoNk
    Tweet from @CNN #2018-01-05 20:17:07+00:00 created on 14:JetBlue says it's handing out $21 million worth of bonuses to employees because of the corporate tax cut -- all 21,… https://t.co/y2Wn0sEvfx
    Tweet from @CNN #2018-01-05 20:16:16+00:00 created on 15:Trump's first-year jobs record was strong. Just not as strong as Obama's last year. https://t.co/OfFNGNxdvu https://t.co/5VRCtjfGLT
    Tweet from @CNN #2018-01-05 19:59:07+00:00 created on 16:World's longest glass bridge opens in Hebei, China https://t.co/ON5zFyFnug (via @CNNTravel) https://t.co/ZPUojOErHv
    Tweet from @CNN #2018-01-05 19:49:04+00:00 created on 17:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/x180bXAbor
    Tweet from @CNN #2018-01-05 19:35:16+00:00 created on 18:Why billions of dollars in lottery prizes go unclaimed https://t.co/aCQrsBKhut https://t.co/qv3vtF3vpE
    Tweet from @CNN #2018-01-05 19:18:38+00:00 created on 19:Barack Obama will be the first guest on David Letterman's new Netflix show https://t.co/fWTkjKCCsV https://t.co/kmMn7Bm6I0
    Tweet from @CNN #2018-01-05 19:11:29+00:00 created on 20:RT @CNNPolitics: President Trump: "We're going to Camp David with a lot of the great Republicans senators and we're making America great ag…
    Tweet from @CNN #2018-01-05 22:20:08+00:00 created on 21:Vermont moves to legalize pot as feds signal possible crackdown https://t.co/SNx8qXKWzQ https://t.co/vrTvPbaMxN
    Tweet from @CNN #2018-01-05 22:10:03+00:00 created on 22:Secretary of State Rex Tillerson says he's never questioned President Trump's mental fitness https://t.co/w4qiCnrCek https://t.co/sBCygN5Hqh
    Tweet from @CNN #2018-01-05 22:00:05+00:00 created on 23:She was roasting marshmallows on New Year's Eve. Then a gas can exploded. https://t.co/dIPVpSksRO https://t.co/iXmHTB1bqz
    Tweet from @CNN #2018-01-05 21:50:08+00:00 created on 24:Rep. Elijah Cummings was admitted to the hospital for a bacterial infection in his knee, his office has announced… https://t.co/RGKgL39aen
    Tweet from @CNN #2018-01-05 21:40:47+00:00 created on 25:"Jeopardy!" host Alex Trebek is taking a medical leave after he experienced complications from a head injury known… https://t.co/KOhQ3vMuqI
    Tweet from @CNN #2018-01-05 21:30:13+00:00 created on 26:Delta Air Lines flew its last Boeing 747 to an aircraft boneyard in Arizona this week, ending operations for 747s b… https://t.co/arSAFoTMg6
    Tweet from @CNN #2018-01-05 21:20:06+00:00 created on 27:What the heck does Steve Bannon do now? | Analysis by @CillizzaCNN https://t.co/XCkYPhDcKi https://t.co/d8bjpcBE6o
    Tweet from @CNN #2018-01-05 21:10:08+00:00 created on 28:GOP Rep. Chris Stewart, a member of the House Intelligence Committee, joined the growing list of Republicans callin… https://t.co/u47xZ7DX4W
    Tweet from @CNN #2018-01-05 21:00:13+00:00 created on 29:Why billions of dollars in lottery prizes go unclaimed https://t.co/jWkE30IsxI https://t.co/p6LRarCGQ5
    Tweet from @CNN #2018-01-05 20:50:04+00:00 created on 30:"Lyin’ Ted"
    "Low Energy Jeb"
    "Little Marco"
    
    Where does "Sloppy Steve" rank among President Trump's other nicknames… https://t.co/bBf534HMy3
    Tweet from @CNN #2018-01-05 20:40:10+00:00 created on 31:The future of how the internet is regulated could be decided by a showdown between states and the federal governmen… https://t.co/OKHMA1zFBM
    Tweet from @CNN #2018-01-05 20:31:20+00:00 created on 32:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/w7UMkqmcHI
    Tweet from @CNN #2018-01-05 20:31:15+00:00 created on 33:"One week. Welcome to 2018." - @BrookeBCNN 
    
    We're only five days in and these are all the things that have already… https://t.co/hKIWFRmoNk
    Tweet from @CNN #2018-01-05 20:17:07+00:00 created on 34:JetBlue says it's handing out $21 million worth of bonuses to employees because of the corporate tax cut -- all 21,… https://t.co/y2Wn0sEvfx
    Tweet from @CNN #2018-01-05 20:16:16+00:00 created on 35:Trump's first-year jobs record was strong. Just not as strong as Obama's last year. https://t.co/OfFNGNxdvu https://t.co/5VRCtjfGLT
    Tweet from @CNN #2018-01-05 19:59:07+00:00 created on 36:World's longest glass bridge opens in Hebei, China https://t.co/ON5zFyFnug (via @CNNTravel) https://t.co/ZPUojOErHv
    Tweet from @CNN #2018-01-05 19:49:04+00:00 created on 37:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/x180bXAbor
    Tweet from @CNN #2018-01-05 19:35:16+00:00 created on 38:Why billions of dollars in lottery prizes go unclaimed https://t.co/aCQrsBKhut https://t.co/qv3vtF3vpE
    Tweet from @CNN #2018-01-05 19:18:38+00:00 created on 39:Barack Obama will be the first guest on David Letterman's new Netflix show https://t.co/fWTkjKCCsV https://t.co/kmMn7Bm6I0
    Tweet from @CNN #2018-01-05 19:11:29+00:00 created on 40:RT @CNNPolitics: President Trump: "We're going to Camp David with a lot of the great Republicans senators and we're making America great ag…
    Tweet from @CNN #2018-01-05 22:20:08+00:00 created on 41:Vermont moves to legalize pot as feds signal possible crackdown https://t.co/SNx8qXKWzQ https://t.co/vrTvPbaMxN
    Tweet from @CNN #2018-01-05 22:10:03+00:00 created on 42:Secretary of State Rex Tillerson says he's never questioned President Trump's mental fitness https://t.co/w4qiCnrCek https://t.co/sBCygN5Hqh
    Tweet from @CNN #2018-01-05 22:00:05+00:00 created on 43:She was roasting marshmallows on New Year's Eve. Then a gas can exploded. https://t.co/dIPVpSksRO https://t.co/iXmHTB1bqz
    Tweet from @CNN #2018-01-05 21:50:08+00:00 created on 44:Rep. Elijah Cummings was admitted to the hospital for a bacterial infection in his knee, his office has announced… https://t.co/RGKgL39aen
    Tweet from @CNN #2018-01-05 21:40:47+00:00 created on 45:"Jeopardy!" host Alex Trebek is taking a medical leave after he experienced complications from a head injury known… https://t.co/KOhQ3vMuqI
    Tweet from @CNN #2018-01-05 21:30:13+00:00 created on 46:Delta Air Lines flew its last Boeing 747 to an aircraft boneyard in Arizona this week, ending operations for 747s b… https://t.co/arSAFoTMg6
    Tweet from @CNN #2018-01-05 21:20:06+00:00 created on 47:What the heck does Steve Bannon do now? | Analysis by @CillizzaCNN https://t.co/XCkYPhDcKi https://t.co/d8bjpcBE6o
    Tweet from @CNN #2018-01-05 21:10:08+00:00 created on 48:GOP Rep. Chris Stewart, a member of the House Intelligence Committee, joined the growing list of Republicans callin… https://t.co/u47xZ7DX4W
    Tweet from @CNN #2018-01-05 21:00:13+00:00 created on 49:Why billions of dollars in lottery prizes go unclaimed https://t.co/jWkE30IsxI https://t.co/p6LRarCGQ5
    Tweet from @CNN #2018-01-05 20:50:04+00:00 created on 50:"Lyin’ Ted"
    "Low Energy Jeb"
    "Little Marco"
    
    Where does "Sloppy Steve" rank among President Trump's other nicknames… https://t.co/bBf534HMy3
    Tweet from @CNN #2018-01-05 20:40:10+00:00 created on 51:The future of how the internet is regulated could be decided by a showdown between states and the federal governmen… https://t.co/OKHMA1zFBM
    Tweet from @CNN #2018-01-05 20:31:20+00:00 created on 52:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/w7UMkqmcHI
    Tweet from @CNN #2018-01-05 20:31:15+00:00 created on 53:"One week. Welcome to 2018." - @BrookeBCNN 
    
    We're only five days in and these are all the things that have already… https://t.co/hKIWFRmoNk
    Tweet from @CNN #2018-01-05 20:17:07+00:00 created on 54:JetBlue says it's handing out $21 million worth of bonuses to employees because of the corporate tax cut -- all 21,… https://t.co/y2Wn0sEvfx
    Tweet from @CNN #2018-01-05 20:16:16+00:00 created on 55:Trump's first-year jobs record was strong. Just not as strong as Obama's last year. https://t.co/OfFNGNxdvu https://t.co/5VRCtjfGLT
    Tweet from @CNN #2018-01-05 19:59:07+00:00 created on 56:World's longest glass bridge opens in Hebei, China https://t.co/ON5zFyFnug (via @CNNTravel) https://t.co/ZPUojOErHv
    Tweet from @CNN #2018-01-05 19:49:04+00:00 created on 57:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/x180bXAbor
    Tweet from @CNN #2018-01-05 19:35:16+00:00 created on 58:Why billions of dollars in lottery prizes go unclaimed https://t.co/aCQrsBKhut https://t.co/qv3vtF3vpE
    Tweet from @CNN #2018-01-05 19:18:38+00:00 created on 59:Barack Obama will be the first guest on David Letterman's new Netflix show https://t.co/fWTkjKCCsV https://t.co/kmMn7Bm6I0
    Tweet from @CNN #2018-01-05 19:11:29+00:00 created on 60:RT @CNNPolitics: President Trump: "We're going to Camp David with a lot of the great Republicans senators and we're making America great ag…
    Tweet from @CNN #2018-01-05 22:20:08+00:00 created on 61:Vermont moves to legalize pot as feds signal possible crackdown https://t.co/SNx8qXKWzQ https://t.co/vrTvPbaMxN
    Tweet from @CNN #2018-01-05 22:10:03+00:00 created on 62:Secretary of State Rex Tillerson says he's never questioned President Trump's mental fitness https://t.co/w4qiCnrCek https://t.co/sBCygN5Hqh
    Tweet from @CNN #2018-01-05 22:00:05+00:00 created on 63:She was roasting marshmallows on New Year's Eve. Then a gas can exploded. https://t.co/dIPVpSksRO https://t.co/iXmHTB1bqz
    Tweet from @CNN #2018-01-05 21:50:08+00:00 created on 64:Rep. Elijah Cummings was admitted to the hospital for a bacterial infection in his knee, his office has announced… https://t.co/RGKgL39aen
    Tweet from @CNN #2018-01-05 21:40:47+00:00 created on 65:"Jeopardy!" host Alex Trebek is taking a medical leave after he experienced complications from a head injury known… https://t.co/KOhQ3vMuqI
    Tweet from @CNN #2018-01-05 21:30:13+00:00 created on 66:Delta Air Lines flew its last Boeing 747 to an aircraft boneyard in Arizona this week, ending operations for 747s b… https://t.co/arSAFoTMg6
    Tweet from @CNN #2018-01-05 21:20:06+00:00 created on 67:What the heck does Steve Bannon do now? | Analysis by @CillizzaCNN https://t.co/XCkYPhDcKi https://t.co/d8bjpcBE6o
    Tweet from @CNN #2018-01-05 21:10:08+00:00 created on 68:GOP Rep. Chris Stewart, a member of the House Intelligence Committee, joined the growing list of Republicans callin… https://t.co/u47xZ7DX4W
    Tweet from @CNN #2018-01-05 21:00:13+00:00 created on 69:Why billions of dollars in lottery prizes go unclaimed https://t.co/jWkE30IsxI https://t.co/p6LRarCGQ5
    Tweet from @CNN #2018-01-05 20:50:04+00:00 created on 70:"Lyin’ Ted"
    "Low Energy Jeb"
    "Little Marco"
    
    Where does "Sloppy Steve" rank among President Trump's other nicknames… https://t.co/bBf534HMy3
    Tweet from @CNN #2018-01-05 20:40:10+00:00 created on 71:The future of how the internet is regulated could be decided by a showdown between states and the federal governmen… https://t.co/OKHMA1zFBM
    Tweet from @CNN #2018-01-05 20:31:20+00:00 created on 72:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/w7UMkqmcHI
    Tweet from @CNN #2018-01-05 20:31:15+00:00 created on 73:"One week. Welcome to 2018." - @BrookeBCNN 
    
    We're only five days in and these are all the things that have already… https://t.co/hKIWFRmoNk
    Tweet from @CNN #2018-01-05 20:17:07+00:00 created on 74:JetBlue says it's handing out $21 million worth of bonuses to employees because of the corporate tax cut -- all 21,… https://t.co/y2Wn0sEvfx
    Tweet from @CNN #2018-01-05 20:16:16+00:00 created on 75:Trump's first-year jobs record was strong. Just not as strong as Obama's last year. https://t.co/OfFNGNxdvu https://t.co/5VRCtjfGLT
    Tweet from @CNN #2018-01-05 19:59:07+00:00 created on 76:World's longest glass bridge opens in Hebei, China https://t.co/ON5zFyFnug (via @CNNTravel) https://t.co/ZPUojOErHv
    Tweet from @CNN #2018-01-05 19:49:04+00:00 created on 77:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/x180bXAbor
    Tweet from @CNN #2018-01-05 19:35:16+00:00 created on 78:Why billions of dollars in lottery prizes go unclaimed https://t.co/aCQrsBKhut https://t.co/qv3vtF3vpE
    Tweet from @CNN #2018-01-05 19:18:38+00:00 created on 79:Barack Obama will be the first guest on David Letterman's new Netflix show https://t.co/fWTkjKCCsV https://t.co/kmMn7Bm6I0
    Tweet from @CNN #2018-01-05 19:11:29+00:00 created on 80:RT @CNNPolitics: President Trump: "We're going to Camp David with a lot of the great Republicans senators and we're making America great ag…
    Tweet from @CNN #2018-01-05 22:20:08+00:00 created on 81:Vermont moves to legalize pot as feds signal possible crackdown https://t.co/SNx8qXKWzQ https://t.co/vrTvPbaMxN
    Tweet from @CNN #2018-01-05 22:10:03+00:00 created on 82:Secretary of State Rex Tillerson says he's never questioned President Trump's mental fitness https://t.co/w4qiCnrCek https://t.co/sBCygN5Hqh
    Tweet from @CNN #2018-01-05 22:00:05+00:00 created on 83:She was roasting marshmallows on New Year's Eve. Then a gas can exploded. https://t.co/dIPVpSksRO https://t.co/iXmHTB1bqz
    Tweet from @CNN #2018-01-05 21:50:08+00:00 created on 84:Rep. Elijah Cummings was admitted to the hospital for a bacterial infection in his knee, his office has announced… https://t.co/RGKgL39aen
    Tweet from @CNN #2018-01-05 21:40:47+00:00 created on 85:"Jeopardy!" host Alex Trebek is taking a medical leave after he experienced complications from a head injury known… https://t.co/KOhQ3vMuqI
    Tweet from @CNN #2018-01-05 21:30:13+00:00 created on 86:Delta Air Lines flew its last Boeing 747 to an aircraft boneyard in Arizona this week, ending operations for 747s b… https://t.co/arSAFoTMg6
    Tweet from @CNN #2018-01-05 21:20:06+00:00 created on 87:What the heck does Steve Bannon do now? | Analysis by @CillizzaCNN https://t.co/XCkYPhDcKi https://t.co/d8bjpcBE6o
    Tweet from @CNN #2018-01-05 21:10:08+00:00 created on 88:GOP Rep. Chris Stewart, a member of the House Intelligence Committee, joined the growing list of Republicans callin… https://t.co/u47xZ7DX4W
    Tweet from @CNN #2018-01-05 21:00:13+00:00 created on 89:Why billions of dollars in lottery prizes go unclaimed https://t.co/jWkE30IsxI https://t.co/p6LRarCGQ5
    Tweet from @CNN #2018-01-05 20:50:04+00:00 created on 90:"Lyin’ Ted"
    "Low Energy Jeb"
    "Little Marco"
    
    Where does "Sloppy Steve" rank among President Trump's other nicknames… https://t.co/bBf534HMy3
    Tweet from @CNN #2018-01-05 20:40:10+00:00 created on 91:The future of how the internet is regulated could be decided by a showdown between states and the federal governmen… https://t.co/OKHMA1zFBM
    Tweet from @CNN #2018-01-05 20:31:20+00:00 created on 92:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/w7UMkqmcHI
    Tweet from @CNN #2018-01-05 20:31:15+00:00 created on 93:"One week. Welcome to 2018." - @BrookeBCNN 
    
    We're only five days in and these are all the things that have already… https://t.co/hKIWFRmoNk
    Tweet from @CNN #2018-01-05 20:17:07+00:00 created on 94:JetBlue says it's handing out $21 million worth of bonuses to employees because of the corporate tax cut -- all 21,… https://t.co/y2Wn0sEvfx
    Tweet from @CNN #2018-01-05 20:16:16+00:00 created on 95:Trump's first-year jobs record was strong. Just not as strong as Obama's last year. https://t.co/OfFNGNxdvu https://t.co/5VRCtjfGLT
    Tweet from @CNN #2018-01-05 19:59:07+00:00 created on 96:World's longest glass bridge opens in Hebei, China https://t.co/ON5zFyFnug (via @CNNTravel) https://t.co/ZPUojOErHv
    Tweet from @CNN #2018-01-05 19:49:04+00:00 created on 97:Ohio Republican Josh Mandel is dropping his bid to unseat Democratic Sen. Sherrod Brown, citing a health issue his… https://t.co/x180bXAbor
    Tweet from @CNN #2018-01-05 19:35:16+00:00 created on 98:Why billions of dollars in lottery prizes go unclaimed https://t.co/aCQrsBKhut https://t.co/qv3vtF3vpE
    Tweet from @CNN #2018-01-05 19:18:38+00:00 created on 99:Barack Obama will be the first guest on David Letterman's new Netflix show https://t.co/fWTkjKCCsV https://t.co/kmMn7Bm6I0
    Tweet from @CNN #2018-01-05 19:11:29+00:00 created on 100:RT @CNNPolitics: President Trump: "We're going to Camp David with a lot of the great Republicans senators and we're making America great ag…
     ---------------TWEET COMPLETED FOR @CNN-------------
    Tweet from @FoxNews #2018-01-07 15:10:47+00:00 created on 1:.@TGowdySC: "We need to talk to everyone who was involved in the drafting of this exoneration memo in May of 2016."… https://t.co/v3RdAN4NXJ
    Tweet from @FoxNews #2018-01-07 15:08:30+00:00 created on 2:.@TGowdySC: "This time last year, Democrats wanted James Comey prosecuted. Remember that? Harry Reid wanted him pro… https://t.co/TiTRlyxmLP
    Tweet from @FoxNews #2018-01-07 15:07:41+00:00 created on 3:.@TGowdySC: "I have been frankly stunned at how little curiosity my Democrat colleagues and how little curiosity so… https://t.co/NmyUMo89bI
    Tweet from @FoxNews #2018-01-07 15:07:01+00:00 created on 4:TONIGHT: @WilliamJBennett hosts "The Wise Guys," a powerful discussion on America's future - Tune in at 8p &amp; 11p ET… https://t.co/uaKJkz5o5P
    Tweet from @FoxNews #2018-01-07 15:04:40+00:00 created on 5:.@TGowdySC: "It's not illegitimate for Congress to ask the FBI and the DOJ, 'What did you do? Why did you do it?'"… https://t.co/IpTb3NXGHi
    Tweet from @FoxNews #2018-01-07 15:00:02+00:00 created on 6:Moments ago @realDonaldTrump slammed @MichaelWolffNYC, calling him a "totally discredited author." https://t.co/IyrbjckvIa
    Tweet from @FoxNews #2018-01-07 14:58:35+00:00 created on 7:.@MariaBartiromo: "I would not be surprised to see many, many more records hit in this upcoming year." https://t.co/te1gcCzACL
    Tweet from @FoxNews #2018-01-07 14:55:56+00:00 created on 8:RT @FoxBusiness: .@KellyannePolls: "I feel confident anytime Donald Trump is at the negotiating table because he is a deal maker... He's of…
    Tweet from @FoxNews #2018-01-07 14:50:32+00:00 created on 9:On @foxandfriends, legendary college coach Dr. Lou Holtz gave his take on the @NFL's declining ratings.… https://t.co/5g0lGMC0HU
    Tweet from @FoxNews #2018-01-07 14:50:20+00:00 created on 10:RT @FoxNewsSunday: .@CLewandowski_on @SteveKBannon's place in the Republican party: If Steve wants to join with the Trump team...he'll be w…
    Tweet from @FoxNews #2018-01-07 14:49:00+00:00 created on 11:RT @FoxNewsSunday: .@CLewandowski_on Wolff statements in book attributed to Bannon: If that's what Steve said he owes the whole Trump famil…
    Tweet from @FoxNews #2018-01-07 14:48:59+00:00 created on 12:RT @FoxNewsSunday: .@CLewandowski_on @realDonaldTrump's fitness to be president: I spent 18 hours a day with candidate Trump, I can tell yo…
    Tweet from @FoxNews #2018-01-07 14:48:57+00:00 created on 13:RT @FoxNewsSunday: CIA Director Mike Pompeo addresses questions raised by "Fire and Fury" on Trump's fitness to be President:... https://t.…
    Tweet from @FoxNews #2018-01-07 14:48:54+00:00 created on 14:RT @FoxNewsSunday: .@CLewandowski_on if he called Jared Kushner "the Butler": I've never done that, that's such conjecture.
    Tweet from @FoxNews #2018-01-07 14:48:52+00:00 created on 15:RT @FoxNewsSunday: .@CLewandowski_on Don Jr. and Jared Kushner: They’re American patriots…they’ve done so much to help the country.
    Tweet from @FoxNews #2018-01-07 14:48:50+00:00 created on 16:RT @FoxNewsSunday: .@CLewandowski_on his relationship with Trump sons and Kushner: I think Eric and Don are fantastic advocates for their f…
    Tweet from @FoxNews #2018-01-07 14:41:44+00:00 created on 17:RT @FoxNewsSunday: .@CLewandowski_on @POTUS’ temper: He expects, demands and deserves the best…when staff wasn’t perfect, he held us accoun…
    Tweet from @FoxNews #2018-01-07 14:38:05+00:00 created on 18:Washington State to allow 'X' as gender option. https://t.co/XE4UKL3M7v
    Tweet from @FoxNews #2018-01-07 14:38:01+00:00 created on 19:RT @FoxNewsSunday: .@CLewandowski_on accuracy of “Fire and Fury”: @MichaelWolffNYC and I never spoke about this book...it is completely mad…
    Tweet from @FoxNews #2018-01-07 14:35:19+00:00 created on 20:RT @FoxNewsSunday: .@CLewandowski_on “Fire and Fury”: This is a book of fiction…so many misrepresentations, it should not be taken seriousl…
    Tweet from @FoxNews #2018-01-07 15:10:47+00:00 created on 21:.@TGowdySC: "We need to talk to everyone who was involved in the drafting of this exoneration memo in May of 2016."… https://t.co/v3RdAN4NXJ
    Tweet from @FoxNews #2018-01-07 15:08:30+00:00 created on 22:.@TGowdySC: "This time last year, Democrats wanted James Comey prosecuted. Remember that? Harry Reid wanted him pro… https://t.co/TiTRlyxmLP
    Tweet from @FoxNews #2018-01-07 15:07:41+00:00 created on 23:.@TGowdySC: "I have been frankly stunned at how little curiosity my Democrat colleagues and how little curiosity so… https://t.co/NmyUMo89bI
    Tweet from @FoxNews #2018-01-07 15:07:01+00:00 created on 24:TONIGHT: @WilliamJBennett hosts "The Wise Guys," a powerful discussion on America's future - Tune in at 8p &amp; 11p ET… https://t.co/uaKJkz5o5P
    Tweet from @FoxNews #2018-01-07 15:04:40+00:00 created on 25:.@TGowdySC: "It's not illegitimate for Congress to ask the FBI and the DOJ, 'What did you do? Why did you do it?'"… https://t.co/IpTb3NXGHi
    Tweet from @FoxNews #2018-01-07 15:00:02+00:00 created on 26:Moments ago @realDonaldTrump slammed @MichaelWolffNYC, calling him a "totally discredited author." https://t.co/IyrbjckvIa
    Tweet from @FoxNews #2018-01-07 14:58:35+00:00 created on 27:.@MariaBartiromo: "I would not be surprised to see many, many more records hit in this upcoming year." https://t.co/te1gcCzACL
    Tweet from @FoxNews #2018-01-07 14:55:56+00:00 created on 28:RT @FoxBusiness: .@KellyannePolls: "I feel confident anytime Donald Trump is at the negotiating table because he is a deal maker... He's of…
    Tweet from @FoxNews #2018-01-07 14:50:32+00:00 created on 29:On @foxandfriends, legendary college coach Dr. Lou Holtz gave his take on the @NFL's declining ratings.… https://t.co/5g0lGMC0HU
    Tweet from @FoxNews #2018-01-07 14:50:20+00:00 created on 30:RT @FoxNewsSunday: .@CLewandowski_on @SteveKBannon's place in the Republican party: If Steve wants to join with the Trump team...he'll be w…
    Tweet from @FoxNews #2018-01-07 14:49:00+00:00 created on 31:RT @FoxNewsSunday: .@CLewandowski_on Wolff statements in book attributed to Bannon: If that's what Steve said he owes the whole Trump famil…
    Tweet from @FoxNews #2018-01-07 14:48:59+00:00 created on 32:RT @FoxNewsSunday: .@CLewandowski_on @realDonaldTrump's fitness to be president: I spent 18 hours a day with candidate Trump, I can tell yo…
    Tweet from @FoxNews #2018-01-07 14:48:57+00:00 created on 33:RT @FoxNewsSunday: CIA Director Mike Pompeo addresses questions raised by "Fire and Fury" on Trump's fitness to be President:... https://t.…
    Tweet from @FoxNews #2018-01-07 14:48:54+00:00 created on 34:RT @FoxNewsSunday: .@CLewandowski_on if he called Jared Kushner "the Butler": I've never done that, that's such conjecture.
    Tweet from @FoxNews #2018-01-07 14:48:52+00:00 created on 35:RT @FoxNewsSunday: .@CLewandowski_on Don Jr. and Jared Kushner: They’re American patriots…they’ve done so much to help the country.
    Tweet from @FoxNews #2018-01-07 14:48:50+00:00 created on 36:RT @FoxNewsSunday: .@CLewandowski_on his relationship with Trump sons and Kushner: I think Eric and Don are fantastic advocates for their f…
    Tweet from @FoxNews #2018-01-07 14:41:44+00:00 created on 37:RT @FoxNewsSunday: .@CLewandowski_on @POTUS’ temper: He expects, demands and deserves the best…when staff wasn’t perfect, he held us accoun…
    Tweet from @FoxNews #2018-01-07 14:38:05+00:00 created on 38:Washington State to allow 'X' as gender option. https://t.co/XE4UKL3M7v
    Tweet from @FoxNews #2018-01-07 14:38:01+00:00 created on 39:RT @FoxNewsSunday: .@CLewandowski_on accuracy of “Fire and Fury”: @MichaelWolffNYC and I never spoke about this book...it is completely mad…
    Tweet from @FoxNews #2018-01-07 14:35:19+00:00 created on 40:RT @FoxNewsSunday: .@CLewandowski_on “Fire and Fury”: This is a book of fiction…so many misrepresentations, it should not be taken seriousl…
    Tweet from @FoxNews #2018-01-07 15:10:47+00:00 created on 41:.@TGowdySC: "We need to talk to everyone who was involved in the drafting of this exoneration memo in May of 2016."… https://t.co/v3RdAN4NXJ
    Tweet from @FoxNews #2018-01-07 15:08:30+00:00 created on 42:.@TGowdySC: "This time last year, Democrats wanted James Comey prosecuted. Remember that? Harry Reid wanted him pro… https://t.co/TiTRlyxmLP
    Tweet from @FoxNews #2018-01-07 15:07:41+00:00 created on 43:.@TGowdySC: "I have been frankly stunned at how little curiosity my Democrat colleagues and how little curiosity so… https://t.co/NmyUMo89bI
    Tweet from @FoxNews #2018-01-07 15:07:01+00:00 created on 44:TONIGHT: @WilliamJBennett hosts "The Wise Guys," a powerful discussion on America's future - Tune in at 8p &amp; 11p ET… https://t.co/uaKJkz5o5P
    Tweet from @FoxNews #2018-01-07 15:04:40+00:00 created on 45:.@TGowdySC: "It's not illegitimate for Congress to ask the FBI and the DOJ, 'What did you do? Why did you do it?'"… https://t.co/IpTb3NXGHi
    Tweet from @FoxNews #2018-01-07 15:00:02+00:00 created on 46:Moments ago @realDonaldTrump slammed @MichaelWolffNYC, calling him a "totally discredited author." https://t.co/IyrbjckvIa
    Tweet from @FoxNews #2018-01-07 14:58:35+00:00 created on 47:.@MariaBartiromo: "I would not be surprised to see many, many more records hit in this upcoming year." https://t.co/te1gcCzACL
    Tweet from @FoxNews #2018-01-07 14:55:56+00:00 created on 48:RT @FoxBusiness: .@KellyannePolls: "I feel confident anytime Donald Trump is at the negotiating table because he is a deal maker... He's of…
    Tweet from @FoxNews #2018-01-07 14:50:32+00:00 created on 49:On @foxandfriends, legendary college coach Dr. Lou Holtz gave his take on the @NFL's declining ratings.… https://t.co/5g0lGMC0HU
    Tweet from @FoxNews #2018-01-07 14:50:20+00:00 created on 50:RT @FoxNewsSunday: .@CLewandowski_on @SteveKBannon's place in the Republican party: If Steve wants to join with the Trump team...he'll be w…
    Tweet from @FoxNews #2018-01-07 14:49:00+00:00 created on 51:RT @FoxNewsSunday: .@CLewandowski_on Wolff statements in book attributed to Bannon: If that's what Steve said he owes the whole Trump famil…
    Tweet from @FoxNews #2018-01-07 14:48:59+00:00 created on 52:RT @FoxNewsSunday: .@CLewandowski_on @realDonaldTrump's fitness to be president: I spent 18 hours a day with candidate Trump, I can tell yo…
    Tweet from @FoxNews #2018-01-07 14:48:57+00:00 created on 53:RT @FoxNewsSunday: CIA Director Mike Pompeo addresses questions raised by "Fire and Fury" on Trump's fitness to be President:... https://t.…
    Tweet from @FoxNews #2018-01-07 14:48:54+00:00 created on 54:RT @FoxNewsSunday: .@CLewandowski_on if he called Jared Kushner "the Butler": I've never done that, that's such conjecture.
    Tweet from @FoxNews #2018-01-07 14:48:52+00:00 created on 55:RT @FoxNewsSunday: .@CLewandowski_on Don Jr. and Jared Kushner: They’re American patriots…they’ve done so much to help the country.
    Tweet from @FoxNews #2018-01-07 14:48:50+00:00 created on 56:RT @FoxNewsSunday: .@CLewandowski_on his relationship with Trump sons and Kushner: I think Eric and Don are fantastic advocates for their f…
    Tweet from @FoxNews #2018-01-07 14:41:44+00:00 created on 57:RT @FoxNewsSunday: .@CLewandowski_on @POTUS’ temper: He expects, demands and deserves the best…when staff wasn’t perfect, he held us accoun…
    Tweet from @FoxNews #2018-01-07 14:38:05+00:00 created on 58:Washington State to allow 'X' as gender option. https://t.co/XE4UKL3M7v
    Tweet from @FoxNews #2018-01-07 14:38:01+00:00 created on 59:RT @FoxNewsSunday: .@CLewandowski_on accuracy of “Fire and Fury”: @MichaelWolffNYC and I never spoke about this book...it is completely mad…
    Tweet from @FoxNews #2018-01-07 14:35:19+00:00 created on 60:RT @FoxNewsSunday: .@CLewandowski_on “Fire and Fury”: This is a book of fiction…so many misrepresentations, it should not be taken seriousl…
    Tweet from @FoxNews #2018-01-07 15:10:47+00:00 created on 61:.@TGowdySC: "We need to talk to everyone who was involved in the drafting of this exoneration memo in May of 2016."… https://t.co/v3RdAN4NXJ
    Tweet from @FoxNews #2018-01-07 15:08:30+00:00 created on 62:.@TGowdySC: "This time last year, Democrats wanted James Comey prosecuted. Remember that? Harry Reid wanted him pro… https://t.co/TiTRlyxmLP
    Tweet from @FoxNews #2018-01-07 15:07:41+00:00 created on 63:.@TGowdySC: "I have been frankly stunned at how little curiosity my Democrat colleagues and how little curiosity so… https://t.co/NmyUMo89bI
    Tweet from @FoxNews #2018-01-07 15:07:01+00:00 created on 64:TONIGHT: @WilliamJBennett hosts "The Wise Guys," a powerful discussion on America's future - Tune in at 8p &amp; 11p ET… https://t.co/uaKJkz5o5P
    Tweet from @FoxNews #2018-01-07 15:04:40+00:00 created on 65:.@TGowdySC: "It's not illegitimate for Congress to ask the FBI and the DOJ, 'What did you do? Why did you do it?'"… https://t.co/IpTb3NXGHi
    Tweet from @FoxNews #2018-01-07 15:00:02+00:00 created on 66:Moments ago @realDonaldTrump slammed @MichaelWolffNYC, calling him a "totally discredited author." https://t.co/IyrbjckvIa
    Tweet from @FoxNews #2018-01-07 14:58:35+00:00 created on 67:.@MariaBartiromo: "I would not be surprised to see many, many more records hit in this upcoming year." https://t.co/te1gcCzACL
    Tweet from @FoxNews #2018-01-07 14:55:56+00:00 created on 68:RT @FoxBusiness: .@KellyannePolls: "I feel confident anytime Donald Trump is at the negotiating table because he is a deal maker... He's of…
    Tweet from @FoxNews #2018-01-07 14:50:32+00:00 created on 69:On @foxandfriends, legendary college coach Dr. Lou Holtz gave his take on the @NFL's declining ratings.… https://t.co/5g0lGMC0HU
    Tweet from @FoxNews #2018-01-07 14:50:20+00:00 created on 70:RT @FoxNewsSunday: .@CLewandowski_on @SteveKBannon's place in the Republican party: If Steve wants to join with the Trump team...he'll be w…
    Tweet from @FoxNews #2018-01-07 14:49:00+00:00 created on 71:RT @FoxNewsSunday: .@CLewandowski_on Wolff statements in book attributed to Bannon: If that's what Steve said he owes the whole Trump famil…
    Tweet from @FoxNews #2018-01-07 14:48:59+00:00 created on 72:RT @FoxNewsSunday: .@CLewandowski_on @realDonaldTrump's fitness to be president: I spent 18 hours a day with candidate Trump, I can tell yo…
    Tweet from @FoxNews #2018-01-07 14:48:57+00:00 created on 73:RT @FoxNewsSunday: CIA Director Mike Pompeo addresses questions raised by "Fire and Fury" on Trump's fitness to be President:... https://t.…
    Tweet from @FoxNews #2018-01-07 14:48:54+00:00 created on 74:RT @FoxNewsSunday: .@CLewandowski_on if he called Jared Kushner "the Butler": I've never done that, that's such conjecture.
    Tweet from @FoxNews #2018-01-07 14:48:52+00:00 created on 75:RT @FoxNewsSunday: .@CLewandowski_on Don Jr. and Jared Kushner: They’re American patriots…they’ve done so much to help the country.
    Tweet from @FoxNews #2018-01-07 14:48:50+00:00 created on 76:RT @FoxNewsSunday: .@CLewandowski_on his relationship with Trump sons and Kushner: I think Eric and Don are fantastic advocates for their f…
    Tweet from @FoxNews #2018-01-07 14:41:44+00:00 created on 77:RT @FoxNewsSunday: .@CLewandowski_on @POTUS’ temper: He expects, demands and deserves the best…when staff wasn’t perfect, he held us accoun…
    Tweet from @FoxNews #2018-01-07 14:38:05+00:00 created on 78:Washington State to allow 'X' as gender option. https://t.co/XE4UKL3M7v
    Tweet from @FoxNews #2018-01-07 14:38:01+00:00 created on 79:RT @FoxNewsSunday: .@CLewandowski_on accuracy of “Fire and Fury”: @MichaelWolffNYC and I never spoke about this book...it is completely mad…
    Tweet from @FoxNews #2018-01-07 14:35:19+00:00 created on 80:RT @FoxNewsSunday: .@CLewandowski_on “Fire and Fury”: This is a book of fiction…so many misrepresentations, it should not be taken seriousl…
    Tweet from @FoxNews #2018-01-07 15:10:47+00:00 created on 81:.@TGowdySC: "We need to talk to everyone who was involved in the drafting of this exoneration memo in May of 2016."… https://t.co/v3RdAN4NXJ
    Tweet from @FoxNews #2018-01-07 15:08:30+00:00 created on 82:.@TGowdySC: "This time last year, Democrats wanted James Comey prosecuted. Remember that? Harry Reid wanted him pro… https://t.co/TiTRlyxmLP
    Tweet from @FoxNews #2018-01-07 15:07:41+00:00 created on 83:.@TGowdySC: "I have been frankly stunned at how little curiosity my Democrat colleagues and how little curiosity so… https://t.co/NmyUMo89bI
    Tweet from @FoxNews #2018-01-07 15:07:01+00:00 created on 84:TONIGHT: @WilliamJBennett hosts "The Wise Guys," a powerful discussion on America's future - Tune in at 8p &amp; 11p ET… https://t.co/uaKJkz5o5P
    Tweet from @FoxNews #2018-01-07 15:04:40+00:00 created on 85:.@TGowdySC: "It's not illegitimate for Congress to ask the FBI and the DOJ, 'What did you do? Why did you do it?'"… https://t.co/IpTb3NXGHi
    Tweet from @FoxNews #2018-01-07 15:00:02+00:00 created on 86:Moments ago @realDonaldTrump slammed @MichaelWolffNYC, calling him a "totally discredited author." https://t.co/IyrbjckvIa
    Tweet from @FoxNews #2018-01-07 14:58:35+00:00 created on 87:.@MariaBartiromo: "I would not be surprised to see many, many more records hit in this upcoming year." https://t.co/te1gcCzACL
    Tweet from @FoxNews #2018-01-07 14:55:56+00:00 created on 88:RT @FoxBusiness: .@KellyannePolls: "I feel confident anytime Donald Trump is at the negotiating table because he is a deal maker... He's of…
    Tweet from @FoxNews #2018-01-07 14:50:32+00:00 created on 89:On @foxandfriends, legendary college coach Dr. Lou Holtz gave his take on the @NFL's declining ratings.… https://t.co/5g0lGMC0HU
    Tweet from @FoxNews #2018-01-07 14:50:20+00:00 created on 90:RT @FoxNewsSunday: .@CLewandowski_on @SteveKBannon's place in the Republican party: If Steve wants to join with the Trump team...he'll be w…
    Tweet from @FoxNews #2018-01-07 14:49:00+00:00 created on 91:RT @FoxNewsSunday: .@CLewandowski_on Wolff statements in book attributed to Bannon: If that's what Steve said he owes the whole Trump famil…
    Tweet from @FoxNews #2018-01-07 14:48:59+00:00 created on 92:RT @FoxNewsSunday: .@CLewandowski_on @realDonaldTrump's fitness to be president: I spent 18 hours a day with candidate Trump, I can tell yo…
    Tweet from @FoxNews #2018-01-07 14:48:57+00:00 created on 93:RT @FoxNewsSunday: CIA Director Mike Pompeo addresses questions raised by "Fire and Fury" on Trump's fitness to be President:... https://t.…
    Tweet from @FoxNews #2018-01-07 14:48:54+00:00 created on 94:RT @FoxNewsSunday: .@CLewandowski_on if he called Jared Kushner "the Butler": I've never done that, that's such conjecture.
    Tweet from @FoxNews #2018-01-07 14:48:52+00:00 created on 95:RT @FoxNewsSunday: .@CLewandowski_on Don Jr. and Jared Kushner: They’re American patriots…they’ve done so much to help the country.
    Tweet from @FoxNews #2018-01-07 14:48:50+00:00 created on 96:RT @FoxNewsSunday: .@CLewandowski_on his relationship with Trump sons and Kushner: I think Eric and Don are fantastic advocates for their f…
    Tweet from @FoxNews #2018-01-07 14:41:44+00:00 created on 97:RT @FoxNewsSunday: .@CLewandowski_on @POTUS’ temper: He expects, demands and deserves the best…when staff wasn’t perfect, he held us accoun…
    Tweet from @FoxNews #2018-01-07 14:38:05+00:00 created on 98:Washington State to allow 'X' as gender option. https://t.co/XE4UKL3M7v
    Tweet from @FoxNews #2018-01-07 14:38:01+00:00 created on 99:RT @FoxNewsSunday: .@CLewandowski_on accuracy of “Fire and Fury”: @MichaelWolffNYC and I never spoke about this book...it is completely mad…
    Tweet from @FoxNews #2018-01-07 14:35:19+00:00 created on 100:RT @FoxNewsSunday: .@CLewandowski_on “Fire and Fury”: This is a book of fiction…so many misrepresentations, it should not be taken seriousl…
     ---------------TWEET COMPLETED FOR @FoxNews-------------
    Tweet from @nytimes #2018-01-05 19:22:03+00:00 created on 1:RT @nytvideo: Omar Salinas is one of nearly 200,000 Salvadorans with Temporary Protected Status. He could lose that status this month. Watc…
    Tweet from @nytimes #2018-01-05 19:12:03+00:00 created on 2:RT @ditzkoff: I wrote about @sethmeyers as he prepares to host a @goldenglobes show for the #MeToo moment and celebrate an entertainment in…
    Tweet from @nytimes #2018-01-05 19:02:10+00:00 created on 3:Chris Christie says he has warned President Trump that you can do nothing to make investigations any shorter, but "… https://t.co/oN6doS4i9p
    Tweet from @nytimes #2018-01-05 18:52:04+00:00 created on 4:How to react if you get caught in black ice https://t.co/nfuWIFMvrG
    Tweet from @nytimes #2018-01-05 18:42:03+00:00 created on 5:Britain is thinking of charging a 25 pence tax for paper coffee cups, hoping to persuade consumers to switch to reu… https://t.co/XqEuhVgyQX
    Tweet from @nytimes #2018-01-05 18:34:11+00:00 created on 6:RT @jonathanweisman: A year after Congress launched its investigation into Russian election interference, Judiciary senators have issued th…
    Tweet from @nytimes #2018-01-05 18:31:48+00:00 created on 7:Breaking News: The first known criminal referral has emerged from Congress's Russia inquiries. The target is the au… https://t.co/7SUxwvG5Sh
    Tweet from @nytimes #2018-01-05 18:22:05+00:00 created on 8:President Trump and Republican leaders in Congress are meeting at Camp David to address an urgent and more conventi… https://t.co/If0eUMY5dB
    Tweet from @nytimes #2018-01-05 18:16:50+00:00 created on 9:A U.S. citizen who has been held in military custody in Iraq for 4 months has told lawyers with the ACLU that he wa… https://t.co/DzfCvFABpg
    Tweet from @nytimes #2018-01-05 18:12:07+00:00 created on 10:XXXTentacion, Kodak Black, Tay-K and 6ix9ine have all been accused of horrible crimes, and they all had hits on the… https://t.co/vAYkKR7YBy
    Tweet from @nytimes #2018-01-05 18:02:03+00:00 created on 11:Customs officers at the border and airports searched 60% more cellphones, computers and other electronic devices in… https://t.co/9hCncKc1vy
    Tweet from @nytimes #2018-01-05 17:56:13+00:00 created on 12:An @nytmag piece on a simple way to prepare kale https://t.co/I6glx0LnSj
    Tweet from @nytimes #2018-01-05 17:55:06+00:00 created on 13:Schools in Baltimore don't have heat. Teachers say students have been forced to attend classes bundled up in coats,… https://t.co/7ZdqywuYHA
    Tweet from @nytimes #2018-01-05 17:46:19+00:00 created on 14:FBI agents are again asking questions about the Clinton Foundation. President Trump has repeatedly urged officials… https://t.co/LxLHKzcZir
    Tweet from @nytimes #2018-01-05 17:42:06+00:00 created on 15:Every president is confronted by unflattering portrayals by former aides who leave and tell their stories. For Pres… https://t.co/TUwTMgw4CS
    Tweet from @nytimes #2018-01-05 17:32:04+00:00 created on 16:Sue Grafton’s private eye heroine, Kinsey Millhone, was a fixture of the best-seller lists. But it took 8 years and… https://t.co/LzyyI86DJL
    Tweet from @nytimes #2018-01-05 17:22:03+00:00 created on 17:RT @UpshotNYT: How the new "association health plans" will work, and why supporters of Obamacare are worried. https://t.co/cWpZ8knjZl
    Tweet from @nytimes #2018-01-05 17:12:07+00:00 created on 18:In post-Weinstein Hollywood, will the Golden Globes rewrite the rules for awards seasons to come?
     https://t.co/QSC0bbaINn
    Tweet from @nytimes #2018-01-05 17:02:07+00:00 created on 19:What you need to know about security flaws in computer chip — and what you need to do to protect yourself https://t.co/eCLHZNQDPm
    Tweet from @nytimes #2018-01-05 16:52:07+00:00 created on 20:It could feel as cold as 100° below zero atop a mountain in New Hampshire. So, there's that. https://t.co/sThy8nLtr3
    Tweet from @nytimes #2018-01-05 19:22:03+00:00 created on 21:RT @nytvideo: Omar Salinas is one of nearly 200,000 Salvadorans with Temporary Protected Status. He could lose that status this month. Watc…
    Tweet from @nytimes #2018-01-05 19:12:03+00:00 created on 22:RT @ditzkoff: I wrote about @sethmeyers as he prepares to host a @goldenglobes show for the #MeToo moment and celebrate an entertainment in…
    Tweet from @nytimes #2018-01-05 19:02:10+00:00 created on 23:Chris Christie says he has warned President Trump that you can do nothing to make investigations any shorter, but "… https://t.co/oN6doS4i9p
    Tweet from @nytimes #2018-01-05 18:52:04+00:00 created on 24:How to react if you get caught in black ice https://t.co/nfuWIFMvrG
    Tweet from @nytimes #2018-01-05 18:42:03+00:00 created on 25:Britain is thinking of charging a 25 pence tax for paper coffee cups, hoping to persuade consumers to switch to reu… https://t.co/XqEuhVgyQX
    Tweet from @nytimes #2018-01-05 18:34:11+00:00 created on 26:RT @jonathanweisman: A year after Congress launched its investigation into Russian election interference, Judiciary senators have issued th…
    Tweet from @nytimes #2018-01-05 18:31:48+00:00 created on 27:Breaking News: The first known criminal referral has emerged from Congress's Russia inquiries. The target is the au… https://t.co/7SUxwvG5Sh
    Tweet from @nytimes #2018-01-05 18:22:05+00:00 created on 28:President Trump and Republican leaders in Congress are meeting at Camp David to address an urgent and more conventi… https://t.co/If0eUMY5dB
    Tweet from @nytimes #2018-01-05 18:16:50+00:00 created on 29:A U.S. citizen who has been held in military custody in Iraq for 4 months has told lawyers with the ACLU that he wa… https://t.co/DzfCvFABpg
    Tweet from @nytimes #2018-01-05 18:12:07+00:00 created on 30:XXXTentacion, Kodak Black, Tay-K and 6ix9ine have all been accused of horrible crimes, and they all had hits on the… https://t.co/vAYkKR7YBy
    Tweet from @nytimes #2018-01-05 18:02:03+00:00 created on 31:Customs officers at the border and airports searched 60% more cellphones, computers and other electronic devices in… https://t.co/9hCncKc1vy
    Tweet from @nytimes #2018-01-05 17:56:13+00:00 created on 32:An @nytmag piece on a simple way to prepare kale https://t.co/I6glx0LnSj
    Tweet from @nytimes #2018-01-05 17:55:06+00:00 created on 33:Schools in Baltimore don't have heat. Teachers say students have been forced to attend classes bundled up in coats,… https://t.co/7ZdqywuYHA
    Tweet from @nytimes #2018-01-05 17:46:19+00:00 created on 34:FBI agents are again asking questions about the Clinton Foundation. President Trump has repeatedly urged officials… https://t.co/LxLHKzcZir
    Tweet from @nytimes #2018-01-05 17:42:06+00:00 created on 35:Every president is confronted by unflattering portrayals by former aides who leave and tell their stories. For Pres… https://t.co/TUwTMgw4CS
    Tweet from @nytimes #2018-01-05 17:32:04+00:00 created on 36:Sue Grafton’s private eye heroine, Kinsey Millhone, was a fixture of the best-seller lists. But it took 8 years and… https://t.co/LzyyI86DJL
    Tweet from @nytimes #2018-01-05 17:22:03+00:00 created on 37:RT @UpshotNYT: How the new "association health plans" will work, and why supporters of Obamacare are worried. https://t.co/cWpZ8knjZl
    Tweet from @nytimes #2018-01-05 17:12:07+00:00 created on 38:In post-Weinstein Hollywood, will the Golden Globes rewrite the rules for awards seasons to come?
     https://t.co/QSC0bbaINn
    Tweet from @nytimes #2018-01-05 17:02:07+00:00 created on 39:What you need to know about security flaws in computer chip — and what you need to do to protect yourself https://t.co/eCLHZNQDPm
    Tweet from @nytimes #2018-01-05 16:52:07+00:00 created on 40:It could feel as cold as 100° below zero atop a mountain in New Hampshire. So, there's that. https://t.co/sThy8nLtr3
    Tweet from @nytimes #2018-01-05 19:22:03+00:00 created on 41:RT @nytvideo: Omar Salinas is one of nearly 200,000 Salvadorans with Temporary Protected Status. He could lose that status this month. Watc…
    Tweet from @nytimes #2018-01-05 19:12:03+00:00 created on 42:RT @ditzkoff: I wrote about @sethmeyers as he prepares to host a @goldenglobes show for the #MeToo moment and celebrate an entertainment in…
    Tweet from @nytimes #2018-01-05 19:02:10+00:00 created on 43:Chris Christie says he has warned President Trump that you can do nothing to make investigations any shorter, but "… https://t.co/oN6doS4i9p
    Tweet from @nytimes #2018-01-05 18:52:04+00:00 created on 44:How to react if you get caught in black ice https://t.co/nfuWIFMvrG
    Tweet from @nytimes #2018-01-05 18:42:03+00:00 created on 45:Britain is thinking of charging a 25 pence tax for paper coffee cups, hoping to persuade consumers to switch to reu… https://t.co/XqEuhVgyQX
    Tweet from @nytimes #2018-01-05 18:34:11+00:00 created on 46:RT @jonathanweisman: A year after Congress launched its investigation into Russian election interference, Judiciary senators have issued th…
    Tweet from @nytimes #2018-01-05 18:31:48+00:00 created on 47:Breaking News: The first known criminal referral has emerged from Congress's Russia inquiries. The target is the au… https://t.co/7SUxwvG5Sh
    Tweet from @nytimes #2018-01-05 18:22:05+00:00 created on 48:President Trump and Republican leaders in Congress are meeting at Camp David to address an urgent and more conventi… https://t.co/If0eUMY5dB
    Tweet from @nytimes #2018-01-05 18:16:50+00:00 created on 49:A U.S. citizen who has been held in military custody in Iraq for 4 months has told lawyers with the ACLU that he wa… https://t.co/DzfCvFABpg
    Tweet from @nytimes #2018-01-05 18:12:07+00:00 created on 50:XXXTentacion, Kodak Black, Tay-K and 6ix9ine have all been accused of horrible crimes, and they all had hits on the… https://t.co/vAYkKR7YBy
    Tweet from @nytimes #2018-01-05 18:02:03+00:00 created on 51:Customs officers at the border and airports searched 60% more cellphones, computers and other electronic devices in… https://t.co/9hCncKc1vy
    Tweet from @nytimes #2018-01-05 17:56:13+00:00 created on 52:An @nytmag piece on a simple way to prepare kale https://t.co/I6glx0LnSj
    Tweet from @nytimes #2018-01-05 17:55:06+00:00 created on 53:Schools in Baltimore don't have heat. Teachers say students have been forced to attend classes bundled up in coats,… https://t.co/7ZdqywuYHA
    Tweet from @nytimes #2018-01-05 17:46:19+00:00 created on 54:FBI agents are again asking questions about the Clinton Foundation. President Trump has repeatedly urged officials… https://t.co/LxLHKzcZir
    Tweet from @nytimes #2018-01-05 17:42:06+00:00 created on 55:Every president is confronted by unflattering portrayals by former aides who leave and tell their stories. For Pres… https://t.co/TUwTMgw4CS
    Tweet from @nytimes #2018-01-05 17:32:04+00:00 created on 56:Sue Grafton’s private eye heroine, Kinsey Millhone, was a fixture of the best-seller lists. But it took 8 years and… https://t.co/LzyyI86DJL
    Tweet from @nytimes #2018-01-05 17:22:03+00:00 created on 57:RT @UpshotNYT: How the new "association health plans" will work, and why supporters of Obamacare are worried. https://t.co/cWpZ8knjZl
    Tweet from @nytimes #2018-01-05 17:12:07+00:00 created on 58:In post-Weinstein Hollywood, will the Golden Globes rewrite the rules for awards seasons to come?
     https://t.co/QSC0bbaINn
    Tweet from @nytimes #2018-01-05 17:02:07+00:00 created on 59:What you need to know about security flaws in computer chip — and what you need to do to protect yourself https://t.co/eCLHZNQDPm
    Tweet from @nytimes #2018-01-05 16:52:07+00:00 created on 60:It could feel as cold as 100° below zero atop a mountain in New Hampshire. So, there's that. https://t.co/sThy8nLtr3
    Tweet from @nytimes #2018-01-05 19:22:03+00:00 created on 61:RT @nytvideo: Omar Salinas is one of nearly 200,000 Salvadorans with Temporary Protected Status. He could lose that status this month. Watc…
    Tweet from @nytimes #2018-01-05 19:12:03+00:00 created on 62:RT @ditzkoff: I wrote about @sethmeyers as he prepares to host a @goldenglobes show for the #MeToo moment and celebrate an entertainment in…
    Tweet from @nytimes #2018-01-05 19:02:10+00:00 created on 63:Chris Christie says he has warned President Trump that you can do nothing to make investigations any shorter, but "… https://t.co/oN6doS4i9p
    Tweet from @nytimes #2018-01-05 18:52:04+00:00 created on 64:How to react if you get caught in black ice https://t.co/nfuWIFMvrG
    Tweet from @nytimes #2018-01-05 18:42:03+00:00 created on 65:Britain is thinking of charging a 25 pence tax for paper coffee cups, hoping to persuade consumers to switch to reu… https://t.co/XqEuhVgyQX
    Tweet from @nytimes #2018-01-05 18:34:11+00:00 created on 66:RT @jonathanweisman: A year after Congress launched its investigation into Russian election interference, Judiciary senators have issued th…
    Tweet from @nytimes #2018-01-05 18:31:48+00:00 created on 67:Breaking News: The first known criminal referral has emerged from Congress's Russia inquiries. The target is the au… https://t.co/7SUxwvG5Sh
    Tweet from @nytimes #2018-01-05 18:22:05+00:00 created on 68:President Trump and Republican leaders in Congress are meeting at Camp David to address an urgent and more conventi… https://t.co/If0eUMY5dB
    Tweet from @nytimes #2018-01-05 18:16:50+00:00 created on 69:A U.S. citizen who has been held in military custody in Iraq for 4 months has told lawyers with the ACLU that he wa… https://t.co/DzfCvFABpg
    Tweet from @nytimes #2018-01-05 18:12:07+00:00 created on 70:XXXTentacion, Kodak Black, Tay-K and 6ix9ine have all been accused of horrible crimes, and they all had hits on the… https://t.co/vAYkKR7YBy
    Tweet from @nytimes #2018-01-05 18:02:03+00:00 created on 71:Customs officers at the border and airports searched 60% more cellphones, computers and other electronic devices in… https://t.co/9hCncKc1vy
    Tweet from @nytimes #2018-01-05 17:56:13+00:00 created on 72:An @nytmag piece on a simple way to prepare kale https://t.co/I6glx0LnSj
    Tweet from @nytimes #2018-01-05 17:55:06+00:00 created on 73:Schools in Baltimore don't have heat. Teachers say students have been forced to attend classes bundled up in coats,… https://t.co/7ZdqywuYHA
    Tweet from @nytimes #2018-01-05 17:46:19+00:00 created on 74:FBI agents are again asking questions about the Clinton Foundation. President Trump has repeatedly urged officials… https://t.co/LxLHKzcZir
    Tweet from @nytimes #2018-01-05 17:42:06+00:00 created on 75:Every president is confronted by unflattering portrayals by former aides who leave and tell their stories. For Pres… https://t.co/TUwTMgw4CS
    Tweet from @nytimes #2018-01-05 17:32:04+00:00 created on 76:Sue Grafton’s private eye heroine, Kinsey Millhone, was a fixture of the best-seller lists. But it took 8 years and… https://t.co/LzyyI86DJL
    Tweet from @nytimes #2018-01-05 17:22:03+00:00 created on 77:RT @UpshotNYT: How the new "association health plans" will work, and why supporters of Obamacare are worried. https://t.co/cWpZ8knjZl
    Tweet from @nytimes #2018-01-05 17:12:07+00:00 created on 78:In post-Weinstein Hollywood, will the Golden Globes rewrite the rules for awards seasons to come?
     https://t.co/QSC0bbaINn
    Tweet from @nytimes #2018-01-05 17:02:07+00:00 created on 79:What you need to know about security flaws in computer chip — and what you need to do to protect yourself https://t.co/eCLHZNQDPm
    Tweet from @nytimes #2018-01-05 16:52:07+00:00 created on 80:It could feel as cold as 100° below zero atop a mountain in New Hampshire. So, there's that. https://t.co/sThy8nLtr3
    Tweet from @nytimes #2018-01-05 19:22:03+00:00 created on 81:RT @nytvideo: Omar Salinas is one of nearly 200,000 Salvadorans with Temporary Protected Status. He could lose that status this month. Watc…
    Tweet from @nytimes #2018-01-05 19:12:03+00:00 created on 82:RT @ditzkoff: I wrote about @sethmeyers as he prepares to host a @goldenglobes show for the #MeToo moment and celebrate an entertainment in…
    Tweet from @nytimes #2018-01-05 19:02:10+00:00 created on 83:Chris Christie says he has warned President Trump that you can do nothing to make investigations any shorter, but "… https://t.co/oN6doS4i9p
    Tweet from @nytimes #2018-01-05 18:52:04+00:00 created on 84:How to react if you get caught in black ice https://t.co/nfuWIFMvrG
    Tweet from @nytimes #2018-01-05 18:42:03+00:00 created on 85:Britain is thinking of charging a 25 pence tax for paper coffee cups, hoping to persuade consumers to switch to reu… https://t.co/XqEuhVgyQX
    Tweet from @nytimes #2018-01-05 18:34:11+00:00 created on 86:RT @jonathanweisman: A year after Congress launched its investigation into Russian election interference, Judiciary senators have issued th…
    Tweet from @nytimes #2018-01-05 18:31:48+00:00 created on 87:Breaking News: The first known criminal referral has emerged from Congress's Russia inquiries. The target is the au… https://t.co/7SUxwvG5Sh
    Tweet from @nytimes #2018-01-05 18:22:05+00:00 created on 88:President Trump and Republican leaders in Congress are meeting at Camp David to address an urgent and more conventi… https://t.co/If0eUMY5dB
    Tweet from @nytimes #2018-01-05 18:16:50+00:00 created on 89:A U.S. citizen who has been held in military custody in Iraq for 4 months has told lawyers with the ACLU that he wa… https://t.co/DzfCvFABpg
    Tweet from @nytimes #2018-01-05 18:12:07+00:00 created on 90:XXXTentacion, Kodak Black, Tay-K and 6ix9ine have all been accused of horrible crimes, and they all had hits on the… https://t.co/vAYkKR7YBy
    Tweet from @nytimes #2018-01-05 18:02:03+00:00 created on 91:Customs officers at the border and airports searched 60% more cellphones, computers and other electronic devices in… https://t.co/9hCncKc1vy
    Tweet from @nytimes #2018-01-05 17:56:13+00:00 created on 92:An @nytmag piece on a simple way to prepare kale https://t.co/I6glx0LnSj
    Tweet from @nytimes #2018-01-05 17:55:06+00:00 created on 93:Schools in Baltimore don't have heat. Teachers say students have been forced to attend classes bundled up in coats,… https://t.co/7ZdqywuYHA
    Tweet from @nytimes #2018-01-05 17:46:19+00:00 created on 94:FBI agents are again asking questions about the Clinton Foundation. President Trump has repeatedly urged officials… https://t.co/LxLHKzcZir
    Tweet from @nytimes #2018-01-05 17:42:06+00:00 created on 95:Every president is confronted by unflattering portrayals by former aides who leave and tell their stories. For Pres… https://t.co/TUwTMgw4CS
    Tweet from @nytimes #2018-01-05 17:32:04+00:00 created on 96:Sue Grafton’s private eye heroine, Kinsey Millhone, was a fixture of the best-seller lists. But it took 8 years and… https://t.co/LzyyI86DJL
    Tweet from @nytimes #2018-01-05 17:22:03+00:00 created on 97:RT @UpshotNYT: How the new "association health plans" will work, and why supporters of Obamacare are worried. https://t.co/cWpZ8knjZl
    Tweet from @nytimes #2018-01-05 17:12:07+00:00 created on 98:In post-Weinstein Hollywood, will the Golden Globes rewrite the rules for awards seasons to come?
     https://t.co/QSC0bbaINn
    Tweet from @nytimes #2018-01-05 17:02:07+00:00 created on 99:What you need to know about security flaws in computer chip — and what you need to do to protect yourself https://t.co/eCLHZNQDPm
    Tweet from @nytimes #2018-01-05 16:52:07+00:00 created on 100:It could feel as cold as 100° below zero atop a mountain in New Hampshire. So, there's that. https://t.co/sThy8nLtr3
     ---------------TWEET COMPLETED FOR @nytimes-------------
    


```python
#print compound mean of sentiments for each media news
print(tweet_compound_mean)
```

    [0.033605000000000003, 0.031355000000000001, 0.088146666666666651, 0.088350000000000012, 0.083834000000000006]
    


```python
#print negative mean of sentiments for each media news
print(tweet_negative_mean)
```

    [0.084949999999999998, 0.089724999999999999, 0.066566666666666663, 0.061137499999999997, 0.058480000000000004]
    


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
sentiment.head()
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
      <td>2017-12-29 13:41:30+00:00</td>
      <td>RT @BBCCiN: Best Day Ever? You weren't kidding...</td>
      <td>0.6407</td>
      <td>0.255</td>
      <td>0.560</td>
      <td>0.185</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@BBC</td>
      <td>2</td>
      <td>2017-12-29 13:32:04+00:00</td>
      <td>Ever wondered what makes a wet dog smell? 🤔🐶 1...</td>
      <td>0.3612</td>
      <td>0.116</td>
      <td>0.884</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@BBC</td>
      <td>3</td>
      <td>2017-12-29 13:00:06+00:00</td>
      <td>How do we control our body temperature? ❄️🌡\n\...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@BBC</td>
      <td>4</td>
      <td>2017-12-29 12:32:03+00:00</td>
      <td>From Trump's presidency to the FIFA corruption...</td>
      <td>-0.4404</td>
      <td>0.000</td>
      <td>0.861</td>
      <td>0.139</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@BBC</td>
      <td>5</td>
      <td>2017-12-29 12:25:38+00:00</td>
      <td>RT @bbc5live: "One of the worst films I can re...</td>
      <td>-0.2960</td>
      <td>0.094</td>
      <td>0.774</td>
      <td>0.132</td>
    </tr>
  </tbody>
</table>
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
plt.savefig("Sentiment_Analysis_of_tweets.png")
plt.show()
```


![png](README_files/README_15_0.png)



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
      <td>0.033605</td>
      <td>0.084950</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@CBS</td>
      <td>0.031355</td>
      <td>0.089725</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@CNN</td>
      <td>0.088147</td>
      <td>0.066567</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@FoxNews</td>
      <td>0.088350</td>
      <td>0.061137</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@nytimes</td>
      <td>0.083834</td>
      <td>0.058480</td>
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
plt.savefig("Overall_Sentiment_Analysis.png")
```


```python
plt.show()
```


    <matplotlib.figure.Figure at 0x217d9b9be80>



```python
sns.barplot(x='User',y='tweet_negative_mean',data=overall_sentiment,palette=colors)
# Sets the y limits of Sentiments
plt.ylim(-0.8,0.8)
plt.ylabel("Tweet Polarity")
plt.title("Overall Media negative Sentiment based on Twitter (%s)" % time.strftime("%x"),loc = "center")
plt.savefig("Overall_negative_Sentiment_Analysis.png")
```


```python
plt.show()
```


![png](README_files/README_22_0.png)

