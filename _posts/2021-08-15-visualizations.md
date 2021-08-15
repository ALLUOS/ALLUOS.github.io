---
layout: post
title: Visualizations
sections:
 - title: Motivation
   tag: \#motivation
 - title: Theoretical Background
   tag: \#background
 - title: Conception
   tag: \#conception
 - title: Implementation
   tag: \#implementation
 - title: Outlook and Discussion
   tag: \#discussion
 - title: References
   tag: \#references
description: Learn more about the design and implementation of our application's visual elements.
image: pic11.jpg
---
<div id="motivation"></div>
## Motivation
 
Visualizations play a central role in the field of user experience. Because of their great potential, they were included in Escapeling. Not only in the previous semester's report it can be found in the outlook that the application should contain more visualizations, but also this semester the idea was brought forth, especially regarding adaptive and situational elements. Visualizations can be simple emojis, as well as situational gifs or adaptive images.
The usefulness of visualizations in a learning environment has been proven in many ways. Our app is explicitly not about visualizing vocabulary using images, for example, as is common in beginner courses. Rather, different types of visualizations have been used as supporting elements to both enhance the user experience and maximize learning.

<div id="background"></div>
## Theoretical Background

Visualizations "displayed in online interactive e-learning (...) represent a critical aspect in teaching and learning" (Jusoh 2019). Among other things, the study shows that visualizations in e-learning increase the interest in the subject matter (Jusoh 2019). Furthermore, visual stimuli can help to focus on the learning content as well as to remember what has been learned (Jusoh 2019) (Zallio 2018).
The emotional aspect of emojis and GIFs as facilitators in learning should also not be underestimated. According to ISO 9241-210, the user's emotions are also part of the user experience. In a study by Zallio (2018) that examined the use of emojis and GIFs to promote student engagement, these visualizations were used "in order to stimulate the emotions such as hilarious and ironic feelings among young students". This is consistent with other findings highlighting the role of emotions in learning processes. For example, it “is known that long-term memory retention is greatly aided by the emotional associations of that memory" (Zallio 2018).
Finally, visualizations also contribute to an appealing design of the learning application. According to Don Norman, a professor well known in the field of design, attractively designed applications are evaluated as more usable by the user than unattractive ones. To a large extent, this is due to the influence of emotions (Norman 2005). This could improve the user's subjective evaluation of the learning app, as well as their feelings about the app's usefulness.

<div id="conception"></div>
## Conception

The selection of the type and manner of visualization elements was based on both the use of the same by our target audience, mostly 15-year-old teenagers, and the functionalities of our app. The visualization elements include GIFs, emojis, stickers and adaptive images. 

### GIFs

<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/semester-three/assets/images/visualizationsIMG01.png" alt="Figure 1: GIF showing the view out of the spaceship after escaping the aliens" class="center">

*Figure 1: GIF showing the view out of the spaceship after escaping the aliens* 

GIFs are short, constantly repeating videos. On the one hand, they can have a pictorial character, such as a GIF shown at the end of the Escapeling story, where the player flies back to Earth in a spaceship, seeing the Earth approaching him. This view is shown in a GIF. 
Furthermore, a GIF can be used to draw focus to important learning sections. According to a study by Zallio (2018) "it is possible to confirm that it is easier for students to better understand certain topics after showing emoticons or GIFs. (...). This is an element that highly stimulate the attention and by levering on emotions, students are more likely to focus their attention after the given emotional stimuli". For this reason, each of the learning tasks is introduced with a GIF. This marks the intersection between the previous storytelling and the subsequent task, which again fully demands the user's attention.
The third function of using GIFs in Escapeling, is to provide feedback and motivate. For example, a GIF can be sent in response to a correct answer, or to repeated failure to guess a vocabulary word.

### Emojis
 
<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/semester-three/assets/images/visualizationsIMG02.png" alt="Figure 2: Emojis in the private chat selection menu" class="center">

*Figure 2: Emojis in the private chat selection menu*

Emojis are small graphics that can be in a line with text. They can be emoticons, representing emotions, as well as representing unemotional things. For quicker orientation, for example, emojis were inserted in the private chat selection menu. This additionally made the previously completely text-based selection a bit more colorful, and various studies have proven that colors can stimulate the brain during learning (Ferrari, 2008) (Althouse, 2003).

### Situation Dependent Images
 
<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/semester-three/assets/images/visualizationsIMG03.png" alt="Figure 3: Image showing the remaining time for a task" class="center">

*Figure 3: Image showing the remaining time for a task*

In some tasks it is useful to know how much time you have left to complete the task. Since Telegram's API does not allow the implementation of a progress bar, other ways had to be found to implement this. The challenge was solved by sending an image at certain points in time showing the time left to complete the task. 

### Stickers
 
<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/semester-three/assets/images/visualizationsIMG04.png" alt="Figure 4: example sticker of the alien Elias" class="center">

*Figure 4: example sticker of the alien Elias*

Stickers are images that have no background. They are displayed much larger than emojis in Telegram, the messenger service used for Escapeling. To enhance the effects of storytelling, a standalone sticker pack was developed that incorporates various components of storytelling. The full contents of the sticker pack can be seen in Figure number 5. It includes the main characters of the story, Harriet and Elias. Since emotions can be an essential factor for learning and provide a better experience of the story, different emotional states of the main characters were visualized, such as joy, disappointment or sadness.
Other storytelling elements were also visualized with the help of stickers, such as the code panel, on which the player is supposed to enter a code several times during the game.
 
<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/semester-three/assets/images/visualizationsIMG05.png" alt="Figure 5: All stickers belonging to the Escapeling sticker pack" class="center">

*Figure 5: All stickers belonging to the Escapeling sticker pack*

### Achievements

As part of the visual overhaul of the app, the medals awarded in the "Achievements" section have also been visualized. You get these for playing Escapeling frequently, for example. Previously, these were purely text-based, but now the player gets a picture of their current status sent to them.   

<img src="https://github.com/ALLUOS/ALLUOS.github.io/blob/semester-three/assets/images/visualizationsIMG06.png" alt="Figure 6: Image that the user gets when he has reached the status of an Admiral as part of the achievements" class="center">

*Figure 6: Image that the user gets when he has reached the status of an “Admiral” as part of the achievements *


<div id="implementation"></div>
## Implementation

The changes mentioned above have been implemented as follows:

### Emojis

Emojis are handled by tags like “:alien:”. These tags where transformed to the correct emoji by using the [emoji-package for python (Kim and Wurster, 2021)](https://pypi.org/project/emoji/). 

### Stickers

Stickers in telegram are handled by using so called IDs. If you upload a sticker to telegram an ID is created. To send out a sticker, get_sticker_id() is called to retrieve the ID out of a .json-file, based on a name. This improves readability and centralizes the organization of the stickers in the .json file.
The Escapeling sticker pack was created by freehand drawings on an iPad using Adobe Fresco software. To turn these drawings into a Telegram sticker pack, they were sent to a Telegram bot called @Stickers.

### GIFS

Since Telegram does not support an ID system for GIFs we used URLs from GIPHY, a website that collects GIFs. These URLs to the GIFs are organized based on Keywords in a .json file structure, which can be accessed by using get_gif_link(). GIFS can be sent into the chat with send_animation().

### Images

The different achievement-images have been created using GIMP 2.10. In the future it could be possible to display additional Achievement-information as additional layers over the current pictures, resulting in one picture depicting all achievements at once. This could be done by using the python image library (PIL). They can be sent into the chat using send_image().


<div id="discussion"></div>
## Outlook And Discussion

The visualizations received very positive feedback after initial user tests, which is why it can be assumed that they achieved their goal of enhancing the user experience. In future implementations of further tasks and changes to storytelling elements, care must be taken to select suitable visualization elements or to adapt existing elements.
One point that should not be ignored, however, is that too many graphics could distract learners more than focus them (Zallio 2018). In addition, especially GIFs and emojis could lower the seriousness of the application (Zallio 2018). While it is desired to break away from what is often perceived as a strict school learning environment and also to convey the joy of learning, Escapeling should also not be perceived as chat game without a serious English learning intention. 
In order to avoid these effects, it was important not to include too many visualizations at once and to use GIFs and emojis in a well-dosed way. Further user tests will show whether this was successful.

<div id="references"></div>
## References

Althouse, R., Johnson, M.H., & Mitchell, S.T. (2003). The colors of learning: Integrating the visual arts into the early childhood curriculum; Vol. 85, Teachers College Press

DIN EN ISO 9241-210:2020-03, Ergonomics of human-system interaction - Part 210: Human-centred design for interactive systems (ISO 9241-210:2019)

Ferrari, V., & Zisserman, A. (2008). Learning visual attributes. Advances in neural information processing systems, 433–440.

Jusoh, S., Almajali, S., & Abualbasal, A.M. (2019). A study of user experience for e-learning using interactive online technologies. Journal of Theoretical and Applied Information Technology, 97(15), 4036-4047

Norman, D. A. (2005). Emotional Design: Why We Love (or Hate) Everyday Things. Basic Books.

Zallio, M., & Damon, B. (2018). Computer Aided Drawing software delivered through Emotional Learning. The use of Emoticons and GIFs as a tool for increasing student engagement. 1-4. 10.14236/ewic/HCI2018.75.





