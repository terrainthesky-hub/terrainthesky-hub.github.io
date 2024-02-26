---
layout: post
title: Time We Spend on Steam Games by Categories
subtitle: Data Exploration for Time Played on Steam Games
---

I looked at the steam games data found at kaggle, here: [Steam Games](https://www.kaggle.com/nikdavis/steam-store-games)

My notebook: [ipynb](https://github.com/terrainthesky-hub/Data-Science-Exploration)

My hypothesis is that there is not a correlation between the price of a game and how much time people spend on it.
There were 27000 steam games. I found that there were around 21000 steam games with an average playtime of 0. That’s a lot of unplayed games and shovel ware. I created a binary column which said whether the game has a playtime of 0 or not. Separating the two, I finally started to recognize games on the list.

I tried to find the correlation between the average time played of all games and the price of all the games. It ended up being .03, and then I removed all the free to play games from the set and ran the correlation again. It was still low at .13. So there didn’t seem to be a strong or weak positive correlation between how much time we spend gaming and how much we pay for games.

But what are people spending their time playing? I subset the data into different genres, including a f2p (free to play) category because although f2p games like dota 2 and fortnite are very different games, they share the fact they have a game loop that keeps you coming back to play more. Many f2p games share a similar philosophy about having the player return to the game and never really beating it. 

I found the means and standard deviations of each of the category subsets I made based on hours played. There were some interesting results! 
<html>
{% include game_meanstd.html %}
 </html>
Most notably, mmo and f2p games had the highest average playtime, but the standard deviations of playtime were staggeringly large! It shows some of the f2p games had a very high average playtime. That’s a lot of variance in how people play those. The mmo and f2p game loops really are working on some people. On the other end of the spectrum you see metroidvania as being roughly similar in mean and std, metroidvanias have tight-knit experiences and there’s not much end game so you don’t see too much variance in how people play them. Puzzle games, strategy games, and hack n slash games tell a similar story to each other, the average is about the same where there are probably a lot of low hour games but some of the games in the genres people played a significant amount. All those hours aren't entirely equal though, some games are enjoyed more than others.

![Positive review average by category](/img/reviews_avgs2.png)

Interestingly, most of the genres even out at a C or low B. Metroidvania people are enjoying their genre even though there’s less overall hours than other genres. MMO people are apparently very critical of their genre despite investing a lot of time.

The data seems to show that the game loop is what drives player time investment so f2p models are definitely viable in terms of player time investment. In conclusion, if we could harness the power of a f2p mmo game loop there’s either a lot of money to be made or a lot of people’s energy and attention being spent (and hardware running the game). Maybe there’s a captcha like interaction we could create with those game loops. No one’s tried having the game loop mine cryptocurrency yet either, or better yet have several different games all with similar time investment scales mine a cryptocurrency. 
