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

There are multiple reasons why we did decided on including achievements: First off, displaying learners streaks, the amount of days the played consecutively, has shown to help building up a habit of using the application regularly (Janukas & Kapelac, 2020). According to the Fogg behaviour model, hope and fear are key elements in the process of developing motivation (Foog, 2009). The fear of loosing the streak and the hope of achieving a new goal can motivate students to keep on using the application and therefore choose to continue learning (Fogg, 2009). On the other hand, giving learners some feedback in form of rewards can work as intrinsic incentives by eliciting emotions in the learners like fun or ambitions that would bind them to the application (Silic & Back, 2017).

We decided to determine the amount of days played in a row after which the learners would gain new achievements based on empirical findings. In the language-learning application ‚Doulingo‘ Januakas & Kapelac (2020) found that 32 % percents of users kept their longest streak for over one month and 27% for one week. To build a habit in using an application, users usually take about 66 days (Lally et al., 2010). We also thought it would be important to use low-treshold milestones for users to get familiar with the achievement system and build up motivation from the beginning. Accordingly, the number of days we chose as milestones were 2,3,5,7,14,21,28,35,45,66 and 100 days. 

Following the self-determination theory by Deci, Koester & Ryan (2001), it is important to foster students experienced competence and autonomy for the achievements or rewards to not undermine intrinsic motivation (Friedrich et al., 2020). Therefore it was important not to only have achievements in the form of a streak, but also as a feedback on achieved learning steps to support the intrinsic motivation of the learners. Therefore we also included an achievement for the amount of words users collected inside the application and scaffolded the presentation of these as well.

## MVP Definition
We decided on two achievements as our minimum viable product for this semester, as this would give us a good starting point to evaluate the overall usefulness through user feedback. It would also allow us to create a framework with a great deal of useful functions, making it easier to extend the module with further achievements in the future.
Storing the required data proved to be relatively straightforward as well, as the database was already in place and all we had to do was update it with an additional data structure.
The two achievements that were chosen are:

- The *Code Cracker* achievement, where the user receives badges for a certain number of cracked codes. The performance on this achievement indicates the time that the user has spent in the application through the number of rounds they have completed.

- The *Streak counter* gives achievements for certain numbers of consecutive days on which the user opened the app. With this, we can track a users' consistency in returning to the app day after day. In order to make becoming invested in the streak easier, we kept the time intervals between each achievement fairly short early on and gradually increase the distance to make it harder to keep the streak alive later on.

We wanted to be able to display achievements at two points of the application:
- In a private chat, where an option exists to view all completed and remaining achievements
- In the group chat, where newly unlocked achievements will be shown after each completed task. We thought this could motivate users to invest more time in language learning, in order to complete achievements and have other people see their progress.

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

Similar to our task framework, our achievement framework is designed in a way to enable a simple and efficient extension of the set of achievements by providing a common interface that all achievements share. We again make use of Python's [abstract base classes](https://docs.python.org/3/library/abc.html) to provide the mentioned shared interface in the `Achievement` class in `achievements.py`. This has the major benefit of simplifying the addition of future achievement types. To seamlessly add and integrate a new type of achievement one would simply create a new class for the new achievement type and make it inherit from our abstract base class `Achievement`, for instance `class NewAchievementType(Achievement)`

In the next paragraphs, we will give an overview of the different components of the achievement framework, including the currently implemented types of achievements.

#### `achievements.py`
This file holds the abstract base class `Achievement` from which all subsequent achievement implementations shall inherit. The base class defines certain attributes in its constructor, which are thought to be shared across all achievement types. Those attributes include for example a name (`self.name`), a text that is displayed upon completion (`self.completion_text`), a text which displays the user progress of the achievement (`self.progress_text`), a corresponding emoji (`self.emoji`), a threshold which means completing the achievement when progressing beyond (`self.threshold`), a corresponding student (`self.student`) and a boolean value whether the achievement has been completed (`self.completed`).

Besides typical getter-functions, the class also holds an important method to check whether the achievement has been completed by the student (`is_completed()`). This function checks if the progress of the achievement has passed the `self.threshold` and sets and returns `self.completed` accordingly. This method has been implemented in the base class since we thought that all achievements usually follow what we call the "progress-needs-to-pass-threshold" principle. We will see later, that modifications can and have been made to certain achievements by overriding the `is_completed()` method.

A key method for all achievements, which depends on the inherent nature of the achievement type, is the measurement of the progress of the achievement. Since this may vary heavily depending on the kind of achievement, we declared the method `get_progress()` as an `@abstractmethod`, making it necessary to be implemented in the corresponding sub-classes inheriting from the base class.

#### `streaks.py`
This file contains the class for our _Streak_ or _Crew Rank_ achievement `ConsecutiveDaysStreak(Achievement)` which inherits from the `Achievement` base class. For this achievement, we do not need any additional attributes other than the ones we have declared in the base class. 

However, as stated above, we need to implement the abstract method `get_progress()` in each new achievement sub-class. In this case, this means getting returning the value of `consecutive_days` from the database. But since the progress of this achievement is inherently highly dynamic by its definition (the user is expected to play on consecutive days and gets penalised for breaking the streak), we cannot only perform a database query for the value of `consecutive_days` but also have to check if the streak is still running and adjust or reset the value of `consecutive_days` when necessary. We, therefore, make use of Python's [_datetime_ module](https://docs.python.org/3/library/datetime.html) to compare the current date with the `last_played` date which is saved and continuously updated in the database.

As mentioned above, the `ConsecutiveDaysStreak` class overrides also the `is_completed()` function from the base class. This is due to the fact, that the completion of this achievement is not solely dependent on the current streak, but also the highest streak ever achieved by the student. Otherwise, the student would lose all previously achieved streak achievements when forgetting to use the application just one day. Since we deemed this behaviour extremely undesired, we have overridden the respective `is_completed()` method in a way, that the completion for the `ConsecutiveDaysStreak` achievement now depends either on the current streak or the highest streak achieved by the user, whichever carries the higher value.

#### `codeword_pieces.py`
 
This file contains the class of our second achievement, the _Codeword Cracker_ achievement. The corresponding class is called `CodewordPiecesCollected(Achievement)` and again inherits from the `Achievement` base class. 

The implementation of this achievement follows the implementation of the base class to a higher extent than the streak achievement does. This means, we only had to implement the `get_progress()` method which is a simple database query for the data field `codeword_pieces_collected` of the respective student. The definition of achievement completion follows the aforementioned "progress-needs-to-pass-threshold" principle, hence, we do not need to override the `is_completed()` method.

#### `all_achievements.py`
This file contains two functions that are used in the context of achievements, namely `set_achievement_list(filepath)` and `create_all_achievements(student)`.

The former loads all achievements instances that we have currently created in the `achievement_info.json` into the global dictionary`achievement_json`. The latter, `create_all_achievements(student)`, creates a list of all achievements from `achievement_json` for the respective `student` and return the list.

## Discussion
Critical assessment of our MVP: What works, what doesn't? How would we view the achievements in retrospect -> Maybe the testing group can jump in here
Everyone -> Bulletpoints


Even though multiple researchers report achievements to be effective in fostering long-term interaction and improving user performance over a span of several weeks to several months, there is almost no evidence on the achievements‘  long term effects, and it needs to be further investigated (Zainuddin, 2020). This means we cannot be exactly sure if the initial positive effects will actually be maintained in longer term interaction and can only assume this is the case.

Furthermore, most available studies fail to establish a theoretical framework that would explain the positive effects of achievements or other gamified elements on user performance/motivation. It is also important to notice that some studies have found achievements ineffective in the first place (Zainuddin, 2020). Therefore, it might be difficult to assess the role of achievements, when improvements occur in a learning application that has achievements as one of its components. 

Another important consideration is that a vast majority of studies were tested on university students whose goals and internal motivation may, strictly speaking, substantially differ from that of school students (Zainuddin, 2020). Since our application’s target audience are teenagers, some of our theoretical considerations hold only under the assumption that experiences of students at a different education level are in line with those of school students and can be generalized to fit them too. 

The last major consideration is that achievements might be effective only at boosting extrinsic motivation, i.e. motivation to get the achievements, rather than increase the more important intrinsic motivation, i.e. the motivation to actually learn from the gamified educational resource (Mekler, 2017). For us it could mean that we motivate the students to mechanically earn the achievements rather than learn and reflect on their study experience.


## Outlook
What can be improved upon and what are next steps w.r.t. achievements e.g. visualization of user progress over time / additional achievements 
Everyone -> Bulletpoints

Other ways for long-term engagement: App updates, push notifications, social media, integrated evaluation

Self-set goals

It has been shown that fewer achievements might actually be more effective, therefore we might want to improve the existing achievements or consider other known gamification methods, rather than create multiple further achievements (Groening, 2019). A possible improvement could be to give the current achievements a more appealing visual representation, e.g. create stickers that will be sent to the user when they view their current achievements. These stickers could be sent to the group chat after successful game rounds too. This will not take too much space in the chat since we have only two achievement categories, namely, Streaks and Code Cracker achievements. If we don’t show all the levels for each achievement in retrospect, achievement display will take just two messages and might look more attractive than just a textual/emoji representation.

Another area for possible improvement is unlockable content, which is another successful means of sustaining long-term engagement. In our app it is only present in the rudimentary form of code digits sent to the user as text after task completion. As in the case of achievements, we could make the codes look better by sending a bright picture with all the unlocked code digits (this would, however, require generating these pictures) or we could simply send a new digit not as text, but use a colorful sticker. Both ways offer an easy way to make the codes that are supposed to play an important role in the game a bit more “tangible”. The existing tasks, if deeper integrated into the narrative, could solve as unlockable content too, for example the players could be required to successfully complete Task A once to unlock Task B and so on.

Moreover, since we already try to activate users‘ social comparison mechanisms via giving them “bragging rights” with their achievements being displayed after game rounds, we could work further in this direction and also display a leaderboard after each game. This could include game statistics, like how many messages/words were posted by each player as a measure of their activity, how many times they gave correct answers, how many code digits they brought to the team as a measure of their performance and how many achievements they got from this game round as a measure of their consistency. This would create space for more comparison and deeper emotional response from players that, in turn, has been reported to be associated with increased engagement and better results (Zainuddin, 2020).

To sum up, we could consider improving the existing achievements with more appealing visual representations or integrating new gamification components that will serve the purpose we originally introduced achievements for, since this may be more effective than multiplying the achievements themselves. One possibility could be adding leaderboards that display game statistics for each player and encourage them to practice more and better. However, there exist many other possibilities to further improve user engagement.





## References
Blair, L., Bowers, C., Cannon-Bowers, J., & Gonzalez-Holland, E. (2016). Understanding the Role of Achievements in Game-Based Learning. International Journal of Serious Games, 3. https://doi.org/10.17083/ijsg.v3i4.114

Deci, E. L., Koestner, R., & Ryan, R. M. (2001). Extrinsic rewards and intrinsic motivation in education: Reconsidered once again. Review of educational research, 71(1), 1-27.

Fogg, B. J. (2009). A behavior model for persuasive design. In Proceedings of the 4th international Conference on Persuasive Technology (pp. 1-7).

Friedrich, J., Becker, M., Kramer, F., Wirth, M., & Schneider, M. (2020). Incentive design and gamification for knowledge management. Journal of Business Research, 106, 341-352.

Groening, C., Binnewies, C. (2019). “Achievement unlocked!” - the impact of digital achievements as a gamification element on motivation and performance. Computers in Human Behavior, 97, 151-166. doi:10.1016/j.chb.2019.02.026

Jankunas, R., & Kapelac, E. (2020). Motivation in language learning and fitness apps.

Lally, P., Van Jaarsveld, C. H., Potts, H. W., & Wardle, J. (2010). How are habits formed: Modelling habit formation in the real world. European journal of social psychology, 40(6), 998-1009.

Mekler, E. D., Brühlmann, F., Tuch, A. N., Opwis, K. (2017). Towards understanding the effects of individual gamification elements on intrinsic motivation and performance. Computers in Human Behavior, 71, 525-534. doi:10.1016/j.chb.2015.08.048

Pham, X. L., & Chen, G. D. (2019). PACARD: A New Interface to Increase Mobile Learning App Engagement, Distributed Through App Stores. Journal of Educational Computing Research, 57(3), 618–645. https://doi.org/10.1177/0735633118756298


Silic, M., & Back, A. (2017). Impact of gamification on user's knowledge-sharing practices: Relationships between work motivation, performance expectancy and work engagement. In Proceedings of the 50th Hawaii International Conference on System Sciences.

Zainuddin, Z., Chu, S. K., Shujahat, M., &amp; Perera, C. J. (2020). The impact of gamification on learning and instruction: A systematic review of empirical evidence. Educational Research Review, 30, 100326. doi:10.1016/j.edurev.2020.100326

