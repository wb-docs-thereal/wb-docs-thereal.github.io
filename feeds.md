---
layout: page
title: Feeds
---


###### The Real provides the following types of feeds:

+ __atom__ - http://en.wikipedia.org/wiki/Atom_(standard)
+ __dfp__ - https://support.google.com/dfp_premium/answer/2476004?hl=en&ref_topic=2475994&rd=1
+ __rss__ - http://en.wikipedia.org/wiki/RSS
+ __mrss__ - http://en.wikipedia.org/wiki/Media_RSS

*Unless otherwise noted, if the URI pattern contains `{feed_type}` then any of the above types will work.*


###### Content Types:

+ __photos__ - May include links to The Real photo galleries, Instagram, etc.
+ __posts__ - Currently renders only The Real articles.  *May include other content types in the future.*
+ __videos__ - May include The Real Kaltura videos, YouTube videos or playlists and Vine videos.

*Unless otherwise noted, if the URI pattern contains `{content_type}` then any of the above types will work.*


###### Paging:

Feeds that support pagination will have an `atom:link` with the next link to follow, e.g.
`<link rel="next" type="application/atom+xml" href="{next_link}"/>`



***



## Available Feeds


###### All Content
+ `https://feeds.thereal.com/{feed_type}`
+ `https://feeds.thereal.com/{content_type}/{feed_type}`


###### Latest News
This is the The Real home page feed.

+ `https://feeds.thereal.com/latest-news/{feed_type}`
+ `https://feeds.thereal.com/latest-news/{content_type}/{feed_type}`


###### Filtered by Hashtag
+ `https://feeds.thereal.com/tags/{hashtag}/{feed_type}`
+ `https://feeds.thereal.com/tags/{hashtag}/{content_type}/{feed_type}`


###### Filtered by List
+ `https://feeds.thereal.com/lists/{list}/{feed_type}`
+ `https://feeds.thereal.com/lists/{list}/{content_type}/{feed_type}`



## ETags

All feeds will return an ETag that can be used to conditionally fetch a feed.  For example,
passing a header `If-None-Match: ETAG` will return a 304 status code if the content has not
been modified.  See [HTTP ETag](http://en.wikipedia.org/wiki/HTTP_ETag).

__CURL Example:__
{% highlight bash %}
# Get the ETag for the feed
etag=$(curl -s -I "https://feeds.thereal.com/rss" 2>|/dev/null | awk '/^ETag: / {print $2}' | sed 's/\"//g')
etag=`echo -n "${etag//[[:space:]]/}"`

# Verify the ETag, should return "HTTP/1.0 304 Not Modified"
eval "curl -s -I -H 'If-None-Match: \"$etag\"' \"https://feeds.thereal.com/rss\""


{% endhighlight %}