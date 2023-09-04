---
title: Visualizing parking spaces with OpenStreetMap
toc: true
toc_sticy: true
excerpt: It might seem like there's never enough parking, but how much parking really is there?
header:
    overlay_image: assets/images/posts/parking-calculator/header.webp
    overlay_filter: linear-gradient(rgba(0, 20, 50, 0.8), rgba(0, 0, 0, 0.4))
short: parking-calculator
---
# A car conundrum
Recently, I've become a bit interested in urban planning, and one of the things that has struck me the most (as a U.S. citizen) is how car-dependent many of our communities are. Take my hometown of Pensacola for example:

{% include picture.html img="1" ext="jpg" alt="Escambia Country transit map" caption="Escambia Country transit map" %}

This is a map of Escambia County's bus system, and though it does have decent coverage, most buses only run until 5 PM and come by every 2-4 hours, making them impractical for many people. 

So, for most of my life I've had to get around town using a car, and one of the consequences of car dependency that I wasn't really aware of until recently was the amount of land that's needed to sustain all these cars. Take, for instance, parking:

{% include picture.html img="2" ext="png" alt="A picture of downtown Houston, with parking lots outlined in red." caption="Downtown Houston, with parking lots outlined in red." %}

Just parking alone takes up [26% of downtown Houston](https://www.houstonchronicle.com/news/houston-texas/article/downtown-houston-parking-one-quarter-area-17888647.php). Which made me wonder: how much parking really is there?

# The Parking Calculator
So, I created a website that displays all of the parking in an area, as well as how much space it uses.

{% include picture.html img="3" ext="png" alt="Screenshot of the parking calculator website." caption="The Parking Calculator! The outlined blue areas represent spaces used for parking, and the purple circle represents the combined area of all the parking spaces." %}

It displays the parking in the area that you select, as well as the percentage of total area that parking takes up. 

The parking data comes from OpenStreetMap using the Overpass API. Each time you press the calculate button, the website basically makes the following query to the API:

```{% raw %}
[out:json][timeout:25];
(
    way["amenity"="parking"]({{bbox}});
    relation["amenity"="parking"]({{bbox}});
    way["highway"]["parking:lane:both"]["parking:lane:both"!~"no"]({{bbox}});
    way["highway"]["parking:lane:right"]["parking:lane:right"!~"no"]({{bbox}});
    way["highway"]["parking:lane:left"]["parking:lane:left"!~"no"]({{bbox}});
);
out body;
>;
out skel qt
{% endraw %}```

This outputs a JSON representation of any areas with the "parking" amenity tag as well as lane parking. Then, the JSON is converted to GeoJSON, and the total area is calculated using [turf.js's area](https://turfjs.org/docs/#area) function. 

Since street parking is represented as a line, I estimated the width of street parking to be 2.7 meters and buffered the lines to add them to the area calculation.

The calculator also displays the total parking area, an estimation of the number of parking spaces, and the percent of bounded area that parking takes up.

{% include picture.html img="4" ext="png" alt="Parking results for the University of Florida" caption="Parking results for the University of Florida" %}

You can try it out here: [Parking Calculator](https://parkingcalculator.com)

And here's a link to the [source code](https://github.com/i-am-mai/parking-calculator).
