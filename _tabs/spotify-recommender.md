---
layout: spotifyproject
icon: fa-brands fa-github
order: 2
---

<div align="center">
    <img width="900" alt="" src="https://github.com/praveen-bandla/SpotifyRecommendationEngine/assets/114946455/16829fc3-bc48-4d93-9ee7-4ad05d054741">
</div>

Click [here](https://github.com/praveen-bandla/SpotifyRecommendationEngine) for the complete project!

## Introduction

Recommendation engines have become a crucial element in the digital landscape, revolutionizing user interactions with content across various domains. A prime example is the music streaming industry, where Spotify owes much of its success to its advanced recommendation engines. Central to this achievement are **Graph Neural Networks (GNNs)**, a rapidly advancing area within the machine learning space over the past 5-10 years. While many established algorithms rely on static graph data, a notably growing subfield within GNNs is the study of inductive learning methods, designed to handle dynamically changing graph data. These algorithms are incredibly relevant in their real-world applications and are set to continue developing over the years to come.
<br>
<br>
For my project, I aimed to develop a GNN-based song recommendation engine, centered around three core principles:  
- Implementing an inductive learning algorithm
- Ensuring efficient database management
- Yielding high-quality training results

<br>

## Overview of Project

The project's goal was to develop a recommendation engine that evaluates a playlist and its tracks to suggest similar songs.
<br>
<br>
I used the 2020 iteration of the `Spotify Million Playlist Dataset`. The data – comprising of playlists and their associated tracks - was then organized into a bipartite graph, where playlists and songs formed the two distinct sets of nodes. These nodes were interconnected by unweighted edges, linking each song to the playlists that feature it. To train the data, I implemented a GNN using the `GraphSAGE` learning algorithm, focusing on a link prediction task. In this setup, the positive edges were denoted by the actual connections between playlists and their tracks, while the negative edges for a playlist were randomly sampled from the remaining set of songs. To generate recommendations, I fed the model with a playlist and its tracks, calculated the probability of a link to every song outside this set, and then recommended the top `n` tracks based on these predictions.
<br>
<br>
The model was trained on a dataset comprising *150,000 playlists*, which included *850,000 unique songs* and a total of *20 million edges* (10 million positive and 10 million negative). Training was completed in *48 minutes using an A100 GPU*. The trained model achieved a ***92% test AUC and an 87% F1 score***. Here is a snapshot of the training progress. See the the linked `plotly` visual for the [full interactive version](https://github.com/praveen-bandla/SpotifyRecommendationEngine/blob/main/model/150K_playlists/training_visual.html).

<br>

<img width="530" alt="Snapshot of the Training Evaluation" src="https://github.com/praveen-bandla/SpotifyRecommendationEngine/assets/114946455/a7c2f430-f8c4-459b-82a2-d50cded16ca5">



## Key Decisions

Here are three key decisions I took during my project along with the underlying rationale:

1. **Bipartite Graph construction** <br> In many of the conventional graphs I came across in my research leading up to the project, songs were represented by nodes, with weighted edges denoting a relation derived from playlists – such as the number of co-occurrences of two songs across playlists. However, this proves inefficient with scaled data, due to a high edge count and a time complexity of $O(n^2)$ for playlist additions. Instead, I elected for a [Bipartite Graph](https://mathworld.wolfram.com/BipartiteGraph.html#:~:text=A%20bipartite%20graph%2C%20also%20called,the%20same%20set%20are%20adjacent.) representing playlists and songs as my two sets of nodes, connected by unweighted edges mapping songs to the respective playlists that contain them. This reduced the edge count by ***~90% (estimated 90 million)***. This significantly increased training speed and enhanced storage efficiency. Moreover, my bipartite graph – unlike the conventional structure – did not require updating previous weights when adding new playlists, yielding a significantly improved time complexity of $O(n)$.

3. **GraphSAGE** <br> Many existing `GNN` learning methods – including `GCN` and `DeepWalk` – are transductive and rely on a static graph to train the data. Thus, any change to the database (adding new playlists for example) would necessitate re-training the entire model. In line with the guiding principles of my project, I wanted to construct a `GNN` that would facilitate a dynamically changing database. To this end, I specifically sought an inductive learning method that I could use to train on my bipartite graph structure. After research, I decided to use [GraphSAGE](https://snap.stanford.edu/graphsage/) with one layer of message passaging. I constructed a corresponding `GNN` with the number of song and playlist embeddings set at 64 (adjustable as a hyperparameter).


4. **Weighted Binary Cross Entropy Loss function** <br> A weighted binary cross entropy loss function was chosen, where the weights assigned for the positive and negative edges were 2.0 and 1.0 respectively. Given that the use case was to identify edges with the highest probability of a link existing, rendering many of the edges irrelevant, the weighted edges ensured that the positive learning was prioritized over the negative. Moreover, given that the negative edges were randomly sampled across all songs, they were deemed a less reliable source of information as compared to the positive edges.

<br>
