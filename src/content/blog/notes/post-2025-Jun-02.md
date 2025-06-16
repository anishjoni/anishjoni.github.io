---
title: Learning note #1
description: Concepts I learnt or things I tried to build. 
duration: 5min
date: 2025-06-02
---

I have been tinkering with a bunch of tools in the realm of data engineering to LLMs.

I like the idea of working on my own data like Spotify listening habits, Home energy consumption or Personal fitness data. After some thought, I ended up starting with running/cycling data from Strava due to two reasons:[

- API support
- I love tracking runs/rides, would be cool to do more with the numbers.

My broad goal was to find data I can play with and add layers to it. 

This led to the first layer: Data pull

**Data pull:**

- Connecting to the Strava API was not straight forward. It has a initial auth manual workflow to get a code and you exchange this code for an active *access token.*
- The access token expires every 6 hours, so I found going through the manual workflow every time to get an active *access token* annoying.
- To evade this, I used [*selenium](https://selenium-python.readthedocs.io/) L*ibrary to get the code for me and it worked and was cool to see the code opening Strava on a browser, consenting to cookie, use stored cookie, hitting the API and finally get the *code* needed for *access token.*
- Once I had the access token, pulling my activities data was just an API call away.

References: https://www.youtube.com/watch?v=nutXnXSyRLY&t=780s

**Data ingestion:**

- I decided to go with a local instance of [MySQL](https://dev.mysql.com/doc/refman/8.4/en/database-use.html) for my database needs, because it fits my needs, simple and is open source.
- I wanted to keep this layer simple, so my workflow was to
    - Pull all activities data from API via a Py script.
    - Write it to DB using pandas/polars library capability.
    - For subsequent pulls, I would filter to only new activities and append them to the DB.
    - Complexity here for me, was to make data pulls and data ingestion automatic without running into auth or data integrity issues.
    

**Data Orchestration:**

- This is an area I’m starting to like more. There are so many tools out there for data orchestration - Airflow, Airbyte, etc..
- I went with Prefect and loving it for it’s simplicity and it works so well for my needs.
- Prefect works with mostly two main concepts - tasks and flows. Tasks can be any functions that does just one thing and flows tie them together to get a series of tasks done that can be scheduled and monitored via the UI.

So far, I have been successfully able to complete the above steps and below are areas I want to extend this further:

- ML/Basic Dashboard.
- Serve to user.
- Add GenAI features:  by setting up local LLMs using LM Studio and Ollama + Open web UI.
