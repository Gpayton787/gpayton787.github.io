---
layout: page
title: WhatsBruin
description: A full-stack web application that enables UCLA students to get the most out of their dining hall experience.
img: assets/img/wb.png
importance: 1
category: work
related_publications: false
---
Link: [whats-bruin.com](https://www.whats-bruin.com/)

## Purpose: 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/food1.jpeg" title="food1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/food2.jpeg" title="food2" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/food3.jpeg" title="food3" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    On the left is food from Bruin Plate. Middle, also Bruin Plate. Right, from Epicuria featuring Jimmy.
</div>

At UCLA we are privileged to have some of the best dining halls in the nation. This also means that the menus are extensive and change constantly (Tragic, I know). If you have a favorite food, you'll often be wondering the next time it'll be served. Instead of checking the menu daily, which gets tedious during busy times like finals weeks, WhatsBruin can check for you and send you email reminders!

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/email.png" title="email" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Additional features include:
- An upvote system
- A filtering system to quickly find the food you're looking for 
    - Ex: You can select to view only vegetarian, vegan, halal, etc. menu options
- Sorting by popularity, upvotes, grams of protein, etc.

## How it works
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wb_diagram.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A diagram of the application architecture
</div>

The design of WhatsBruin is very simple and scaleable. The main component of the system is the database, which stores all of the menu information collected since the project began. Because we were not allowed direct access to UCLA dining's menu API, we opted to use webscraping to populate our own master database. 

The second component of the system is automation. The application's functionality is provided by two python scripts that are hosted as Google Cloud functions. The first is "webscraper.py". This script uses a websracping library called <a href="https://www.crummy.com/software/BeautifulSoup/bs4/doc/">Beautiful Soup</a> to scan the <a href="https://menu.dining.ucla.edu/Menus">UCLA Dining</a> website for the latest menu information. The script then proceeds to update Todays Menu, transferring yesterday's information to the Complete Menu<sup>*</sup>. The script is also optimized to only check the menu websites of the dining halls that are open. This script is currently set to run everyday at midnight.

<sup>*</sup>There are three menus: Todays Menu which lists all the foods being served today, Complete Menu which lists all the foods that have been served this academic school year, and All-Time Menu which lists all foods that have been served starting from the apps conception (2023).

The second script is called "emailer.py". This script uses the <a href="https://docs.python.org/3/library/smtplib.html">smtplib</a> library to send reminder emails to users. The script first fetches the user's information including their email, favorited list, and email preferences. It pre-composes all personalized emails and then opens a server, sending the emails quickly before closing the server. This script is currently set to run each morning at 6 am.

The last component is the User Interface, providing users with an easy way to access the features that our application offers. It focuses on good user experience and simplicity.

## Collaboration
The beginning of the app came to fruition as a final class project for CS35L (Software Construciton). We worked together to build a functioning app within the span of around 5 weeks. It was both a fun and rewarding experience to lead and build a project as part of team. We all had different schedules, starting points, and ideas for the app. Through meetings we were able to compromise on disagreements and assign roles and that had us working like a well oiled machine. After the class ended, I continued working on the project to present day where it's first production version is hosted and available for all UCLA students to use.

A big thanks to my groupmates:
- Ethan Peng (backend, emailing)
- Heran Yang (backend, firebase)
- Jimmy Hou (UI)
- Haoji Wang (Review page)
{% include figure.liquid loading="eager" path="assets/img/commits.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}






