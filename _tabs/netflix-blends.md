---
layout: netflixblends
icon: fa-brands fa-github
order: 3
---

<div align="center">
    <img width = "900" alt="" src="/assets/img/netflixblends.jpg">
</div>

Click [here](https://github.com/praveen-bandla/netflixblends) for the complete project!


## Introduction


With the ever-expanding catolog of available content, the process of selecting a new movie to enjoy with friends has never been more frustrating. This stems from [Choice Overload](https://www.behavioraleconomics.com/resources/mini-encyclopedia-of-be/choice-overload/), a well-documented phenomnenon by which an excess of options can trigger decision fatigue and paralysis, often causing consumers to revert to familiarity or to avoid decision-making altogether.

Taking inspiration from [Spotify Blends](https://support.spotify.com/us/article/social-recommendations-in-playlists/), our project aims to address this by offering a solution - a web application that analyzes the Netflix viewing patterns of two individuals and delivers personalized, mutually appealing movie suggestions. Built on `Flask`, our webapp processes user data to generate insightful data visualizations and comprehensive analytics, ultimately offering tailored movie recommendations. 


<br>

## Overview of Project

**Data Collection and Processing** <br>
In this project, we utilized two main data sources: a Netflix Library Dataset and User Watch History Data. The Netflix Library Dataset was sourced from [Kaggle](https://www.kaggle.com/datasets/victorsoeiro/netflix-tv-shows-and-movies), while the User Watch History Data was downloaded from a user's Netflix page and uploaded onto the webapp as a `csv` file. The user data in particular, required significant cleaning owing to formatting inconsistencies; while we addressed many complexities and edge cases, we encountered some data loss, though it was minimal and did not significantly impact the overall dataset's utility.


**Webapp Management** <br>
The interactive webapp, built with `Flask`, primarily consists of `HTML` files for page rendering and a `CSS` file for styling, with a base template for consistency across pages. The [app.py](https://github.com/praveen-bandla/netflixblends/blob/main/Webapp/app.py) file forms the backbone of the webapp, containing essential functions for page rendering and user interactions. Management of the user uploaded data and corresponding queries were handled through a `SQL` database. This database stores user data in unique tables, managed through functions such as `get_user()`, `insert_history()`, and `get_history()`. 

**Visualizations** <br>
Upon evaluating watch histories, our webapp provided visualizations on users' most watched content, a breakdown of genre watch history, and most viewed actors, among other key insights. The interactive data visualizations in this project were created using `Plotly`. They were then converted to a `JSON` format using `json.dumps()`, which facilitated their integration into the webapp using `Plotly.js`. These visualizations are displayed on the [insights.html](https://github.com/praveen-bandla/netflixblends/blob/main/Webapp/templates/insights.html) page, offering users an engaging way to explore data insights.




**Recommender System** <br>
The recommender system's development was created balancing efficiency, simplicity, and the complexity of movie data. Utilizing the [MovieLens 100K Dataset](https://grouplens.org/datasets/movielens/100k/), movie tags on 1000 parameters were stored for each movie. The recommendation algorithm creates corresponding user preference vectors by aggregating label values from the user's watch history, then comparing these with movie label vectors to generate interest metrics. The final recommendations are derived by averaging these metrics for each title across users, ensuring a balanced recommendation that caters to the preferences of all involved users.

<br>
<br>

<div align="center">
    <img width = "250" alt="" src="https://user-images.githubusercontent.com/114946455/235496959-ff467f56-a595-4ddc-bf28-46f3bd9eac84.png">
</div>