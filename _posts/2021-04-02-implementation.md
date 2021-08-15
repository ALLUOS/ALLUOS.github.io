---
layout: post
title: Implementation
sections:
 - title: Technical Architecture
   tag: \#architecture
 - title: Telegram
   tag: \#telegram
 - title: Task Framework
   tag: \#framework
 - title: Adaptive Module
   tag: \#adaptive
 - title: Database
   tag: \#database
 - title: References
   tag: \#references
description: Learn more about the application's implementation and technical architecture.
image: pic03.jpg
---
## Contents
1. Technical Architecture
1. Telegram Integration
1. Task Framework
1. Adaptive Module
1. Database
1. References


<div id="architecture"></div>
## Technical Architecture

In this section we will provide a brief overview of the technical architecture that we relied on to build our application. All of our code is hosted in a GitHub repository which also includes a step-by-step guide on starting the application.

In general, we used the model-view-controller (MVC) software design pattern to structure the implementation. Figure one gives an overview of the different application elements within this MVC design pattern. It also shows where the associated source code can be found in the repository.

<img src="https://github.com/ALLUOS/ALLUOS.github.io/raw/master/assets/images/software_architecture_overview.png" alt="Figure 1: Overview of Repository Content" class="center">

*Figure 1: Overview of Repository Content*

As we decided to use the Telegram application as the front-end for our application, the view element of our MVC design pattern has been reduced to an interface with two Telegram APIs. This will be further explained in the section _Telegram Integration_.

Concerning the programming language, we relied on Python 3. This decision was made mostly due to Python being the common denominator in the programming experience of the implementation team. Given the pythonic tendencies of the Cognitive Science program, we also hope that this eases onboarding of new team members in future stages of the study project.

We employed an object-oriented approach to programming, especially in the controller module of our application. The *Task Framework* section will elaborate more on how this approach was used for the modeling of different tasks.

A cloud-hosted PostgreSQL database serves as our data storage. PostgreSQL is a free and open-source relational database management system. For now the database is hosted on a free platform but we intend to migrate to a university server in the upcoming semester. Motivation for this change is both the limited capacity of the free hosting as well as the need for more management tools such as backups.

Generally, the chosen architecture enabled us to quickly implement first task prototypes in the initial stages of the project. However, especially during later stages we found that small changes to the task design entailed quite substantial changes to the implementation. For example, changes to the task data had to be reflected at least in the database, the functions that interact with the database, and the entity that refers to this data. For the next phase of the study project, we suggest to devote more time to fine-tuning a task's design before greenlighting it for implementation. Also, we could think about employing a holistic model of the database entities in Python using a preexisting framework such as [SQLAlchemy](https://www.sqlalchemy.org/) to speed up adaptions to our data model.


<div id="telegram"></div>
## Telegram Integration

This section describes the technical components of the Telegram front-end integration, including relevant handlers required to realize the application's intended functionality.

### Python-telegram-bot for Bot-user-interactions
As we decided to use a Telegram chatbot as our front-end and Python as programming language, we investigated different methods to manage the communication between the chatbot and the learning application. First, we explored the [Telegram Bot API]( https://core.telegram.org/bots/api), which is a HTTP-based interface for developers that want to build bots for telegram. It is developed and maintained by Telegram itself and offers all the methods that are currently available for Telegram bots. Nevertheless, we decided not to use the Telegram Bot API directly, because we do not want to do the HTTP calls and the corresponding error handling ourselves. Therefore, we decided to use a python library that wraps the Telegram Bot API. On the Telegram [Bot Code Examples]( https://core.telegram.org/bots/samples) site we found three Python wrapper: [python-telegram-bot]( https://github.com/python-telegram-bot/python-telegram-bot), [pyTelegramBotAPI]( https://github.com/eternnoir/pyTelegramBotAPI), and [AIOgram]( https://github.com/aiogram/aiogram). After a short investigation we decided to use the **python-telegram-bot** for the following reasons. First, it offers us all the elements of the Telegram Bot API and is actively maintained. Second, it implements a conversational handler that allows us to easily structure complex conversations. Third, the documentation is better than the documentation of the other wrappers. Last, some of the developers had already experience with it.

### Telethon for Group Handling
The downside of using a Telegram bot and the Telegram Bot API is that bots are not able to create groups or invite users to groups. Thus, we had to find a way to realize this feature. After some research we found a python package that wraps the standard [Telegram API]( https://core.telegram.org/#telegram-api), which allows the creation of groups and sending invitation to users. In order to use the Telegram API with the [Telethon]( https://github.com/LonamiWebs/Telethon) wrapper we needed a Telegram account to get access to the Telegram API. After setting up the Telegram account and register an application for this user, we were able to create groups and invite users to this group.

### Conversation Handlers
To structure the conversations between the bot and the users we implemented different conversation handlers. A conversation handler consists of four different collections of other handlers, e.g. message handlers and command handlers. The **entry points** list is used to instantiate the conversation and normally contains a command handler that reacts to commands like */start* sent as a message to the bot. When a user sends a message to the bot, the conversation handler tries to find a matching handler within the list of entry points. A handler matches when the message passes the specified filter of the handler. If a matching handler is found, the callback function of that handler is executed that reacts to the message of the user. Thereby the function returns a value, which determines the state of the conversation handler. The second collection is a **states** dictionary that maps the different states of a conversation to a list of handlers. If a message is sent to the bot while the conversation is already instantiated, the conversation handler looks for a matching handler in the list of handlers associated to the current state it is in. If no matching handler is found, the conversation handler searches for a matching handler in the **fallbacks** list, which is the third collection. The last collection is a dictionary called **map_to_parent**. It is used in nested conversation handlers to map a specific state of the nested conversation handler to a state of the parent conversation handler. This allows the transition from the nested handler back to the parent handler. To date we have implemented four different conversation handlers that all have a different purpose.

#### Private Chat Handler
The interaction between the bot and a user in a private chat is handled by the private chat handler. Its purpose is to register a user for the app, add the user to a group or create a group, and tell the user the beginning of the story. It can also deal with some problems that may occur when creating the group. The handler is started with the */start*-command sent to the bot in a private chat. Afterwards the user is guided through the conversation by the use of reply keyboards sent to the user. Figure 2 shows the general conversation flow of the private chat handler. For simplicity, error handling is not covered in the figure.

<img src="https://github.com/ALLUOS/ALLUOS.github.io/raw/master/assets/images/PrivateChatInteraction.png" alt="Figure 2: Conversation flow of the private chat handler including backend activities" class="center">

*Figure 2: Conversation flow of the private chat handler including backend activities*

#### Room Handler
The room handlers’ purpose is to lead the users from one task to the next task. This handler is also started with the */start* command but only if the command is sent in a group chat. After starting the bot checks whether enough players are currently available and which tasks they can solve next. After the users have selected a task the related nested task handler is called.

#### Task Handlers
To handle the users interaction with the bot during the different tasks, i.e. the sentence correction task and the vocabulary guessing task, we created a task handler for each task. Both task handlers lead the users in a similar way through the conversation. First, they send a message to one of the active users containing the task they have to perform. Then, the handlers wait for the response of either a group member or the selected user. The answer is evaluated upon reception. Depending on the outcome, the user can correct his answer, move on to the next subtask, or, if the task is completed, the next user is selected. This procedure is continued until the task is fully finished. After finishing the complete task, control is returned to the room handler.

The only way a task handler can be entered is through the room handler. In order to ensure that a task handler is not entered with an insufficient number of users we developed a custom filter to prevent this.


<div id="framework"></div>
## Task Framework
This section aims to explain the implementation decisions and to describe the resulting components that have been implemented for the application’s task framework. In the next sections, we will cover the role and necessity of a `RoomManager`, as well as the structure and idea of different task classes, especially the `SequentialTask`. Lastly, this section concludes with an outlook subsection, which discusses the current limitations of and possible additions to the task framework.

It should be noted, that this part of the documentation solely documents the tasks of the application from a technical point of view. For a more detailed, conceptual description of the tasks, please refer to the [Design](https://alluos.github.io/2020/05/03/design.html) section.

### RoomManager
The `RoomManager` is thought of as a host for the group of players. Since we decided on having some instance that controls the general task and game flow, we created the `RoomManager` class. Each game session gets assigned exactly one `RoomManager`
instance, that handles the session. An object of the `RoomManager` class captures important meta information about the group of players, such as the number and names of active players (stored in `self._full_student_list`), the minimum amount of time after starting the bot (`self._min_time_for_students_to_join_in_sec`) , and the minimum number of players needed to start the game (`self._min_number_of_students`), as well as a check, if all actively participating students have given their consent to start the game (`can_execution_start()`).

### Tasks
During the design phase of our application, we were able to pin down a few requirements for the technical implementation of the tasks for our prototype. These main requirements include:


1. The enablement of collaborative task solving
1. The ensuring of participation of each player
1. An adaptive module that controls for user-specific task difficulty
1. A winning condition

Since we decided to aim for one to two different tasks as the first milestone of the project, we chose to create an [abstract base class](https://docs.python.org/3/library/abc.html) `Task` for the tasks, which then can be further expanded depending on the type of task. The abstract base class `Task(ABC)` defines through abstract methods which minimal properties and functions every task to follow should inherit, such as getting the instructions for the respective task (`get_task_instructions()`). To capture the requirements 1-4 defined above, we implemented a subclass that inherits from `Task(ABC)`, namely the `SequentialTask(Task)`.

#### SequentialTask
`SequentialTask(Task)` is a subclass of `Task(ABC)` which in turn is a superclass of various tasks, that all follow a sequential manner. As stated above, to ensure requirements 1) and 2), we created a sequential task scaffolding. This means, that each task consists of multiple rounds, in which each player takes the active role of solving the main task exactly once, during which time the other players take a slightly more passive role, e.g. by assisting the active player to solve the task. Each `SequentialTask` has properties such as `self.all_users`, `self.remaining_users` and `self.selected_user`. Those properties enable the sequential handling of a task, in which each player gets the active role at least once.

After having successfully completed a round, the group gets awarded with one piece of the codeword `self.code`. This codeword consists of a series of random digits, which size corresponds to the number of players. This covers the winning condition in requirement 4). Additionally, after completing the whole task, the corresponding user proficiencies and the group’s proficiencies get updated according to the performance with the call of the function `update_proficiencies(correct)`.

For the implementation of requirement 3), the adaptive module, please refer to the section _Adaptive Module_ below.

##### SentenceCorrection
The `SentenceCorrection` class inherits from its superclass `SequentialTask`, in order to have all the sequential features. Since this class also has the property `self.selected_user`, it is trivial to implement turn-taking for this task. The function `select_sentence()` selects a sentence from the corpus, based on the selected user’s proficiency `self.selected_user.get_grammar_proficiency()`. When the sentence gets send to the group, only answers of the selected user will be evaluated. This allows the other players to contribute by assisting the selected user, e.g. by giving hints to identify the error.

##### VocabularyDescription
Similar to the `SentenceCorrection` class, the `VocabularyDescription` class also inherits from `SequentialTask` and selects the word to be described for the selected user based on their proficiency with the call of `select_word()`.

In contrast to the task above, now answers will only be evaluated by non-selected users. However, to prevent cheating, if the `self.selected_user` mentions the word itself in their description, they will receive a warning through the call of `get_word_warning()`.

Additionally, to increase difficulty, we implemented a time constraint for this task. Upon start, `set_time_reminders()` will initiate the reminders, that will be sent to the group regularly by the call of `get_time_info()`.

##### DiscussionTask

`Discussion(Task)` is a subclass of `Task(ABC)`, however, unlike the other two tasks, the discussion task is not sequential in nature and therefore does not inherit from `SequentialTask`. Nevertheless, we were still able to reuse a significant part of the methods already implemented in said class.
The task always starts off with the presentation of the text, which is retrieved from the database based on the users' proficiency. This is followed by three rounds of discussion.
At the start of each round a text-related question is sent and a timer is added to the job queue to determine when the round finishes, at which point a warning is sent, reminding users' to finish their discussion of the question.

Since we don't have to select for a specific user, all user input can be handled in the same `handle_question` state. Word counts and participation for each user are stored in two dictionaries, which are required to satisfy the completion criteria of the task.
After a message is sent, `update_word_counts(message, user)` is called in order to update those dictionaries with the new word counts.

Following the third and final discussion round, three polls will be presented using reply keyboards and used along with the dictionaries by the `is_correct(self)` method to determine whether the task was successfully completed or not. Figure 3 shows a flow chart of the task up until the point where the users' have to input the code and choose the subsequent task.

<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/semester_two/assets/images/DiscussionFlowchart.png" alt="Figure 3: Flow of the discussion task including backend activities" class="center">

*Figure 3: Flow of the discussion task including backend activities*


### Outlook
With our implementation of the `SequentialTask`, we set a very flexible code foundation for additional tasks, that are designed in the same manner. However, having to fit new tasks in this pattern might limit us in the conceptual designing of new tasks. To create more flexibility in the addition of new tasks and to broaden the scope of task patterns, we are going to try to implement another kind of task in the next semester, which not necessarily follows the sequential pattern.

Additionally, we will be working on embedding the tasks more into the escape room scenario. Thus far, due to the lack of alternatives, we are stuck with two tasks which can be repeated as desired until the codeword is reached. After reaching and successfully entering the correct codeword, we return to the task selection screen, which initiates a new round. Thus, we will also work on an overall win-condition, which completes the escape room scenario.


<div id="adaptive"></div>
## Adaptive Module

Our implementation of the adaptive module of the language learning application relies on a model of the user’s proficiency. In this section we will briefly explain the model and then dive deeper into the implementation of the model, updating user proficiency, and the adaptive selection of the next task iteration. Finally, we reflect on drawbacks of adaptive modeling approach that we have taken.

### Language Proficiency Model
The hierarchical model contains two language domains on the first level that reflect the main set of skills: Grammar and vocabulary (cf. Figure 3). The sentence correction tasks focuses on grammar skills whilst the vocabulary guessing task aims to augment the users' vocabulary. On the second level, the domains are subdivided into more granular sub-skills, e.g. relative clauses or gerunds in the grammar domain.

<img src="https://github.com/ALLUOS/ALLUOS.github.io/raw/master/assets/images/difficulty-model.png" alt="Figure 3: Graph visualization of the hierarchical difficulty model." class="center">

*Figure 3: Graph visualization of the hierarchical difficulty model.*

For each subskill there are multiple task variations of different difficulties on a three-point scale (low – medium – high). For more information on the structure of this data, see the database section.

We implemented the hierarchical structure by creating an [enum](https://docs.python.org/3/library/enum.html) for each domain that contains all sub-skills. In Python, an integer value can be assigned to each symbolic name. We chose the integers to be consistent with the values we use to specify the sub-skills in the database. This allowed us to easily retrieve task data from the database and convert the sub-skill to a member of the enum by simply calling the enum with the ID (e.g. `Grammar(2)` would resolve to the `Grammar.TENSE` enum member).

In order to adaptively navigate a task, we also need to store and change the proficiency of the users engaged in that task. Thus, each user object that we use in the backend contains an instance of the `Proficiency` class. This class mainly consists of two dictionaries for the language domains, where each member of the corresponding enum is used as a key and the user's proficiency for that particular subskill is stored as the value. Additionally, the average domain proficiency is also stored as a numerical user variable for performance sake. Singular and average values range from one to ten with the former being the lowest proficiency and the latter the highest. For proficiency storage over multiple sessions, please refer to the _Database_ section.

### Proficiency Updates
When a group finishes a single task iteration, the proficiency of all users in the group is updated. The update is handled by the `update_proficiencies` function in the `Proficiency` class. This function takes a list of sub-skill enum members and updates the corresponding value based on a Boolean that indicates a correct or false answer to the task as well as another Boolean declaring a passive or active update. For passive group members we want softer updates compared to the selected active user. Thus, we use a step size of 0.25 for passive and 1.0 for active updates. In case of a correct answer, the step size is added to the proficiency and it is subtracted for an incorrect answer. We limit the result to be within our predefined range of one to ten. If a sub-skill had no value previously as the user has not yet participated in a task relevant to that sub-skill, an initial estimate of the proficiency is made based on the answer to the task. If it was correct, we estimate 7.5 as the proficiency. An incorrect response yields 2.5 as the initial proficiency estimate. After all sub-skill proficiencies have been updated, the domain averages are recalculated and stored in their respective variable.

### Task Selection
For the adaptive selection of the next task iteration, two properties of the tasks have to be considered: Language sub-skill and difficulty. As we have no previous knowledge of the user’s sub-skill proficiencies, we made the choice to always prioritize tasks for unknown sub-skill proficiencies of the selected user. We randomly draw one sub-skill from a list of all keys that are without a value in the proficiency dictionary. The difficulty is always set to medium in this case. This enables us to create initial estimates of new values, as outlined in the proficiency update section above.

If all sub-skill proficiencies in the task’s domain are known for the active user, we randomly chose a sub-skill based on probability inverse to the corresponding proficiency. Thus, sub-skills that the user already excels in are selected less frequently than those the user might still need more practice with. In our implementation, we compute a draw probability based on the proficiency by subtracting the squared proficiency from 110 for each sub-skill. These probabilities are then re-normalized and used in the `random.choice` function of the [NumPy module](https://numpy.org/).

Finally, the difficulty for the selected sub-skill is based on the user proficiency of that sub-skill and the domain average of that user. For this, we compute a weighted mean between the sub-skill proficiency and the domain average. Currently, we place more emphasis on the sub-skill proficiency by weighting the average with 0.5. Task difficulty is then chosen based on this mean proficiency. A proficiency of one to three results in a low, four to seven in a medium, and eight to ten in a high difficulty. A random sentence or word with the selected sub-skill and difficulty is then loaded from the database.

### Evaluation
Although we put a lot of thought into the design and implementation of the adaptive module, there is no substantiated scientific model to back it up. While studies have shown that adaptive difficulty adjustments positively impact learning (Nebel et al., 2020; Sampayo-Vargas et al., 2013), the target difficulty has to strike the balance between being easy enough to motivate and – in the context of educational games – hard enough to optimize learning outcomes. We find it unlikely that our current adaptive module strikes that balances for a number of reasons: Task difficulty is only represented on three levels that have been assigned based on intuitions. The difficulty level of the current question does not affect the update strength. We did not evaluate the perceived difficulty of tasks from users in our target group and we did not relate this to our proficiency estimates or learning outcomes.

With our current model, we also put more emphasis on language skills that are not strongly developed yet instead of fortifying existing strengths. While this serves the implicit educational goal of a good overall language knowledge, we should reassess if this is in fact the aim of our application.

Therefore, we propose to devote more resources towards the adaptive model in the second semester of this study project. For the evaluation of the current model we would ideally need objective measures for the actual language competency of our users in order to compare those to our proficiency estimates. Moving forward from there, we should also track the user performance more closely depending on the sub-skill and difficulty. This data could prove very useful in the later fine-tuning of the adaptive difficulty.


<div id="database"></div>
## Database

### Introduction
Given the implementation design, it is necessary to store data for efficient retrieval during application runtime. The bot deals with user data and other essential data from Telegram that we had to store, update, and retrieve in real time. As the data is relational in nature, we structured these relationships in a backend PostgreSQL database. Each table in the database contains records and attributes which may be visualized as rows and columns (cf. Figure 4).

<img src="https://github.com/ALLUOS/ALLUOS.github.io/raw/master/assets/images/sentence_data.png" alt="Figure 4: Sentence Correction Data" class="center">

*Figure 4: Sample database table entry from the sentence correction task table.*

### SQL Features
SQL databases allows you to assign each column with a data type which indicates which kind of data can be stored in each column. PostgreSQL permits several data types, each with their own constraints. Table 1 summarizes the data types and constraints of the attributes represented in the aforementioned sentence correction database table.


| Column Name | Data Type | Constraints |
| :-- | :-- | :-- |
|id |SERIAL |PRIMARY KEY|
|sub_type | SMALLINT | NOT NULL|
|difficulty_level | SMALLINT | NOT NULL |
|sentence_corpus | TEXT | NOT NULL |
|correct_answers | TEXT ARRAY | NOT NULL|
|error_words | TEXT ARRAY | NOT NULL|

*Table 1: Data types of the sentence correction data.*

Table 1 mentions the constraints which limit the values that can be entered for a given attribute. Constraints are generally column specific, but some can be for the whole table. Example: `PRIMARY KEY` – Ensures that the values in the cells of that column are not empty or NULL and each entry is unique. Primary key is made of two constraints `NOT NULL` and `UNIQUE`.

`Array` represents that the column can have more than one value present. For columns like `correct_answers` when we have more than one possible correct answer for a given sentence, we would like to store all the possible correct answer in that one cell. Example: If the sentence is "Do you want they to help you?" the correct answer is *them*, but there are other correct answers which we would like to store as well.

### Tables Created for the Project

Table 2 summarizes the database tables of the backend PostgreSQL database.

|Table Name | Table Description | Table Columns |Entities|
| :-- | :-- | :-- | :-- |
|group_table |Data of every group for tasks| id, chat_id, invitation_url, invitation_code, current_task_id |Group|
|student_group_table |Connecting data between student and group|student_id, group_id ||
|sentence_correction_data |Raw data of the sentence correction task|id, sub_type, difficulty_level, sentence_corpus, correct_answer, error_words|Sentence|
|vocabulary_guessing_task | Raw data of the vocabulary guessing task|id, sub_type, word, difficulty|Word|
|student_proficiency_table |Mapping of each user and their proficiency | student_id, proficiency_id, value||
|proficiency_table |This table has the mapping of the proficiency levels and ids| proficiency_domain, proficiency_id, proficiency_name||
|student_table |Data about each student using the bot|id, telegram_name, name|Student|
|task_table |Every task data| id, name, min_num_of_players, num_of_iteration|Task|

*Table 2: Tables Names and Descriptions*

### Database Operators for Entities

We needed different queries to extract data based on different use cases. The following are the examples of queries with example of use cases.

1. Querying data from a table. We need such kind of queries to just explore the data and check the structure of the table.
2. Filtered Query based on certain columns. If you want to extract data based on a certain value from a specific column. We need such queries when we need data (sentences or word) based on certain difficulty level OR if we need the proficiency level of a particular user
3. Limited Query. Limiting your output to a specific number of outputs. For example: when we need 1 random sentence from the sentence correction data.

Similar query functions are used while inserting data into specific tables, In which case one uses `Insert` instead of `Select`. The SQL insert statement helps to push the data in the database. Based on the query types above, we defined functions to parse data from the database into the corresponding Python entities. Example:`get_student_proficiency` returns the proficiency level of the users and `get_sentence_information_from_df` parses the information from the database into sentence corpus, one error word and one correct answer.

### Database Connection Class
We defined a database connection class which connects with the database based on provided configuration parameters. We set the database configuration required to connect with the database in a separate source file and created a function to form this connection. Once the connection is created we can use functions like `get_random_sentence_based_on_sub_type_and_difficulty`, which allows the code to get a random sentence to use in the task based on the sub_type and difficulty level. Every Task is further mapped with entities to get certain data from the database. Like the sentence correction task is mapped to sentence and student entities. Its similar for the vocabulary guessing task.  For more information please refer the next subsection.

One more function is created that parses the data once the connection is open. The data we query for any table is converted into a [DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) for further use. The function to extract a DataFrame is `read_query_into_df`.

### Entity Classes

We created classes for entities and created functions in them to get certain values from those entities. This allows one to extract a certain value from the entire object rather than extracting all relational values of the entity. We have five entities. We have already seen that Tasks are mapped to entities and each entity is mapped to a table in the database. The first entity is called *Group* and helps to get the group id and the chat id of the groups performing tasks. The second entity is the *Sentence* entity which initiates the extraction of a sentence for the task. One of the important functions in this class is `is_correct`, which determines whether a sentence is correct or not. We also have a function to get the proficiency level. *Student* is the next entity class which gives us information about the user for example, their id or name. It also helps to update their proficiency level when they complete their task. The next entity is called *Task* which helps to return information about the task, its id, name, minimum users required, etc. The final entity of mention is *Word*, which has the mapping of the word and its difficulty level.

## Technical Challenges & Development Productivity

This section gives insights into the development experience and challenges to develop a telegram chat bot application for a group learning experience. Hindrances that slowed down the development process are highlighted and solutions that improve the development productivity are presented. Finally, bugs and their corresponding fixes are described as well as an outlook for future development.

### Actual state evaluation & Goals

To get familiar with the project and evaluate the inherited state of the application the new constellation of study project members in the third semester played the game themselves. This was done in three groups of three people each. During these game sessions it was noticeable that the application is in an unstable condition, because multiple bugs occurred that hinder the progression in the game. For example after solving all of the current tasks the players are supposed to receive a code word to enter the next room. Either this code word was not received or the prompt to enter the code word did not appear, ultimately resulting in a deadlock state that can only be recovered by restarting the bot and thereby starting all over again. Sometimes the application even crashed entirely. Consequently, one goal for the semester was the elimination of any bugs that hinder a successful play through of the game.
Another goal was to improve the development productivity. It was noticeable that playing through the game takes a lot of time, which can slow down the play testing of newly introduced code changes, e.g. changes at the very end of the game. Additionally, it was not possible to play the game by oneself. This makes sense from a user experience-based perspective, because the game is meant to be a group learning experience, but from a development standpoint this impedes productivity. Most of the developers had to pair up in groups of two, because only one telegram account can be registered per mobile phone number and they only own one. Therefore, a so-called debug-mode was newly implemented to better these circumstances.

### Debug mode

This debug-mode allows a developer to play through the vocabulary guessing and sentence correction task much faster, because only one iteration is played instead of three. This means instead of the usual three required answers per group member only one answer is required. In addition, the discussion task is accelerated by shortening the duration in which answers are allowed. Moreover, hints or solutions to the task at hand are now printed to the console, so that a progression through the tasks takes less effort. Also, the sentence correction task can be played by a single developer in the debug-mode. The debug-mode can be activated by a developer by setting a debug option in the bot config file to `true`. All in all, the new debug-mode reduced the time of a full play through of the game from around 30 minutes to 10 minutes.

### Rejection of Automated Testing

To tackle the known bugs which have been mentioned above one approach would have been the adoption of automated tests. Unfortunately, the structure of the given code base was not implemented with automated testing in mind. Also, setting up a testing suite itself in the context of telegram bot development is not trivial. Getting familiar with bot testing tools and rewriting the whole project would have taken a lot of time. Even if done in smaller chunks this was estimated to take away too much time that could be better spent on new features or other improvements. Therefore, setting up and implementing automated tests has been discarded entirely.

### Bugs

During this semester the most notable bugs have been found and fixed so that the game can be completed successfully. Surprisingly one severe bug could be fixed by simply appending parentheses to the statement `sentence_task.is_success`. Without the parentheses python evaluates this statement as a reference to the function object `is_success` instead of calling the function with `is_success()`. Despite the elimination of such bugs, every so often there still occur errors that lead to a crash of the application in the worst case. For example when too many or too long messages are sent in a fast succession a `telegram.error.RetryAfter: Flood control exceeded` error occurs. Furthermore, during a bad internet connection a `telegram.error.NetworkError` can occur or when no messages have been sent for a longer period of time a special case of the NetworkError the `telegram.error.TimedOut` can happen.
To understand the cause of these errors it is necessary to grasp the underlying architecture of the telegram bot which is already described in the [Telegram Integration](/#telegram-integration) section. Just as a short reminder, the [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot) library is a wrapper that handles HTTP requests towards the [Telegram Bot API](https://core.telegram.org/bots/api). The API has some restrictions like the above mentioned message limits that result in a response with an error [status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes). This error is handled by the library and passed on to be handled in the application. Handling these errors and retrying the last action would be a workaround only treating the symptoms. A better solution would be the usage of a [MessageQueue](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Avoiding-flood-limits). Unfortunately, the `MessageQueue` has [four known bugs](https://github.com/python-telegram-bot/python-telegram-bot/issues/2139) and is therefore already marked as deprecated with no release date of a replacement in sight. Additionally it is noteworthy that such errors can happen in all sorts of places in the code making the implementation challenging. All in all, there is no clear path on how to go about these problems. It might even be a possible approach to completely rewrite the application in a different chat bot technology.

<div id="references"></div>

## References

Nebel, S., Beege, M., Schneider, S., & Rey, G. D. (2020). Competitive Agents and Adaptive Difficulty Within Educational Video Games. Frontiers in Education, 5. https://doi.org/10.3389/feduc.2020.00129

Sampayo-Vargas, S., Cope, C. J., He, Z., & Byrne, G. J. (2013). The effectiveness of adaptive difficulty adjustments on students’ motivation and learning in an educational computer game. Computers & Education, 69, 452–462. https://doi.org/10.1016/j.compedu.2013.07.004
