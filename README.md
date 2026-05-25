# baseline-drift-vox-site

A website to provide live-updating public-facing information on the happenings in the Baseline Drift O/U-like larp. This website is in-character, but the github repo is not.

## Structure of the site

Here are the different pages in the site. Users should be able to switch between pages with the top bar.

- About Us
    - Will include information on our organization andbios on key members of Vox Omnia Rex
    - General information is found at text/about_us.md, and specific bios are found in text/bios/. The yaml front matter of these markdown docs may contain links to images which will be displayed beside the text.
- Events Calendar
    - A constantly updated events calendar on different things happening in game- Faction Wages
    - Data pulled from a google calendar as an iframe
        - https://calendar.google.com/calendar/embed?src=3d9439c9a8dd0c708b00a5a7053233b3c7672512d4a2e1179ea9448a8f0e5fb7%40group.calendar.google.com&ctz=America%2FNew_York
        - <iframe src="https://calendar.google.com/calendar/embed?src=3d9439c9a8dd0c708b00a5a7053233b3c7672512d4a2e1179ea9448a8f0e5fb7%40group.calendar.google.com&ctz=America%2FNew_York" style="border: 0" width="800" height="600" frameborder="0" scrolling="no"></iframe>
- Wages
    - A constantly table that publically displays wages for all factions at each rank
    - Data sourced from the followning spreadsheet: https://docs.google.com/spreadsheets/d/1xnhPtCq2zggGPrTUrNewlYY8JbiMfsZK9CXn8QuSppc/edit?usp=sharing

The website will have a color CRT monitor asthetic.

## Tools

- Deploying + hosting with Netlify
- Website developed with React
- [marked](https://github.com/markedjs/marked) used to convert md files into html
