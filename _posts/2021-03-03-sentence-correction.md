---
layout: post
title: Sentence correction task
sections:
 - title: Design process
   tag: \#design
 - title: Implementation process
   tag: \#implementation
description: Learn more about the sentence correction task.
image:
---

## Contents

<span class = "content-overview"><a href = "#design">Design process</a></span>

<span class = "content-overview"><a href = "#implementation">Implementation process</a></span>

<span class = "content-overview"><a href = "#references">References</a></span>

<hr />

<div id="design"></div>
## Design Process

For task 2, the goal was to come up with an activity that, just like Task 1, gives users a chance to interact with one another. The other goal was to help users in another aspect of language learning than Task 1. In Task 1, users focus on grammatical error detection, and in Task 2, they are supposed to work on vocabulary learning.

For that, a word guessing task was designed in which each user is given a word within a certain difficulty level, and he/she is supposed to describe it and what it means to the other people in the group in order for them to guess the word. In case the person whose turn is up doesn’t know the meaning of the word given to him/her, there is an option to choose an alternate word. And in case each user successfully describes the word and other users guess it correctly, one round of the game is over, and according to the narration of the game, they unlock one part of the passcode to exit the escape room.

Finally, the task is timed so that participants do not have the chance to cheat (check the definition of the words outside of the game), and the pace is fast enough to keep the game exciting.

This task was designed in order to enhance vocabulary knowledge of language learners and for them to collaboratively expand their vocabulary. For it, the design group had to create a dataset of words that are appropriate for CEFR B1-B2 English learners of approximately 15 years old. These words were extracted from Cambridgeenglish.org that were divided according to their relevant topics into 11 subcategories. These topics were then translated into seven subskills obtained from Wordnet (Free_time, Humanities, Society, Nature and science, Ailment, Body and soul, and Home and building).

The next challenge was to categorize these words according to their difficulty level, which was not given in the dataset as in Task 1. Group 2, therefore, had to come up with features that helped them in determining word difficulty. One of the indicators of word difficulty is word length(Leroy et al., 2014) – short words tend to be easier to read. This feature could be easily computed by only relying on the list of words. However, word length does not cover all of the difficulty definition for a word.

Another feature that was helpful in measuring word difficulty was the number of word senses (Medero, & Ostendorf, 2009). This could not be inferred from the list of words alone and required an extra resource for which we used Wordnet. We extracted the number of senses each word had in Wordnet. This feature was negatively correlated with word difficulty (the more meanings a word has, the easier it usually is). However, this feature also had exceptions and in and of itself, was not sufficient to measure word difficulty.

The most prominent feature that was used in different studies to estimate word difficulty was word frequency (Chen & Detmar 2016; Rudell, 1993). This, by far, was the most difficult feature to extract as well because it required a large enough and widespread - among different topics - corpus to show a reliable result for word frequency within a language. For that, we used SUBTLEX US corpus and measured word frequency based on that. This feature was also negatively correlated with word difficulty.

With these three metrics, we now had to find a way to calculate word difficulty accordingly. Since the importance of each of these features in estimating word difficulty were not equal, we needed to find out to what extent each metric contributes to word difficulty. For that, these metrics needed to have non-equal weights for making a final unified measure for word difficulty. However, we didn’t know what weight to give to each of them.

<div id="neural"></div>

To find their weights, we created a hand-labeled small training set of these words chosen randomly to run a neural network over and find the proper weights for each of the above-mentioned features. As we expected, the highest weight was for word frequency, the second weight was for word length, and the least contributing feature to difficulty estimation was the number of senses. In the next step, we normalized all three of these features’ original values and then computed the sum of the weighted values (subtracting for the negatively correlated features). The resulting weighted sum became our final metric for word difficulty. We sorted the words according to this metric, and based on the three-level difficulty that we had in Task 1, we divided this sorted list into three difficulty levels.

Of course, we should keep in mind that this difficulty estimation does not result in 100% accurate categorization. One prominent counterexample is the word “businesswoman” that is low in frequency and is a rather long word, has only one entry in Wordnet, and according to all these measurements, it should be categorized as a difficult word (difficulty level 3). However, it is actually a rather easy word. This indicates that there is room for improvement, and semantic features of words (like difficulty) are not easily quantifiable by word’s formal features.  

<hr />

<div id="implementation"></div>
## Implementation Process

General information about the overarching task framework of our application can be found on the [implementation page]({{ "" | absolute_url }}/2021/04/02/implementation.html#task-framework). In sum, the sentence correction task is a subclass of `SequentialTask(Task)`, which in turn is a subclass of the abstract base class `Task(ABC)`.

The `SentenceCorrection` class inherits from its superclass `SequentialTask` in order to have all the sequential features. Since this class also has the property `self.selected_user`, it is trivial to implement turn-taking for this task. The function `select_sentence()` selects a sentence from the corpus, based on the selected user’s proficiency `self.selected_user.get_grammar_proficiency()`. When the sentence gets sent to the group, only answers of the selected active user will be evaluated. This allows the other players to contribute by assisting the selected user, e.g. by giving hints to identify the error.

<hr />

<div id="references"></div>
## References

Chen, Xiaobin & Meurers, Detmar. (2016). Characterizing Text Difficulty with Word Frequencies. 84-94. 10.18653/v1/W16-0509.

Leroy, G., & Kauchak, D. (2014). The effect of word familiarity on actual and perceived text difficulty. Journal of the American Medical Informatics Association : JAMIA, 21(e1), e169–e172. <https://doi.org/10.1136/amiajnl-2013-002172>

Medero, Julie & Ostendorf, Mari. (2009). Analysis of vocabulary difficulty using Wiktionary. SLaTE: 61-64

Rudell, A. P. (1993). Frequency of word usage and perceived word difficulty: Ratings of Kučera and Francis words. Behavior Research Methods, Instruments & Computers, 25(4), 455–   463. <https://doi.org/10.3758/BF03204543>
