Title: Podlove Simple Chapters  
Author: Tim Pritlove  
Version: 1.2  
Tags: podlove, podcast, chapters, feeds, atom, rss  
Status: Current  

# Podlove Simple Chapters #

Podlove Simple Chapters is an XML 1.0 based format meant to extend file formats like Atom Syndication [^ATOM] and RSS 2.0 [^RSS2]  that reference enclosures (podcasts). As the name implies, this format defines simple chapter structures in media files.

## Change Log ##

    2013-03-31: 1.2 - Replaced time format spec with Normal Play Time
    2012-11-18: 1.1 - Add option to link external file in feed.
                      XML namespace prefix changed to "psc:"
                      Added optional image attribute
    2012-03-26: 1.0 - Initial Release

## Motivation and Scope ##

Chapters are an increasingly asked-for feature for long media files. Talk shows and interviews spanning multiple hours and addressing lots of different topics are very common these days and chapters make things easier for the listener to either fast forward to certain topics or to recap stuff already listened to.

However, chapter marks are currently only available in MPEG-4 media files, the only "standard" being Apple's way of tagging media files that has been reengineered by some developers but is still officially undocumented. Chapter marks are also supported in MP3 [^ID3CHAPTERS] and Ogg [^OGGCHAPTERS] media files but support for these is close to nonexistent.

Placing the information in podcast feeds not only allows easy extraction of chapter information for podcast clients regardless of the media file types in use, but also enables systems to display this information before the media files themselves have been downloaded.

Simple Chapters does not try to be a comprehensive solution for all kinds of possible timeline meta data. It neither allows links to be placed independent from chapters nor does it reference images in the timeline (both features that would be supported by popular media players like iPod or iPhone).

The format simply expresses a straightforward, non-hierarchical linear structure by linking chapter titles and an optional hypertext reference to specific points in time. It is intended to be easy to author, easy to parse and easy to present to the user.

## XML Namespace ##

The XML namespace URI for this format is

>    http://podlove.org/simple-chapters

*Please note: Examples included here use the namespace prefix "psc:" but the XML standard allows you to use any namespace prefix you prefer.*

## Structure ##

The Simple Chapters specification defines two elements: `<psc:chapters>` and `<psc:chapter>`.

### <psc:chapters> ###

The `<psc:chapters>` element defines a simple chapter list within an `<atom:entry>` element in an Atom [^ATOM] feed or within an `<item>`  element within an RSS 2.0 [^RSS2] channel.

There **must** be only one `<psc:chapters>` element. Clients **should** ignore extra `<psc:chapters>` elements. The `<psc:chapters>` elements contains **one or more** `<psc:chapter>` elements.

The `<psc:chapters>` element accepts one attribute:

>**version="number"**
:    Specifies the version of this specification (current version is 1.1). This enables future enhancements of this standard. This attribute is optional and defaults to "1.0".

### <psc:chapter> ###

The <strong>&lt;psc:chapter&gt;</strong> element defines a single chapter mark within an associated media file. It does so by stating the absolute time. It does not contain any media file dependent links to specific byte ranges within that file.

The `<psc:chapter>` element accepts the following attributes:

>**start="time"**
:    Refers to a single point in time relative to the beginning of the media file. The syntax of *time* is defined below. This attribute is **mandatory**.

>**title="name"**
:    Defines *name* to be the title of the chapter. The title could be any length, but authors should consider the titles to be displayed on small devices with limited screen estate. Also, shorter titles are easier to grasp. This attribute is **mandatory**.

>**href="link"**
:    This is an additional hypertext reference as an extension of the title that refers to a resource that provides related information. This link **should** be presented to the user only in context with the chapter and it's title. This attribute is optional.

>**image="url"**
:    This is an URL pointing to an image to be associated with the chapter. This attribute is optional.

The list of `<psc:chapter>` elements within an `<psc:chapters>` element **should** be in the order of the value of their time attribute but clients **must** be able to  resort the list based on time when presenting it to the user.

## Time ##

The time is expressed in Normal Play Time [^NPT] which allows for a flexible input of time markers. 

Here are some examples that show how the time field can be used:

01:35:52
:    1 hour 35 minutes 52 seconds

7:48
:   7 minutes, 48 seconds

35:12.250
:    35 minutes, 12 seconds, 250 ms

05:12:03.500
:    5 hours, 12 minutes, 3 seconds, 500 ms

37
:    37 seconds

## Embedding Example ##

This is an simple example of an Atom-based podcast feed with embedded chapter information in Podlove Simple Chapter format:

	<?xml version="1.0" encoding="utf-8"?>
	<feed xmlns="http://www.w3.org/2005/Atom">
		<title>Podlove Podcast</title>
		
		<link type="text/html" href="http://podlove.org/" />
		<updated>2012-03-26T23:25:19Z</updated>
		<author>
			<name>Tim Pritlove</name>
		</author>
		<id>urn:uuid:8d5261ba-bf62-ac81-946c-9276ea617917c</id>
		
		<entry>
			<title>Fiat Lux</title>
			<link href="http://podlove.org/podcast/1"/>
			<id>urn:uuid:3241ace2-ca21-dd12-2341-1412ce31fad2</id>
			<updated>2012-03-26T23:25:19Z</updated>
			<summary>First episode</summary>
			<link rel="enclosure" type="audio/mpeg" length="12345" href="http://podlove.org/files/fiatlux.mp3"/>
			
				<!-- specify chapter information -->
				<psc:chapters version="1.2" xmlns:psc="http://podlove.org/simple-chapters">
					<psc:chapter start="0" title="Welcome" />
					<psc:chapter start="3:07" title="Introducing Podlove" href="http://podlove.org/" />
					<psc:chapter start="8:26.250" title="Podlove WordPress Plugin" href="http://podlove.org/podlove-podcast-publisher" />
					<psc:chapter start="12:42" title="Resumée" />
				</psc:chapters>
		</entry>
	</feed>


This is an simple example of an Atom-based podcast feed with embedded chapter information in Podlove Simple Chapter format:

	<?xml version="1.0" encoding="utf-8"?>
	<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
		<channel>
			<title>Podlove Podcast</title>
			<atom:link type="text/html" href="http://podlove.org/" />
			
			<item>
				<title>Fiat Lux</title>
				<link href="http://podlove.org/podcast/1"/>
				<guid isPermaLink="false">urn:uuid:3241ace2-ca21-dd12-2341-1412ce31fad2</guid>
				<pubDate>Fri, 23 Mar 2012 23:25:19 +0000</pubDate>
				<description>First episode</description>
				<link rel="enclosure" type="audio/mpeg" length="12345" href="http://podlove.org/files/fiatlux.mp3"/>
				
				<!-- specify chapter information -->
				<psc:chapters version="1.2" xmlns:psc="http://podlove.org/simple-chapters">
					<psc:chapter start="0" title="Welcome" />
					<psc:chapter start="3:07" title="Introducing Podlove" href="http://podlove.org/" />
					<psc:chapter start="8:26.250" title="Podlove WordPress Plugin" href="http://podlove.org/podlove-podcast-publisher" />
					<psc:chapter start="12:42" title="Resumée" />
				</psc:chapters>
			</item>
		</channel>
	</feed>

## External Chapter Information ##

Instead of embedding the chapter information as described above, you can also use an `<atom:link>` Element to link to an external resource containing the chapter information. If you want to do this, replace the chapter information in the examples above simply by including the following in the `<feed>` (Atom) or `<channel>` (RSS 2.0) element of the feed:

    <atom:link rel="http://podlove.org/simple-chapters" href="http://podlove.org/examples/chapters.psc" />

This is especially practicable to keep the feed size down or in situations where feed and chapter information are generated and/or updated separately.

[^ATOM]: The Atom Syndication Format: <http://www.atomenabled.org/developers/syndication/atom-format-spec.php>

[^RSS2]: RSS 2.0 Specification: <http://cyber.law.harvard.edu/rss/rss.html>

[^ID3CHAPTERS]: ID3v2 Chapter Frame Addendum: <http://www.id3.org/id3v2-chapters-1.0>

[^OGGCHAPTERS]: VorbisComment Chapter Extension: <http://wiki.xiph.org/Chapter_Extension>

[^NPT]: Media Fragments URI 1.0 (basic): Temporal Dimension <http://www.w3.org/TR/media-frags/#naming-time>
