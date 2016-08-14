Think of yourself as scientist creating a controlled environment in whic
to test hypotheses. You want to eliminate as many confounding variables as possible so you can have confidence in the validity of the
data you collect.

 will be tempted to jump ahead or take a shortcut. This seldomYou'll have a tendency to want to jump ahead because things seem obvious and redundant, especially because in your head making
bigger leaps seems like a sign that you are progressing. This is a sinister illusion, you'll get better by practicing a disciplined and deliberate
approach. Your process is the path that leads you through the wilds of your own mind. It's straightforward and simple but the beckoning of sirl

 jump ahead to use that brain of yours and try to plot a course, but chances are that course
in mindfulness as it is in



One of the biggest insights I've had about my apprenticeship so far, Is that my success with whatever I'm working on is largely
dependent on how aware I am of what I'm doing. What I mean is that a slower deliberate pace with a steady adherence to and even reliance on
priciples like TDD is often going to get me to place i want to get to faster and with more understanding of the current problem.

When I try to think ahaed or anticipate things that aren't immediately, ahead of me that's when I get into trouble. It can really be as simple as red green refactor
but it takes a lot of discipline to not jump ahead to take things in baby steps.

More so than anything else I've encountered coding is a process, All aspects, defining the story to deploying and it's understanding the steps in those processes that I
think will make me a better coder.

One thing that I've had to start doing with the project I've been working on most recently is deine my own user stories. I'll go into a meeting with
my mentors and come out with a jumbled pile of notes that makes sense in the context of the meeting, but I often find are useless a couple of days later.
As a remedy to this, I've started deliberately trying to mkae well worded and constructed user stories that can serve as a solid reference to

PRECODING

- Defining the story -- Is there a way to clear up ambiguity in the requirments by rewriting the story.

CODING

- Jump right in -- stick to the basics. Write the easiest test you can think of and think of the easiest way to pass it. No more. No less
- If testing right away is difficult, write it out in pseudo code and then find the syntax after you ahve an outline.
- Don't be afraid to be wrong, you will be most of the time -- It's a process.
- If a test is passing is there any way you can make the test or code you wrote better? -- If not don't worry and move on chances are you will be able to later on
- Don't worry about writing too many test cases or ugly code -- You're going to delete most of what you write.
- Take care to notice all the actual actions you are repeatedly doing. Is there a way to optimize in you editor?

GETTING STUCK

- Have you mispelled something? -- Honestly, that's usually it.
- Do you not know how a method works? look at the source code it will probably help.
- Be patient -- Thoroughness and attention to detail are worth a whole lot more than expedience when stuck -- The goal is not to get unstuck, but to understand why you are stuck
- Are you not in a mindset to be patient and thorough? Move on to something else -- walk away until you are.

ON AUTHORSHIP

- Writing code is writing, you have an audience and you want to build a coherent and cohesive narrative for them.
- Unlike a book, or an article, code is often alinear and therefore hard to follow -- There's missing context and the ideosynchracies of the autor to deal with.
- As such, a dutiful author will try and build as much context for their reader as possible to make their story as easy to follow as possible.
- Editing through version control if often a big part of this, not only writing code that tells a story but submitting it for review in a way that imeediately makes sense to the
  people reading a pull request or commit history.
- As in coding if you find yourself often repeating actions, there might be a better way.

ON PROCESS

- Work in chunks. mandating a small rest in between is a good way to keep up energy and attentiveness throughout the day
- Be a scavenger -- Whenever something useful come's along store it somewhere. Doug Bradbury talks about this in one of his blog post and I think it's an important, yet oft overlooked
  aspect of workflow.
- I've mentioned this a couple of times already, but look for opportunities to optimize your current workflow

ON COMMUNICATION

- It's your job to make sure other people understand what you are talking about
- If asking a question, take time to help build the appropriate context for your audience -- I find that by trying to experess the question as clearly as possible I usually have some
  better insight into why I'm asking it in the first place, and now maybe don't need to take up someone elses time.
- Be as slow and deliberate in your communication with others on slack or in person as you would in communicating through your code, take time to understand what it is you actually wan
  to ask or say.



I start off with the requirement "I can make a bin private, which means only the current browser can view the details of the bin." This is a
requirement written in my own words, it's not very clear so the first step in completing this story is to make the the story a bit more clear
using the "As a \<type of user\>, when \<something happens\>, then <\something else happens\> and \< something else happens etc.\>"


As a user.
  When I click on the private bin toggle
  Then I create a bin that only I can make requests to and inspect in my current broswer session


Ok, so that's a lot clearer in my mind. There now exists the notion of a private toggle and the idea of private is further elucidated. Now that
the requirement is clearer it becomes easier to tackle, but where do I start? I know what sessions are intuitively, but that knowledge could definitely benefit
from a refresh. I know I have a tendency to want to have a perfect understanding of all aspects of the topic I'm investigating, so I'll want to time box this preliminary research
so that I don't spend all day going down a rabbit-hole.


-- 20 minutes later --


The time I've given myself has elapsed, but I'm still not 100% certain on what to do next. I have some questions that I've formulated based on
the past 20 minutes of research. Let me write those down below.


- Does it make sense to store a session-id as an attribute of my bin table? If not, how would that validation occur?
- Since a cookie is a small piece of text stored on my computer by my browser, in the case of a `Ring.middleware.session.cookie/cookie-store`
   what does it mean for session data to be encrypted in a cookie.
- How might I go about "TDDing" session and cookie and management using Ring?


These could be the wrong questions to ask. They are potentially filled with incorrect assumptions and attempting to answer them outright might be a mistake
that could lead to frustration. For the time being, I think that instead of attempting to answer these questions, it might be more helpful to consider them
artifacts of my current level of knowledge on the current topic, useful as reference to see if there still exists ambiguity in my understanding of the topic
even after working on it a bit. This list ought ot be ongoing. Questions can and should be added as they arise and one of the last things I might to do before
submitting this story for review is reexamine my questions to affirm my understanding of components so that I can be reasonably confident that given a similar
story I can execute it without much fuss. In the same vein it might make sense to add surprising and interesting aspects of the story that can be referenced later.


With my initial questions compiled, I have two routes forward. I could time block another 20 minutes for additional research or jump right into the code. As much as possible, I
would want to opt for the latter. By jumping in and trying things out, a feedback loop is created and that is immensely valuable not only for moving forward with the story, but
also for the learning process. The only time I might opt for the doing more research is if I still had no idea where to even start. Again, timeboxing this is crucial, at least for me
since I have a tendency to get carried away. It might be worth noting that in the case of jumping into the code, I might still need to do a bit of research, but the empahsis is on creating
the feedback loop and learning that way, it can be exploratory but there should be some tangible output. Furthermore, I think it's also a good idea to timebox this approach as well.
It's just a good way for me to stay focused.


-- 30 minutes later --


In this first attempt at tackling the problem I made a classic mistake. I lost my bearings and my feedback loop, I ran into a question assumed an answer and pursued validating that answer
without pausing to contemplate where it might lead me. In more concrete terms. I decided to create a HTML checkbox as the implementation of the toggle for deciding whether or not a bin
should be private or not and then came to realization that I had no idea how to translate that HTML into backend code that would tell my app whether a bin was private or not. I became convinced
that I should be able to get the boolean value from the HTML form that contains the checkbox and that value should be present in the the `:form-params` key of the Ring request map. I started Googling all
over the place trying to figure out how to get HTML form data to appear in the `:form-params` key. I put the blinders on and didn't really get anywhere. It might turn out that
the path I went down happens to be the right one, but the thing I'm fighting against and the mistake in more clear terms is really about losing awareness, which I'd argue
is the most important thing when solving a problem.


After that first cycle here are the questions that provide a coherent snapshot of where I'm at with my understanding of the problem.


- How is the `:form-params` key in a ring request populated
- What is the difference between marking something as private and adding a session to it?
- How do you get the value of a checkbox from an HTML form
- Does it make sense to create a new migration for the addition of a private boolean? or should I just update the current one?
  - what goes into making that decision?






