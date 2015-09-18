<dl class="dl-horizontal">
  <dt>Course</dt>          <dd>CSE 405 Server Programming</dd>
  <dt>School</dt>          <dd>CSUSB</dd>
  <dt>Quarter</dt>         <dd>Winter 2015</dd>
  <dt>Prerequisites</dt>   <dd>CSE 322 or or knowledge of Web programming</dd>
  <dt>Class meetings</dt>  <dd>There is an optional on-campus lab<br>Mon 4:00 - 5:50 JB 359 (Lab 1) <br>Wed 4:00 - 5:50 JB 359 (Lab 2) </dd>
  <dt>Textbook</dt>        <dd>Various readings available for free on the Web</dd>
  <dt>Instructor</dt>      <dd>Dr. David Turner</dd>
  <dt>Email</dt>           <dd>dturner@csusb.edu</dd>
  <dt>Office hours</dt>    <dd>Mon. 3:30 - 4:00, Tue 2:00 - 5:00, Wed 3:30 - 4:00 (JB 340)</dd>
  <dt>Course website</dt>  <dd><a href="http://github.com/csusbdt/405-2015/wiki">http://github.com/csusbdt/405-2015/wiki</a><dd>
  <dt>Grade Sheet</dt>     <dd><a href="https://docs.google.com/spreadsheet/pub?key=0Aq3la2PXzB0YdGRYUzE5WGJHejlGeThsVDNkM0xKMWc&single=true&gid=1&output=html">grade sheet</a></dd>
  <!--dt>Google group</dt>    <dd><a href="http://groups.google.com/d/forum/405-2015">405-2015</a></dd-->
</dl>

## Course Format

This course is 100% Web-­based with an optional weekly meeting on campus. There are no exams. All reading material is freely available on the Web. Your grade is based on the timely completion of a sequence of assignments, which is maintained on [the course website](http://github.com/csusbdt/405-2015/wiki).

<!--In addition to the optional weekly meetings, you can communicate with the class as a whole by posting to the CSUSB CSE 405 Winter 2015 Google Group.-->

You will maintain your work in a remote Git repository that I will set up for you. Only you and I will have access to your repository. I will evaluate your work by looking at your source code in addition to accessing your deployed application code from a browser.

## System Requirements

You can do all required work in this course using any of the three common operating systems: Windows, Mac OS X and Linux. All required software can be downloaded and installed for free. It is probably more convenient to complete the course assignments using a personal computer. However, computers are available in JB 358 and JB 359 that you can also use. The open times for accessing these labs is available from the CSE website or by contacting the CSE main office.

## Course Goals

The goal of this course is to increase your understanding of server programming. Certain topics covered in this course are common to all server programming problems, such as communication protocols, authentication, security, database access, and logging. Other topics are specific to the development environments of individual projects, such as Git, Android SDK, Javascript, Node.js, and PostgreSQL. Therefore, the course encourages the acquisition of knowledge that is common to all server programming problems while developing specialized skills.

The techniques that you learn in this course for building Web applications can be applied to solve a larger set of programming problems that rely on the HTTP protocol, including native desktop and mobile applications that store application state in the cloud and interact with other users and systems.

## Learning Objectives

* Learn to use ajax and DOM manipulation to create single-page Web apps.
* Learn to develop Web-based mobile applications.
* Learn to develop server-side logic in Javascript using Node.js.
* Learn to use both documented-oriented and relational databases for server-side persistence.
* Learn to deploy mobile and desktop applications using scalable cloud computing services.
* Learn to code defensively to avoid common security problems.

## Labs/Assignments

In this course, you will complete a sequence of assignments. The assignments involve research, programming and problem solving.

Programs that are incorrect or do not solve the stated problem will lose some or all points.

Work that is submitted late will lose some or all points.

Source code that is copied from outside sources without mention of its source will be considered plagiarism.
Source code taken from outside sources needs to be commented to show from where the code was obtained.
Not all source code taken from outside sources -- even when described as such -- will be accepted
as fulfilling the requirements of an assignment because it may defeat the purpose of the assignment,
which is to develop programming skills.  If in doubt, check with the instructor
whether using an particular outside piece of code is acceptable for the assignment. 

Writing a program to produce required behavior is not good enough for a full score in this class; you must also write code that is readable by humans.  Program readability is important because real ­world programs are read over and over again in the process of fixing bugs and adding new functionality.  Program readability will be evaluated according to the following set of criteria.


| Criterion | Description |
|-----------|-------------|
| Cleanliness | Have unnecessary variables and logic been removed from the code? |
| Logical indentation | Does indentation show logical structure? |
| Consistent indentation | Does indentation follow a consistent policy? |
| Portable indentation | If tabs are used for indentation, are they used everywhere instead of spaces? <br><br> Keep in mind that tabs display differently in different viewing and editing tools.  This is a problem if you mix spaces with tabs to achieve indentation.  If you use tabs, then make sure that you do not use spaces anywhere in your code to achieve indentation. |
| Logical spacing | Does spacing show logical structure? |
| Consistent spacing | Does spacing follow a consistent policy? |
| Expressive and clear naming | Do variables, functions and classes have names that clearly express their purpose in the program? |
| Clear responsibilities | Are responsibilities of functions and classes clear and consistent with their names? Is the code structured to avoid reliance on side effects produced by functions? |
| Necessary comments | Are comments included when needed? |
| Unnecessary comments | Are superfluous comments omitted? |
| Spelling | Are user-defined identifiers free of spelling errors? Are comments and other documentation free of spelling and grammatical errors? |
| Nonredundant | When 2 or more places inside a program need to perform the same activity, is that activity defined as a function and called as such from where it is needed? |
| Robust to changes | Does the code avoid breakage problems when modified. For example, the following code is not robust to changes. <br><code>var s = 'hello';</code><br><code>print 'The length of the string is ' + 5;</code><br>This should be rewritten as follows.<br><code>var s = 'hello';</code><br><code>print "The length of the string is " + s.length;</code> |
| Organization | Is source code well organized into files and folders? |

Keep in mind that tabs display differently in different viewing and editing tools.  This is a problem if you mix spaces with tabs to achieve indentation.  If you use tabs, then make sure that you do not use spaces anywhere in your code to achieve indentation.

## How to submit work

In general, each assignment in the course will correspond to a folder in your git repository.
You submit work by pushing this assignment folder into your remote Git repository
and then sending me an email to let me know you are submitting the assignment.
After you submit work on an assignment for evaluation, I am willing to discuss with you the reasons for the
score you receive, but I will not accept additional work in order to improve your score on the assignment.

## Plagiarism

You should do the coding assignments on your own without copying code from other students or outside sources. If you see a situation where you think it is OK to embed the work of another in your program, than include a note in your source code that identifies the source of the code and send me an email to check whether this is acceptable for the purpose of completing an assignment.

## Grading

Your score will be computed by dividing the total of all points earned by the total possible points. The following scale will be used to assign a letter grade. 

| Percent  | Grade |
|----------|:-----:|
| 94 - 100 |   A   |
| 90 - 92  |   A-  |
| 87 - 89  |   B+  |
| 83 - 86  |   B   |
| 80 - 82  |   B-  |
| 77 - 79  |   C+  |
| 74 - 76  |   C   |
| 70 - 73  |   C-  |
| 67 - 69  |   D+  |
| 63 - 66  |   D   |
| 60 - 62  |   D-  |
|  0 - 59  |   F   |

## Students with Disabilities

If you are in need of an accommodation for a disability in order to participate in this class, please let me know as soon as possible, and also contact Services to Students with Disabilities at UH-183, (909)537-5238. You are advised to establish a buddy system and alternate in the class if you require assistance in the event of an emergency. Individuals with disabilities should prepare for an emergency ahead of time by instructing a classmate and the instructor.

## Academic Regulations and Procedures

See the CSUSB Bulletin of Courses for the University's policies on course withdrawal, cheating, and plagiarism.

## Computer Science and Engineering Club

The [Computer Science and Engineering Club](http://cse-club.com/) is a student-run organization that uses a combination of email and campus meetings to plan events, ask and answer technical questions, post job and internship openings, and discuss other topics of interest to computing majors at CSUSB. Club­-sponsored events include seminars, workshops, tutoring and fun activities.