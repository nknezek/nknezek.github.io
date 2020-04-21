---
layout: page
permalink: /blog/
title: Blog
---
This is my personal blog. Posts will generally focus on whatever I happen to be interested in at a particular time in my life and want to share with others. Topics might include statistics, physics, space, science, trees, coding, travel, or outdoor adventures.


## Most Recent Blog Post:
<div>
    <ul class="post-list">
      {%- for post in site.posts limit:1 -%}
      <li>
        {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
        <span class="post-meta">{{ post.date | date: date_format }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
        {{ post.content | strip_html | truncatewords: 50 }}
      </li>
      {%- endfor -%}
    </ul>
</div>

## Good News
Several of my blog posts focus on highlighting the "good news" of the world. The world is becoming [objectively](https://ourworldindata.org/grapher/life-expectancy-globally-since-1770) [better](https://ourworldindata.org/wp-content/uploads/2018/03/Famine-death-rate-since-1860s-revised.png) [over](https://ourworldindata.org/grapher/literate-and-illiterate-world-population?stackMode=relative) [time](https://ourworldindata.org/grapher/world-pop-by-political-regime), yet the news is filled with [death](https://en.wikipedia.org/wiki/War_on_Terror#Casualties), [decay](https://www.economist.com/blogs/graphicdetail/2018/01/daily-chart-21), and [despair](https://www.washingtonpost.com/news/wonk/wp/2018/02/06/dont-kid-yourself-the-future-is-bleak). To re-focus attention onto the positives in the world, this blog aims to highlight one good thing in the world each Monday, Wednesday, and Friday.

Generally, the topic should be relevant within ten years in the past or future (no "we're better than the Neanderthals!"), be relatively certain in outcome (no "nuclear fusion in 5 years!"), and have a positive impact on a large portion of humanity (no "a firefighter saved a cat from a tree in Po-Dunk, KS!"). "Positive Impact" is hard to define, but this blog will generally take a medium-term secular humanist approach looking at human health, happiness, agency, freedom, and sustainability.

## Archives:
{% for post in site.categories.good-news %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}

<p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | relative_url }}">via RSS</a></p>
