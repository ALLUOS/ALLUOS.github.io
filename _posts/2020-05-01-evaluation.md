---
layout: post
bibliography: evluation.bib
title: Summary and Evaluation
description: Under construction!
image: pic04.jpg
---

# Software Testing

## Table of content

1. Test Strategy
2. Usability Test
 - Strategy Overview
 - Evaluation
 - Outlook
3. Technical Test
 - Overview
 - Test Dimensions
 - Test Cases
 - Test Instructions
 - Defects
 - Results
4. Outlook
5. References

## Test Strategy

The testing strategy we designed was inspired by the paper *A Software Testing Primer* [@jenkins2008software]. In the paper Jenkins diferentiates between two goals of software testing: verification and validation. A test can either verify, that the specified requirements are fulfilled or validate that the application design serves the purpose of the application. As both goals come with there own methodology, we splitted our testing strategy along this line. One part of our group conducted a technical test to verify the implementation, the other part of our group conducted an usability test in order to validate the usefulness of the application as a group learning scenario for the english language. The usability test was further divided into performance and satisfaction subparts.

## Usability Test

### Usability Test Strategy Overview


When testing usability, we aimed to find a testing group resembling the target audience as closely as possible, i.e. schoolchildren in the ages of 14 to 17 with English knowledge between pre-intermediate and upper-intermediate. Out initial idea was to remotely recruit 15 to 25 testers from a group of language course students who almost perfectly matched our ideal testing/target group. Given that no further adjustments between sessions were planned based on the intermediary results, the data from a testing group of this size more than sufficed to assess the application’s usability.

Testers received two instruction sets, one with a short overview of our work and software/hardware requirements, distributed a few days before the test, and the other with the test instructions themselves, e.g. “Join the game” or “Play the error correction game”, distributed immediately before the test. To minimize misunderstanding, the second instruction set was accompanied by a short video session where testers could receive further necessary explanations to a degree that would not bias their testing activity. The testers were also asked to share their screen recordings with the test’s facilitator.

The initial step of the usability test was a background questionnaire gathering information about testers’ age, English learning experience, computer literacy, previous e-learning experiences, etc., generating information to match a tester against our target audience. 
The next step was the performance test. A common practice in usability testing is to assess the application’s performance based on its effectiveness and efficiency. However, choosing appropriate metrics to estimate them in a learning task presented a challenge because not all common approaches would have worked in a reasonable way. For example, a learning task’s goal is to transfer knowledge, even if it requires extra processing effort and delays from the user, it is therefore irrelevant how quick the user is with the task, provided they learn from it. 

Diah et al. [@diah_usability_2010] previously encountered the same issue with their learning application and chose success rate, defined as the proportion of successful trials to the total amount of trials, as their basic metric. We decided to use this approach in our usability tests as well and defined success and partial success criteria for the application’s two tasks, being worth 1 or 0.5 points accordingly. In the error correction task success meant three out of four users in the game room interacting throughout the task session and partial success was defined as two out of four active users interacting in the session. Success in the word guessing task was defined as at least one description posted, followed by an incorrect guess and further description or by a correct guess, partial success was defined as a description followed by a guess posted. Though rough, success rate provides a general picture and answers the essential question of whether the users can even manage the required task. Its seeming sketchiness is also compensated for by low cost and straightforward interpretation.

The second measure employed to test the application’s performance was the error count. It is calculated as the number of times users performed erroneous actions per task session, such as giving out their secret word in the word guessing tasks or unnecessarily using the buttons/Telegram API. The error count shows, if the users are misguided by the application’s flow and instructions and how far their understanding of the application overlaps with our intentions.

The usability test was concluded with a satisfaction questionnaire that contained questions about the user’s experience overall and with the separate tasks, their willingness to recommend such an application to their friend and basic understanding of the tasks’ goals. Both the opening and closing questionnaires were distributed via Google Forms.

### Evaluation


After the guidance and instructions have been provided to the testing group and a video Q&A session has been conducted the usability test begun. As a first step of the testing procedure participants were asked to fill in the pre-questionnaire to collect some information about the testing group, about their background and experience in learning foreign languages with the help of apps or e-learning platforms and their experience using Telegram. Then participants, following the instructions, have played two rounds of each game (“Sentence Correction” and “Vocabulary guessing”) in the Escapeling app. Testers made screen recordings and provided us with them. Screen recordings were used in order to evaluate the results afterwards. Finally, participants had to fill in the post-questionnaire to evaluate the tasks and rate the app in terms of their satisfaction with the tasks and the Escapeling app in general and their willingness to use this app for further collaborative English language learning. 

As Escapeling is designed for collaborative English learning, the tasks are supposed to be played in a group of four people. Participants have the common goal to solve the task and escape the room. Therefore they are allowed to communicate with each other in the group chat, contribute to the task success and thereby help each other progress learning the foreign language. We recruited two groups of four people to conduct the test. 

Examining the results of the pre-questionnaire, we extracted the following information about testers: our test groups consists of high school students being 14-18 years old. Concerning their English level, the pre-questionnaire has shown that 42.9 % of testers have upper-intermediate level of English, 28.6 % have intermediate level, and 28.6 % pre-intermediate level. The majority of the group has prior-experience in using Telegram. The majority of participants have used various language-learning apps before as well, e.g. Duolingo, Reword, Drops, HelloTalk, PuzzleEnglish. Furthermore, 71.4 % of testers have shown their interest in using the app for collaborative language learning. 
Concerning performance results, we conducted two test sessions. For session one, performance could not be objectively measured because the app crashed during testing. Basic precondition to evaluate the performance appears to be a properly working app, hence, we have taken into consideration only results of the second session for evaluation of user acceptance. To evaluate the success rate we calculated the proportion of completed successfully to the total amount of attempts. Errors were counted, as well. 

The success rate of the first task was 8.0/8.0 = 1.0. During the first task (“Error correction”) participants were asked to play twice (such that each participant had his/her turn twice). During the test all participants made a guess, whether the sentence was correct or not, tried to find the mistake in the sentence and correct it. So that, the success rate in the first round is 1.0, and in the second round 1.0 as well. In total equaling 2.0. Success rate in the second task was 8.0/8.0 = 1.0. During the second task (“Vocabulary guessing”) participants were asked to play two rounds again, so everyone had their turn twice. This task was completed successfully. All participants posted the description of the provided word, all of them made at least one guess and the task was completed. The error count was 2.0. The most common error occured during the second task naming the shown word to the other participants, as this violates the instructions. 

Examining the results above, we can conclude that performance results are quite good. The results of the post-questionnaire confirm that as well. Participants were highly satisfied with the app and both tasks. The majority of the group would highly recommend this app to their friends. Based on the tests, the application was accepted by the target user and mostly met the requirements that have been set initially. 


### Outlook


An important point that we have seen during the test procedure is that it became obvious that participants barely communicated with each other in the group chat, most of the time they were waiting for the messages and instructions from the bot. When they experienced some difficulties to give the correct answer, they were working individually. As the initial idea of the app was collaborative learning, some reminders and encouraging messages from the bot, when the silence in the chat lasts too long, might be helpful to engage users to interact more with each other, as they have a common goal while completing the task. Addditionaly it will help them practice the language in a more natural way, communicating with there peers.

Another issue is, that despite being provided with written and oral instructions some testers, especially those who had no prior experience with Telegram bots, got confused easily while using the bot commands. I t might be a good idea to provide explicit instructions inside the bot (e.g. pin the message with the instructions in the chat). It will make the interface of our chatbot more user-friendly and will help players to remember how to use the bot. 

Another possible improvement to the application's usability is clearly indicating when it crashes. Throughout the tests,not seeing whether the bot is still running sometimes caused misunderstanding, as the users were expecting a reaction that was impossible because the bot was offline. This could be implemented by simply sending an explicit error message when errors occur. 

Concluding the usability tests, we should note that one of the biggest usability issues was possibly not the application’s design, but Telegram’s API commands employed to run tasks and arrange people into game rooms. We should possibly consider wrapping them into buttons or providing extensive instructions on how to operate them.
 
## Technical Test

### Overview

The goal of a technical software test is to detect the defects or bugs that a software exhibits before it is released. Defects in this context are diviations from the behavior specified in the softwares requirements. The task of a technical software test is to design and execute routines that reveal as many of the softwares defects as possible.

In technical software testing the standard procedure is to analyze the software in its planning stage allread. The goal is to detect components, that will exhibit defectious behavior and will need improvement later on in the process. This is necessary, as software testing is the last step in software production prior to release. This means it is not possible to wait until the software is implemented to design the software tests.
A common way to arrive at a test strategy is to
* analyse the important and vulnerable components of the software as dimensions,
* think about possible and likely errors that might occur in each dimension,
* define test cases, that cover as many dimensions and errors as possible,
* create test instructions instanciating the test cases.
In our project we kept to this industry standard, as it is a straight forward way to detect the most important error, is easy to implement for people not acquainted with software testing, is easy to analyse afterwards and easy to communicate to other parts of the team.

### Test Dimensions

The first step of this strategy is to analyse the software and detect important and vulnerable parts of the software as software dimensions. Typical categories, that help detecting software dimensions are functionality, code structure, user interface elements, internal and external interfaces, the input and output space, the data, the environment and use case scenarios. Important for us were the functionality in form of the technical specifications, the code structure as communicated by the development team, the environment in form of telegram and different operating systems and the use case of a group learning scenario. We did not test the data structure or internal and external interfaces, as our project is still rather small and lacks a complicated data structure or internal and external interfaces.

The dimensions we identified were
* the group handling functionality,
* the AI adaption,
* the sentence production,
* the operating systems
  * windows and
  * android,
* the telegram UI,
* requirement functionalities of
  * task 1,
  * task 2.
  
### Test Cases

While the test dimensions span the space of defects, that might occur in a software the test cases try to cover this space. Ideally one would create a test case for every conceivable error that can occur in a software. This is impossible. First, because it is presumptuous to think, that the identified dimensions or created test cases cover all errors and second, because testing has limits as i.e. time constraints. The task in test case selection is thus to create and select test cases, that cover the error space ideally, meaning in such a way, that all major and as many errors as possible are detected in the software test.
A test case consists out of a description of what part of the error space it tries to cover, how it tries to achieve this and what is needed in order to do so.
In our case the test cases consisted out of the following information:
* the software modules it involved,
* the specified requirements is tries to test,
* a description of the test case including what it tries to test and how,
* a one-by-one description of the steps of the test case,
* conditions that have to be met for the test case to be realized,
* data required for the test case,
* the result expected for the test case.

### Test Instructions

The test instructions are derived from the test cases in such a way, that the software testing can be conducted with them. Rather than being a one to one mapping of test cases, a test instructions can cover multiple test cases or a test case could be divided between several test instructions. Nevertheless the test instructions are created in such a way, that all selected test cases are covered and thus the test space is covered as well.
Our test instruction consisted of
* a group handling test, trying to detect defects during the creation and management of the bots group creation and group joining, as well as the task start-up,
* a combined UI and task progress handling test, that required playing the tasks of the application to their end and detecting any defect within the task flow or the Telegram UI elements,
* a difficulty adaption test, verifying, that the difficulty adaption module does work as specified in the requirements,
* a sentence handling test, trying to detect defects with the message handler during the tasks execution.

### Defects

The defects that we detected were reported in a table containing the following dimensions:
* a summary of the defect,
* a detailed description of what happend,
* a instruction how to replicate the defect,
* the expected behavior of the application,
* the actual behavior of the application,
* possible attachements and remarks,
* and the severity of the defect for the application.
The severity of a defect was decided by a simple matrix, that had a value of zero or one for the two dimensions frequency and impact. The defect was treated of low severity if both dimension were of value zero, of medium if one dimension was of value one and high, if both had a value of one.
For each defect a corresponding github issue was created. This issue was then used by the development team to fix the defect. This issue contained the same information as the defect table entry of the defect.

### Results

After conducting the tests we detected ~15 defects, some of them were very similar or even the same defect occuring on a different setup. As a software test should detect all possible mistakes of a software, we were surprised by the small quantity. For a future test we want to optimise our approach, analyzing why we detected so few defects. This is of special importance to us, because the usability test, that was conducted shortly after revealed, that we missed defects the software had. Nevertheless we can conclude, that we tested a solidly build software being a good basis for the further improvement of our project.

## Prospects For The Next Semester


The idea behind the project is unique in the education system and is to provide a user-friendly application for collaborative group activities. At the moment, the application is already a fairly high-quality product with many useful functions, but it still needs improvement.
The project contributed to:
* stimulating interest and enhancing the cognitive and cognitive activity of students in the mainstream of the educational process.
* formation of skills and development of communication skills on the Telegram platform.

In the "Overview" section, you learned about the main difficulties encountered during usability testing, namely:
* Participants hardly communicate with each other but prefer to do everything on their own.
* Difficulties in using the built-in commands and functions of the API Telegram.

Also, an analysis of the survey of all participants in the target groups of the project showed an increase in the interest of all target groups in receiving remote communication services.

The project opened up great opportunities for further research and development in the field of distance interaction and education, therefore we assume the following possible ways of developing this product:

Project prospects:

* Development of a model for remote interaction of all participants.
* Study, analysis, and application of didactic capabilities, properties and functions of Telegram.
* Selection, organization, and structuring of the content of instructions in conjunction with the application.
* Development of requirements for the creation of tasks.
* Study of the psychological characteristics of the interaction of all subjects of the educational process in conditions of distance interaction.
* Development of pedagogical technologies. (adding the learning side of the project)
* Methodology for organizing feedback and consultations for application users.
* Solving problems of assessment and control in group learning.


## References

Diah, N. M., Ismail, M., Ahmad, S., & Dahari, M. K. (2010). Usability testing for educational computer game using observation method. 2010 International Conference on Information Retrieval & Knowledge Management (CAMP). C

Nielsen Norman Group. Why You Only Need to Test with 5 Users. Retrieved August 27, 2020, from https://www.nngroup.com/articles/why-you-only-need-to-test-with-5-users/

Tullis, T., & Albert, B. (2013). Measuring the user experience collecting, analyzing, and presenting usability metrics. Amsterdam, Netherlands: Elsevier.

| Chapter  | Person&commitment level | Word count |  
| ------------- | -------------  | ------------- |
| Test Strategy; Technical Testing | Luis, SP | 1138 |
| ------------- | Victoria | ------------- |
| ------------- | Ivan | ------------- |
| Test Strategy Intro;Usability Test Strategy Overview;Outlook;References | Maksim, SP |50;601;104;70|


