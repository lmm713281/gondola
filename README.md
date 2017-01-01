# Gondola

*Note that this project isn't in a usable state yet*

Sick of your kids' DVD's getting scratched and unusable?

Gondola is a media center that is designed to work from a cheap+silent single board computer (SBC) like
a [Chip](https://getchip.com/) or a [Raspberry Pi](https://www.raspberrypi.org/).

It accomplishes this feat by pre-processing your media into [HLS](https://developer.apple.com/streaming/),
then serving it using nginx. This can take a very long time eg overnight, so the recommended use case for this is to make backups of DVDs that you're likely to watch more than once. Eg your kids' movies, so you don't have to worry about the discs getting scratched.

## Features

* Cheap - you don't need to buy an expensive computer that's fast enough to transcode in real time.
* Not hot - my old media center in my garage gets quite hot, and I worry about it in summer! This one won't.
* Silent - my old media center spins its fans all day - this one won't, as most SBC's have no fan.
* Simple - therefore, hopefully more reliable than the other common alternatives.
* Seekable - because it pre-processes your media into HLS, which makes individual files for every few seconds, your media seeks perfectly (important for kids!).
* Just drop your eg VOB files into a 'New' folder using eg [ForkLift](http://www.binarynights.com/Forklift/), and it'll 
wait until transfer has complete to begin importing it automatically.

## Drawbacks

* Media must be pre-processed, which can take a long time if it's high quality. Eg I tried a 2-hour 1080p movie, and my Chip took 2 days to transcode it. This is why I recommend this for movies you'll watch over and over again, eg your kids' movies.

## How to install

* Create `~/.gondola` as a config file, with the contents: `root = "~/Gondola"` or wherever you wish your root storage to be.
* See `exclusive.go` for instructions for configuring this to get sudo access to lsof, for watching the folder for changes.

## Notes

* Gondola, after transcoding to HLS, removes the source file. The assumption is that the user ripped their original from their DVD so doesn't care to lose it. Plus this saves storage space.

## Config

Configuration goes into ~/.gondola

It uses TOML format (same as windows INI files). Options include:

`root = "~/Some/Folder/Where/I/Want/My/Data/To/Go"`

This allows you to disable the transcoding, which is useful to speed up dev.

`debugSkipHLS = true`

## File naming conventions

When you dump a movie into the 'New/Movies' folder, the following will work:

	* Big.Buck.Bunny.2008.1080p.blah.vob
	* Big Buck Bunny 2008 1080p blah.vob
	* Big.Buck.Bunny.vob

If it finds a year, it assumes the text to the left is the title. Text to the right is ignored, as it's usually resolution/codec/other stuff. Dots/periods are converted to spaces, which it then uses to search OMDB for the movie metadata.

If it cannot find a year, it still searches OMDB to find the movie, but it stands less of a chance finding the correct movie if there's no year.

For TV shows placed in `New/TV` folder, use the following:

	* Some.TV.Show.S01E02.DVD.vob
	* Some TV Show S01E02 Blah blah blah.vob

So long as it can find 'SxxEyy' (for season x episode y), it assumes the show's title is to the left, and ignores anything to the right. It then searches OMDB to find the show's metadata.

## Name

The name is a tortured metaphor: A real gondola transports you down a stream; this Gondola transports your media by streaming it ;)