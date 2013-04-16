Title: Podlove Podcast Description
Author: Tim Pritlove  
Version: 0.1  
Tags: podlove, podcast, description
Status: Under developement  

# Podlove Podcast Description #

Podlove Podcast Description is a general specification listing essential metadata items that are necessary to describe a podcast on the Internet. The specification also defines specific file formats based on the same nomenclature for practical use.

## Change Log ##

	2013-04-09: 0.1 - Initial Draft

## Motivation and Scope ##

Podcasts were created on top of the technical grounds of blogs but have since developed into an independent media type on the Internet that adheres to different rules and needs different treatment.

This specification is meant to support auto-detection and self-description of podcast in a general manner. The metadata can be used by podcast clients, podcast directories, internet radio devices, search engines, social networks and any other system that has a notion of metadata and is capable and willing to detect podcast as separate entities.

## Elements ##

The following elements define a single podcast.

### Title ###

Identifier: `title`  
Type: `text`  
Properties: n/a  
Mandatory: yes

`title` is the title of the podcast that is the primary field to be used to represent the podcast in directories, lists and other uses. A title SHOULD not be too long. If you want to augment the core title with an explanatory extension, use the `subtitle` element.

The `title` is made up of plain text only and MUST NOT contain any HTML or other markup.


### Subtitle ###

Identifier: `subtitle`  
Type: `text`  
Properties: n/a  
Mandatory: no

The `subtitle` is an extension to the `title` element. The subtitle is meant to clarify what the podcast is about. While a title can be anything, a subtitle should be more descriptive in what the content actually wants to convey and what the most important information is, you want everybody want to know about the offering.

Subtitles are very useful for podcast directories and extended lists in podcast clients as they can explain a lot more detail to the user without any additional user interaction necessary.

Subtitles should be slightly more verbose than titles but SHOULD still try to keep it short in order to support compact presentation forms like Internet radio devices, mobile screens etc. Subtitles MUST NOT be longer than 256 characters.

The `subtitle` is made up of plain text only and MUST NOT contain any HTML or other markup.


### Summary ###

Identifier: `summary`  
Type: `text`  
Properties: n/a  
Mandatory: no

A `summary` is a much more precise and elaborate description of the podcast's content. While `title` and `subtitle` are rather concise, a summary is meant to consist of one or more sentences that form a paragraph or more.

However, the summary should still be a rather compact explanation of the podcast that is not yet conveyed by `title` and `subtitle` alone. And while technically a summary can be provided without also listing a subtitle, it would be bad practice to do so. The cascade of *Title > Subtitle > Summary* is an ideal source for software and devices that dynamically want to unfold more information about a podcast in its user interface.

The `summary` is made up of plain text only and MUST NOT contain any HTML or other markup.

### Mnemonic ###

Identifier: `mnemonic`  
Type: `text`  
Properties: n/a  
Mandatory: no

A `mnemonic` is a shorthand or abbreviation of the podcasts title meant to create simple references to the podcast itself or its episodes (usually in combination with episode numbers or other identifiers). A mnemonic is rather popular for referring to shows especially in communication environments with constrained message lengths.

### Link ###

Identifier: `link`  
Type: `text`  
Properties: n/a  
Mandatory: no

While a podcast can technically exist on the Internet without a home page, it is very rare and definitely not recommended not to have a human readable presence on the web. `link` represents the URL under which this web page can be reached.

### Image ###

Identifier: `image`  
Type: `text`  
Properties: n/a  
Mandatory: no

A podcast SHOULD be represented by an image that acts a logo and visual identifier  for the production. This is a very useful resource for all kinds of media devices and directories.

The image pointed to by the stated URL SHOULD be square (1:1 aspect ratio) and provide enough resolution in order to be useful for a variety of applications. However, huge images might also impose restrictions on less capable devices so the size should be chosen wisely. A common practice is to choose a pixel resolution between 600x600 and 1400x1400 pixels (the latter being the 2013 recommendation of the iTunes Podcast Directory).

An image SHOULD be provided in a commonly used media file format such as JPEG or PNG as these can be considered sufficiently supported by all software and devices.

### Feeds ###

Identifier: `feeds`  
Type: object  
Properties: `feed`  
Mandatory: yes

Identifier: `feed`  
Type: object  
Properties: `feed-title`,  `feed-url`,  `enclosure-mime-type`  
Mandatory: yes

The content podcast of the podcast is accessed by means of a podcast feed. A podcast might have just one or even more feeds. The description lists all available feeds within a `feeds` object which contains one or more `feed` objects. Each `feed` object itself contains the properties `feed-title`,  `feed-url` and  `enclosure-mime-type`  that describe the feed in detail.

## XML Representation ##

	<?xml version="1.0" encoding="utf-8"?>
	<podcast xmlns="http://podlove.org/dtd/podcast-description">
		<title>Podlove Podcast</title>
		<subtitle>
			Insights and outlooks by the Podlove project
		</subtitle>
		<summary>
			The Podlove team updates creators and developers
			on the latest thinking behind the Podlove project,
			it's software projects and standards.
			
			The podcast is delivered bi-weekly and welcomes
			feedback on any level. Join as we try to shape the
			future of podcasting for the greater good.
		</summary>
		<homepage>http://podlove.org/example/podcast</homepage>
		<image>http://podlove.org/example/podcast-image.jpg</image>
		<feeds>
			<feed>
				<feed-title>MP3 Audio</feed-title>
				<feed-url>
					http://podlove.org/example/podlove-feed-mp3.rss
				</feed-url>
				<enclosure-mime-type>audio/mpeg</enclosure-mime-type>
			</feed>
			
			<feed>
				<feed-title>MP4 Audio</feed-title>
				<feed-url>
					http://podlove.org/example/podlove-feed-m4a.rss
				</feed-url>
				<enclosure-mime-type>audio/mp4</enclosure-mime-type>
			</feed>
		</feeds>
	</podcast>

## JSON Representation ##

The JSON representation is a general specification on how to map the above elements in JSON structures that can be used in many applications (like [App.net](http://App.net) Annotations).

In comparison to the XML representation described above, the JSON representation does not support namespaces and therefore it can't be mixed in easily.

	{
		"title": "Podlove Podcast",
		"subtitle": "Insights and outlooks by the Podlove project",
		"summary": "The Podlove team updates creators and developers on the latest thinking behind the Podlove project, it's software projects and standards.\nThe podcast is delivered bi-weekly and welcomes feedback on any level. Join as we try to shape the future of podcasting for the greater good.",
		"homepage": "http://podlove.org/example/podcast",
		"feeds": [
			{
				"feed-title": "MP3 Audio",
				"feed-url": "http://podlove.org/example/podlove-feed-mp3.rss",
				"enclosure-mime-type": "audio/mpeg"
			},
			{
				"feed-title": "MP4 Audio",
				"feed-url": "http://podlove.org/example/podlove-feed-m4a.rss",
				"enclosure-mime-type": "audio/mp4"
			}
			]
		
	}

		
		
