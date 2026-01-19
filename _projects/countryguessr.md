---
layout: page
title: CountryGUESSR
description: Inspired by the famous geography guessing game GeoGUESSR, take your world knowledge to the next level with CountryGUESSR.
img: assets/img/countryguessr.png
importance: 2
category: work
related_publications: false
published: true
---

## Overview

In GeoGUESSR you're dropped into a street view of a random place and tasked with determing where on earth you are by analyzing your surroundings. In a similar fashion, CountryGUESSR gives you a random fact about a random country, and you try to guess the country in the least amount of facts possible. Which country boasts over 60% of the worlds lakes? Which countries national animal is the unicorn? Test and expand your world knowledge with CountryGUESSR!

### How to Play

{% include video.liquid path="assets/video/cg.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
Important things to know:
* Facts are associated with a country, a difficulty value, and a category. Facts are shown in order of decreasing difficulty. The earlier you guess the country, the more difficult the fact, and the more points you earn.
* Difficulties range from 0.0 to 10.0 with 10.0 being the most difficult.
* Categories include: geography, culture, history, economy, demographics, fun, and misc

## System Architecture

<div class="row mt-3">
    <div class="col-md-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/cg_architecture.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-md-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/cg_schema.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

The data flow for the application is the following...

**Authentication:** When the user first visits the website they are redirected to the login page where they can sign in with Google. If successful, a session is created and stored in the backend and users are directed to the home page '/'. To logout, the can visit their profile at '/profile' and click logout. On the backend, their session is invalidated and they are redircted to '/'.

**Gameplay:** After logging in, the user will click a play now button to be routed to '/play'. Upon page load, an IndexedDB (IDB) manager is loaded which acts as an interface to the cache. The cache is first prepared by checking if the data is present and not stale. If it isn't, a call is made to **fetchAndCache** that calls  and caches responses from '/api/countries' and '/api/facts'. Next, game state is initialized and cached: **currCountry** is set to undefined, **countriesGuessed** is set to an empty array, and **currFactsPtr** is set to 0 (the first fact). If the data is present, then we have everything we need to continue play.

## Tech Stack

Frontend
* [Tailwind CSS](https://tailwindcss.com/docs/installation/framework-guides)
* [SveleteKit framework](https://svelte.dev/docs/kit/introduction)
* [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) for caching in the browser

Backend
* [Google Oauth and Lucia](https://lucia-auth.com/tutorials/google-oauth/sveltekit) for authentication and sessions
* [SveleteKit](https://svelte.dev/docs/kit/introduction) for APIs
* [REST Countries API](https://restcountries.com/)
* [Drizzle ORM](https://orm.drizzle.team/) w/ [NeonDB](https://neon.com/) for the database
* [OpenAI API](https://openai.com/index/openai-api/) w/ structured JSON responses for generating facts
* Node.js server hosted on [Google App Engine](https://cloud.google.com/appengine) for deployment

## Future

This game quite literally has a global market to expand into, everyone loves to learn more about the countries of the world. Future improvements would include:
* Online lobbies in which players compete to see who can guess countries the quickest (similar to [skribbl.io](https://skribbl.io/))
* User stats should be stored for analysis and comparison on global leaderboards. Users can track just how fast they can guess each country.
* Choose a subset of countries to guess from
* Crowd-sourced facts that people can upload, so that we are constantly introducing new and original facts from actual citizens of the world
*  Curated fact sets that serve as benchmarks, i.e. you can compare scores (lowest number of facts used to guess all countries) on a given fact set
* A thriving community. My hope is that this game could have a good community surrounding it that makes sure the facts about the countries are accurate, interesting, and fun.