---
layout: page
permalink: /good-news/
title: Good News
---

The world is becoming [objectively](https://ourworldindata.org/grapher/life-expectancy-globally-since-1770) [better](https://ourworldindata.org/wp-content/uploads/2013/05/ourworldindata_life-expectancy-cumulative-over-200-years.png) [over](https://ourworldindata.org/grapher/literate-and-illiterate-world-population?stackMode=relative) [time](https://ourworldindata.org/grapher/world-pop-by-political-regime), yet the news is filled with [death](https://en.wikipedia.org/wiki/War_on_Terror#Casualties), [decay](https://www.economist.com/blogs/graphicdetail/2018/01/daily-chart-21), and [despair](https://www.washingtonpost.com/news/wonk/wp/2018/02/06/dont-kid-yourself-the-future-is-bleak). To re-focus attention onto the positives in the world, this blog aims to highlight one good thing in the world each Monday, Wednesday, and Friday. 

Generally, the topic should be relevant within ten years in the past or future (no "we've better than Neanderthals!"), be relatively certain in outcome (no "nuclear fusion in 5 years!"), and have a positive impact on a large portion of humanity (no "a firefighter saved a cat from a tree in Po-Dunk, KS!"). "Positive Impact" is hard to define, but this blog will generally take a medium-term secular humanist approach looking at human health, happiness, agency, freedom, and sustainability. 

## Archives:
{% for post in site.categories.good-news %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}