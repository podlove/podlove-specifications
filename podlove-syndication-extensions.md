Title: Podlove Syndication Extensions
Author: Tim Pritlove  
Version: 0.2  
Tags: podlove, podcast, syndication, feeds, auto-discovery 
Status: Under development
To Do: Combine HTML and RSS elements; come up with better ideas for feed declaration (maybe without atom:link); add examples

# Podlove Syndication Extensions #

The Podlove Syndication Extensions are a set of XML elements and attributes meant to complement and enrich HTML web pages and RSS podcast feeds.

The aim is to make podcast media much more visible on the web, improve the overall integrity of the podcasting process and to introduce new and helpful features for podcast clients, directories and other elements of the podcast infrastructure.

## Change Log ##

	2013-07-24: 0.1 - Under Development

## Motivation and Scope ##

Podcasts were created on top of the technical grounds of blogs but have since developed into an independent media type on the Internet that adheres to different rules and needs different treatment.

The Podlove Syndication Extensions treat podcasts as a first class web citizen and are meant to improve auto-detection of podcasts and their feeds and allow for more podcast metadata in feeds and web sites.

The extensions try not to reinvent the wheel but to build up on what is already used and well-established. Much like the iTunes RSS Extensions [^iTunesRSSExtensions] did a good job improving feed metadata empowering clients and directories, the Podlove Syndication Extensions address areas where information tends to be missing or ambiguous and where automatic discovery of important information is difficult.

By supporting the Podlove Syndication Extensions systems and applications will gain support for:

* reliably auto-detecting podcast websites
* reliably auto-detecting podcast feeds in websites
* automatically selecting podcast feeds based on its enclosure media types
* refer to podcasts using a globally unique identifier
* check media file integrity by using checksums
* provide/use more metadata

The extensions specifically target the needs of podcast directories and podcast clients by providing information that can both be easily provided and read.

##General Definitions##

The podcast world suffers from a lack of good standards and best practice specifications which has troubled developers and podcasters alike too long.

To understand the thinking behind the meaning of particular elements and attributes we first explain certain assumptions and define usage of some elements that either exist today (which we assign a certain meaning to) or concepts that we introduce with this specification.

### Podcast URI ###

This specification assumes that each podcast can and should be assigned a globally unique URI. This is possible with the `<podlove:podcast>`  element as described in this specification.

In the past, a podcast has usually been identified by the URL of its primary feed. However, secondary feeds either conveying other media types (like alternate audio or video formats) or other forms or schedules of delivery are usually left unassociated with the podcast itself causing confusion about what belongs to what.

The Podlove Syndication Extensions assume such a URI exists and provides the elements to define the URI both in web pages and feeds. A URI could be basically anything as long as is inherently globally unique. However, we encourage everybody to use the URL of the podcast home page as the URI as this information is always available (both on web pages and feeds) and is a good start to get things assigned and sorted out.

A Podcast URI makes life easier for podcast directories (as they can easily group feeds within the database representing the same podcast) and podcast clients (as they can safely select from a list of feeds and exclude non-associated feeds when presenting options to the user). Other uses may arise in the future when the idea spreads.


### Episode GUID ###

An episode GUID is the globally unique ID that is commonly known as the `<guid>` element in RSS (or the `<id>`  element in Atom). The episode GUID already plays an important role in the podcast infrastructure as it is the deciding element that podcast clients should use to reference an episode.

This specification considers the episode GUID to refer to the episode *as such*, not just the entry in a feed. If a podcast has multiple feeds and a single episode is referred to by more than one of the feeds, each entry in every referring feed *must* use the same episode GUID.

### RSS vs. Atom ###

The web world has been fighting the feed format war since the early beginning. We try to not to be too religious about this so let's just explain how we think things have turned out and should be treated today.

RSS kicked of the syndication revolution and while being a rather under defined  standard leaving a lot of things in the open, it worked really well for most applications. Atom was pushed by people who cared a lot about semantics and saw the flaws of RSS from the beginning. But it never completely replaced RSS, especially not in the podcast world.

While many blogs syndicate content via Atom feeds, the Atom standard – while being a very sophisticated well-designed format – has never caught on with podcasts. Almost every podcast feed out there is based on RSS 2.0 [^RSS2.0] and popular extensions like the iTunes Extensions [^iTunesRSSExtensions] explicitly extend RSS while they could also be used with Atom but usually nobody really cares to test the cases.

As RSS is an XML-based format, extending RSS is straightforward and well-defined. That's what XML is all about and Atom is actually used more as an extension to RSS than as a standalone format. The rather strict definitions in Atom are also not always helpful when trying to come up with practical solutions.

That's why we define the Podlove Syndication Extensions to extend HTML and RSS primarily. But you could use it on Atom feeds (and probably other formats) too. Your mileage may vary. However, this specification does not define usage of the extensions in Atom standalone documents.


### Podcast Type ###

Podcasts are technically just feeds pointing to arbitrary files (the so-called "enclosures"). However, practical usage of podcasts has created a well-established set of types of podcasts that software has learned to expect and deal with.

The types "audio", "video" and "ebook" are considered to be the basic media types. of podcasting. By far most of the content delivered via podcasts is of these types. Other types  are possible but not common. This specification reflects this.

In order to inform applications about a particular podcast type, this specification uses a "podcast type" attribute with some elements. The value is defined to be one of the following:

**audio**
:    The podcast is an audio podcast. Every episode is considered to be of type audio too.

**video**
:    The podcast is a video podcast. Every episode is considered to be of type video too.

**ebook**
:    The podcast is an eBook podcast. Every episode is considered to contain an ebook  media file

**mixed**
:   The podcast is a mixed podcast. Every episode can be of any of the types above (audio/video/ebook).

**other**
:     The podcast is a special podcast that does not fit in the usual concept of a podcast while still delivering enclosures of some kind.

### Podcast Feeds ###

Podcasts were created out of a late addition to the RSS specification. RSS 2.0 [^RSS2.0] defined the `enclosure` element that links to a media file. This could be seen as a media file being "attached" to an original article but in reality, the podcast landscape treats the enclosure as being the original feed content with the rest being treated as "descriptions" or "show notes".

Podcast support in iTunes 4.7 supported this notion  to the extreme, essentially ignoring entries without enclosures and treating enclosure-less feeds as "invalid podcast feeds". It is common practice to convey blog information in separate feeds although web based feed readers have been encouraging the dual use of feeds.

But the reality is that podcast feeds stand out and can and should be considered to be something different from blog feeds. 

The Podlove Syndication Extensions explicitly treat podcast feeds as a separate entity and assign special importance and assumes distinct behaviour of both creating and consuming applications.


##XML Namespace##

The Podlove Syndication Extensions extend both web pages (HTML) and podcast feeds (RSS). Some elements are just meant to extend HTML, others only make sense within feeds. But since some elements are useful in both groups, all elements share the same XML namespace.

The XML namespace for the Podlove Syndication Extensions is

>*http://podlove.org/syndication*

Throughout this document, we refer to this XML namespace using the namespace prefix "podlove"

    <?xml version="1.0" encoding="utf-8"?>
    <rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom"
        xmlns:podlove="http://podlove.org/syndication">


## Web Page Extensions ##

Within a HTML page that represents a podcast, the following elements and attributes are defined by the Podlove Syndication Extensions.

###Elements###

#### <podlove:podcast> ####

The `<podlove:podcast>` element should be placed into the `<head>` section of a HTML page when the page represents a podcast. The presence of this element also indicates that more meta data should be available pointing to one or more podcast feeds. So the element can (and should) be included on any page that also contains links to the corresponding podcast feeds.

>`<podlove:podcast type="*audio|video|ebook|mixed|other" uri="*podcast-uri*">`

The `type` attribute is optional and informs applications of the type of media that is associated with the podcast (see definition above).

The  `uri`  attribute is optional and should contains the podcast URI (see definition above)


#### <podlove:episode> ####

The `<podlove:episode>` element should be placed into the `<head>` section of a HTML page when the page represents a podcast episode.

> `<podlove:episode type="audio|video|ebook|other" guid="*episode-guid*">`

This element is helpful web applications that want to automatically detect podcast episodes when looking at an URL. The episode guid can be used to look up the episode within a podcast feed to get to the associated media files.

###<atom:link> ###

(THIS SECTION IS UNDER CONSTRUCTION AND TOTAL WORK IN PROGRESS AND SHOULD BE IGNORED - FOR NOW)

The `<atom:link>` element is used in HTML feeds to point to other web resources including but not limited to podcast feeds. The Podlove Syndication Extensions provide attributes that augment this pointer with valuable information for podcast clients or podcast directory crawlers.

These attributes can be used both in HTML as well as RSS when pointing to other feeds. They work especially well with the Podlove Alternate Feeds [^Podlove Alternate Feeds] recommendation.

> `<atom:link rel="alternate" type="application/rss+xml" href="*feed-url*" podlove:feed-type="podcast|blog|comments" podlove:podcast-type="audio|video|ebook" podlove:enclosure-mime-type="audio/mpeg" podlove:protocol="http|bittorrent">`

## Podcast feed extensions ##

Within a podcast feed that represents a podcast, the following elements and attributes are defined by the Podlove Syndication Extensions.

###Elements###

#### <podlove:podcast> ####

The `<podlove:podcast>` element should be placed into the `<channel>` section of a RSS podcast feed or the `<feed>` section of an Atom podcast feed. The element enables applications to automatically treat a feed as a podcast feed. The URI can be helpful to associate the feed with other feeds on the fly.

>`<podlove:podcast type="*audio|video|ebook|mixed|other" uri="*podcast-uri*">`

The `type` attribute is optional and informs applications of the type of media that is associated with the podcast (see definition above).

The  `uri`  attribute is optional and should contains the podcast URI (see definition above)

[^iTunesRSSExtensions]: [iTunes RSS Tags](http://www.apple.com/itunes/podcasts/specs.html#rss)
[^Podlove Alternate Feeds]: [Podlove Alternate Feeds](http://podlove.org/alternate-feeds)
[^RSS2.0]: RSS 2.0 Specification: <http://cyber.law.harvard.edu/rss/rss.html>
