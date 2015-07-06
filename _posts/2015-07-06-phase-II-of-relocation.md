---
layout: post
title: Planning for Relocation - Phase II
categories:
- blog
- repo
- hoh-gwuhn
---

Geocoding itself was actually straight forward.  Google's Map APIs are surprisingly easy to use and
have the advantage of not requiring an API key to use (Unlike their Bingified competition), so
getting the geocoding data was not really a problem.

However, since I'm going to be going this with an actual web browser (Since I'll be getting
employment locations in text in all likelihood, I'll probably need redefine
the phases as:

  * **Phase I**: Actually get the school data for the US.  Since I'm not likely to move to
    the EU, we can skip that for now.
  * **Phase II**: Geocode the schools.
  * **Phase III**:  Build a fun Chrome plugin that {can load the geo data | preloads the geo data} 
    and {shove them into a _kd-tree_ or some other distance-query friendly structure | brute force
    all the things} to  finding the nearest school to a given relocation city, 
    returning the distance of the best match (in time to drive, mi. or whatever)

Also, since I had the data anyway, I decided to do something a little more interesting: 
Actually generate maps of the schools as is.

I've never actually _seen_ a real distribution of where the current schools are located, and,
although I know that the SF Bay does have a high concentration of schools (Because I needed to find
a school to play after I started work traveling back to Mountain View) and that there's probably
a similar effect near Houston, 

Once I started digging into it, though, the static map APIs for Bing and Google both are limited in
what they can provide:

  * [Google's Static Map API](https://developers.google.com/maps/documentation/staticmaps/#Limits)
    is limited to request URLs of 2048 characters.  Doing some
    [comical math](https://github.com/adyates/ksw-school-scrape/blob/master/geovis.py#L10) ends up
    meaning that I'm guaranteed 65 markers.  Beyond that, I would need to recheck string lengths to
    squeeze out the maximum value, or if I was feeling particularly fanciful, throw it all away
    to use an approximation algorithm for binpacking.
  * [Bing's Imagery REST API](https://msdn.microsoft.com/en-us/library/ff701724.aspx) is limited
    to 18 pushpins for requests using GET, but will do up to 100 using a POST and request body.
    However, the fact that you need to specify either the center point or bounding box manually
    coupled with a zoom level is... undesirable. 
    
Since there's 106 schools, I'm pretty much resigned to using two maps.  So, because it's easier,
Google wins again.  Bing, I am disappoint.

The results are pretty self-explanatory, but some of the things that I immediately notice are:

  * Schools cluster a lot more than I thought.  The OK/AR cluster I wouldn't have expected at all,
    and there's a handful of others as well.  To be sure, there is a small handful of people who
    own two schools, but that's definitely not the norm.
  * I've always heard that WKSA considers the MN school somewhat isolated.  Which although true,
    has nothing on Montana, which has a solid one-state buffer from everyone else (Although there
    are a few Canadian schools).  It ends up having the unfortunate honor of being the only school
    on Mountain time.
  * Sault Ste. Marie, MI looks like a bug at first glance, but it's a real town.  I feel warm and
    fuzzy knowing it's there.
  * The mountainous regions are surprisingly barren.  Even places that I would expect people to
    eventually migrate to from tech community to tech community (There are a ton of schools in SF
    and two in Austin) have zero presence (e.g. Seattle, Boulder).
  * From the school list, I always kinda assumed that the eastern seaboard didn't have many schools
    at all.  But that perception seems based on being split across 3 of the 4 regional designations,
    because there is a fairly nice spread.

And for some pretty pictures:

![KSW School Map 1](https://maps.googleapis.com/maps/api/staticmap?size=1280x720&markers=40.1013126,-88.236018%7C40.94781580000001,-90.3712395%7C40.9083715,-90.28484759999999%7C40.5620444,-89.6543421%7C40.751192,-89.636731%7C39.67671,-85.137948%7C40.1933767,-85.3863599%7C39.8289369,-84.8902382%7C41.554781,-90.516629%7C41.6011836,-90.3469592%7C43.0344822,-83.59652779999999%7C42.4897411,-83.1054001%7C46.4905722,-84.3589714%7C43.083894,-86.15338899999999%7C44.965072,-93.1849075%7C38.6161385,-90.3499551%7C38.8106075,-90.69984769999999%7C38.7790601,-90.577469%7C41.5050985,-81.55415789999999%7C39.86342,-84.211535%7C41.2392227,-81.3459405%7C43.08822319999999,-89.4366996%7C43.069564,-87.90000099999999%7C39.8219769,-75.464353%7C38.9205563,-76.99772949999999%7C38.888634,-76.86861390000001%7C38.7205475,-76.66059729999999%7C38.9116297,-76.8675245%7C39.1426709,-76.860565%7C42.4225752,-71.6839306%7C42.9311065,-76.5664901%7C43.0936257,-73.7435465%7C43.22486199999999,-77.36005399999999%7C43.22391,-77.1868255%7C43.2232876,-76.8231938%7C36.0726354,-79.7919754%7C39.0614476,-121.4456215%7C37.86254,-122.281507%7C38.444632,-121.81705%7C37.7058405,-121.8811949%7C38.7118121,-121.0860436%7C39.2526252,-121.0249792%7C37.464095,-122.42944%7C33.660297,-117.9992265%7C34.673073,-118.439895%7C38.8875157,-121.3144153%7C37.4526658,-122.178043%7C37.5993939,-122.383542%7C37.3974938,-122.1047489%7C34.1647093,-118.3712922%7C37.5933405,-122.5040197%7C34.57572469999999,-118.1175771%7C38.6799211,-120.8701946%7C37.4660422,-122.2409369%7C38.7154013,-121.2908245%7C38.35574,-122.706833%7C38.5725551,-121.336341%7C37.7749295,-122.4194155%7C37.6754168,-122.1244614%7C37.5437629,-122.3064895%7C34.364558,-118.5544679%7C37.7123009,-122.40729%7C39.0102838,-121.4228166%7C38.6734438,-121.7933615%7C39.137391,-121.608767)

![KSW School Map 2](https://maps.googleapis.com/maps/api/staticmap?size=1280x720&markers=47.5076688,-111.2931679%7C35.311605,-94.4019982%7C35.3859242,-94.39854749999999%7C35.2070891,-94.23668819999999%7C34.8671921,-92.11643699999999%7C36.2827795,-94.1271666%7C30.4881959,-86.4948222%7C26.562145,-81.86275479999999%7C30.4213635,-86.69671110000002%7C29.895945,-90.050314%7C29.9393052,-90.12142349999999%7C36.294226,-95.3038407%7C35.8894532,-94.9738778%7C34.512242,-82.65543799999999%7C34.6969826,-82.87716379999999%7C34.196969,-79.70708359999999%7C34.93380700000001,-82.280835%7C34.8190273,-82.29764019999999%7C32.443455,-99.75143899999999%7C29.401743,-95.24654799999999%7C30.2779841,-97.81636669999999%7C30.3317278,-97.70515800000001%7C30.2282792,-97.8630366%7C29.7527408,-94.94431740000002%7C25.9822238,-97.4861093%7C29.5656837,-95.1447204%7C29.833673,-95.818742%7C30.0640252,-95.226948%7C30.190219,-95.7168502%7C33.01663,-96.61356459999999%7C29.5525014,-95.2551358%7C33.2364694,-96.80278179999999%7C29.5512844,-95.6975781%7C29.7422287,-95.4344541%7C29.4962399,-98.6217374%7C29.4699256,-98.43833540000001%7C29.5102885,-98.37928400000001%7C30.144805,-95.44932%7C30.2097374,-95.57755300000001%7C30.07924059999999,-95.7397793)

As a side note, if I ever add more regions, the maps would probably vary so much in zoom that simply
going by limit would no longer make sense (The likely scenarios being mixed US / Korea and 
Korea / UK sets, among others).  However, before I do that I would probably re-export the
data to include higher-level region designation for the US (e.g. Central, Southern) and work it
that way.  Korea would be similar, since it has an eye count of ~100 schools, but the rest should be
okay since the UK doesn't have the density of Korea.