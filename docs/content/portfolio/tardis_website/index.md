---
title: The TARDIS Website
description: TARDIS Website
date: "2022-1-2"
jobDate: 2021
work: [coding, design, documentation]
techs: [html]
thumbnail: tardis_website/tardis_website_background.jpg
projectUrl: https://tardis-sn.github.io/
draft: false
---
The [TARDIS website](https://tardis-sn.github.io/) is an official place to display the research, community, 
documentation rules, and more. The website was made to give TARDIS a proper place 
to display information that isn't relevant to the documentation of the TARDIS codebase,
as it originally was all in one place on GitHub. 

I was tasked with creating the website for the most part. The template was picked 
out by my mentor for the website, [Jalahd Singhal](https://github.com/jaladh-singhal), 
and I created a rodemap for how we should customize it to fit the collaboration's needs.

A lof of content and some re-organization was required, along with three large backend 
additions: a development pipeline, a dropdown menu and a picture grid.

The development pipeline was a workflow that I had to create. This files job was to build the website
every time a user pushed to their local github branch, as well as another that would display the main github repository 
to allow for it to be previewed while it was in development. These two workflows were critical, as they allowed
other people to check their commits, as well as make sure the changed worked on GitHub as well as locally. 

I had to learn about GitHub's CircleCI pipeline, and see how it ran in order to create two for the website.
This was somewhat easier, as I was able to use the an old TARDIS documentation preview, and just had to customize 
it for usage on the website. It took a lot of experimenting, and it was the first and most important 
step in the website development, as it made sure that your branch would deploy and allowed for other
collaborators to review your changes.




The second biggest task was creating the dropdown menu.
The dropdown menu's purpose it so easily allow for people
to select the specific subitems of a category from the main page. I had seen a dropdown 
numerous times before, and made a mini dropdown menu during my time spent learning the 
basics of HTML and CSS using the ``` z-index``` feature.

In the design of the TARDIS website, we wanted it to be that anyone could come along
later and add categories or pictures by editing a basic data file like a ```.toml``` or ```.yaml```,
so all of the work to do the dropdown menu had to be back-end, and use the Hugo
framework for how it builds the website. I began by deciding how to implement the 
dropdown menu inside of the configuration file, and then build from there. 

The items in the header were made in the ```.yaml``` file under a certain dictionary, 
so I expanded those items by creating a new style, dropdown, and then giving that style sub items.

```.toml {.scroll}
      - label: Home
        url: /
        style: link
      - label: Documentation
        url: /documentation
        style: dropdown
        sub_items:
          - label: tardis package
            url: https://tardis-sn.github.io/tardis
            style: redirect-link
```

After I had created the basic structure for how the dropdown menu is seen by Hugo, 
the website builder, I had to go and create the structure and the rules behind it. This was
the hard part for me.

I first had to learn this website's layout, and what files created the header and how they were called. 
Afterwards, I made my own edits to these files to properly format and add the rules and displays for
how they would be called. This required learning a lot about Hugo shortcodes, which is a mini 
pre-defined template, as well as experimentation for layouts and calling to make the most 
intuitive way for this to be done. The work that I added to accomplish this in the Hugo framework 
can be seen [in this file](https://github.com/tardis-sn/tardis-sn.github.io/blob/master/layouts/partials/action.html#L7)
on the GitHub. 

While I was working on the changing and creating a framework for Hugo to use, I also
was creating the css that would properly display the rules. It took a lot of revisions and help
to get a proper rule set to display a dropdown menu in both desktop and mobile view. It was the most
complex set of rules I had written, and the mobile formatting changed it from a vertial to horizontal layout
for the home menu, so I had to almost design the dropdown menu twice. 

The result of all of this hard work and learning was a professional menu for TARDIS, so overall
this effort was well spent.

Here is what the current dropdown menu looks like.
{{<figure src="dropdown_menu_pc.jpg" title="Desktop view">}}
{{<figure src="dropdown_menu_mobile.jpg" title="Mobile view">}}




The greatest challenge of making the website was the picture grid, viewable 
[here](https://tardis-sn.github.io/team/community_roles/). 
{{<figure src="picture_grid.jpg">}}

This would be a way to display the key members of TARDIS, viewable on mobile 
and desktop. I had to design a card that would be able to show their title, image, name, and
additional information about them. The display of these cards had to be able to grow and shrink
depending on the monitor, so it couldn't be fixed on the screen. Additionally, I had to think of how
to make it so that longer titles or names wouldn't break the grid; each card in the row had to adjust
the size of the box that held it so that they would be uniform, but the cards below could revert to 
the original sizes. 

This was the hardest task, because it was the most abstract for me. I had never 
designed something like this before; I had no baseline to fall back to. In order 
to tackle this, I started by hardcoding the HTML displays, manually organizing 
the divs to hold the menu, and making each card. I started with 6 cards, which would 
be two rows. I then used titles and names of varying lengths to test the extreme inputs 
of my picture grid, to see what I needed to do in order to keep the formatting when
edge case occurred. 

``` html {.scroll}
<div class ="picture-grid">
    <div class ="individual-container">
        <div class ="info-container">
            <img class="rounded-picture" src="sunset_image.jpg">
            <div class ="person-name">Brilliant Sunset</div>
            <div class ="small-bio">
                <a href="#" target="_blank" rel="noopener nofollow">More Info</a>
            </div>
            <div class ="role-box">
                <a href="#science-coordinator">Shining Star</a>
            </div>
        </div>
    </div>
</div>
```

This is the final version of how it was displayed, and hardcoding like this allowed me 
to experiment with the order of items being displayed, as well as which items are under 
the same parent. It also allowed me to focus on just the ruleset behind the picture grid.
In the dropdown menu, I had to deal with rulesets, hugo shortcode organization, and making
an easy input method for modifying the menu. Here, I decided to skip both of these, and just
focus on the ruleset for doing this, as the other features could come later. 

I spent a long time trying out different rules, organizations, and display methods for this 
grid. I ended up using divs for all of the organization, and flex for the displays. I chose these
two because they had the features that I needed to organize the individual cards in the picture grid. 

In making the pictute grid I got some lessons on design. How whitespace affects which word is related
to the picture, as well as visual design of using softer colors and curved edges. 

Creating the picture grid took me a long time, as I had to learn a lot of new skills and 
go into the unknown for me, but the result of all of this work is a more professional website 
for the TARDIS collaboration.