---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: "Yale S&DS 3350/5350: Social Algorithms"
---
<!--
## NOTE: Sylalbus in progress -- will be removed when syllabus is finalized.
-->


## Course Description

Algorithms that learn from data play increasingly central roles within modern complex social systems. In this course, we examine the design and behavior of algorithms in such contexts, including search, content recommendation, social recommendation, feed ranking, content moderation, and more. The course has a split focus on the technical design of such algorithms, as well as the literature on theoretical and empirical evaluations, particularly in the presence of strategic behavior, network effects, and algorithmic confounding.

## Instructors
Instructor: Johan Ugander (johan.ugander@)

TF: Zhongren Chen (zhongren.chen@)

## Schedule

**Class**
MW 9a-10:15a, KT 205

**Office Hours**
Johan: W 10:30-11:30 KT1325
Zhongren: TBD

<!--
* Greg: Tues 11:30a -- 1:30p (starting October 4) @ Encina W 101
* Monte: Weds 3:00p -- 5:00p (starting October 5) @ Littlefield 103
	* **NOTE:** Monte will hold office hours on Monday, 10/17 from 4--6pm @ Littlefield 103 instead of Wednesday to accommodate the new Assignment 1 deadline of 10/18.
* Johan: Thurs 10:20a -- 11:15a (starting Sept 29)
-->

## Important Links
- [Canvas](https://yale.instructure.com/courses/116525) 
- Github TBD

<!--
* [Canvas page](https://canvas.stanford.edu/courses/x)
* [course Github repo](https://www.github.com/mse231/mse231_f22)
-->

## Course structure and evaluation
The class is organized around two lectures per week. Generally speaking, the first lecture of each week will on the technology, with the second lecture of the week being focused on impacts and evaluations. Lectures are designed to be discussion-driven, especially when we debate conclusions from the impacts and evaluations literature.

There are three problem sets and a project. See the weekly schedule below for a timeline. There are no quizzes, midterms, or exams. All submissions are due on Canvas by the start of lecture on the described days. Attendance is expected but not strictly mandatory. Please contact me if you expect to miss more than an occasional lecture.

Homework assignments are to be done in groups of 1-3, and the final project in groups of 2-5. All group members should be involved in completing each part of the homework assignments (i.e., think pair programming as opposed to divide-and-conquer). Projects are fine to divide-and-conquer.

There is an ungraded “Problem Set 0” for self-evaluation, available on the course homepage. It is intended to make sure you feel comfortable coding at the level that will be assumed as prerequisite. It is very strongly encouraged (if only to clean up the coding environment on your machine). No submissions of PS0 will be accepted.

Completing the assignments will require setting up accounts and using APIs that aren't always free, or where paid versions are far superior to free versions. I constantly keep an eye out for free alternatives, but also want you to train with the best tools of today. Therefore I ask students be prepared to spend approximately $30-50 as part of this class. If this would be a hardship then please reach out to me. There are no required textbooks or other course materials. 

## Grading and late policies
* 45% Problem Sets (15% each)
* 10% Project proposal
* 40% Project presentation and report
* 5% Participation and collegiality

Problem sets and project proposal: your best bet is to hit all the deadlines. Failing that, the following policy is designed to prioritize completing problem sets over not completing them, submitting problem sets over not submitting them.  
- One single delay of up to 2 days (48 hours), no questions asked. 
- After that: 5 points deducted per day (per 24h, including weekends), up to 6 days. Limited CA availability. 
- After that: no CA support will be provided and max grade is a 70. Must be submitted by 11:59p on the final day of semester (after that, grade is 0), and no guarantees on grading turn-around time.

Project: No extensions on the project. If you need an extension, please contact me to discuss my policy for taking an incomplete for the course.

In the event of a family or medical emergency, I am fundamentally a reasonable person. Please contact me as soon as possible.

## Academic integrity 

Academic integrity is a core institutional value at Yale. It means, among other things, truth in presentation, diligence and precision in citing works and ideas we have used, and acknowledging our collaborations with others. 

Collaboration in this course is strongly encouraged, but all collaboration must be acknowledged. Failure to disclose collaboration (including allowing someone else to represent your work as their own) will be considered a violation of academic integrity and will be reported to Yale College for arbitration.

## AI Policy

I encourage you to use and sharpen your use of AI tools as part of this course. For each problem set and for the project, I will ask you to document how you used AI, what was useful and what wasn't, so we can all learn together about best practices. Mindless use of AI (i.e., submitting work that is nonsensical or wrong in ways where you can't explain what you did) will be graded harshly. Undocumented use of AI will be considered a violation of academic integrity. 


## High level course schedule

| Week | Topic | Assignments |
| --- | --- | --- |
| Week 1  | Introduction	| 	PS0; PS1 Out |
| Week 2  | Social Data; Causal inference | |
| Week 3  | Friend recommendations | PS1 Due; PS2 Out  |
| Week 4  | Product recommendations	|  |
| Week 5  | Search engines | |
| Week 6  | Feed algorithms |  PS2 Due; PS3 Out |
| Week 7  | Content moderation | |	
| Week 8  | Network interventions	   | PS3 Due |
| | **Spring break** | |
| Week 9   | Ad targetting	   | Project Proposal Due |	
| Week 10 |  Misinformation | |
| Week 11  | AI in social environments	| Project Check-ins |
| Week 12  | Meta 2020 elections studies                 | |
| Week 13 | Project presentations                       | | 
| Exam week | (No exam)	 | Report Due | 

<!--
Social contagion, diffusion, social influence
Online surveys; digital demography
Cell phone and mobility data
-->

Assignment overview:

| . | Topic | Release | Due |
| --- | --- | --- | --- |
| Problem set 1: 	| Surveys and post-stratification   | W 1/14 | M 1/26 | 
| Problem set 2:  	| Network analysis of social media data | W 1/28 | M 2/16 | 
| Problem set 3: 	| Social AI | W 2/18 | M 3/2 | 
| Project proposal: | Short write-up | | W 3/25 | 
| Project check-in: | Meetings with Prof. Ugander | | 4/6-10 |
| Project presentation: | (in class) | | M 4/20, W 4/22 |
| Project report: | written | | M 4/29 |


## Accommodations

Yale University is committed to providing equal access to its academic programs. Students who may need an academic accommodation based on the impact of a disability should contact **Student Accessibility Services (SAS)** to initiate the process. SAS will review appropriate documentation, determine eligibility, and issue an official accommodation letter.

If you are approved for accommodations for this course (e.g., extended exam time or other adjustments), please email me as early as possible and include your accommodation letter from SAS so that we can make appropriate arrangements in a timely manner. Early communication is important to ensure that accommodations can be implemented effectively.

For more information, please visit: <https://sas.yale.edu>


## Computing Environment

A Unix-like setup is strongly recommended (e.g., Linux, Mac OS X, or Cygwin). We will use Python 3 (JupyterLab is recommended). 
