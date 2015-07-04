---
layout: post
title: Planning for Relocation - Phase I
categories:
- blog
- repo
- hoh-gwuhn
---

At some point in the future, I will move away from Minnesota.  I have no idea when, or where I will
be going, but it's inevitable.

The problem, however, is that as of this writing, there are 125 schools for Kuk Sool Won in the US,
with a fair number concentrated in the SF Bay area (the first US headquarters location) and Texas
(HQ is currently located in Houston).  I'm pretty determined to get my black belt at a minimum
before I start venturing off the pasture, but to do so within the next three years I'm shooting for
I really can't afford to isolate myself.

A project is born.

There's three main parts to this:

  * **Phase I**: Actually get the school data for the US.  Since I'm not likely to move to
    the EU, we can skip that for now.
  * **Phase II**: Geocode the schools and shove them into a _kd-tree_ or some other distance-query
    friendly structure. 
  * **Phase III**: Find the nearest school to a given relocation city, returning the distance
    (Ideally driving distance, but miles would suffice) of the best match.
    
This ignores a lot of details, like not all KSW schools are entirely equal
(Schedules, location, etc.), but they are close enough because of the unification efforts that it's
not too much of a worry, especially compared to other things like rush hour traffic considerations.

In either case, Phase I seems to be more or less done, the result for now
[hanging out here](https://github.com/adyates/ksw-school-scrape). Pulling the data from the site is a 
semi-normal BeautifulSoup4 / Requests combination in Python, but the data is a mix of addresses,
phone numbers, and non-useful text (Primarily "New Location!!" appears a lot, but some locations
have vanity phone numbers), so turning it into
something more structured was interesting.  Although missing the "HTML form !== AJAX" in body
formation took longer than I'd care to admit.

When I was at Location Labs, one of the fun projects I worked on introduced me to Google's
libphonenumber, which back then was a Java jar and JS port only.  Looking into it now, it's part
of the googlei18n Github, but also has annoying Android bits to it.  There's also an 
address-equivalent library for offline geocoding, but that's another "strip and self build"
process I'm a bit too lazy for.

Luckily, there's a Python port of libphonenumber, and the geocoding is doable by both Bing's
REST API as well as Google's Geocoding API (Which seems to be a bit more reliable and lenient,
although lacking in the detail Bing provides).  So a little manual cleanup and I can resume with
hacking this thing together.
