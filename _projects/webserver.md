---
layout: page
title: C++ Web Server
description: A simple web server implementation capable of receiving Http requests, processing them, and returning Http Responses
img: assets/img/main_webserver.jpg
importance: 1
category: work
related_publications: false
published: true
---

**In the capstone of UCLA's Computer Science degree, after three years of rigorous training...** we are tasked with one last project to prove our readiness to enter the wider world: a C++ web server. While not particularly exciting, it's basic enough to provide isolated practice with certain principles, and complex enough to emulate what working on real industry software would be like. Split into groups of four, and mentored by three Google engineers, each team works their way to the finish line. How well will they understand and apply software engineering principles? How well will they work together? What critical decisions will they make at each fork in the road? Their fate will be determined wholly by themselves. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/main_webserver.jpg" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A high level server architecture diagram. It is intentionally lacking specifc detail for readability.
</div>

## How it went

I joined a team of three that would end up becoming some of my best friends. We named our team "degenerative-ai" and we got to work on buildling our web server. Each week we rotated the position of tech lead (TL), updated our documentation, built out new features, and added unit and integration tests. This was as close as I'd ever been to the actual software development process in industry and it was very efficient and sustainable. In the end, we completed our web server on time and built a fun web application on top of it for our final project. Ryan T and I both won one of the superlative awards and got some unique prizes. Overall, this was my favorite class I've ever taken at UCLA and I learned so much from the experience of the lecturers (Alex, Michael, and Philo) and my own experience with my team (Ryan O, Ryan T, and Jarod).

<div class="row mt-3">
    <div class="col-md-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/cs130group.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            Team brunch at Blu Jam Cafe
        </div>
    </div>
    <div class="col-md-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/cs130awards.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            Me: "Most comments given as TL"
            Ryan T: "Most lines of code committed"
        </div>
    </div>
</div>

## What I learned



### 👨‍💻 Acting as Tech Lead: The Art of Code Review

Being TL was my favorite part of this class. As TL, you didn't write any code and your main purpose is to make sure the team stays unblocked. You make the important design decisions that week, set up meetings, monitor the CI/CD pipeline and of course, perform code reviews. Code review is a mandatory step, and should be taken seriously. The reviewer has a responsibility to question code that they don't understand and prevent bugs from entering the code base. Think of them as the guardian at the gates of a great city. Those who wish to enter must meet certain requirements and should not be all suspicious looking. If the guardian starts getting lazy and fails to properly check entering caravans, that's when enemies and trouble makers get in.


So having a tech lead, or a similar role, is crucial to the health of a project. Their responsibilites should be taken seriously, and they should be making as many comments as they see fit. A non-goal of the TL is to be nice. They must be confrontational about code that does not meet the standards of the code base, especially when new contributors begin working on it. 

### 📝 Documentation and Extensibility

Documentation is so important. Sometimes the person to read it next, is you 9 months later. Think about what it's like to start on a new project with very little documentation. It sucks right? So don't let your codebase be the same way. Write down your thought processes as well as how to navigate the project. Code is not documentation. If the way you write your code implies something, you can go ahead and assume that the next engineer will miss the implication. It's always best to be explicit. 

With AI tools that can explain code to you, it may feel like documentation is less important, however, it's the contrary. Documentation is as important as ever as it gives the AI critical context so that it can actually be useful.

### 🙂 Everything has an Interface

Everything interacts with each other through interfaces, and it's important to design them FIRST, before you begin to code. For example, imagine you write a function that produces an output that you came up with. You figured that you only needed to produce A, B, and C. However, you later find out that the consumer of your function actually wants A, B, C in a different format that you didn't consider. In addition to that, you also need to produce new outputs X, Y, Z but your implementation did not consider that. Now it might be easy to modify your function accordingly, but it could also be a real pain.

You can't always get the interface perfect everytime, but spending time upfront designing the interface will save you a lot of time. It is also exponentially more important as you start producing code that is going to interact with someone elses code, which is almost always the case.

### ⏱️ How To Save Your Precious Time: Attention to Detail and Proper Testing

Early on, a few subtleties can ultimately lead to lots of debugging later on down the line. If you're careful and employ proper integration testing you can save yourself the headache.

For example, we had a bug caused by a disappearing write buffer after adding thread level parallelism. The integration tests still passed, but we would soon see that our server was sending over corrupted images. Had we an integration test that verified a request for a large image by doing a byte-level comparison, we would not have had to deal with this at the last minute.

### How We Used AI and How We Will Continue to Use it

Over the course of the project, we all used AI to speed up the development process. It was especially helpful for asking questions about the Boost libraries and the HTTP Protocol, or implementing simple, self-contained blocks of code. While the use of AI made us so much more productive, we came to a firm conclusion: The crucial limiting factor was manpower and not one of us could have been dropped from the project.


If we had even one less person, the amount of work for each of us would have significantly increased. While we may have been able to complete it, the code quality probably would have suffered and the workload would not have been sustainable. That being said, AI is clearly indispensable for a software engineer to be more productive, efficient, and spend their time doing the important thinking and coding rather than writing monotonous code and searching through stack overflow.







