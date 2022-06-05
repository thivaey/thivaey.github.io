# hi

i'm **divey** (IPA: [/ðɪˈveɪ/](http://ipa-reader.xyz/?text=ðɪˈveɪ%20)) and i like doing data science stuff and making visualizations in d3.js. 

i graduated with a BS in computer science from the university of illinois urbana-champaign in 2021. i currently work at [ServiceNow](https://www.servicenow.com/) as an associate systems engineer/data analyst. i also help support progressive casues as a data fellow at [Bluebonnet Data](https://www.bluebonnetdata.org).

# posts

<ul style="padding-left:0px;">
  {% for post in site.categories.blog %}

      <h2>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>

      <span class="text-warning">{{ post.date | date: "%b %-d, %Y" }}</span>
      <p>{{ post.content | strip_html | truncatewords:75}}</p>
      <a href="{{ post.url | prepend: site.baseurl }}">Read more...</a><br>

  {% endfor %}
</ul>

<!-- 
[md](_posts/2018-04-29-jazz-scale-networks.md)
[html](_posts/2018-04-29-jazz-scale-networks.html) -->
s