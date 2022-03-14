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
This section is about errors that were made the code. It describes, how the bug influenced the game, what caused it and how we solved it.

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

_problematic Behaviour:_ Right at the start of the group room, the players were asked, whether they are ready. If a player responded with "Yes", they got a message consisting of a ✔️-emoji for everyone who is ready and a ❌ who did not answer yet. If all people stated, that they were ready the game started. One problem was, that this feature sometimes showed the wrong number of participants. This could lead to the bot not starting because it thought, there was a fourth person. An alternative condition to start would be, that if at least 3 people were ready and a certain time passed, the game started.
The problem with this was, that the time started after one of the first messages, that was sent in the chat. In combination with the increased time per message, that we implemented in semester 3, this lead to the time already being passed, before the players were able to state their readiness. In turn, this lead to the bot starting immediately after the third person wrote, that they are ready, even if there was a fourth person present.
_intended Behaviour:_ The bot should only start, if all the players, that want to play, wrote, that they were ready.
_Solution:_ We fixed the counter, that was counting the players in the group and decided to remove the countdown. Instead of the countdown, we decided to have an inline-keyboard, that allowed to "start without the missing players".

<div id="minorimprovements"></div>
## Improvements
This part describes the small things, that we reprogrammed in order to make the user experience better and decrease the difficulty of the game.

### levenshtein distance typo correction

_Problem description:_ One of the complaints we got during our playthroughs, was that many of our tasks were not typo-friendly. In the vocabulary guessing task, when writing the correct word, the users got the message, that they are incorrect when writing an answer that was slightly misspelt. Even though the bot is, of course, right, when classifying them as incorrect, a message, that tells the users, that their spelling was incorrect would help them understand and solve their problem better. In the sentence correction task, the users could fail, because they misspelt a word.
_How it should be:_ The bot should tell the user, that the word was spelt incorrect, instead of telling them, that the word itself is incorrect. A misspelt word should never lead to a failed task.
_Solution:_ The solution for this problem was the so-called "Levenshtein-distance". This is a string-comparison algorithm, that finds out, how many insertions, deletions and substitutions have to be done to change one string to another. If the amount of these edits was 1, the bot did send out a message, that implied, that there was a typo in the word. All above was still counted incorrect, all below 1 edit, was counted as correct.

### html formatting

_Problem description:_ Some players noted, that important pieces of information, like codewords, were hard to find in chat, because of the sheer amount of textual input.
_How it should be:_ We decided, that the saliency of these key-informations should be increased.
_Solution:_ Therefore we decided to use the HTML formatting option. This way certain information could be highlighted, by changing the colour, underlining it or printing it boldly. Passcodes were even more highlighted, by using the 1️⃣2️⃣3️⃣-emojis.

### multiple answers

_Problem description:_ In some cases one question allowed for multiple correct answers. For example words like "theatre" and "theatre", that change, depending on if British or American English is used, or words like "can't", that are similar in meaning to "can not" or "cant". Up until this semester, the other options were simply considered incorrect.
_How it should be:_ These alternative answers should work as well.
_Solution:_ We implemented an option to have multiple correct answers in the database. Therefore the pipeline to check, if a word was correct had to be adjusted as well.

### instructions: What to do, if the answering options do not pop up

_Problem description:_ Especially people who never worked with a bot on telegram, often wondered, how to reopen the answering options, if they accidentally closed it.
_How it should be:_ The bot should show, how to open the answering options.
_Solution:_ The first thing the bot sends to the user now, is a small explanation of how to open the answering options.

### fixes for the deployment of the bot on the server (Janosch)

TODO @Janosh

### Github-fixes?

_Problem description:_ At the beginning of the Semester multiple people had the problem, that their GitHub refused to correctly pull the current branch.
_Explanation:_ This was caused by git LFS, where large files were stored. Since semester 3 added a huge amount of .mp3 files and a big model for the use of the grammatical error correction, the amount of data, that was allowed to be downloaded from git LFS has quickly been exceeded.
_Solution:_ The solution was to put these files into an external drive, where they could be downloaded separately. Simultaneously these big files were removed from the Github repository. If the Bot is started without these files, it gives out clear instructions on how to download and insert them into the file structure.

<div id="outlook"></div>
## Outlook
- installer für 
TODO @ Frederik
