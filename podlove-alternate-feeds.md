Title: Podlove Alternate Feeds
Author: Tim Pritlove
Version: 1.0
Tags: podlove, podcasts, alternate feeds, feeds, atom, rss
Status: Deprecated

# Podlove Alternate Feeds #

Podlove Alternate Feeds is a method for describing related podcast feeds from within another feed, therefore creating a group of related feeds that podcast clients can automatically detect and present to the user.

## Motivation and Scope ##

Some podcasts create multiple feeds relating to the same or similar content. Depending on which podcast feed a user has subscribed to, different types of media containers or codecs might be used. Alternative feeds might also present either audio or audiovisual representation of an original recording or present different transport mechanisms like *BitTorrent*.

Podlove Alternate Feeds is a very simple definition to make discovery of alternative feeds easy and automatic as users often never look up background information on feeds on websites and therefore might not know of their options.

Using the method described below podcast clients can decide to present this information to the user when subscribing to a feed initially (or whenever they feel appropriate) to provide additional options.

## Setting a Title for Feeds ##

Each podcast feed SHOULD include `<atom:link rel="self" ...>` element in the feeds global section to link back to its original feed. This is a common procedure. But the optional <code>title</code> attribute is usually left out. We recommend setting this title to describe the nature of the feed in the podcast feed's original language.

	<?xml version="1.0" encoding="utf-8"?>
	<feed xmlns="http://www.w3.org/2005/Atom">
	
	    <title>My Cool Podcast</title>
	    <link rel="self"
	          href="http://mycoolpodcast.com/feed/mp3"
	          type="application/atom+xml"
	          title="MP3 audio"/>

By providing a title for the current feed itself, podcast clients can display this title alongside the titles of alternate feeds (see below)

## Including Alternate Feeds ##

Extending the above section, the podcast feed CAN now list all other available podcast feeds that serve the same content.

The following example states feeds delivering different media containers for the same audio content:

	<link rel="alternate"
	      href="http://mycoolpodcast.com/feed/mp3-low"
	      type="application/atom+xml"
	      title="MP3 audio low-bandwidth"/>
	<link rel="alternate"
	      href="http://mycoolpodcast.com/feed/mp4"
	      type="application/atom+xml"
	      title="MP4 audio"/>
	<link rel="alternate"
	      href="http://mycoolpodcast.com/feed/ogg"
	      type="application/atom+xml"
	      title="Ogg Vorbis audio"/>

This example lists video variants as well as BitTorrent feeds for the same content:

	<link rel="alternate"
	      href="http://mycoolpodcast.com/feed/video"
	      type="application/atom+xml"
	      title="MP4 video"/>
	<link rel="alternate"
	      href="http://mycoolpodcast.com/feed/video-bt"
	      type="application/atom+xml"
	      title="MP4 video (via BitTorrent)"/>

Please note the only real difference in these elements is the feed URL and the title. Make sure the title is descriptive enough for the user to understand the difference at first glance. Maintain a common style among all titles to achieve coherency for the user.

The title is not meant to be machine readable or automatically selected by software. A podcast client SHOULD provide a list of options and make the user choose from the list.

## Linking in RSS ##

The above examples describe Atom feeds only. In RSS, you can use the very same method but MUST make sure you declare the Atom Syndication namespace properly and use the namespace prefix with the link element.

	<?xml version="1.0" encoding="utf-8"?>
	<rss version=2.0" xmlns:atom="http://www.w3.org/2005/Atom">
	
	    <title>My Cool Podcast</title>
	    <atom:link rel="self"
	               href="http://mycoolpodcast.com/feed/mp3"
	               type="application/atom+xml"
	               title="MP3 audio"/>
