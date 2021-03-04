---
layout: post
title: Achievements
sections:

description: Learn more about the application's achievement system.
image: 
---
## Contents
1. Motivation
1. Background
1. MVP Definition
1. Implementation
1. Discussion
1. Outlook
1. References

## Motivation
In an initial design thinking workshop at the start of the second semester of the Escapeling project, multiple students came up with several ideas to facilitate long-term engagement in our application. Thus, we decided to combine them into the achievements feature by extracting the core principles of the ideas and using these to lead the realization of a MVP for the achievements. 

The achievements module tackles one of the central problems of learning applications: Low long-term engagement of users. Learning apps rank lowest in the user retention after one month (Pham & Chen, 2019). While Escapeling technically is not a standalone application, we feel that the same challenges also apply here. One key method to increase user retention is to make sure that users have a goal they can work towards inside the application. Achievements can serve as such a goal if their fulfillment criteria are displayed to users transparently. Additionally, achievements can also be utilized to increase other measures of engagement such as session length if their completion criteria are set accordingly. 

Finally, achievements can also increase learning outcomes in learning games if employed thoughtfully (Blair et al., 2016). They also provide a means for tracking learning goals.

## Background
Research on motivating role of achievements + learning

Joana

## MVP Definition
What did we decide on for our achievements MVP (explanation of the two achievements + how do they tie in with the application)

Cedric

## Implementation
Explanation of implementation of achievements

### Achievement Data
In order to tracks user progress along for achievements, we need to track all relevant pieces of information. Because we want to employ a framework approach that allows for quick implementation of future achievements, we need a data structure with high flexibility for different data types. Additionally, this data also needs to be persistent across multiple sessions to enable long-term goals. It must also not be lost if the backend of the application is shut down.

We achieve flexibility regarding different data types by creating a simple python dictionary in the student object that contains the achievement data for that student in the form of key-value pairs. In addition to an interface to set values using the key, we also created a function to increment values easily. We expect may achievements to rely on data that is incremental in nature (e.g. the number of codeword pieces collected). These interfaces are then used to update the achievement data at a given point in the application (e.g. incrementing the codeword piece counter after the group managed to unlock a piece for each student in the group).

The content of the student data dictionary is then persisted to the database table student_data at the end of each task. We load the data from the database when a student object is created in the backend runtime i.e. at the start of a session in the private or group chat. The student_data table contains three columns `student_id`, `data_field`, and `value`. The `student_id` denotes the student whom the data belongs to whilst the `data_field` contains the key of the data dictionary and `value` its corresponding value. For the `value` field we chose a `VARCHAR` datatype to also allows non-numeric values. Table 1 shows an excerpt from the current database content for a single student.

| student_id | data_field | value |
| :-- | :-- | :-- |
| 4 | last_played | 20210207 |
| 4 | consecutive_days | 2 |
| 4 | highest_streak | 5 |
| 4 | codeword_pieces_collected | 9 |

*Table 1: Exemplary Achievement Data*

The last row is used as a simple counter for the *Code Cracker* achievement whilst the first three rows store all information needed for the *Crew Rank* achievement. In order to track the current streak, we need to know the last date that the user engaged with the application `last_played`. If that date is exactly one day in the past when the user engages with the application, we increment the counter of the current streak `consecutive_days`and update `last_played` to the current date. If the date is in the past, we reset `consecutive_days` to one and update `last_played`. If the users re-opens the application on the same day, we do nothing. Finally we compare the current streak `consecutive_days` to the all-time longest streak `highest_streak` for that user and update the latter a new highscore has been achieved. This example illuminates how our simple data structure allows modeling of more complex achievements thanks to its flexibility.

### Achievement Framework
How does the framework work and which functions does it provide?
Benefit: Easy implementation of additional achievements (explanation of achievement base class and the concept of inheriting from it)

Tom 

## Discussion
Critical assessment of our MVP: What works, what doesn't? How would we view the achievements in retrospect -> Maybe the testing group can jump in here
Everyone -> Bulletpoints

## Outlook
What can be improved upon and what are next steps w.r.t. achievements e.g. visualization of user progress over time / additional achievements 
Everyone -> Bulletpoints

Other ways for long-term engagement: App updates, push notifications, social media, integrated evaluation

Self-set goals


## References
Blair, L., Bowers, C., Cannon-Bowers, J., & Gonzalez-Holland, E. (2016). Understanding the Role of Achievements in Game-Based Learning. International Journal of Serious Games, 3. https://doi.org/10.17083/ijsg.v3i4.114

Pham, X. L., & Chen, G. D. (2019). PACARD: A New Interface to Increase Mobile Learning App Engagement, Distributed Through App Stores. Journal of Educational Computing Research, 57(3), 618–645. https://doi.org/10.1177/0735633118756298


