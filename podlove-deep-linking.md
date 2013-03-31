Title: Podlove Deep Linking  
Author: Tim Pritlove  
Version: 1.1  
Tags: podlove, podcast, chapters, feeds, atom, rss  
Status: Current  

# Podlove Deep Linking #

Podlove Deep Linking is a method for describing direct access to audio or video content in Podcast Feeds. It is a very simple extension to a feed that tells a podcast client how to link back to a certain point in time on the podcast's web site.

The same addressing scheme can also be used by web-based media players to construct links to specific points or ranges in the media, either by generating URLs for users to pass on or to be updated in the URL field of the browser.

## Change Log

    2012-05-07: 1.1 - Significantly changed spec to be a
                      subset of Media Fragments URI
    2012-05-07: 1.0 - Initial Release

## Motivation and Scope ##

There is usually a huge disconnect between podcast clients playing back content and the content creators presence on the web. Listeners may want to refer to certain parts of a show to either "bookmark" something or share it with other people on the web. This is be done by giving away a simple link that people can use to directly access that particular content.

In order to allow websites to directly react to those links, the website must communicate which URL to use to link to content to the podcast client. It does this by providing a simple base URL that the podcast client can extend to include the bookmarked point in time. The website can then extract the time information from that URL and pass it along to a web player that can start playback at the exact time given.

## Media Fragments URIs ##

This specification relates to the Media Fragments URI 1.0  proposed recommendation of the W3C [^FRAGMENTS] but clients must only use the temporal fragment dimension and the normal play time (npt) syntax to reference points in time or time ranges. Web Players conforming to this specification do not need to support additional fragment dimensions or time notations.

## Including Player Information in a Podcast Feed ##

In order to provide playback URL information in your podcast feed, you have to put in an extra element `<atom:link>` in each of your episode entries.

This element's ref attribute needs to be set to the URI value `http://podlove.org/deep-link` to denote the proper and Atom-compliant meaning to the podcast client.

The `href` attribute then specifies the base URL to be appended with time range information. Please replace the example given here with whatever your podcast publication platform requires.

Please understand that this link is just a base URL that needs to be extended with proper time range information by the client. The link alone does merely communicate to the client that the web page in question is actually able to accept media fragment URIs.

### Example ###

This is how that element would show up in an Atom podcast feed:

	<?xml version="1.0" encoding="utf-8"?>
	<feed xmlns="http://www.w3.org/2005/Atom">
	
		...
	
		<entry>
			<link href="http://mypodcast.com/episode/index.html" />
			<link rel="http://podlove.org/deep-link" href="http://mypodcast.com/episode/player.php?episode=23#" />
		
			...
		
		</entry>
	
		...
		
	</feed>
This is how it would look like in an RSS 2.0 podcast feed:

	<?xml version="1.0" encoding="UTF-8"?>
	<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
		
		...
		
		<item>
			<link>http://mypodcast.com/episode/index.html</link>
			<atom:link rel="http://podlove.org/deep-link" href="http://mypodcast.com/episode/player.php?episode=23#" />
			
			...
			
		</item>
		
		...
		
	</rss>


## Appending Time Range Information ##

The podcast client appends the time range selected by the user. The podcast specifies the selected range in the "normal play time" syntax of the Media Fragment specification.

	t=[npt:][start][,end]

If you omit the *start* time, it defaults to the beginning of the media file, if you omit *end* it defaults to the end of the media file.


Here are some examples that show how the time field can be used:

>**`t=1:35:52,1:42:16`**
:play from 01:35:52 to 01:42:16

>**`t=263`**
:start playback at 4 minutes and 23 seconds

>**`t=,24:12`**
:play from beginning to 24 minutes and 12 seconds

For more examples and a full specification of the Normal Play Time syntax, consult the Media Fragments [^FRAGMENTS] specification.

## Additional Fragment Parameters ##

The Media Fragments [^FRAGMENTS]specification requires the media fragment specifier to be a parameter following a hash sign ("#") and allows for additional parameters to be passed along. As the podcast client is expected to add just the time range information, any additional parameters that your web player might need must be included in the URL stated in the feed following the fragment identifier.

The Podcast Client **MUST** honor existing parameters and append the time specification with an ampersand sign ("&") if a parameter already exists. The following URL

	http://mypodcast.com/episode/player.php?episode=23#style=external

must be extended as described to look something like this:

	http://mypodcast.com/episode/player.php?episode=23#style=external&t=10,200

Right now, this specification does not define any other parameters but might in do so in a future version.

## Website Player Behavior ##

Once the deep link information has been placed in the podcast feed, the website referenced *SHOULD* be ready to play back the media content directly. The website player *SHOULD* start playing at the given start time and *SHOULD* pause playback after the optionally given end time is reached for the first time.

It is up to the player to decide if playback should be immediate after loading the URL or if the user still needs to explicitly start playback using UI controls.

[^FRAGMENTS]: Media Fragments URI 1.0 <http://www.w3.org/TR/2012/PR-media-frags-20120315/>