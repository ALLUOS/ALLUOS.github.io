---
layout: post
title: Minor Improvements
sections:
  - title: Motivation
    tag: \#motivation
  - title: Bug-Fixes
    tag: \#bugs
  - title: Minor Improvements
    tag: \#minorimprovements
  - title: Outlook
    tag: \#outlook
  - title: References
    tag: \#references
description: Learn more about the application's achievement system.
image: pic04.jpg
---

## Contents

1. Motivation
1. Bug Fixes
1. Minor Improvements
1. Outlook
1. References

<div id="motivation"></div>
## Motivation

One of the main goals for the fourth semester was "Making the game playable". The wish for this goal was caused by a multitude of small to severe bugs and partial usability problems.
Of course, the bot was running before, but in some situations, it needed developer knowledge to complete the game: The game was usable, but it wasn't playable.

The following section will provide an overview of the problematic bugs and their fixes. Afterwards, there will be a section, regarding minor usability improvements.
Finally, we will talk about what's left to improve, what future works could and should do.

<div id="bugs"></div>
## Bug Fixes
This section is about errors that were made in programming as well as incorrect parts in the data, that have been corrected.

### The flood-error

_problematic Behaviour:_ After sending many messages, the bot stopped sending messages for a while. This often leads to omitted prompts and questions.
_intended Behaviour:_ The Bot should send every message.
*Explanation: Our code sends out requests to the telegram API. This API is responsible for sending and receiving messages. Since this API is running on the servers of Telegram FC-LLC and is available for free, access to it is somewhat restricted. Part of these restrictions is, that only 20 requests per minute are answered. Every other request is answered with the `telegram.error.RetryAfter` exception.
*Solution:\* To solve this bug, we decided, to catch the exception and resend the same request after 15 seconds. This leads to the user having to wait for a moment, but is still a better solution, than missing out on possibly important messages.
An alternative solution would have been the 'telegram.ext.MessageQueue'. Sadly it is being deprecated, which is why we ultimately decided against using it.

### Task-Pressuring

_problematic Behaviour:_ In the listening task, the users typically have 90 seconds, till one of them has to answer. Instead of waiting these 90 seconds, they could press a button on the inline-keyboard, labelled "I want to answer now". So far this button could be pressed by everybody, not only the person, who has to answer. This could, potentially, lead to pressuring the answering person.
_intended Behaviour:_ The button should only be clickable by the answering person. For all the other players it should tell them, that only the answering players are allowed to click this button.
_Solution:_ A simple if-block, to test, whether the person who clicked is the person who has to answer or someone else, resolved this problem.

### Amount of users was counted wrong / Problematic joining behaviour

_problematic Behaviour:_
_intended Behaviour:_
_Explanation:_
_Solution:_

### text improvements in task materials

_Behaviour:_
_intended Behaviour:_
_Explanation:_
_Solution:_

Possible future - Refinements

<div id="minorimprovements"></div>
## Improvements

### git fixes?

### levenshtein distance typo correction

*problematic Behaviour:*One of the complaints we got during our playthroughs, was that many of our tasks were not typo-friendly. In the vocabulary guessing task, when writing the correct word, the users got the message, that they are incorrect when writing an answer that was slightly misspelt. Even though the bot is, of course, right, when classifying them as incorrect, a message, that tells the users, that their spelling was incorrect would help them understand and solve their problem better. In the sentence correction task, the users could fail, because they misspelt a word.
_intended Behaviour:_ The bot should tell the user, that the word was spelt incorrect, instead of telling them, that the word itself is incorrect. A misspelt word should never lead to a failed task.
_Solution:_ The solution for this problem was the so-called "Levenshtein-distance". This is a string-comparison algorithm, that finds out, how many insertions, deletions and substitutions have to be done to change one string to another. If the amount of these edits was 1, the bot did send out a message, that implied, that there was a typo in the word. All above was still counted incorrect, all below 1 edit, was counted as correct.

### instructions: What to do, if the keyboard does not pop up

### html formatting

### multiple answers

### fixes for deployment of the bot on the server (Janosch)

| student_id | data_field                | value    |
| :--------- | :------------------------ | :------- |
| 4          | last_played               | 20210207 |
| 4          | consecutive_days          | 2        |
| 4          | highest_streak            | 5        |
| 4          | codeword_pieces_collected | 9        |

_Table 1: Exemplary Achievement Data_

<div id="outlook"></div>
## Outlook

<div id="references"></div>
## References
