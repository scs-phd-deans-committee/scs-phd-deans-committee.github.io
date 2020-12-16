# SCS PhD Coffee Chats - A Semester in Review

_Author: Helen Zhou_

_The PhD Coffee Chats program was made possible by the collective hard work and insight of [the Social Connectedness Working Group](https://scs-phd-deans-committee.github.io/working-groups); thank you to Catherine King, Abhinav Adduri, Tobias Durschmid, Ritam Dutta, Roger Iyengar, Shilpa George, Leo Chen, and anyone else who helped at various stages of ideation, creation, and deployment!_

--

This semester (Fall 2020), we kicked off the inaugural SCS PhD Coffee Chats program! The goal of the program is to encourage spontaneous connection, friendship, and mentorship throughout SCS. In light of the pandemic, we felt it especially important to connect students of all years and departments in the School of Computer Science. Over the course of the semester we've received a lot of positive and constructive feedback, and heard from students who've wanted to implement smaller-scale version of coffee chats in their own departments. Thus, we figured we'd explain the details of the program in a reproducible way, including the matching algorithm, lessons learned, and some statistics about participation.

## How it Works - Participant View

To be matched for the following week, students fill out [this 2-minute form](https://forms.gle/bFBAQmfyTxbewdL3A) (edit access to a blank copy given upon request). Students must be signed into their school email account in order to fill out the form.

The form asks for the student's name, pronouns, department, year, 
availability, whether they prefer zoom or in-person interactions, and whether they want to be in a pair or placed in a group of 3 or 4 students. Then, they select which type of match they are looking for: friendship outside of work, research topic, and mentorship. 

Depending on the type of match they select, students (optionally) enter different supplementary information which they are subsequently matched on:

1. _Friendship outside of work:_ hobbies and interests (freetext and categorical), anything else they want us to know for matching (e.g. people they would not want to be matched with) (freetext)
2. _Research topic:_ research interests (freetext and categorical), anything else (freetext)
3. _Mentorship:_ whether they want to be a mentor or mentee, professional background (categorical), cultural background/ identity (categorical), anything else (freetext)
4. _Random:_ anything else (freetext)

Below is an example email that a participant would receive:

```
Hi Alice and Bob,

Thank you for participating in the SCS Coffee Chats program!

This week, we've matched you on the theme(s) of: Research topic.

Alice (she/her/hers) is in year 1 of PhD in the Machine Learning Department, and Bob (he/him/his) is in year 3 of PhD in the Computational Biology Department.

You are all available at: Friday Morning, Wednesday Evening, and can meet Over Zoom

Note: Please be respectful of each other's time, and try to confirm with your partner(s) in a timely manner. If you don't receive a response and would like to be re-matched, let us know and we can see if we can rematch you.

Alice's research topics/interests include:
time series, causality, healthcare

Bob's research topics/interests include:
neuroscience, time series, computer vision

To help kick off the conversation, here are some optional icebreaker questions/activities for when you meet:
1. What's the best piece of advice you've ever heard?
2. If you had 25 hours a day, how would you use your extra time?
3. Do a short workout together (e.g. 10 jumping jacks, 10 pushups, 10 sit ups).
4. Take a creative selfie/screenshot and send it to us! (feel free to use this email thread)

Finally, once you've had your coffee chat we'd love to hear how it went & any feedback/suggestions you might have: [link]

Happy coffee-chatting!

Social Connectedness Working Group (part of the SCS PhD Advisory Committee)
```


## Matching Algorithm
Code for the full matching algorithm is available at: [https://github.com/scs-phd-deans-committee/coffee-chats-public](https://github.com/scs-phd-deans-committee/coffee-chats-public)

**Greedy matching algorithm**: Within each match type (friendship, research, mentorship, or random) we use a greedy score-based algorithm where the scoring function depends on the match type. At a high level, given the scoring function, the matching algorithm works by computing a score for every possible grouping of 2, 3, or 4 students. Next, the algorithm acts greedily, selecting the highest-scored match first and removing its members from the remaining pool of potential groups. Then, the next highest-scored match is selected and removed from the pool. This process continues until everyone is matched. If there are any remaining unmatched people, then they are matched manually.

**Scoring functions**: For each match type, we define a scoring function that takes in a pair of individuals and outputs a score. To get scores for larger groups, we add the pairwise scores for all possible pairs in that group and rescale the sum. 

Across all match types, a logistical compatibility score is computed based on overlapping availability, location (zoom vs. in-person), and group size (pair vs. 3-4 people). In addition to logistical compatibility, we include the following considerations into the scoring functions for each match type (see the [code](https://github.com/scs-phd-deans-committee/coffee-chats-public) for complete details):

1. **Friendship**: Basic parsing of the freetext field is done to extract hobbies. The score increases with the number of hobbies in common, and a hobby is weighted more heavily if it is a rare hobby overall.
2. **Research topic**: Similarly, the score increases with the number of research topics in common, and a topic is weighted more heavily if it is rare overall.
3. **Mentorship**: The score increases if there are more cultural or background elements in common, and increases dramatically if one is a mentor and the other is a mentee.  The score also increases greatly if the mentor and mentee are in the same department, and if the mentor PhD year is greater than the mentee PhD year. If there are not enough mentors, individuals are pulled in from the Random match type, and scoring is done in the same way (except the Random individuals will have empty cultural and background fields). 
4. **Random**: There are no additional considerations (participants are solely matched on logistics).

At the end when all participants have been matched, a manual spot check is done for the matchings, and we check the "anything else" freetext to see if there are any issues with the matchings. If so, manual adjustments are made.

Finally, emails are sent to each matched group using an automated script that populates the body of the email with basic information about each person, as well as recommended logistics based on their responses.

## Feedback
Over the course of the semester, we went through several iterations of the program as we received feedback from participants. Here's a summary of the changes we made in response to feedback:

* including a short introduction of participants in the email, including pronouns, year, department, etc.
* suggest ice-breaker activities
* give guidelines to ensure proper social distancing
* ask participants to confirm times with each other
* allow participants to list people who they don't want to be matched with
* some people prefer small groups, while some prefer large groups
* after the form closes on Thursday, send out pairings by Saturday so they have time to coordinate in case they want to meet on Monday
* allow logistics to not play a factor in matching (i.e. some may want to work out a time on their own)
* provide specific multiple choices for hobbies 

In addition to constructive feedback, we got a lot of positive feedback. Here are some excerpts:

* "It went well!"/ "Great!"/ "Good!" variations (x19)
* "Excellent! I learned a lot of concerns incoming PhD students are facing in the COVID pandemic."
* "I think this is a very good idea. I got to meet new people which has been especially difficult in these times"
* "It went pretty well, got to hear interesting ideas of computational neuro research and talk about post phd career options"
* "It was amazing! Even though the research topic pairing didn’t match up well, we were able to find commonalities in our approaches to posing and answering scientific questions."
* "It went well. It was really helpful that we were in the same program, so I could provide guidance on faculty/courses."
* "It was great as usual. I was matched with my officemate, and I'm really glad to see a familiar face."
* "Great! Things went well; I had a nice hour and twenty minutes of conversation, and ended with a smile on my face. Thank you for running this program!"
* "Great! This was a really awesome way to meet people doing different things. We were all from different CS schools."
* "It was nice to meet a new friend that share similar CS research background."
* "I think a group of three people was more comfortable than a one-on-one chat would have been. Thank you for making that possible!"
* "Please keep coordinating these events! This was the first “spontaneous” research conversation I’ve had since Spring Break. I’ve really missed this."

## Participation Statistics
Here are some numbers-driven takeaways from our program:

* Over the course of six rounds of coffee chats this semester, we had **over 300 responses** to the coffee chat signup form. 
* Coffee chats helped first year students get to know other PhD students despite pandemic times&mdash;**over 40% of responses were from participants in their first year**. 
* Over 40% of responses **had a preference for being matched in a pair versus a group of 3-4**, with almost equal amounts preferring each.
* Over 10% of responses preferred to **only meet in person** (following state & CDC guidelines, socially distanced and wearing a mask). Note: in the future we may remove this option depending on recommendation from the school and local/national governments.
* **Friendship outside of work** was the most popular topic, followed by Random.
* Participants came from **all seven departments/ institutes** in the School of Computer Science.

Thank you to all of you who participated in and/or provided feedback on the program! If you're interested in getting involved or have any questions, comments, or concerns, please direct them to hlzhou[at]andrew.cmu.edu.


