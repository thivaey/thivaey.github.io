# Hello!

My name is Tyler McLaughlin and I'm a PhD-trained scientist living in San Francisco, CA.  I am currently working at [AbbVie biopharmaceuticals](https://www.abbvie.com/), doing a postdoc in Computational Immuno-Oncology.  My scientific career began with computational systems biology research in Pittsburgh, PA and Farmington, CT.  I had a math and molecular biology double major in undergrad and so this focus felt natural.  Continuing in this scientific direction, during my PhD in Systems, Synthetic, and Physical Biology at Rice University in Houston, TX, my research involved human cell biophysics and systems biology, conducting both wet lab experiments and extensive amounts of image-based and statistical data analysis.   After that I was a Health Data Science Fellow at [Insight Data Science](https://www.insighthealthdata.com) in Silicon Valley during the Fall 2018 session where I did a project involving deep learning and brain waves.  You can learn more about me [here on LinkedIn](www.linkedin.com/in/r-tyler-mclaughlin-phd).

# Posts

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




[md](_posts/2018-04-29-jazz-scale-networks.md)
[html](_posts/2018-04-29-jazz-scale-networks.html)



### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/TylerMclaughlin/tylermclaughlin.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
