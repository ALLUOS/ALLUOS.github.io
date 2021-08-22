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
1. Motivation
2. Task Flow
3. Materials
4. Implementation


<div id="motivation"></div>
## Motivation

The first three tasks have been designed to provide practice with core language skills concerned with literacy – the ability to read, write and construct sentences in a foreign language. However, as Renukadevi (2014) suggests, complete language proficiency depends crucially on another skill, namely listening. Being able to comprehend spoken words is necessary for face-to-face communication, which is the most common end goal of studying a foreign language (e. g., Morley, 2001). Not only is it necessary in order to come up with an appropriate response, but, importantly, it also helps students to acquire the correct pronunciation, word stress and a more confident use of vocabulary and syntax, which all improve the students’ speaking competencies. Yildirim and Yildirim (2016) as well as others have pointed out that ever since school curricula have transferred to a more communicative approach of foreign language teaching, listening skills have also been gaining in popularity in school environments. For this reason, we decided to create an additional task that can provide users with practice and more exposure to spoken English.

According to Gilakjani and Sabouri (2016), some of the most common issues for students with listening tasks are: quality of the materials, cultural differences, accent, unfamiliar vocabulary, as well as speed and length of the audio input. In order to make the learning experience meaningful for the users, we have taken these into consideration in our data preparation and more details can be found in the Materials section below.

<div id="task_flow"></div>
## Task Flow

Due to the fact that the game session with three previously implemented tasks already seems to be time intensive as well as requires some sort of perseverance, it was decided to make the new listening task voluntary. Participants are not obliged to do the listening but given a choice after successfully finishing the main part of the game. 

In the following, the description of the task flow will be presented in more detail.

In case the users choose to perform a listening task, they are at first presented with the instructions of what they are expected to do in the following task session. Then, an audio file will be sent to the users so that they can prepare for the upcoming questions. Thus, participants are able to start listening to a clip before actually seeing the related questions. After the audio clip was sent in the chat, the first question together with the three possible answers is being presented to the participants. The chatbot also sends the name of a random player who is selected to designate the group's final choice. Players are given 90 seconds to reflect on the question. They are not prohibited from revisiting the audio clip during this time. Players can either use all the 90 seconds for preparation or answer before the indicated amount of time has elapsed. After the selected player stated their answer, next question is being shown in the chat. Same procedure applies to all questions. In total, each text is assigned four different questions, three of which are being randomly sampled and presented during each game run. 

However, because of the Telegram chosen as a platform for the Escapeling, we have encountered a series of difficulties while designing this task. 

First of all, in the typical classroom scenario the list of questions is presented to the students before the audio task, which in turn is usually played twice. In case of the Telegram, it is impossible to control the amount of times users get exposed to the audio, since we did not manage to come up with a tool that can get the audio file deleted from the chat after a certain amount of times it was played or certain amount of time that has passed after the file was sent to the players. Thus, a new scenario had to be created in such a way, that it both satisfies the learning goals of improving the users' listening skills and seems to be suitable for its implementation in the Telegram. Hence, we concluded that presenting the audio before the questions was the most suitable solution implementation-wise as well as the means to get players interested in the task and better engaged.

<div id="materials"></div>
## Materials

At the moment, the audio database contains 45 recordings, each centered around a certain topic and labelled according to a difficulty level. All files come from publicly available websites which are intended for learners and teachers of English as a foreign language, namely [www.listenaminute.com](www.listenaminute.com), [www.elllo.org](www.elllo.org) and [www.esl-lounge.com](www.esl-lounge.com). We made sure that all sources legally allow the use of their audio materials for non-commercial purposes.
 
The users should be familiar with a task of answering questions related to the heard materials, as this methodology is often used in language learning courses. However, in order to create an experience that resembles the real world rather than a test environment, the users find out the questions after, not before listening. In such cases it can be challenging to catch every detail at first attempt, so we decided to allow them to self-regulate the number of repetitions.
 
Along these lines, to keep the overall length of the task to an appropriate time window even if the users decide to listen more times, we decided to standardize the length of every audio file to approximately one minute. Files of such length are also going to be much easier on the device memory storage of the users, which is certainly an added plus.
 
In order to provide a more authentic learning experience, we aimed to provide a good variety of speakers with regard to their voices and accents, as well as a wide range of everyday topics, for which intermediate learners are expected to have the required core vocabulary. Some examples include Health, Media, French Fries and Job Duties. Unfamiliar accents can severely hinder students’ abilities to understand the spoken words (Gilakjani & Sabouri, 2016), so only speakers of the most common native English accents are included in the final data set.
 
Since the maximum number of players is four, we have designed four questions per audio, each with three possible options to choose from where exactly one is correct. The focus of the questions is also diverse. Some require a general understanding of the speaker’s attitude about the topic, others ask for a more detail-oriented listening or distinguishing between homonyms (equally or similarly sounding words) based on context.
 
In terms of the difficulty level, the main motivation for the division was to eventually accommodate the adaptive module, which assigns a specific question to the users based on their previous performance. There does not seem to be a unified official method for determining the difficulty level of a listening task. Therefore, our process was as follows: the content (in form of a textual transcript) was first evaluated based on the language complexity and vocabulary using a combination of automatic tools ([1](http://www.roadtogrammar.com/textanalysis/), [2](https://cefr.duolingo.com)) to indicate a CEFR level [3] of a text. In addition to this indication, the data was also subjectively evaluated for speech qualities such as tempo and clarity, and the final level of difficulty resulted from agreement between three judges.
 
Although our target audience is mainly B1-B2 students, since the intermediate proficiency group tends to be quite heterogeneous in terms of the individuals’ strengths and weaknesses, we also included slightly simpler audios that could theoretically be used with motivated A2 students, too.
 
Each recording was carefully reviewed by at least three people, checking for the sound quality of the recording, correctness of the language and multiple choice questions, flagging potentially sensitive subjects or unsuitable language, and as mentioned, checking the indicated level of difficulty.


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

[1] http://www.roadtogrammar.com/textanalysis/ (accessed 29.06.21)

[2] https://cefr.duolingo.com (accessed 29.06.21)

[3] Council of Europe (2020), Common European Framework of Reference for Languages: Learning, teaching, assessment – Companion volume, Council of Europe Publishing, Strasbourg, available at www.coe.int/lang-cefr

Gilakjani, A. P., & Sabouri, N. B. (2016). Learners' Listening Comprehension Difficulties in English Language Learning: A Literature Review. English language teaching, 9(6), 123-133.

Morley, J. (2001). Aural Comprehension Instruction: Principles and Practices. In M. Celce-Murcia (Ed.), Teaching English as a Second or Foreign Language (pp. 69-85) Boston: Heinle and Heinle.

Renukadevi, D. (2014). The role of listening in language acquisition; the challenges & strategies in teaching listening. International journal of education and information studies, 4(1), 59-63.

Yildirim, S., & Yildirim, Ö. (2016). The importance of listening in language learning and listening comprehension problems experienced by language learners: A literature review. Abant İzzet Baysal Üniversitesi Eğitim Fakültesi Dergisi, 16(4).
