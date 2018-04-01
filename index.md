---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---

<!-- # Nicholas R. Knezek -->
This is the personal website of Nicholas R. Knezek, PhD candidate in the Department of Earth and Planetary Science at UC Berkeley and aspiring data-scientist. You can read about my [projects]({{site.baseurl}}{% link projects.md %}) or check out my full [resume]({{site.baseurl}}{% link resume.md %}). I've also recently started a [blog]({{site.baseurl}}{% link goodnews.md %}) focusing on good news stories in the world:

# [Recent Good News]({{site.baseurl}}{% link goodnews.md %})
<div class="home">
    <ul class="post-list">
      {%- for post in site.posts limit:3 -%}
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
feel free to <a href="{{ "/feed.xml" | relative_url }}">subscribe via RSS</a> for updates.

