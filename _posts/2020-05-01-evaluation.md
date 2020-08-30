---
layout: post
title: Summary and Evaluation
description: Under construction!
image: pic04.jpg
---

test
### Test Strategy

To assess the application’s performance in full, it was decided to split the testing into technical testing and usability testing. The usability testing was further divided into performance and satisfaction subparts.

### Usability Test Strategy Overview
When testing usability, we aimed to find a testing group resembling the target audience as closely as possible, i.e. schoolchildren in the ages of 14 to 17 with English knowledge between pre-intermediate and upper-intermediate. Out initial idea was to remotely recruit 15 to 25 testers from a group of language course students who almost perfectly matched our ideal testing/target group. Given that no further adjustments between sessions based on the intermediary results was planned, the data from a testing group of this size more than sufficed to assess the application’s usability.
Testers received two instruction sets, one with a short overview of our work and software/hardware requirements, distributed a few days before the test, and the other with the test instructions themselves, e.g. “Join the game” or “Play the error correction game”, distributed immediately before the test. To minimize misunderstanding, the second instruction set was accompanied by a short video session where testers could receive further necessary explanations to a degree that would not bias their testing activity. The testers were also asked to share their screen recordings with the test’s facilitator.
The initial step of the usability test was a background questionnaire gathering information about testers’ age, English learning experience, computer literacy, previous e-learning experiences, etc., generating information to match a tester against our target audience. 
The next step was the performance test. A common practice in usability testing is to assess the application’s performance based on its effectiveness and efficiency. However, choosing appropriate metrics to estimate them in a learning task presented a challenge because not all common approaches could work in a reasonable way. For example, a learning task’s goal is to transfer knowledge, even if it requires extra processing effort and delays from the user, it is therefore irrelevant how quick the user is with the task, provided they learn from it. 
Diah et al. previously encountered the same issue with their learning application and chose success rate, defined as the proportion of successful trials to all trials, as their basic metric. We decided to recycle this approach in our usability tests and defined success and partial success criteria for the application’s two tasks, being worth 1 or 0.5 points accordingly. In the error correction task success meant 3 out of 4 users in the game room interacting throughout the task session and partial success was defined as 2 out of 4 active users in the session. Success in the word guessing task was defined as at least one description posted, followed by an incorrect guess and further description or by a correct guess, partial success was defined as either no description or no guesses posted. Though rough, success rate provides a general picture and answers the essential question of whether the users can even manage the required task. Its seeming sketchiness is also compensated for by low cost and straightforward interpretation.
The second measure employed to test the application’s performance was the error count. It is calculated as the number of times when users performed erroneous actions per task session, such as giving out their secret word in the word guessing tasks or unnecessarily using the buttons/Telegram API. It shows if the users are mislead by the application’s flow and instructions and how much their understanding of the application overlaps with ours.
The usability test was concluded with a satisfaction questionnaire that contained questions about the user’s experience overall and with separate tasks, their willingness to recommend such an application to their friend and basic understanding of the tasks’ goals. Both the opening and closing questionnaires were distributed via Google Forms.


A summary of the goals and straegies we used in our testing.
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
 
### Technical Testing

In technical software testing the standard procedure is to analyze the software, as it is planned, in order to detect the components, that most likely lead to a defect and might need improvement later on in the process. This is necessary, as software testing is the last step in software production prior to release. This means it is impossible to wait until the software is implemented to define the software tests.
A common way to arrive at a  software test strategy is to
* analyse the important and vulnerable dimensions of the software,
* think about possible and likely errors that might occur in each dimension,
* define test cases, that cover all dimensions and predicted errors,
* create test instructions realizing the test cases.
In our project we kept to this industry standard, as it is a straight forward way to detect the most important error, that is easy to implement even for people not acquainted with software testing, easy to analyse afterwards and easy to communicate to other teams.

## Test Dimensions

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
  
## Test Cases

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

## Test instructions

The test instructions were derived from the test cases in such a way, that the software testing could be directly conducted from them. Rather then being a one to one mapping one test instructions could cover multiple test cases or a test case could be divided between several test instructions. Nevertheless the test instructions were created in such a manner, that all selected test cases are covered by them and they do thus cover the planned test space.

### Conclusions 
Concluding the usability tests, we should note that one of the biggest usability issues was possibly not the application’s design, but Telegram’s API commands employed to run tasks and arrange people into game rooms. We should possibly consider wrapping them into buttons or providing extensive instructions on how to operate them.

