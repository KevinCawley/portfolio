---
title: "Learning HTML and CSS"
date: 2022-01-03T10:27:30-06:00
draft: false
---
In the spring of 2021 I was assigned to help create the TARDIS website.
In order to do this I had to learn both the basics of HTML and CSS, which I did 
using the website [internetting is hard](https://www.internetingishard.com/).

I learned about positioning, displays, margings, and interactive elements. After I 
finished all of the website modules I went on to my first task, which was the creation
of this website in order to put my skills to the test. It mostly centered around using 
existing templates and archetypes, and then manipulating what was necessary for customization.

I was told that this is the most efficient way to start with, as what I am trying to do 
has been done before, so examples exist that I can look at. This website took a while, as 
I had to get familiar with a much more complex layout than the basic one-page projects I 
had been doing. Instead of HTML and CSS in unison, Hugo uses a framework of templates in order 
to build the website quickly, so I had to learn this organization and how to manipulate these templates
to apply my changes, instead of just inserting a class when I wanted a certain action. Instead of every page
having itself built in HTML, it would instead have frontmatter that would interact with templates to build
out the page, with the content being plain text. An example below is for how the [portfolio](/portfolio) section
is built

```html {.scroll}
<div class="portfolio-list">
    <a href="{{.Permalink}}">
      <div class="portfolio-container">
        <img title="{{.Title}}" alt="{{.Title}}" src="portfolio/{{ .Params.Thumbnail }}" />
        <div class="img-overlay">
        </div>
        <div class="portfolio-details">
          <h2><span>{{ .Title }}</span></h2>
          <p class="small">{{ delimit .Params.Work ", " | safeHTML }}</p>
          <p class="small">{{ .Params.jobDate }}</p>
          
        </div>
      </div>
    </a>
  </div>
```

This is very different from building a website using divs, and it took a 
while to learn. However, now that I understand this type of framework I can 
easily modify large portions of my website. I also learned how to create custom shortcodes,
such as my ```{.scroll}``` shortcode, to deal with long bits of codeblocks such as the one above!

After this website's initial version was finished, I went on to working on the
[TARDIS webiste](https://tardis-sn.github.io/), which present a lot of new challenges.
I talk about it more [here](/portfolio/tardis_website).