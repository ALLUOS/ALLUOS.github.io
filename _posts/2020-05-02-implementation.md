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
* Overview of repository content
* Python + PostgreDB + JSON 
* Config-files

## Telegram Integration
* APIs used
* Application states and handlers

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
* Tables with columns 
* Database connector module
* When is which data written / extracted from the database?

## References
Nebel, S., Beege, M., Schneider, S., & Rey, G. D. (2020). Competitive Agents and Adaptive Difficulty Within Educational Video Games. Frontiers in Education, 5. https://doi.org/10.3389/feduc.2020.00129

Sampayo-Vargas, S., Cope, C. J., He, Z., & Byrne, G. J. (2013). The effectiveness of adaptive difficulty adjustments on students’ motivation and learning in an educational computer game. Computers & Education, 69, 452–462. https://doi.org/10.1016/j.compedu.2013.07.004
