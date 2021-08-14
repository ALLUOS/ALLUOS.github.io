---
layout: post
title: Discussion task
sections:
 - title: Design process
   tag: \#design
 - title: Implementation process
   tag: \#implementation
- title: References
   tag: \#references
description: Learn more about the discussion task.
image:
---

<div id="design"></div>
##Design process

###Motivation
In review of the intermediate results from semester one of the study project, we decided to design and implement one additional task during the second semester, which includes a number of aspects to complement the first two tasks and approaches the aim of the group application further.

Our goals for the application include tasks which benefit from group learning and encourage English communication between learners which is essential for practicing language skills. Therefore, for this task we want to focus on language production and writing skills, as the first two tasks don’t necessarily require users to write in whole sentences.

Finally, resulting from what was observed in the first semester, the goal is to allow for more direct and natural communication of the users instead of a sequential task pattern. Interaction between the individuals should inspire discussions among users in English and help them practice and advance their skills together.
For these reasons, the aim for the third task is to stimulate and to guide a discussion between users concerning a certain topic.

As progress in communicative competence of the learners is the main requested outcome of this third task, the Communicative Approach to language learning, also called Communicative Language Teaching or Functional Approach (Jin, 2008), served as the basis for the design and the idea of the discussion task. An important aspect of the Communicative Approach is the focus on meaning in contrast to structure (Jin, 2008). Learners are emboldened to interact with each other, for example, in dialogues in order to fulfil a task. One important aspect of the task in this framework is, therefore, the function and not the accuracy of language (Jin, 2008). The developed linguistic skills within this functional dynamic can then be recalled by the learners in situations requiring similar abilities with a higher likelihood (Barson, 1993).
Another important aspect of communication is, nevertheless, correctly using grammatical structures. Therefore, based on the results of previous semesters, in semester three of the project we add an additional learning goal to the discussion task: The users are encouraged to practice some important grammatical rules and are also provided qualitative feedback regarding their language.

The input for the discussion task is a short text with information about a specific topic in different areas which serves as a departure point for content-based language learning. The main idea thereby is to use language as a medium rather than explicitly study it (Lyster, 2018). These areas cover different domains which were already used for the vocabulary subskill categorization during the [vocabulary task]( link to vocab task). After this short input, the users are invited to discuss the topic, guided by three questions.

### Task Flow

In the first step, the reading material is presented to the users in a short text. On the one hand, the goal of the text input is to give an introduction to a topic which users might not have been in contact with, yet, and offer background knowledge for the discussion. On the other hand, the text supports the learner’s context-related vocabulary. This way, English learners are already presented with example uses of words and phrases. Additionally, vocabulary learning from context may not be more efficient, but more permanent and stable and is central to most everyday learning settings (Sternberg, 1987). The introductory text is explicitly short to be easily read from a telegram chat and not to be overwhelming for the learners.

Subsequently, a first discussion question is presented to the users. These questions allow for different opinions on the topic and should stimulate the exchange of these. After a time limit is reached, the [completion criteria](link) for the task will be updated and the users will be provided intermediate feedback on their performance. Then, the next question will be prompted to view the topic from another angle. This procedure continues in a loop until the third question is reached. The diversity of questions allows for a bit more reserve from users who might have problems to answer one of them because they can in the following contribute more for another question.
Final feedback on users' participation closes the discussion task.

### Theoretical framework

A great number of existing studies have appraised the role played by discussion tasks in second/foreign language acquisition. In the introduction to this section, we explained the motivations and intentions that led us to develop a discussion task. We wanted to build a space for our users to interact more freely among each other, and prior research suggests that group discussion relaxes the stress on the learners because the focus is not on getting a specific target exercise right, but rather on communicating more generally (Puspitasari 2016). Naturally, a number of specific design choices had to be made when deciding how to tailor a discussion task to our specific learning environment. In this section we will review the literature that informed each of these choices.

As already noted, we wanted this task to enhance language production skills. Discussions are a staple of classroom teaching, not only in the context of second language learning. However, discussions are critical for language learners especially (Knight 2014). Discussions provide opportunities to produce language in contextualized and purposeful ways: learners can practice applying form (e.g., grammar, vocabulary) and function (e.g., language used to clarify, explain, argue) to communicate and build ideas. Moreover, learners benefit from redundancy of ideas and the vocabulary associated with them: discussion allows for multiple opportunities to hear new concepts and content continuously explained, analyzed, and interpreted. Furthermore, discussions via written communication have been likened to the immersive experience of studying abroad (Oliva & Pollastrini 1995), notoriously effective for language learning.

A number of authors have recognized that group discussions are facilitated by an online learning environment. “Electronic communication”, as it used to be called, has been integrated into the second language classroom ever since the 1980s (Warschauer 1996). Most relevant for us, several studies and reports have indicated that computer-assisted classroom discussions are great equalizers of student participation (Beauvois 1992, Kelm 1992, Kern 1995, Sullivan & Pratt 1996). Among the recognized merits of online discussions, we find that it lowers the affective filter associated with student production (Beauvois 1997), that it improves motivation (Oliva & Pollastrini 1995), and that it favors greater syntactic complexity (Kern 1995) compared to in-person discussions.

The inclusion of an initial text prompt is meant to address one of the shortcomings of group discussions pointed out in the literature, namely that insufficient content knowledge is a key issue which inhibits students’ active participation (Han 2007). The text prompts are meant to bring users up to speed on the core ideas of the topic under discussion, creating a pool of shared knowledge which the users can then collectively build upon.

The inclusion of guiding follow-up questions is meant to frame the bot as a discussion facilitator: effective discussion facilitation is one of the key skills required for teachers who want to conduct fruitful discussions in class (Finn & Schrodt 2016). This is in line with the framework of Guided Participation (Rogoff 1990), where students socially interact with their peers with guidance from a teacher (in this case, the bot). The guiding questions are also meant to steer the discussion in stimulating and engaging directions, thus avoiding quick stagnation in the opinion exchanging process.

A final key point in the implementation of a discussion task for our learning environment is deciding when and how a discussion can be considered “complete”, which is strictly related to the issue of user performance evaluation. Given the importance of this issue, we return to it on a section of its own (see completion metrics).

### Materials

The idea of a discussion task was based on the assumption that the improvement of existing foreign language skills, as well as the acquisition of new linguistic skills, can be formed as a result of a natural dialogue between the English learners. We believe that initially providing users with a text on a specific topic is the best way to engage them into interaction with each other. Thus, there are two types of data required for the discussion task: texts, that should be informative and interesting enough to spark a discussion, as well as follow-up questions based on the given texts.

As the first step, we defined 9 different categories, which can serve as guidelines for collecting texts: Body and soul; Free time; Home and building; Humanities; Nature and science; Society; Technology; Alignment; Economy. Each text should then be a subtopic of one of the stated above categories.

Since our application is a Telegram chatbot, we assume, it will be used mainly by phone users, which provides a series of other issues that we have to take into consideration. First of all, the size of a phone screen is relevantly small and does not allow for sharing big texts with the users. Secondly, even if this problem can potentially be solved by presenting text in a form of an image, we want to avoid situation, in which users loose access to the chatroom every time they open the picture. Hereby, it was decided to present texts as messages. The ideal size of the text then to be sent to the users was determined to be maximum 120 words.

The data was collected from different Internet sources. The sources varied from Wikipedia pages to blog posts, articles in online magazines and materials posted on online platforms for English courses (see Excel sheet with all relevant information provided). Our next step was then to select a small excerpt from the discovered source that should be of the stated size. Such an excerpt should give an overview of the specific topic, we would like English learners to talk about, as well as present this topic in such a way that enables users to get engaged into conversation easily.

Our second step was to review these texts and check if all of them meet the following criteria:
1. Is this text interesting enough to spark a discussion?
2. Does this text allow for sharing different opinions?
Besides, the texts should be appropriate for our target group and grammatically correct. In total, 37 texts were collected.

Since our chatbot should be able to provide users with tasks of different difficulty (namely, levels 1, 2 and 3), our next task was to rate the texts according to the levels of their proficincy. This could be achieved in two different ways: via automatic evaluation and self evaluation, based on the information provided in the scientific literature. The automatic text analyzer by roadtogrammar.comdetermines the text difficulty according to the CEFR levels, not only by looking at average word and sentence length, but also comparing each word to a list of the 10,000 most frequent words in English. This is relevant for evaluating language complexity of the found texts. The more of frequent words is presented in the text, the less difficult this text is rated to be. Text evaluation we did by hand, on contrary, was based on the idea that criteria, used by text analyzer, are only surface features of text complexity and are limited in their capacity to predict text difficulty. These features should be taken into account in combination with the deeper features (ideation, organisation, structure, representational models) and reader's characteristics (background knowledge, motivation and goals of engagement in reading). For example, text difficulty is often dependent on the genre, with non-fiction texts usually presenting more challenges to the readers (Murphy, 2013).

Having combined both these attitudes towards text evaluation, we ended up in dividing all texts according to three language proficiency levels, that we focus on in this chatbot.

Further, the next step was to provide texts with three different questions each. Our goal was to avoid questions on text comprehension, but find those that can encourage users to share their thoughts and opinions. Below you can see an example, illustrating questions we came up with for the text about Christmas Shopping (Category: Economy):
1. Where do you personally prefer to buy your Christmas presents?
2. What are the advantages and disadvantages of shopping on the internet?
3. What is better: giving or receiving gifts? Why?

All questions were written by hand and checked for grammatical correctness afterwards. At the same time, exactly the latter turned out to be the main difficulty we encountered while working with the texts, as well as in the process of subsequent compilation of questions, while we are not native English speakers. Overall, all the obtained materials are considered suitable for English learners with different interests and allow the exchange of different opinions.

### Completion Metrics

 The goal of the “Discussion Task” is to motivate users to converse and make sentences around certain topics, to use grammar and vocabulary as well as expressing their opinions in the second language they are learning (English here). Here, the goal is primarily to make a clear, understandable conversation and be able to understand what others say and express one’s own opinion, while also paying attention to basic grammatical rules. Another goal is to generate sentences and through that, become more proficient in expressing oneself in the second language. So in short, language comprehension and expression are the goals of the discussion task.

With these goals in mind and since the users’ outputs in this task, compared to the previous two tasks (sentence correction and vocabulary guessing) are less quantifiable, we needed to define a set of metrics to decide when the task is done and whether it is done successfully or not.

Therefore, for the completion metrics of this task, we came up with the following items:

Computing grammar scores, user scores, a group score. Reference GEC section.
Each user must contribute a minimum number of words to the topic in discussion. This is tracked in the individual user scores.  
Alongside the first metric, we set another requirement for considering the task as successfully done, and that is, if the users participate in the discussion of at least 2 out of 3 topic questions offered to them. This is tracked via the mean scores.
Storytelling related levels (put into table below).

<p align="center">
Completion Metrics
</p>

| Metrics | Pass | Fail |
| ---- | ---- | ---- |
 | A. What is your overall rating of the group performance in this task? (5 being the best)| Average score >= 3 - Task is completed | Average score < 3 - Task is not completed |
 | B. Number of the generated words (overall) | >= 400- Task is completed | < 400 - Task is not completed |
 | C. Number of the words each user generated during the task | >= 100 - Task is completed | < 100 - Task is not completed|
 | D. Participation in at least two topic questions | >= 2 - Task is completed | < 2 - Task is not completed |


In addition, there is a time limit to the task and each topic question being asked and answered, to make the it manageable and engage users to participate within a limited time frame.

### Outlook

We think that the discussion task is a great new feature of our application and a nice addition to the first two tasks. This could even be further developed in the future.

Possible future extensions of the task can include messages from the bot to discuss with users. An advantage of these contributions to the discussion can be more guidance and the effect of “model answers” which can serve for less experienced English speakers as an orientation. This also includes examples for the use of discussion phrases which can add to the learning effect.

Another additional feature can be corrections and helpful hints by the bot. Meanwhile, the bot should be similar as Barson (1993) describes the teacher in a similar classroom setting: only play a supportive role and help discreetly to identify errors while mostly appreciating the learner’s contributions.

Furthermore, vocabulary hints and short descriptions of words which are possibly new to the learners would be a valuable addition. This way probably an even bigger vocabulary learning effect could be achieved.


<div id="references"></div>
### References
Barson, J., Frommer, J. & Schwartz, M. (1993). Foreign Language Learning Using E-Mail in a Task-Oriented Perspective: Interuniversity Experiments in Communication and Collaboration. Journal of Education and Technology, 2 (4), 565-584.
Jin, G. (2008). Application of Communicative Approach in College English Teaching. Asian Social Science, 4 (4), 81-85.
Lyster, R. (2018). Content-Based Language Teaching. New York: Routledge.
Sternberg, R. J.(1987): Most Vocabulary is Learned From Context. In: M. G. McKeown & M. E. Curtis (Eds.) The Nature of Vocabulary Acquisition (pp. 89-106). New York, London: Psychology Press.
