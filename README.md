---
title: "Final Project"
author: "Olivia Richards"
date: ""
output: 
  html_document:
    fig_height: 11
    fig_width: 15
---
<!-- Don't edit in between this line and the one below -->
```{r include=FALSE}
# Don't delete this chunk if you are using the DataComputing package
library(DataComputing)
library(readr)
```
*Source file* 
```{r, results='asis', echo=FALSE}
includeSourceDocuments()
```
<!-- Don't edit the material above this line -->

##Introduction

For my STAT 184 Final Project, I decided to analyze my own personal data that I haven't taken the opportunity to analyze. There is plenty of consistency in the data since I've been wearing a FitBit since June 2015 and there are probably only a handful of days that I didn't wear the band. I downloaded my data from the online FitBit portal.
I was primarily interested in looking into the difference in my active lifestyle between the summer months and school year. I find that it is much easier to stay fit in the summer. 

##Data Wrangling
```{r}
#Read all FitBit data
Oct <- read.csv(file = "october17.csv")
Sept <- read.csv(file = "Sept17FitbitData.csv")
August <- read.csv(file= "August.csv")
July <- read.csv(file= "July17.csv")
June <- read.csv(file= "June17.csv")

#add column for what month each day is a part of
Oct1 <- Oct %>%
  mutate(month = "October")
Sept1 <- Sept %>%
  mutate(month = "September")
Aug1 <- August %>%
  mutate(month = "August")
July1 <- July %>%
  mutate(month = "July")
June1 <- June %>%
  mutate(month = "June")
```

As I was interested in the average number of flights that I traveled throughout the month, I decided to use the summarise() function to find that answer for one month and I'm interested in finding it and comparing it to other months. More specifically, I'm interested in comparing average flights over the school 
```{r}
Oct1 %>% 
  summarise(mean_flights_per_day_Oct17 = mean(Floors))

Sept1 %>% 
  summarise(mean_flights_per_day_Sept17 = mean(Floors))
```

More specifically, I'm interested in comparing average flights of steps climbed during the school months versus summer months.
```{r}
July1 %>% 
  summarise(mean_flights_per_day_July17 = mean(Floors))

June1 %>% 
  summarise(mean_flights_per_day_June17 = mean(Floors))
```

This makes sense as I was at a desk all between 7:30am and 5pm for 5 days per week and I was on the 40th floor of a Manhattan office building, needless to say, I wasn't taking the stairs like I do in State College.


After I Showing that these should be obviously correlated

##Visualizations

```{r}
ggplot(Oct1, aes(x= Date, y= Distance)) + geom_point() + theme(axis.text.x = element_text(angle=30 , hjust=1)) 
```

```{r}
#compare
comp <- rbind(July1, Sept1)

ggplot(data=comp, aes(x=Date, y=Distance))+geom_point()+aes(colour=Floors)+facet_wrap(~month,ncol=4) + theme(axis.text.x = element_text(angle=30 , hjust=1)) 
```

FitBit breaks up your level activity into Sedentary, Lightly Active Fairly Active and Very Active. Because

```{r}
ggplot(Oct1, aes(x= Date, y= Minutes.Fairly.Active)) + geom_point() + theme(axis.text.x = element_text(angle=30 , hjust=1)) 
```

```{r}
#concatenate the two data frames
joined<- rbind(Aug1,Sept1)
ggplot(data=joined,aes(x=Date,y=Calories.Burned))+geom_bar(stat='identity',position='stack', width=.9) +xlab('Date') +ylab('Calories Burned') + theme(axis.text.x = element_text(angle=30 , hjust=1)) 
```

```{r}
#join academic months and separately combine 5 months 
school_joined<- rbind(Aug1,Sept1,Oct1)

summer_school<- rbind(June1,July1, Aug1, Sept1,Oct1)
```

I am interested in showing that on days when I am more active and in a "fitness" mindset, I tend to take the stairs more often.
```{r}
ggplot(data=school_joined,aes(x=Distance,y=Minutes.Very.Active))+geom_point()+aes(colour=month)+aes(size=Floors) 
```

Additionally, during the summer months of my research internships, I have much less responsibility than the school year. This can be visualized below by the large ranges of Very Active Minutes during the summer months, as compared to the small ranges during the year.
```{r}
ggplot(data=summer_school,aes(x=Distance,y=Minutes.Very.Active))+geom_point()+aes(colour=Floors)+facet_wrap(~month,ncol=4) 
```
