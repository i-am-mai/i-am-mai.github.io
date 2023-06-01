---
title: Making a graph of Wikipedia links
toc: true
toc_sticky: true
excerpt: A project inspired by learning about graphs in Data Structures goes a bit off-kilter...
header:
    overlay_image: assets/images/posts/wikipedia-graph/header.webp
    overlay_filter: linear-gradient(rgba(0, 0, 0, 0.3), rgba(153, 87, 196, 0.5))
short: wikipedia-graph
---
# Graphs
Graphs are awesome. Not the kind of graphs that have a bunch of random bars or squiggly lines that no one cares to interpret, but _these_ graphs:

<iframe src="https://vasturiano.github.io/force-graph/example/load-json/" title="A graph of character interactions from Les Miserables" width="100%" height="400" frameborder="0"></iframe>

The structures made of nodes (or vertices), connected by edges. They look pretty cool, don't they? It's a nice way to see the relationships between data and how dense/sparse these connections are.

In my Data Structures class, my final project was to solve some problem using two data structures or algorithms on some dataset with at least 100,000 data points, then comparing the two. One of the ideas that my group came up with was scraping Wikipedia articles and sorting them in some way. Though the idea didn't pan out, I later decided to transform the idea into something else: creating a graph of links between Wikipedia articles.

Thus, the Wikipedia Rabbit Hole was born.

# Wikipedia Rabbit Hole

<iframe src="https://iammai.pythonanywhere.com/" title="wikipedia rabbit hole" width="100%" height="500" frameborder="0"></iframe>

The Wikipedia Rabbit Hole displays the outlinks of any Wikipedia article that you search and any following links that you click. The idea behind this website was that whenever I browse Wikipedia, I always end up on a completely different topic than what I usually start with, and I wanted to see a visualization of this "journey" across articles. 

It uses the [Wikipedia API](https://en.wikipedia.org/w/api.php) to get the article links and [this force graph library](https://github.com/vasturiano/force-graph) for the visualization.

# Some issues...
When I was first testing out the Wikipedia API, I quickly realized one small issue: each Wikipedia article linked to hundreds of other articles. If I displayed all these links at once, then the graph would be pretty much unnavigable, defeating the whole "traversing Wikipedia" idea that I initially had. So, I decided to limit the number of links displayed at once to 10, and allow the user to repeatedly click the corresponding article to generate more links.

I also eventually realized that the graphs generated would almost certainly be pretty sparse, since there are about 6,661,812 English Wikipedia articles, and each article only had around up to 1000 links. So in the graphs generated, there weren't very many links in between articles, even if they sounded like they should be related. This kind of defeated the purpose of a graph visualization, since the purpose of a graph is to see the connections between nodes (I think), and there weren't many connections.

Ultimately, I do still think that this website was an interesting exercise in using external APIs and learning about graphs, so it wasn't all for naught 🙂.

Check out the website: [Wikipedia Rabbit Hole](https://iammai.pythonanywhere.com/)