---
layout: post
title: Implementation
description: Under construction!
image: pic03.jpg
---

# Overview
1. Technical Architecture
1. Telegram Integration
1. Task Framework
1. Adaptive Module
1. Database
1. References

## Technical Architecture
In this section we will provide a brief overview of the technical architecture that we relied on to build our application. All of our code is hosted in a [GitHub repository](https://github.com/ALLUOS/app) which also includes a step-by-step guide on starting the application. 

In general, we used the model-view-controller (MVC) software design pattern to structure the implementation. Figure one gives an overview of the different application elements within this MVC design pattern. It also shows where the associated source code can be found in the repository.

![Figure 1: Overview of Repository Content](https://github.com/ALLUOS/ALLUOS.github.io/blob/impl_master/assets/images/software_architecture_overview.png)*Figure 1: Overview of Repository Content*

As we decided to use the Telegram application as the front-end for our application, the view element of our MVC design pattern has been reduced to an interface with two Telegram APIs. This will be further explained in the next section.

Concerning the programming language, we relied on Python 3. This decision was made mostly due to Python being the common denominator in the programming experience of the implementation team. Given the pythonic tendencies of the Cognitive Science program, we also hope that this eases onboarding of new team members in future stages of the study project. 

We employed an object-oriented approach to programming, especially in the controller module of our application. The task framework section will elaborate more on how this approach was used for the modeling of different tasks.

A cloud-hosted PostgreSQL database serves as our data storage. PostgreSQL is a free and open-source relational database management system. For now the database is hosted on a free platform but we intend to migrate to a university server in the upcoming semester. Motivation for this change is both the limited capacity of the free hosting as well as the need for more management tools such as backups.

Generally, the chosen architecture enabled us to quickly implement first task prototypes in the initial stages of the project. However, especially during later stages we found that small changes to the task design entailed quite substantial changes to the implementation. For example, changes to the task data had to be reflected at least in the database, the functions that interact with the database, and the entity that refers to this data. For the next phase of the study project, we suggest to devote more time to fine-tuning a tasks design before greenlighting it for implementation. Also, we could think about employing a holistic model of the database entities in Python using a preexisting framework such as [SQLAlchemy](https://www.sqlalchemy.org/) to speed up adaptions to our data model. 


## Telegram Integration

### python-telegram-bot for bot-user-interactions
As we have decided to use a Telegram chatbot as our front-end and Python as programming language, we investigated different methods to manage the communication between the chatbot and the learning application. First, we explore the [Telegram Bot API]( https://core.telegram.org/bots/api), which is a HTTP-based interface for developers that want to build bots for telegram. It is developed and maintained by Telegram itself and offers all the methods that are currently available for Telegram bots. Nevertheless, we decided to not use the Telegram Bot API directly, because we do not want to do the HTTP calls and the corresponding error handling by ourself. Therefore, we decided to use a python library that wraps the Telegram Bot API. On the Telegram [Bot Code Examples]( https://core.telegram.org/bots/samples) site we found three Python wrapper: [python-telegram-bot]( https://github.com/python-telegram-bot/python-telegram-bot), [pyTelegramBotAPI]( https://github.com/eternnoir/pyTelegramBotAPI), and [AIOgram]( https://github.com/aiogram/aiogram). After a short investigation we decided to use the **python-telegram-bot** for the following reasons. First, it offers us all the elements of the Telegram Bot API and is actively maintained. Second, it implements a conversational handler that allows us to easily structure complex conversations. Third, the documentation is better than the documentation of the other wrappers. Last, some of the developers have already experience with it.

### Telethon for group handling
The downside of using a Telegram bot and the Telegram Bot API is that bots are not able to create groups or invite users to groups. Thus, we had to find a way to realize this feature. After some research we found a python package that wraps the standard [Telegram API]( https://core.telegram.org/#telegram-api), which allows the creation of groups and sending invitation to users. In order to use the Telegram API with the [**Telethon**]( https://github.com/LonamiWebs/Telethon) wrapper we needed a Telegram account to get access to the Telegram API. After setting up the Telegram account and register an application for this user, we were able to create groups and invite users to this group. 

### Conversation handlers
To structure the conversations between the bot and the users we implemented different conversation handlers. By now we have implemented four different conversation handlers that all have a different purpose. 

#### Private chat handler
The interaction between the bot and a user in a private chat is handled by the private chat handler. Its purpose is to register a user for the app, add or create the user to a group and tell the user the beginning of the story. It can also deal with some problems that may occur when creating the group. The different states and transitions are shown in the following figure.

TODO: Add figure of private chat handler

#### Room handler
The room handler purpose is to lead the users from one task to the next task. Thereby the bot checks whether enough players are currently available and which tasks they can solve next. After the user have selected a task the related task handler is called. The different states and transitions can be found in the following figure.

TODO: Add figure of room handler

#### Task handlers
To handle the users interaction with the bot during the different tasks, i.e. the sentence correction task and the vocabulary guessing task, we created a task handler per task. Both task handlers lead the users in a similar way through the conversation. First, they send a message to one of the active users containing the task they have to perform. Then, the handlers wait for the response of either a group member or the selected user. After receiving an answer, it is evaluated. Dependent on the outcome the user can correct his answer, move on to the next subtask, or if the whole task is completed the next user is selected. This procedure is continued until the task is fully finished. After finishing the complete task, the control is returned to the room handler.

TODO: Add figures of sentence correction and vocabulary guessing handler


## Task Framework
This section aims to explain the implementation decisions and to describe the resulting components that have been implemented for the application’s task framework. In the next sections, we will cover the role and necessity of a `RoomManager`, as well as the structure and idea of different task classes, especially the `SequentialTask`. Lastly, this section is concluded with an outlook subsection, which discusses the current limitations of and possible additions to the task framework.

It should be noted, that this part of the documentation solely documents the tasks of the application from a technical point of view. For a more detailed, conceptual description of the tasks, please refer to the Design chapter.

### RoomManager
The `RoomManager` is thought of as a host of the group of players. Since we decided on having some instance that controls the general task and game flow, we created the `RoomManager` class. Each game session gets assigned exactly one `RoomManager`
instance, that handles the session. An object of the `RoomManager` class captures important meta information of the group of players, such as the number and names of active players (stored in `self._full_student_list`), the minimum amount of time after starting the bot (`self._min_time_for_students_to_join_in_sec`)  and the minimum number of players needed to start the game (‘self._min_number_of_students’), as well as a check, if all active participating students have given their consent to start the game (`can_execution_start()`). 

### Tasks
During the design phase of our application, we were able to pin down a few requirements for the technical implementation of the tasks for our prototype. Amongst those main requirements have been:


1. the enablement of collaborative task solving
1. the ensuring of participation of each player
1. an adaptive module that controls for user-specific task difficulty
1. the possession of a winning condition

Since we decided that we will aim for one to two different tasks as the first milestone of the project, we chose to create an [abstract base class](https://docs.python.org/3/library/abc.html) `Task` for the tasks, which then can be further expanded depending on the type of task. The abstract base class `Task(ABC)` defines through abstract methods which properties every task to follow should inherit, such as getting the instructions for the respective task(`get_task_instructions()`).
To capture the requirements 1-4 defined above, we implemented a subclass that inherits from `Task(ABC)`, namely the `SequentialTask(Task)`. 

#### SequentialTask
`SequentialTask(Task)` is a subclass of `Task(ABC)` which in turn is a superclass of various tasks, that all follow a sequential manner. As stated above, to ensure requirements 1) and 2), we created a sequential task scaffolding. This means, that each task consists of multiple rounds, in which each player takes once the active role of solving the main task, while the other players take a slightly more passive role, e.g. by assisting the active player to solve the task. Each `SequentialTask` has properties such as `self.all_users`, `self.remaining_users` and `self.selected_user`. Those properties enable the sequential handling of a task, in which each player gets the active role at least once.

After having successfully completed a round, the group gets awarded with one piece of the codeword `self.code`. This codeword consists of a series of random digits, which size corresponds to the number of players. This covers the winning condition in requirement 4). Additionally, after completing the whole task, the corresponding user proficiencies and the group’s proficiencies get updated according to the performance with the call of the function `update_proficiencies(correct)`.

For the implementation of requirement 3), the adaptive module, please refer to the section Adaptive Module below.   

##### SentenceCorrection
The `SentenceCorrection` class inherits from its superclass `SequentialTask`, in order to have all the sequential features. Since this class also has the property `self.selected_user`, it gets easy to implement the turn-taking for this task. The function `select_sentence()` selects a sentence from the corpus, based on the selected user’s proficiency `self.selected_user.get_grammar_proficiency()`. When the sentence gets send to the group, only answers of the selected user will be evaluated. This allows the other players to contribute by assisting the selected user, e.g. by giving hints to identify the error.

##### VocabularyDescription
Similar to the `SentenceCorrection` class, he `VocabularyDescription` class also inherits from `SequentialTask` and selects the word to be described for the selected user based on their proficiency with the call of `select_word()`.

In contrast to the task above, now answers will only be evaluated by non-selected users. However, to prevent cheating, if the `self.selected_user` mentions the word itself in their description, they will receive a warning through the call of `get_word_warning()`.

Additionally, to increase difficulty, we implemented a time constraint for this task. Upon start, `set_time_reminders()` will initiate the reminders, that will be sent to the group regularly by the call of `get_time_info()`.


### Outlook
With our implementation of the `SequentialTask`, we set a very flexible code foundation for additional tasks, that are designed in the same manner. However, having to fit new tasks in this pattern might limit us in the conceptual designing of new tasks. To create more flexibility in the addition of new tasks and to broaden the scope of task patterns, we are going to try to implement another kind of task in the next semester, which not necessarily follows the sequential pattern.

Additionally, we will be working on embedding the tasks more into the escape room scenario. Thus far, due to the lack of alternatives, we are stuck with two tasks which can be repeated as desired until the codeword is reached. After reaching and successfully entering the correct codeword, we return to the task selection screen, which initiates a new round. Thus, we will also work on an overall win-condition, which completes the escape room scenario. 


## Adaptive Module
Our implementation of the adaptive module of the language learning application relies on a model of the user’s proficiency. In this section we will briefly explain the model and then dive deeper into the implementation of the model, updating the user proficiency, and the adaptive selection of the next task iteration. Finally, we will reflect on drawbacks of adaptive modeling approach that we have taken.

### Language Proficiency Model
The hierarchical model contains two language domains on the first level that reflect the main set of skills: Grammar and vocabulary. The sentence correction tasks focuses on grammar skills whilst the vocabulary guessing task aims to augment the users vocabulary. On the second level, the domains are subdivided into more granular single skills e.g. relative clauses or gerunds in the grammar domain. 

For each subskill there are multiple task variations of different difficulties on a three-point scale (low – medium – high). For more information on the structure of this data, see the database section.

We implemented the hierarchical structure by creating an [enum](https://docs.python.org/3/library/enum.html) for each domain that contains all sub-skills. In Python, an integer value can be assigned to each symbolic name. We chose the integers to be consistent with the values we use to specify the sub-skills in the database. This allowed us to easily retrieve task data from the database and convert the sub-skill to a member of the enum by simply calling the enum with the ID (e.g. `Grammar(2)` would resolve to the `Grammar.TENSE` enum member).

In order to adaptively navigate a task, we also need to store and change the proficiency of the users engaged in that task. Thus, each user object that we use in the backend contains an instance of the `Proficiency` class. This class mainly consists of two dictionaries for the language domains, where each member of the corresponding enum is used as a key and the users proficiency for that particular subskill is stored as the value. Additionally, the average domain proficiency is also stored as a numerical user variable for performance sake. Singular and average values range from one to ten with the former being the lowest proficiency and the latter the highest. For proficiency storage over multiple sessions, please refer to the database section.

### Proficiency Updates
When a group finishes a single task iteration, the proficiency of all users in the group is updated. The update is handled by the `update_proficiencies` function in the `Proficiency` class. This function takes a list of sub-skill enum members and updates the corresponding value based on a Boolean that indicates a correct or false answer to the task as well as another Boolean declaring a passive or active update. For passive group members we want softer updates compared to the selected active user. Thus, we use a step size of 0.25 for passive and 1.0 for active updates. In case of a correct answer, the step size is added to the proficiency and it is subtracted for an incorrect answer. We limit the result to be within our predefined range of one to ten. If a sub-skill had no value previously as the user has not yet participated in a task relevant to that sub-skill, an initial estimate of the proficiency is made based on the answer to the task. If it was correct, we estimate 7.5 as the proficiency An incorrect response yields 2.5 as the initial proficiency estimate. After all sub-skill proficiencies have been updated, the domain average are recalculated and stored in their respective variable. 

### Task Selection
For the adaptive selection of the next task iteration, two properties of the tasks have to be considered: Language sub-skill and difficulty. As we have no previous knowledge of the user’s sub-skill proficiencies, we made the choice to always prioritize tasks for unknown sub-skill proficiencies of the selected user. We randomly draw one sub-skill from a list of all keys that are without a value in the proficiency dictionary. The difficulty is always set to medium in this case. This enables us to create initial estimates of as outlined in the proficiency update section above.

If all sub-skill proficiencies in the task’s domain are known for the active user, we randomly chose a sub-skill based on probability inverse to the corresponding proficiency. Thus, sub-skills that the user already excels in are selected less frequently then those he or she might still need more practice with. In our implementation, we compute a draw probability based on the proficiency by subtracting the squared proficiency from 110 for each sub-skill. These probabilities are then re-normalized and used in the `random.choice` function of the [NumPy module](https://numpy.org/). 

Finally, the difficulty for the selected sub-skill is based on the user proficiency of that sub-skill and the domain average of that user. For this, we compute a weighted mean between the sub-skill proficiency and the domain average. Currently, we place more emphasis on the sub-skill proficiency by weighting the average with 0.5. Task difficulty is then chosen based on this mean proficiency. A proficiency of one to three results in a low, four to seven in a medium, and eight to ten in a high difficulty.

A random sentence or word with the selected sub-skill and difficulty is then loaded from the database.

### Evaluation
Although we put a lot of thought into the design and implementation of the adaptive module, there is no substantiated scientific model to back it up. While studies have shown that adaptive difficulty adjustments positively impact learning (Nebel et al., 2020; Sampayo-Vargas et al., 2013), the target difficulty has to strike the balance between being easy enough to motivate and – in the context of educational games – hard enough to optimize learning outcomes. We find it unlikely that our current adaptive module strikes that balances for a number of reasons: Task difficulty is only represented on three levels that have been assigned based on intuitions. The difficulty level of the current question does not affect the update strength. We did not evaluate the perceived difficulty of tasks from users in our target group and we did not relate this to our proficiency estimates or learning outcomes. 

With our current model, we also put more emphasis on language skills that are not strongly developed yet instead of fortifying existing strengths. While this serves the implicit educational goal of a good overall language knowledge, we should reassess of this is in fact the aim of our application.

Therefore, we propose to devote more resources towards the adaptive model in the second semester of this study project. For the evaluation of the current model we would ideally need objective measures for the actual language competency of our users in order to compare those to our proficiency estimates. Moving forward from there, we should also track the user performance more closely depending on the sub-skill and difficulty. This data could provide very useful in the later fine-tuning of the adaptive difficulty.
References

## Database
### Introduction
The way the bot is designed we had to store data so it can be queried as and when possible. The bot also deals with user data and other essential data from telegram that we had to store, update and/or retrieve in real time. We realized that the data we have is relational in nature which means that we had pre-defined relationships in our data. For each table we have, every column represented a certain kind of data and the field in the column stored the value. Every row was a collection of data from all the columns for one datapoint / object / entity.  For example, 

![**Figure 4.1: Sentence Correction Data**](https://github.com/ALLUOS/ALLUOS.github.io/blob/impl_master/assets/images/sentence_data.png)

### PostgreSQL and Features
We stored this data in [PostgreSQL]( https://www.postgresql.org/). Its is an open source relational management database. It is compatible with a wide range of programming language and futuristic it is easy to migrate between operating systems. It can handle multiple queries at a time. PostgreSQL adheres with the SQL standards better than other [SQL databases]( https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems). 
SQL databases allows you to assign each column with a data type which indicates which kind of data can be stored in each column. PostgreSQL allows to have several data types for example, (referring to data types used in sentence correction data shown in Figure 4.1) 



| Sentence Correction Column | Data Type | Constraints |
| --- | --- | --- |
|id |SERIAL |PRIMARY KEY|
|sub_type | SMALLINT | NOT NULL| 
| difficulty_level | SMALLINT | NOT NULL |
| sentence_corpus | TEXT | NOT NULL |
| correct_answers | TEXT ARRAY | NOT NULL|
|error_words | TEXT ARRAY | NOT NULL|


The Table above mentions constraints which limits the values that can be entered. Constraints are generally column specific but some can be for the whole table. 
Example: `PRIMARY KEY` – It makes sure that there is no value in the column is empty or NULL and each entry is unique. Primary key in itself is made of two constraints `NOT NULL` and `UNIQUE` 

`Array` represents that the column can have more than one value present. 

### Tables Created for the project 



|Table Names | Table Description | Columns in each table |
| --- | --- | --- |
|group_table |Data of every group for tasks| id, chat_id, invitation_url, invitation_code, current_task_id |
|student_group_table |Connecting Data between student and group|student_id, group_id |
|sentence_correction_data |Raw Data of the Sentence Correction Task|id,sub_type,difficultt_level, sentence_corpus, correct_answer, error_words|
|vocabulary_guessing_task | Raw data of the Vocabulary Guessing Task|id, sub_type, word, difficulty|
|student_proficiency_table |Mapping of each user and their proficiency | student_id, proficiency_id, value|
|proficiency_table |This table has the mapping of the proficiency levels and ids| proficiency_domain, proficiency_id, proficiency_name|
|student_table |Data about each student using the bot|id, telegam_name, name|
|task_table |Every Task Data| id, name, min_num_of_players, num_of_iteration|

### Database Operators for Entities
#### Different Examples of Querying to Extract Data
1)	Querying data from a table: "SELECT * FROM sentence_correction_data;” 
2)	Filtered Query based on certain columns. 
Meaning if you want to extract data based on a certain value from a specific column.: "SELECT * FROM student_proficiency WHERE student_id = '%s';"
3)	Limited Query. 
Limiting your output to a specific number of outputs: "SELECT * FROM sentence_correction_data WHERE sub_type = %s LIMIT 1;"
**Similar queries functions are used while inserting data in specific tables. We just use `Insert` instead of `Select`


Following are a few important functions used to query data from the database. Most of the function names are self-explanatory. 

|Function Name| Purpose of the Function|
| --- | --- |
| get_sentence_information_from_df| Parses the information from the database into sentence corpus, one error word and one correct answer|
| get_random_sentence_based_on_sub_type_and_difficulty| Queries the data based on sub_type and difficulty_level and returns one sentence|
| insert_proficiency_data| Inserts data in the proficiency table|
| get_student| Queries students data based on their telegram_name and returns their id, telegram name and name|
| rows_to_proficiency| Parses rows from database into Proficiency object|
| get_student_proficiency| Returns a proficiency object that is initialized with the values from the database|
| get_all_tasks_for_number_of_participants | Returns a list of all tasks that can be started with the number of participants|

### Bot and the Database Interaction
#### Database Connection Class
Step 1: We set the database configuration we would like to use.

Step 2: In order to enable a connection to the database we store the necessary information in a dictionary

Step 3: Create a database configuration that does not use the options just in case.

Step 4: We create a function to form a new connection to the database.

Step 5: We create another function in order to Close the given connection. 

One more function is create the parse the data once the connection is open. The data we query for any table is converted into a dataframe for further use. The function to extract a dataframe is `read_query_into_df`

#### Classes for Objects

We created classes for objects and created functions in them to get certain values from those objects. This allows to extract a certain value from the entire object rather than extracting all relational values of the entity. 

|Class Name| Class Description| Arguments to extract values |
| --- | --- | --- |
| Group| Initializes a new group entity |  Groups id, chat_id, invitation_url, invitation_code| 
|Sentence| Initiates the extraction of a sentence for the task| get_sentence, get all words, get error words, get proficiency sub level, check if the sentence is correct or not | 
|Student | Initiates the extraction of information of the user | Users telegram id, internal user id, name, proficiency level, average of various proficiency, updating function for all proficiency |
|Task | Represents a task entity from the database| Tasks id, name, minimum number of users for the task, number of iterations | 
|Word | Mapping of the word to proficiency| Get word and proficiency_sub_type|

#### Mapping Tasks to Database Tables and Entities

#### Sentence Correction Task

This task deals with the sentence correction data, task table and student proficiency data. Sentence Correction data to access the `get_random_sentence_based_on_sub_type_and_difficulty` function which allows the code to get a random sentence to use in the task. Task table to get the name of the task and student proficiency to update the change in proficiency after the task. It uses the sentence and subject class from the entities. 

#### Vocabulary Guessing

This task deals with the vocab guessing table, student proficiency  and task table. The function of task and student proficiency is the same as of the sentence correction task. The vocab guessing table is use to get a random word based on the difficulty level and sub type. Word and student class is used from the entities. 

## References
Nebel, S., Beege, M., Schneider, S., & Rey, G. D. (2020). Competitive Agents and Adaptive Difficulty Within Educational Video Games. Frontiers in Education, 5. https://doi.org/10.3389/feduc.2020.00129

Sampayo-Vargas, S., Cope, C. J., He, Z., & Byrne, G. J. (2013). The effectiveness of adaptive difficulty adjustments on students’ motivation and learning in an educational computer game. Computers & Education, 69, 452–462. https://doi.org/10.1016/j.compedu.2013.07.004
