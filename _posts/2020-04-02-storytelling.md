---
layout: post
title: Storytelling
sections:
 - title: Motivation
   tag: \#motivation
 - title: Characters
   tag: \#characters
 - title: Plot and storylines
   tag: \#story
 - title: Structure
   tag: \#structure
 - title: Storyline delivery methods
   tag: \#delivery
 - title: Outlook
   tag: \#outlook 
description: Learn more about the application's storytelling features.
---

<div id=“motivation”></div>
## Motivation

<div id=“characters”></div>
## Characters

<div id=“story”></div>
## Plot and storylines

(plot description)

In addition to the main storyline of the escape scenario, several session variants were designed. Each of these story variants consists of 4 blocks of text, in which the introduction and the conclusion stay the same as in the first storyline to keep the original setting and story frame. Meanwhile, the story in between differs with respect to the different escape rooms, the items to interact with, like a map, and the incidents, like a slipping team mate.

The story variants are created to keep the story engaging and interesting for users who have already interacted with the application but want to go on with their learning experience, even for those, who have already successfully escaped. 

<div id=“structure”></div>
## Block Structure
 
 
The structure of the story was divided into blocks. This was done in order to integrate the story with the tasks that are to be completed by the participants. The block is the representation of the stage of the escape the participants are in. The story right now is based on 5 blocks which allows the participants to take part in 3 tasks before they finish the learning process. The block flowchart basically shows the flow of the blocks and the blocks which are completed, and which are not.
The blocks that are completed: 
1)	Introduction (Block 1)
2)	Before Task 1 (Block 2)
3)	Between Task 1-2 (Block 3)
4)	Between Task 2-3 (Block 4)
5)	Conclusion Escape (Block 5)
The Blocks that are to be implemented in the coming semester: 
1)	Before task 4 (Block 6)
2)	Before task 5 (Block 7)

The signs that are denoting Success Message and Failure Message in other words are messages from the Elias and/or Harriet whether the participants have successful completed their tasks or not and the further steps to be taken in each case. The participants will be participating the different task before block 3, 4 and 5. After every completion of the task further steps would be indicated and the final escape is possible at block 5. 
	Keeping in mind the Adaptive learning module and the possibilities of task list being increased we have devised a flow in such a way that allows the expansion of more blocks before the participants can escape. This would mean that the participants will have to take part in more than 3 tasks in a session. For now, we have shown 2 more block which indicates that the participants will have to do 5 tasks and not 3 to escape. This gives the participants the option to escape after 3 escapes or do 2 more tasks. In this case they do not have to start a new session and start with the introduction block all over again. 

### Block Description
	This section we will talk about what every block will describes and what stage of story we are in during the sessions.
#### Block 1 
Is the introduction block. This block covers the overall plot and the backstory. It gives the basic understanding of the storyline to the participants. The summary of what is included in the introduction block is:
The users of our app are a part of the group of humans who was tricked into joining the aliens on one of their spaceships. They thought they would be entering into a collaboration with the aliens, but they realized they’ve been tricked. Now, they must find their way out of the spaceship
Also, Block 1 is presented to the participants in their personal chats. And all other blocks are in the groups joined by the participants. 
#### Block 2 
Block two is the interactions of the 2 bot characters, which are a part of the storyline, with the participants. The two bot personas are:
1)	Sympathetic Alien: Elias – He is the guide and the spokesperson for the adaptive learning module. He basically helps in adjusting the difficulty level of the task based on the participants performance. He is also responsible for taking feedback. 
2)	Hacker from Earth: Harriet – She is the Tester and task conductor to assess if they are humans or not.
The basic concept of the block 2 is to introduce them to the idea of doing tasks in order to escape. The participants start their first task after this block. A sample message from Harriet would look like:
		“Alright! However, at first, I must check whether you are really humans, not aliens, in order not to put humanity under the threat. In order to prove that you are humans you must complete a few tasks and prove your English knowledge. After successful completion, you’ll get a code to escape”
#### Block 3 and 4 
Block 3 and 4 are block which are between task. These blocks depend on the result of the performance of the participants in the previous task. There are 2 possibilities after finishing the tasks are failure or success. The outcomes of the tasks has the following outputs: 
1) Failure: A message from Harriet regarding how she cannot give the password to the participant and better luck next time. 
2) Success (if the task is completed successfully): A message from Harriet to move forward to other task and a suggestion from Elias in respect to the adaptive module. 
#### Conclusion block
The Conclusion block is the final block where the participants realizes whether they can escape or not. Basically, whether the participants have successfully finished all the tasks in the session. The two possibilities after finishing the task is:
1) Success: “Congratulations! You have taken the last step on the path to escape. I was glad to meet you, human friends, but it's time to say goodbye” Message from Harriet
2) Failure: “I'm sure there is still hope! You can start your escape again” Message from Elias

<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/semester_two/assets/images/Block%20Structure.png" alt="Figure 1: Block Structure" width="700">

*Figure 1: Block Structure*

<div id=“delivery”></div>
## Storyline delivery methods

The storytelling features described above are delivered to the users via a mix of textual, visual and audiovisual inputs.

Given its Telegram infrastructure, the default way to interact with the application is via text messages. The users begin their Escapeling journey by sending a simple /start message to the Telegram bot. Given this prompt, the bot is programmed to respond with an introduction to the Escapeling narrative. This introduction is delivered in the form of a second-person narration, beginning with the words “You regain consciousness and find yourself in a strange and unfamiliar place…”. See figure 1 to get a better sense of this first exchange with the bot.

<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/9b38a671bf4e6083b1d299d72c4f4168f60b07af/assets/images/StorytellingFig01.png" alt="Figure 2: First exchange with the bot" width="700">

*Figure 2: First exchange with the bot*

A focal point of this introduction is the delivery of a short informative video. The purpose of this medium is to immerse the users in the narration by hearing the characters’ voices and getting glimpses of the spaceship they are trapped on. The video is narrated by Harriet, who introduces herself and explains the situation to the users. Click [here](https://youtu.be/qIMSD-m6QaM) to view the video in full.

As a whole, this short introductory sequence is delivered to each user individually when they first come in contact with the application. Afterwards, the game progresses in iterations of group sessions (see #structure). During these sessions, the bot has three main modes of interaction: a) as a second person narrator; b) as Harriet; c) as Elias. These different bot personas are distinguished through the use of emojis (see figure 2).

<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/9b38a671bf4e6083b1d299d72c4f4168f60b07af/assets/images/StorytellingFig01.png" alt="Figure 3: Bot personas" width="700">

*Figure 3: Bot personas*

<div id=“outlook”></div>
## Outlook

After two semesters of work on the project, we have achieved a satisfactory sketch of the narration, and the visual media we presented are effective in guiding users through an introduction to the application and the storyline.

In the future, our wish is to create a more immersive game experience by including more visual and audio-visual content throughout the whole application. Moreover, we hope to give users more agency by providing additional possibilities for interaction, both with the characters and with the game environment. Finally, we intend to devote more time and thought to the further development of a captivating and motivating storyline.
