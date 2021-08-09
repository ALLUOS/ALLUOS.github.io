---
layout: post
title: Listening task
sections:
 - title: Design process
   tag: \#design
 - title: Implementation process
   tag: \#implementation
description: Learn more about the listening task.
image:
---

## Contents
1. Whatever
1. Whatever
1. Implementation


<div id="architecture"></div>
## Technical Architecture

The task consists of two loops that sheath the answering function. The first loop is the “Iteration-Loop”. In there the topic of the next audio file is selected and the matching audio file is sent, using the newly implemented “send_audio”-function. After sending the file, the questioning-loop is started. In there a question, regarding the audio file, is selected from a group of up to 4 different questions. Additional to that, the answering Options are presented, and a User is announced who is supposed to answer. Now the Users have 90 Seconds to discuss the given answering options. To check for these 90 seconds, the function “time_is_up()” is being called to be executed in 90 seconds. If they have found a solution earlier, they can additionally press a button in the previous message, to skip their discussion time. Since the “time_is_up()” function was not able to change the state for technical reasons, a second type of state has been included: The answer-mode. Whether a group is in answer-mode is saved in the instance of the task. While the answer-mode is deactivated all the chat-messages are still reaching the “evaluate_answer()”-function, but they are not evaluated, but discarded as part of the discussion. If either the “time_is_up()”-function is called or the button is pressed, the answer-mode is activated. This causes the Bot to evaluate the next answer which is given by the earlier announced user. After the User gives the Answer, the answer-mode is deactivated again. If the Answer is correct the group gets a point. Otherwise, they do not. This process is repeated until each User in the group had one question regarding this topic. This closes the questioning-loop. If the questioning-loop is completed a new topic begins and a new Iteration begins. The complete task consists of three Iterations. If they were able to collect more than 2/3rds of the Points, they were successful. To get a better overview over the flow of the task have a look at figure one.

<img src="https://github.com/ALLUOS/ALLUOS.github.io/raw/master/assets/images/listening_task_flow.png" alt="Figure 1: Flow-Diagram for the listening task" class="center">

*Figure 1: Flow-Diagram for the listening task*

<div id="InlineKeyboard"></div>
## Inline-Keyboards:
This is the first time an inline keyboard is used in this bot. It is used in the form of a button that says “I want to answer now”. The receiver of the event, when this button is clicked, is a CallbackQueryHandler which calls the “time_is_up” function. A picture of the Inline-Keyboard can be seen in Figure two.

<img src="https://github.com/ALLUOS/ALLUOS.github.io/raw/master/assets/images/inline_keyboard_example.jpg" alt="Figure 2: Example of the inline-keyboard used for this task" class="center">

*Figure 2: Example of the inline-keyboard used for this task*

<div id="AccesToAudio"></div>
## Access to the Audio-files
To reduce the coding-Work a decision was made to collect the different topics in a json-file. The file consisted of multiple topics, which followed the structure which can be seen in the image below. One Topic contained the filename of the mp3 that gets played, the proficiency level needed as well as four or more questions and their answers.

<img src="https://github.com/ALLUOS/ALLUOS.github.io/raw/master/assets/images/audio_file_dict_scheme.jpg" alt="Figure 3: Example of a typical topic-structure for the listening-task." class="center">

*Figure 3: Example of the inline-keyboard used for this task*


<div id="exploits"></div>
## Possible Exploitations
There are two possible exploitations in this task.
The first being that anyone in the group could press the “I want to answer now”-Button, not only the one who is supposed to answer. This could lead to pressure from the other students and cutting short the discussion time.
The second possible Exploitation is, that, after the time runs out or the button is pressed, the game waits for the answer of the player who got selected. This is not timed in any way, which could potentially lead to the user waiting forever until he answers. In addition to that, all other students can still write, potentially giving tips, even after the 90 seconds.
These two problems could not be solved completely, due to time constraints and the shortage of manpower for the implementation of the task.
The first problem could be solved by checking who clicked the button and only calling the “time_is_up”-function when the right player clicks.
The second problem could be addressed by implementing a second timer, which counts down from ten.


<div id="references"></div>
## References
