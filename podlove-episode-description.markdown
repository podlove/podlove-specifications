Title: Podlove Episode Description
Author: Tim Pritlove  
Version: 0.1  
Tags: podlove, podcast, description
Status: Under developement  

# Podlove Episode Description #

Podlove Episode Description is a general specification listing essential metadata items that are necessary to describe a single episode of a podcast on the Internet. The specification also defines specific file formats based on the same nomenclature for practical use.

## Change Log ##

	2013-04-13: 0.1 - Initial Draft

## Motivation and Scope ##

Podcasts were created on top of the technical grounds of blogs but have since developed into an independent media type on the Internet that adheres to different rules and needs different treatment.

This specification is meant to support auto-detection and self-description of podcast episodes in a general manner. The metadata can be used by podcast clients, podcast directories, internet radio devices, search engines, social networks and any other system that has a notion of metadata and is capable and willing to detect podcast as separate entities.

## Elements ##

The following elements define a single podcast.

### Title ###

Identifier: `title`  
Type: `text`  
Properties: n/a  
Mandatory: yes

`title` is the title of the podcast episode that is the primary field to be used to represent the podcast episode in directories, lists and other uses. A title SHOULD not be too long. If you want to augment the core title with an explanatory extension, use the `subtitle` element.

The `title` is made up of plain text only and MUST NOT contain any HTML or other markup.


### Subtitle ###

Identifier: `subtitle`  
Type: `text`  
Properties: n/a  
Mandatory: no

The `subtitle` is an extension to the `title` element. The subtitle is meant to clarify what the podcast episode is about. While a title can be anything, a subtitle should be more descriptive in what the content actually wants to convey and what the most important information is, you want everybody want to know about the offering.

Subtitles are very useful for podcast directories and extended lists in podcast clients as they can explain a lot more detail to the user without any additional user interaction necessary.

Subtitles should be slightly more verbose than titles but SHOULD still try to keep it short in order to support compact presentation forms like Internet radio devices, mobile screens etc. Subtitles MUST NOT be longer than 256 characters.

The `subtitle` is made up of plain text only and MUST NOT contain any HTML or other markup.


### Summary ###

Identifier: `summary`  
Type: `text`  
Properties: n/a  
Mandatory: no

A `summary` is a much more precise and elaborate description of the podcast episode's content. While `title` and `subtitle` are rather concise, a summary is meant to consist of one or more sentences that form a paragraph or more.

However, the summary should still be a rather compact explanation of the episode that is not yet conveyed by `title` and `subtitle` alone. And while technically a summary can be provided without also listing a subtitle, it would be bad practice to do so. The cascade of *Title > Subtitle > Summary* is an ideal source for software and devices that dynamically want to unfold more information about a podcast in its user interface.

The `summary` is made up of plain text only and MUST NOT contain any HTML or other markup.


### Link ###

Identifier: `link`  
Type: `text`  
Properties: n/a  
Mandatory: no

`link` points to an URL of a human readable web page that represents the podcast episode and that may provide additional information (show notes and other metadata) about it.

The link can be used by podcast clients to forward the user tot that page or to extract additional information about the episode that is beyond the scope of this document.

### Image ###

Identifier: `image`  
Type: `text`  
Properties: n/a  
Mandatory: no

A podcast episode could be represented by an image that acts a logo and visual identifier  for the production. An episode image is meant to replace/augment an existing image representing the podcast itself. A client can either choose to use this image instead of the podcast image or in addition to it however it sees fit.

The image pointed to by the stated URL SHOULD be square (1:1 aspect ratio) and provide enough resolution in order to be useful for a variety of applications. However, huge images might also impose restrictions on less capable devices so the size should be chosen wisely. A common practice is to choose a pixel resolution between 600x600 and 1400x1400 pixels (the latter being the 2013 recommendation of the iTunes Podcast Directory).

An image SHOULD be provided in a commonly used media file format such as JPEG or PNG as these can be considered sufficiently supported by all software and devices.

### Media File Enclosures ###

Identifier: `media-files`  
Type: object  
Properties: `media-file`  
Mandatory: yes

Identifier: `file`  
Type: object  
Properties: `file-title`,  `file-url`,  `file-mime-type`  
Mandatory: yes

A podcast episode is provided to the user in form of media files. These may be audio or video media files - or even PDF or ePub documents. There may be multiple media files 


## XML Representation ##

	<?xml version="1.0" encoding="utf-8"?>
	<episode xmlns="http://podlove.org/dtd/episode-description">
		<title>Podlove Episode</title>
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
		<link>http://podlove.org/example/podcast</link>
		<image>http://podlove.org/example/podcast-image.jpg</image>
		<media-files>
			<media-file>
				<file-title>MP3 Audio</feed-title>
				<file-url>
					http://podlove.org/example/episode-xxx.mp3
				</file-url>
				<file-mime-type>audio/mpeg</enclosure-mime-type>
			</media-file>
			
			<media-file>
				<file-title>MP4 Audio</feed-title>
				<file-url>
					http://podlove.org/example/episode-xxx.m4a
				</file-url>
				<file-mime-type>audio/mp4</enclosure-mime-type>
			</media-file>
		</media-files>
	</podcast>

## JSON Representation ##

The JSON representation is a general specification on how to map the above elements in JSON structures that can be used in many applications (like [App.net](http://App.net) Annotations).

In comparison to the XML representation described above, the JSON representation does not support namespaces and therefore it can't be mixed in easily.

	{
		"title": "Podlove Episode",
		"subtitle": "Insights and outlooks by the Podlove project",
		"summary": "The Podlove team updates creators and developers on the latest thinking behind the Podlove project, it's software projects and standards.\nThe podcast is delivered bi-weekly and welcomes feedback on any level. Join as we try to shape the future of podcasting for the greater good.", 
		"link": "http://podlove.org/example/podcast",
		"media-files": [
			{
				"file-title": "MP3 Audio",
				"file-url": "http://podlove.org/example/episode-xxx.mp3",  
				"file-mime-type": "audio/mpeg"
			},
			{
				"file-title": "MP4 Audio",
				"file-url": "http://podlove.org/example/episode-xxx.m4a", 
				"file-mime-type": "audio/mp4"
			}
		]
	}

		
In the case of an App.net annotation, the JSON representation should be wrapped into a **value** field with a type value of **org.podlove.episode-description**.

	{
		"type": "org.podlove.episode-description",
		"value": 
		{
			"title": "Podlove Episode",
			...
		}
	}	
