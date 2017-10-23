Title: Podlove Streaming Extension  
Author: Tim Pritlove, Stefan Trauth  
Version: rc1  
Tags: podlove, podcast, live, streaming, stream, feeds, atom, rss  
Status: Current  

# Podlove Streaming Extension #

Podlove Streaming Extension is an XML 1.0 based format meant to extend file formats like Atom Syndication [^ATOM] and RSS 2.0 [^RSS2]  that reference enclosures (podcasts). This format defines a way to schedule podcast livestream events in the podcast feed.

## Change Log ##

    2016-XX-XX: 1.0 - Initial Release

## Motivation and Scope ##

Many podcasts livestream their shows to a big audience in addition to only distribute recordings. Currently there is no open standard for podcasts to communicate livestream related information like:

* When does a livestream take place? How long will the show be?
* How can one listen to it?
* Title, Subtitle, Description

It is important to distribute livestream information as open as possible to keep it independant from a specific livestreaming service. The best place for this kind of information is the podcast feed as everyone can fetch and parse it to get the information.

## XML Namespace ##

The XML namespace URI for this format is

>    http://podlove.org/live-streaming

*Please note: Examples included here use the namespace prefix "pse:" but the XML standard allows you to use any namespace prefix you prefer.*

## Structure ##

The Streaming Extension specification defines five elements:

* `<pse:live-streaming>`
* `<pse:time>`
* `<pse:streams>` and `<pse:stream>`
* `<pse:webhook-registration>`

The main idea is to schedule livestream shows as `items` in the feed without adding an `enclosure`. This way the live show items will be ignored by standard podcast clients.

### <pse:live-streaming> ###

The `<pse:live-streaming>` element works as an indicator to show that this podcast does live shows. This allows podcast directories to filter for just this information.

As it is independant from a specific scheduled show this element is added to the `<channel>` element. The `<pse:live-streaming>` element **must be present** if the podcast supports live streaming.

The `<pse:live-streaming>` element accepts the following attributes:

>**`info-url="url"`**
:    This is an URL pointing to a website which provides all additional information the podcaster wants to share with his livestream listeners (like chat information, iCal for the live shows, ...). This attribute is optional.

Example:

```xml
<pse:live-streaming info-url="http://freakshow.fm/live"/>
```

### <pse:time> ###

The `<pse:time>` element defines the start and end time of a live show represented as an `<atom:entry>` element in an Atom [^ATOM] feed or within an `<item>`  element within an RSS 2.0 [^RSS2] channel.

There **must** be only one `<pse:time>` element. Clients **should** ignore extra `<pse:time>` elements.

The `<pse:time>` element accepts the following attributes:

>**`start="date"`**
:    This attribute defines the start date and time of the live show. This attribute is **mandatory**.

>**`end="date"`**
:    This attribute defines the end date and time of the live show. This attribute is optional.

Example:

```xml
<pse:time start="2015-03-28T12:34+1" end="2015-03-28T17:34+1">
```

*TODO: Define time format*

### <pse:streams> and <pse:stream> ###

The `<pse:streams>` element defines the available stream sources within an `<atom:entry>` element in an Atom [^ATOM] feed or within an `<item>`  element within an RSS 2.0 [^RSS2] channel.

There **must** be **one or more** `<psc:streams>` elements. Each `<psc:streams>` element **must** contain **one or more** `<psc:stream>` elements.

The `<pse:streams>` element accepts the following attributes:

>**`media="string"`**
:    This attribute defines if this is an audio or a video livestream. Possible values are: `audio` or `video`. This attribute is **mandatory**.

>**`title="string"`**
:    This attribute defines a title for this livestream source. This attribute is optional.

>**`lang="string array"`**
:    This attribute defines the language of this livestream source. It contains a comma separated list of language codes, e.g. `de` or `en`. This attribute is optional.

Example:

```xml
<pse:streams media="audio" title="Standard Audio" lang="de">...</pse:streams>
```

The `<pse:stream>` element accepts the following attributes:

>**`title="string"`**
:    This attribute defines the title of this stream source. This attribute is optional.

>**`mime-type="string"`**
:    This attribute defines the mime-type of this stream source, e.g. `audio/opus`, `audio/mp3`, `application/x-mpegURL` (hls), `application/dash+xml` (dash). This attribute is **mandatory**.

>**`bitrate="number"`**
:    This attribute defines the bitrate of the audio stream source. It is defined as bits per second. This attribute is optional.

>**`url="url"`**
:    This attribute defines the stream source url. This attribute is **mandatory**.

Example:

```xml
<pse:stream title="Deutsch OPUS" mime-type="audio/opus" bitrate="128000" url="http://streams.xenim.de/metaebene.opus" />
```

### <pse:webbook-registration> ###

The feed only contains basic metadata about scheduled live shows. If you are interested in realtime information about a running livestream you can consider registering to a webhook. The webhook is usually provided the by streaming backend of the podcaster.

This way you can get realtime information:

* Is the stream already running?
* Did the show start?
* How many people are currently listening to the livestream?

The `<pse:webhook-registration>` element defines 

Example:

```xml
<pse:webhook-registration>URL</pse:webhook-registration>
```

*TODO: registration protocol*

*TODO: webhook protocol*

## Embedding Example ##

This is a simple example of a RSS podcast feed with embedded livestream information in the Podlove Streaming Extension format:

```xml
<?xml version="1.0" encoding="UTF-8"?><rss version="2.0"
	xmlns:atom="http://www.w3.org/2005/Atom"
	xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd" xmlns:psc="http://podlove.org/simple-chapters" xmlns:content="http://purl.org/rss/1.0/modules/content/" xmlns:fh="http://purl.org/syndication/history/1.0" xmlns:pse="http://podlove.org/live-streaming">

<channel>
	<title>Funkenstrahlen</title>
	<itunes:subtitle>Die Nachlese aus dem Apple Special Event</itunes:subtitle>
	<link>http://podcast.funkenstrahlen.de</link>
	<description>Technik, Internet, Gesellschaft.</description>
	<lastBuildDate>Fri, 13 May 2016 11:22:17 +0000</lastBuildDate>
	<image><url>http://podcast.funkenstrahlen.de/wp-content/cache/podlove/b8/384e4a23bd372ef6ef74855365d14e/funkenstrahlen_original.png</url><title>Funkenstrahlen</title><link>http://podcast.funkenstrahlen.de</link></image>
	<pse:live-streaming info-url="URL"/>
	...
	<item>
	    <title>FS002 - Arduino Blinkenlichter</title>
	    <link>http://podcast.funkenstrahlen.de/2013/02/27/fs002-arduino-blinkenlichter/</link>
	    <pubDate>Wed, 27 Feb 2013 08:46:18 +0000</pubDate>
	    <pse:time start="2015-03-28T12:34+1" end="2015-03-28T17:34+1">
	    <guid isPermaLink="false">podlove-2015-05-17t15:21:00+00:00-9a6539d4d72f6e7</guid>
	    <description>Die zweite Folge und schon eine Sonderfolge. Ich erzähle von meinem Arduino Projekt, an dem ich die letzten Tage gebastelt habe. Dabei versuche ich zu erklären wie man die LED-Leuchtleisten von IKEA an den Arduino anschließen kann, welche Bauteile man dafür braucht und wie ich es geschafft habe, dass man die Farbe der LEDs dann mit dem iPhone steuern kann. Durch die simple API bieten sich nun unendlich viele Möglichkeiten.</description>
	    <atom:link rel="http://podlove.org/deep-link" href="http://podcast.funkenstrahlen.de/2013/02/27/fs002-arduino-blinkenlichter/#" />
	    <itunes:image href="http://podcast.funkenstrahlen.de/wp-content/cache/podlove/fe/cf0a7a7dfb680f8da110c73274b623/fs002-arduino-blinkenlichter_original.png" />
		<pse:webhook-registration>URL</pse:webhook-registration>
		<pse:streams media="audio" title="Standard Audio" lang="de">
			<pse:stream title="Deutsch MP3" mime-type="audio/mpeg" bitrate="128000" url="http://streams.xenim.de/metaebene.mp3" />
			<pse:stream title="Deutsch OGG" mime-type="audio/ogg" bitrate="128000" url="http://streams.xenim.de/metaebene.ogg" />
			<pse:stream title="Deutsch OPUS" mime-type="audio/opus" bitrate="128000" url="http://streams.xenim.de/metaebene.opus" />
			<pse:stream title="Deutsch ADTS" mime-type="audio/aac-adts" bitrate="128000" url="http://streams.xenim.de/metaebene.aac" />
			<pse:stream title="Deutsch AACP" mime-type="audio/aac-aacp" bitrate="128000" url="http://streams.xenim.de/metaebene.heaac" />
		</pse:streams>
		<pse:streams media="audio" title="Standard Audio with Live Translation" lang="en,gsw">
			<pse:stream title="English MP3" mime-type="audio/mpeg" bitrate="128000" url="http://streams.xenim.de/metaebene-translation.mp3" />
			<pse:stream title="English OGG" mime-type="audio/ogg" bitrate="128000" url="http://streams.xenim.de/metaebene-translation.ogg" />
			<pse:stream title="English OPUS" mime-type="audio/opus" bitrate="128000" url="http://streams.xenim.de/metaebene-translation.opus" />
			<pse:stream title="English ADTS" mime-type="audio/aac-adts" bitrate="128000" url="http://streams.xenim.de/metaebene-translation.aac" />
			<pse:stream title="English AACP" mime-type="audio/aac-aacp" bitrate="128000" url="http://streams.xenim.de/metaebene-translation.heaac" />
		</pse:streams>
		<pse:streams media="video" title="Standard Audio with Live Translation" lang="de">
			<pse:stream title="Deutsch MPEG" mime-type="application/x-mpegURL" bitrate="128000" url="http://freakshow.fm/stream/hls" />
		</pse:streams>
	</item>
</channel>
```

## Pubsubhubbub ##

If you parse podcast feeds for livestream information please consider supporting Pubsubhubbub. With Pubshubhubbub you can get a realtime notification if the feed content is updated. This is especially helpful for shows that are scheduled only a short time before going live. This only works for podcast feeds supporting Pubsubhubbub of course.

[^ATOM]: The Atom Syndication Format: <http://www.atomenabled.org/developers/syndication/atom-format-spec.php>

[^RSS2]: RSS 2.0 Specification: <http://cyber.law.harvard.edu/rss/rss.html>
