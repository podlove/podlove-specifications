Title: Podlove Syndication Extensions
Author: Tim Pritlove  
Version: 0.1  
Tags: podlove, podcast, syndication, feeds, auto-discovery 
Status: Under development  

# Podlove Syndication Extensions #

The Podlove Syndication Extensions are a set of XML elements and attributes meant to complement and enrich HTML web pages and XML podcast feeds to make podcast media much more visible on the web, improve the overall integrity of the podcasting process and to introduce new and helpful features for podcast clients, directories and other elements of the podcast infrastructure.

## Change Log ##

	2013-07-23: 1.0 - Initial Draft

## Motivation and Scope ##

Podcasts were created on top of the technical grounds of blogs but have since developed into an independent media type on the Internet that adheres to different rules and needs different treatment. The Podlove Syndication Extensions help improve auto-detection of podcasts and their feeds and allow for more metadata in feeds and web sites.

The extensions try not to reinvent the wheel but to build up on what is already used and well-established. Much like the iTunes RSS Extensions did a good job improving feed metadata empowering clients and directories, the Podlove Syndication Extensions address areas where information tends to be missing or ambiguous.

Systems and applications supporting the Podlove Syndication Extensions will gain support for:

* auto-detecting podcast websites
* auto-detecting podcast feeds in websites
* automatically selecting podcast feeds based on its enclosure media types
* refer to podcasts using a globally unique identifier
* check media file integrity by using checksums
* provide/use more metadata

The extensions specifically target the needs of podcast directories and podcast clients by providing information that can both be easily provided and read.

## Podcast GUID ##

This specification assumes that each podcast can and should be assigned a globally unique ID (GUID). This is possible with the `<podlove:podcast>`  element as described in this specification.

In the past, a podcast has usually been identified by the URL of its primary feed. However, secondary feeds either conveying other media types (like alternate audio or video formats) or other forms or schedules of delivery are usually left unassociated with the podcast itself causing confusion about what belongs to what.

The Podlove Syndication Extensions assume such a GUID exists and provides the elements to define the GUID both in web pages and feeds. A GUID could be basically anything as long as is inherently globally unique. However, we encourage everybody to use the URL of the podcast home page as a GUID as this information is always available (both on web pages and feeds) and is a good start to get things assigned and sorted out.

A Podcast GUID makes life easier for podcast directories (as they can easily group feeds within the database representing the same podcast) and podcast clients (as they can safely select from a list of feeds and exclude non-associated feeds when presenting options to the user). Other uses may arise in the future when the idea spreads.


## Episode GUID ##

An episode GUID is the globally unique ID that is commonly known as the `<guid>` element in RSS or the `<id>` (Atom) elements in feeds


## Podcast Type ##

Podcasts are technically just feeds pointing to arbitrary files (the so-called "enclosures"). However, practical usage of podcasts has created a well-established set of types of podcasts that software has learned to expect and deal with.

The types "audio", "video" and "ebook" are considered to be the basic media types. of podcasting. By far most of the content delivered via podcasts is of these types. Other types  are possible but not common. This specification reflects this.

In order to inform applications about a particular podcast type, this specification uses a "podcast type" attribute with some elements. The value is defined to be one of the following:

audio
:    The podcast is an audio podcast. Every episode is considered to be of type audio too.

video
:    The podcast is a video podcast. Every episode is considered to be of type video too.

ebook
:    The podcast is an eBook podcast. Every episode is considered to contain an ebook  media file

mixed
:   The podcast is a mixed podcast. Every episode can be of any of the types above (audio/video/ebook).

other
:     The podcast is a special podcast that does not fit in the usual concept of a podcast while still delivering enclosures of some kind.



##XML Namespace##

The extensions are split in two groups:

* Web Page Extensions
* Feed Extensions

Because these groups are highly related, all elements share the same XML namespace although most of the elements are useful in just one group. The aim is to bring websites and feeds closer together.

The XML namespace for the Podlove Syndication Extensions is

>*http://podlove.org/syndication*

Throughout this document, we refer to this XML namespace using the namespace prefix "podlove"

    <?xml version="1.0" encoding="utf-8"?>
    <rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom"
        xmlns:podlove="http:podlove.org/syndication">



## Web Page Extensions ##

Within a HTML page that represents a podcast, the following elements and attributes are defined by the Podlove Syndication Extensions.

###Elements###

#### <podlove:podcast> ####

The `<podlove:podcast>` element should be placed into the `<head>` section of a HTML page when the page represents a podcast. The presence of this element also indicates that more meta data should be available pointing to one or more podcast feeds.

* <podlove:podcast type="*audio|video|ebook|mixed|other" guid="*podcast-guid*">

The `type` attribute is optional and informs applications of the type of media that is associated with the podcast (see definition above).

The  `guid`  attribute is optional and should contains the podcast guid (see definition above)


#### <podlove:episode> ####

The `<podlove:episode>` element should be placed into the `<head>` section of a HTML page when the page represents a podcast episode.

* <podlove:episode type="audio|video|ebook|other" guid="*episode-guid*">

This element is helpful web applications that want to automatically detect podcast episodes when looking at an URL. The episode guid can be used to look up the episode within a podcast feed to get to the associated media files.

###Attributes for <atom:link> element ###

The `<atom:link>` element is used in HTML feeds to point to other web resources including but not limited to podcast feeds. When pointing to a feed, 

* podlove:feed-type="podcast|blog|comments"
* podlove:podcast-type-type="audio"
* podlove:enclosure-mime-type="audio/mpeg"
* podlove:protocol="http|bittorrent"


## Podcast feed extensions ##

Within a podcast feed that represents a podcast, the following elements and attributes are defined by the Podlove Syndication Extensions.

###Elements###

#### <podlove:podcast> ####

The `<podlove:podcast>` element should be placed into the `<channel>` section of a RSS podcast feed or the `<feed>` section of an Atom podcast feed. The element enables applications to automatically treat a feed as a podcast feed. The GUID can be helpful to associate the feed with other feeds on the fly.

* <podlove:podcast type="*audio|video|ebook|mixed|other" guid="*podcast-guid*">

The `type` attribute is optional and informs applications of the type of media that is associated with the podcast (see definition above).

The  `guid`  attribute is optional and should contains the podcast guid (see definition above)
