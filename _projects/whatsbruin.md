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

**Date completed**: October 2024
**Contributors**: Gregory Payton, Heran Yang, Haoji Wang, Ethan peng, Jimmy Hou

## Purpose

At UCLA we are privileged to have some of the best dining halls in the nation. This also means that the menus are extensive and change constantly (Tragic, I know). If you have a favorite food, you'll often be wondering the next time it'll be served. Instead of checking the menu daily, which gets tedious during busy times like finals weeks, WhatsBruin can check for you and send you email reminders! Currently, WhatsBruin has over 40 active users.

### Additional features

- An upvote system
- Ability to search for any UCLA dining food with advanced filters
- Sorting by varioius metrics: popularity, upvotes, grams of protein, etc.

## Technologies Used

**Frontend:** NextJS, Tailwind CSS
**Backend:** Firebase, Python, Google Cloud Functions

## How it works

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wb_diagram.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A diagram of the application architecture
</div>

The design of WhatsBruin is very simple and scaleable. The workhorse of the system are Python scripts that carry out webscraping and emailing. The database stores user information and the data gathered from webscraping. Finally the applications services are made available via a web application. 

#### Python Scripts
There are two main python scripts:

**The "Webscraper"** <br/>
Holds the daily responsibility of scanning the <a href="https://menu.dining.ucla.edu/Menus">UCLA Dining</a> websites, processing the gathered data, and updating the database accordingly. This script uses a websracping library called <a href="https://www.crummy.com/software/BeautifulSoup/bs4/doc/">Beautiful Soup</a>.

**The "Emailer"**  
Holds the responsibility of emailing reminders. Uses the <a href="https://docs.python.org/3/library/smtplib.html">smtplib</a> library to send emails. These scripts are hosted to run at designated times everyday with <a href="https://cloud.google.com/functions?hl=en">Google Cloud Functions</a>

#### Database
The database that we used was <a href="https://firebase.google.com/docs/database"> Firebase Realtime Database </a>. It stores all of the data gathered and organizes it into TodaysMenu, CompleteMenu, and AllTimeMenu. CompleteMenu consists of all menu items for this academic year. AllTimeMenu consists of all menu items ever. And TodaysMenu... you get it.

#### Web Application
The <a href="https://www.whats-bruin.com/">web application</a> provides a quick and easy way for ANYONE to check out and use our services. The frontend utilizes React and <a href="https://nextjs.org/">NextJS</a> to render pages as quickly as possible and give a good user experience. For authentication we used <a href="https://auth0.com/auth0">Auth0</a>. For styling a combination of <a href="https://tailwindcss.com/">Tailwind CSS</a> and <a href="https://mui.com/material-ui/">Material UI</a> were used. Finally the application itself is hosted on <a href="https://vercel.com/">Vercel</a>

## Collaboration
The beginning of the app came to fruition as a final class project for CS35L (Software Construciton). We worked together to build a functioning app within the span of around 5 weeks. It was both a fun and rewarding experience to lead and build a project as part of team. We all had different schedules, starting points, and ideas for the app. Through meetings we were able to compromise on disagreements and assign roles and that had us working like a well oiled machine. After the class ended, I continued working on the project to present day where it's first production version is hosted and available for all UCLA students to use.

A big thanks to my groupmates:
- Ethan Peng (backend, emailing)
- Heran Yang (backend, firebase)
- Jimmy Hou (UI)
- Haoji Wang (Review page)
{% include figure.liquid loading="eager" path="assets/img/commits.png" title="wb_diagram" class="img-fluid rounded z-depth-1" %}






