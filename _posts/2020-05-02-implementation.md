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
* Overall handling of tasks
* Idea of task classes and extension of sequential task base class

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
