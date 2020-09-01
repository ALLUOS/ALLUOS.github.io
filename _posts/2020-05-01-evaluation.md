---
layout: post
bibliography: evluation.bib
title: Summary and Evaluation
description: Under construction!
image: pic04.jpg
---

# Recommended Structure

A summary of the goals and strategies we used in our testing.
1. Software testing in general
2. Validation and Verification
3. Test Methods we applied

### Validation

1. Goals
2. Planning of technical testing
3. Test preparation
4. Test execution
5. Results
6. Recommendation for future testing

### Verification

1. Methods of Verification
2. Expert interview
  - Goals
  - Questions
  - Results
3. Usability Testing 
  - Goals
  - Test group
  - Pre-questionnaire
  - Usability test
  - Post-questionnaire
  - Results
  
### Recommendation

Here the actual article begins:

# Software Testing

## Table of content

1. Test Strategy
2. Usability Test
 1. Strategy Overview
 2. Evaluation
 3. Outlook
3. Technical Test
 1. Overview
 2. Test Dimensions
 3. Test Cases
 4. Test Instructions
 5. Defects
 6. Results
4. References

## Test Strategy

The testing strategy we designed was inspired by the paper *A Software Testing Primer* [@jenkins2008software]. In the paper Jenkins diferentiates between two goals of software testing: verification and validation. A test can either verify, that the specified requirements are fulfilled or validate that the application design serves the purpose of the application. As both goals come with there own methodology, we splitted our testing strategy along this line. One part of our group conducted a technical test to verify the implementation, the other part of our group conducted an usability test in order to validate the usefulness of the application as a group learning scenario for the english language. The usability testing was further divided into performance and satisfaction subparts.

## Usability Test

### Usability Test Strategy Overview


When testing usability, we aimed to find a testing group resembling the target audience as closely as possible, i.e. schoolchildren in the ages of 14 to 17 with English knowledge between pre-intermediate and upper-intermediate. Out initial idea was to remotely recruit 15 to 25 testers from a group of language course students who almost perfectly matched our ideal testing/target group. Given that no further adjustments between sessions based on the intermediary results was planned, the data from a testing group of this size more than sufficed to assess the application’s usability.

Testers received two instruction sets, one with a short overview of our work and software/hardware requirements, distributed a few days before the test, and the other with the test instructions themselves, e.g. “Join the game” or “Play the error correction game”, distributed immediately before the test. To minimize misunderstanding, the second instruction set was accompanied by a short video session where testers could receive further necessary explanations to a degree that would not bias their testing activity. The testers were also asked to share their screen recordings with the test’s facilitator.

The initial step of the usability test was a background questionnaire gathering information about testers’ age, English learning experience, computer literacy, previous e-learning experiences, etc., generating information to match a tester against our target audience. 
The next step was the performance test. A common practice in usability testing is to assess the application’s performance based on its effectiveness and efficiency. However, choosing appropriate metrics to estimate them in a learning task presented a challenge because not all common approaches could work in a reasonable way. For example, a learning task’s goal is to transfer knowledge, even if it requires extra processing effort and delays from the user, it is therefore irrelevant how quick the user is with the task, provided they learn from it. 

Diah et al. [@diah_usability_2010] previously encountered the same issue with their learning application and chose success rate, defined as the proportion of successful trials to all trials, as their basic metric. We decided to recycle this approach in our usability tests and defined success and partial success criteria for the application’s two tasks, being worth 1 or 0.5 points accordingly. In the error correction task success meant 3 out of 4 users in the game room interacting throughout the task session and partial success was defined as 2 out of 4 active users in the session. Success in the word guessing task was defined as at least one description posted, followed by an incorrect guess and further description or by a correct guess, partial success was defined as a description followed by a guess posted. Though rough, success rate provides a general picture and answers the essential question of whether the users can even manage the required task. Its seeming sketchiness is also compensated for by low cost and straightforward interpretation.

The second measure employed to test the application’s performance was the error count. It is calculated as the number of times when users performed erroneous actions per task session, such as giving out their secret word in the word guessing tasks or unnecessarily using the buttons/Telegram API. It shows if the users are mislead by the application’s flow and instructions and how much their understanding of the application overlaps with ours.

The usability test was concluded with a satisfaction questionnaire that contained questions about the user’s experience overall and with separate tasks, their willingness to recommend such an application to their friend and basic understanding of the tasks’ goals. Both the opening and closing questionnaires were distributed via Google Forms.

### Evaluation


As the guidance and instructions have been provided to the testing group and a video Q&A session, concerning questions about the performance test itself, has been conducted, the usability test has begun. As the first step of the testing procedure in order to collect some information about the testing group, participants were asked to fill in the pre-questionnaire about their background and experience in learning foreign languages with the help of apps or e-learning platforms, and using Telegram. Then participants, following the instructions have played each game (“Sentence Correction” and “Vocabulary guessing”) two rounds in Escapeling app. Testers have made screen recordings and provided us with them, screen recordings are used in order to evaluate the results afterwards. Finally, participants had to fill in the post-questionnaire to evaluate the tasks and rate the app in terms of their satisfaction with the tasks and Escapeling app in general and their willingness to use this app for further collaborative learning of the English language. 

As Escapeling is designed for collaborative learning of English, games are supposed to be played in the group of 4 people. Participants have a common goal to solve the task and escape the room, so that they are allowed to communicate with each other in the group chat and contribute to the task success and help each other to progress in learning of the foreign language.  We recruited two groups of four people to conduct the test. 

Examining the results of the pre-questionnaire, we extracted the following information about testers: representatives of our test groups are high school students in the age of 14-18 years old. Speaking of their English level, the pre-questionnaire has shown that 42.9 % of testers have upper-intermediate level of English, 28.6 % have intermediate level, and 28.6 % – pre-intermediate level. The majority of the group has prior-experience in using Telegram. The majority of participants have used before various language-learning apps as well, e.g. Duolingo, Reword, Drops, HelloTalk, PuzzleEnglish. Furthermore, 71.4 % of testers have shown their interest in using the app for collaborative language learning. 
What performance results concerned, there were conducted two test sessions. Speaking of the Session 1, performance cannot be objectively graded because the app crashed during testing. Basic precondition to evaluate the performance appears to be a properly working app, hence, we have taken into consideration only results of the Session 2 for evaluation of user acceptance. To evaluate the success rate we calculated the proportion of completed successfully to all attempts. Errors were counted, as well. 

So, success rate in the Task 1: 8.0/8 = 1. During the first task (“Error correction”) participants were asked to play twice (so that each participant had his/her turn twice). During the test all participants made a guess, whether the sentence was correct or not, tried to find the mistake in the sentence, and correct it. So that, the success rate in the first round is 1.0, and in the second as well. In total = 2.0. Success rate in the Task 2: 8.0/8 =1. During the second task (“Vocabulary guessing”) participants were asked to play again 2 rounds, so everyone has their turn twice. The task was completed successfully. All participants posted the description of the provided word, all of them made at least one guess, and the task was completed. Error count is 2. Most common error is naming the shown word in the Task 2 during the explanation of the word to the other participants of the group, as it violates the instructions. 

Examining the results above, we can conclude that performance results are quite good; the results of the post-questionnaire confirm that as well. Participants were highly satisfied with the app and both tasks and majority of the group would highly recommend this app to their friends. So, based on tests, the application was accepted by the target user and mostly met the requirements that have been set initially. 


### Outlook


An important point that has been seen  during the test procedure is that it became obvious that participants barely communicated with each other in the group chat, most of the time they were waiting for the messages and instructions from the bot. When they experienced some difficulties to give the correct answer, they were working individually. As the initial idea of the app is collaborative learning, some reminders and encouraging messages from the bot, when the silence in the chat lasts for a while, might be helpful to engage users to interact with each other more, as they have a common goal while completing the task and it will help them to practice language more in a natural way, communicating with peers.

Another issue is that despite being provided with written and oral instructions some testers, especially those who had no prior experience with Telegram bots, got confused easily while using commands. So that, it might be a good idea to provide explicit instructions inside the bot (e.g. pin the message with the instructions in the chat). It will make the interface of our chatbot more user-friendly and will help players to remember how to use the bot. 

Another possible improvement to the application's usability is clearly indicating when it crashes. Throughout the tests,not seeing whether the bot is still running sometimes caused misunderstanding, as the users were expecting a reaction that was impossible because the bot was offline. This could be implemented by simply sending an explicit error message when errors occur. 

Concluding the usability tests, we should note that one of the biggest usability issues was possibly not the application’s design, but Telegram’s API commands employed to run tasks and arrange people into game rooms. We should possibly consider wrapping them into buttons or providing extensive instructions on how to operate them.
 
## Technical Test

### Overview

In technical software testing the standard procedure is to analyze the software, as it is planned, in order to detect the components, that most likely lead to a defect and might need improvement later on in the process. This is necessary, as software testing is the last step in software production prior to release. This means it is impossible to wait until the software is implemented to define the software tests.
A common way to arrive at a  software test strategy is to
* analyse the important and vulnerable dimensions of the software,
* think about possible and likely errors that might occur in each dimension,
* define test cases, that cover all dimensions and predicted errors,
* create test instructions realizing the test cases.
In our project we kept to this industry standard, as it is a straight forward way to detect the most important error, that is easy to implement even for people not acquainted with software testing, easy to analyse afterwards and easy to communicate to other teams.

### Test Dimensions

The first step of this strategy is to analyse the software and detect important and vulnerable parts of the software as software dimensions. Typical categories, that help detecting software dimensions are functionality, code structure, user interface elements, internal and external interfaces, the input and output space, the data, the environment and use case scenarios. For us were important the functionality in form of the technical specifications, the code structure as communicated by the development team, the environment in form of telegram and different operating systems and the use case of a group learning scenario. We did not test the data structure or internal and external interfaces, as our project is still rather small and lacks a complicated data structure or internal and external interfaces.

The dimensions we identified were
* the group handling functionality
* the AI adaption
* the sentence production
* the operating systems
  * windows and
  * android
* the telegram UI
* requirement functionalities of
  * task 1
  * task 2
  
### Test Cases

While the test dimensions span the space of defects, that might occur in a software the test cases try to cover this space. Ideally one would create a test case for every conceivable error that can occur in a software. This is impossible first, because it is presumptuous to think, that the identified dimensions or created test cases cover all errors and second, because testing has limits as i.e. time constraints. The task in test case selection is thus to create and select test cases, that cover the error space ideally, meaning in such a way, that all major and as many errors as possible are detected in the software test.
A test case consists out of a description of what part of the error space it tries to cover, how it tries to achieve this and what is needed in order to do so.
In our case the test cases consisted out of the following information:
* the software modules it involved,
* the specified requirements is tries to test,
* a description of the test case including the what it tries to test and how,
* a one-by-one description of the test case steps ,
* conditions that have to be met for the test case,
* data needed for the test case,
* the expected result of the test case.

### Test Instructions

The test instructions were derived from the test cases in such a way, that the software testing could be directly conducted from them. Rather then being a one to one mapping, a test instructions could cover multiple test cases or a test case could be divided between several test instructions. Nevertheless the test instructions were created in such a manner, that all selected test cases are covered by them and they do thus cover the planned test space.
The final test instruction covered:
* A group handling test, trying to detect defects during the creation or management of the bots group creation and joining, as well as tasK start-up,
* A combined UI and task progress handling test, that requires playing the task of the application to it's end and detecting any defect with the task flow or Telegram UI elements,
* A difficulty adaption test, verifying, that the difficulty adaption module does work as specified in the requirements,
* A sentence handling test, trying to detect defects with the message handler during the tasks.

### Defects

The defects that we detected were reported in a table containing the following dimensions:
* a summary of the defect,
* a detailed description of what happend,
* a instuction how to replicate the defect,
* the expected behavior of the application,
* the actual behavior of the application,
* possible attachements and remarks,
* and the severity of the defect for the application.
The severity was decided with a simple matrix, that had a value of 0 or 1 on The two dimensions frequency and impact ef the defect. The defect was treated of low severity if both dimension were of value 0, of medium if one dimension was of value 1 and high, if both had value 1.
For each defect a corresponding github issue was created for the development team to fix it. This issue contained the same information as the defect table itself.

### Results

After conducting the tests we detected ~15 defects, some of them were very similar or the same defects occuring on different setups. As a software test should detect all possible mistakes of a software we were surprised by the small quantity and will try to analyse our approach for optimisation. Especially, because the usability test, that was conducted shortly after revealed, that we missed defects the software had. Nevertheless we can conclude, that we tested a solidly build software being a good basis for further improvement of our project.


## References

Diah, N. M., Ismail, M., Ahmad, S., & Dahari, M. K. (2010). Usability testing for educational computer game using observation method. 2010 International Conference on Information Retrieval & Knowledge Management (CAMP). C

Nielsen Norman Group. Why You Only Need to Test with 5 Users. Retrieved August 27, 2020, from https://www.nngroup.com/articles/why-you-only-need-to-test-with-5-users/

Tullis, T., & Albert, B. (2013). Measuring the user experience collecting, analyzing, and presenting usability metrics. Amsterdam, Netherlands: Elsevier.

| Chapter  | Person&commitment level | Word count |  
| ------------- | Luis | ------------- |
| ------------- | Victoria | ------------- |
| ------------- | Ivan | ------------- |
| Test Strategy Intro; Usability Test Strategy Overview; Outlook | Maksim, SP |50;601; 104; |


