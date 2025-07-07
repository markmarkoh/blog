---
title: UI Algorithms Part 1
slug: ui-algorithms
description: >-
  This is a written version of a [talk I gave at JSConf in
  2014](https://www.youtube.com/watch?v=90NsjKvz9Ns), but its lessons are even
  more relevant today.
tags: []
added: 2025-07-07T00:52:01.892Z
---

User Interface Algorithms: Part 1

\=================================

\> 💡 This is a written version of a \[talk I gave at JSConf in 2014]\(https\://www\.youtube.com/watch?v=90NsjKvz9Ns), but its lessons are even more relevant today.

Admittedly, the title “User Interface Algorithms” has a good deal of ambiguity, especially back in 2014 when the most popular frameworks were Backbone.js and Angular, and modern UI development was only just picking up steam.

This article is about one particular UI Algorithm that can be leveraged to vastly improve interactivity with a data visualization.

Defining Algorithm

\==================

Let’s start with a baseline for “algorithm” in this context, finding the largest item in an unsorted list:

\`\`\`

largest ← L\\\[0\\]  

for item in lists L\\\[1..n\\], do  

   if item > largest  

      largest ← item  

return largest

\`\`\`

Simple enough! Maybe something you’d see in an entry-level Leetcode interview.

2^N Paths to the White House

\============================

During the Obama/Romney presidential race in 2012, the New York Times released an interactive data visualization called \[“512 Paths To The White House”]\(http\://archive.nytimes.com/www\.nytimes.com/interactive/2012/11/02/us/politics/paths-to-the-white-house.html).

Since we’re talking about Computer Science already, the number 512 is very round in our field — \`2^9\`, or \`1000000000\` in binary.

Based on the title alone, we can assume 9 swing states could either cast their electoral votes in 1 of 2 ways — Obama or Romney.

Here is what the data visualization looked like:

The data visualization in its default state

The visualization is highly interactive, and on election night I noticed something seemingly innocuous — the way the node highlighting acted as I moved my mouse around.

Notice how the mouse cursor doesn’t need to mouse over one of the small circles or thin path lines to interact.

As a UI engineer and curious person, my first thought was: \_How did they do that?\_ Let’s take a stab at this together.

Approach #1: Variable Sized Padding

\===================================

Perhaps we can just pad each of the nodes to make their hit state larger.

A simple algorithm to this effect could be:

\`\`\`

for each node in all(Length(L)≥0), do  

   node.padding ← node.radius \\\* 5 + ‘px’

\`\`\`

This would pad the smaller nodes less than the larger nodes, which seems like an effective approach given that there are many smaller nodes in our visualization.

The net effect of this could be visualized as:

As you can see, there is quite a bit of overlap. It’s possible that we even completely cover a neighboring node, making it completely inaccessible.

We could tune this algorithm a bit, but I think it’s a dead-end, so we’ll cut our losses and move on.

Approach #2: Closest Node

\=========================

Let’s consider a different approach — this time by calculating the distance from the current mouse position to the closest node available, and selecting that closest node, as the mouse moves around.

\`\`\`

on mouse move => (event)    

  mX, mY ← event.mouseX, event.mouseY  

  smallest ← L\\\[0\\]  

  for node in L\\\[1..n\\], do  

    dist ← distance(\\\[mX, mY\\], node)  

    if dist \< smallest, then  

      smallest ← node  

  return smallest

\`\`\`

This assumes we have a method called \`distance\` that takes the Cartesian coordinates of our mouse position and that of a node, and then it operates similar to the \`find largest item in unsorted list\` algorithm, except for looking for the smallest item instead.

As for the results? \*\*It’s great!\*\* The actual UX is identical to that of the original NYT visualization, down to the last pixel!

\_However\_, we have a major problem: That is a heavy algorithm to stick onto an event such as\`mousemove\`, which may fire thousands of times within a second or 2 of moving the mouse around. Even on modern hardware, this is going to make the fans spin and the computer heat up, maybe even crash the browser tab.

Also, consider \`2^10 Paths To The White House\`, which would be 1,024 nodes. Or \`2^12\` at 4,096 nodes. This won’t scale.

Possible workaround: \_debounce the handler\_. This would be effective at reducing CPU usage, but now we’re seeing a UI lag as the mouse moves around, giving the appearance of a choppy UX. Not ideal, and I don’t see any lag when using the NYT version.

Ultimately, I think this algorithm is a dead-end too. \*\*We were so close!\*\*

A Cholera outbreak in 1854

\==========================

Personally, after 2 failed attempts at finding the right algorithm for any problem, I usually head straight into some archaic epidemiology for inspiration.

But first, let us set the scene: It is 1854, we’re in London. Germs haven’t been discovered yet, and the leading theory of disease spread was the miasma theory — a “noxious cloud of bad air” that carried the disease from house to house.

‘A Court for King Cholera’ — scene typical of the crowded, unsanitary conditions in London slums, Punch cartoon, 1852. Photograph: Print Collector/Getty Images

After a series of devasting cholera outbreaks in London, a physician named John Snow had a theory of his own — that cholera was spreading from contaminated water, rebuking the miasma theory.

Dr. John Snow

He decided to head to the epicenter of the 1854 outbreak and walk house to house, recording data and taking notes. Ultimately, he created a map of the area, which would eventually change the world for the better.

John Snow’s data map of the 1854 cholera outbreak

A black tick mark was used to tally a death at that particular address, and a round black mark, labeled here as “pump”, was positioned at each water pump located in the area. Indoor plumbing wasn’t available everywhere at the time, so communal water pumps were the primary source for a household’s water needs.

Close up view of the map: ticks for deaths, and the communal water pumps marked as “pump”

If we were to draw a polygon around each pump, where every location within that polygon was closest to its encapsulated pump than any other pump, we can see a terrible pattern.

A modern rendering of a Voronoi tesselation laid over John Snow’s map. Source \[http\://www\.udel.edu/johnmack/frec682/cholera/cholera2.html]\(http\://www\.udel.edu/johnmack/frec682/cholera/cholera2.html)

The deaths seemed concentrated in 1 particular polygon surrounding the Broad Street pump.

In his interviews with victims’ families whose homes were closer to different pumps, he discovered most of them got their water from that center pump on Broad Street, either out of convenience, or school or work is closer to that pump.

The pump handle was removed at his urging, and the cholera outbreak stopped.

Although Dr. Snow’s new theory was initially dismissed, it was proven that a cesspool wall had failed near the pump’s well, leaking sewage into the water. It would be a decade or so until Louise Pasteur found the evidence needed to dismiss the miasma theory and solidify the Germ Theory as we know it today.¹

So why does this matter? The polygons shown above represent what we call a Voronoi tesselation. Given a series of Cartesian coordinates, there exists an algorithm to draw polygons \_around\_ each coordinate, such that all of the possible coordinates within that polygon are as-the-crow-flies closest to that point.

This is the exact effect that we’re looking for in the “512 Paths To The White House” interaction, where they are highlighting the node closest to the mouse at any given time.

If we can find a way to draw those polygons as SVG elements, and attach the browser’s native \`mouseenter\` and \`mouseexit\` events, we can have an extremely performant way to have the same interaction as our non-performant “closest nodes” algorithm, with no UI lagging or stuttering, and something that would scale well in other use cases.

Voronoi in HTML and JS

\======================

Libraries like Paper.js and D3.js have their own Voronoi APIs. In D3.js, for example:

\`\`\`

const voronoi = d3.voronoi().extent(\\\[0,0\\], \\\[500, 500\\])

\`\`\`

With this \`voronoi\` object, you can now add some DOM SVG elements:

\`\`\`

d3.select('#voronoi')  

&#x9;.selectAll('path')  

&#x9;.data(voronoi.polygons(data))  

&#x9;.enter()  

            .append('path')  

&#x9;    .on('mouseenter', (evt, {data}) => {  

           	//handle event  

&#x9;     })

\`\`\`

You can find \[a lot more examples here.]\(https\://observablehq.com/collection/@d3/d3-delaunay)

To learn more about an algorithm for generating Voronoi tesselations, \[I highly recommend this interactive demo.]\(http\://www\.raymondhill.net/voronoi/rhill-voronoi.html)

Voronoi and 512 Paths To The White House

\========================================

Using the web inspector and adding 2 quick CSS properties, we can reveal the Voronoi tessellation on top of the visualization. Your mouse is only really interacting with that Voronoi layer, which has no color or stroke, so it appears invisible.

With the Voronoi borders visible, it’s clear to see how the NYT achieved that level of interactivity. Looking around the internet for more examples of this type of interactivity yields interesting results when I open the inspector and add a \`#00000\` 5px stroke to the hidden Voronoi layers:

\[https\://www\.nytimes.com/interactive/2014/02/13/sports/baseball/jeter-long-lived-greatness.html?\\\_r=0\&mtrref=undefined\&gwh=83B1F3EFD80A90EAFECDBF8993A80061\&gwt=pay\&assetType=PAYWALL]\(https\://www\.nytimes.com/interactive/2014/02/13/sports/baseball/jeter-long-lived-greatness.html?\_r=0\&mtrref=undefined\&gwh=83B1F3EFD80A90EAFECDBF8993A80061\&gwt=pay\&assetType=PAYWALL)\[https\://bl.ocks.org/mbostock/8033015]\(https\://bl.ocks.org/mbostock/8033015)

Next up

\=======

In the next part, we’ll discuss hierarchical drop-down menus, and an algorithm that was invented 35 years ago but subsequently lost, then recently rediscovered.

¹ Read more in \[\*\*\_The Ghost Map : The Story of London’s Most Terrifying Epidemic — And How It Changed Science, Cities, and the Modern World\_\*\*]\(https\://www\.booksamillion.com/p/Ghost-Map/Steven-Johnson/9781594482694?id=8470580372820#)
