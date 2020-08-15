---
layout: post
title: Summary and Evaluation
description: Under construction!
image: pic04.jpg
---


### Test Strategy

A summary of the goals and straegies we used in our testin.
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
