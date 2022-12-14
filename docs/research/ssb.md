# SSB

Research into SSB recorded here for posterity.

## Links

- [SSB Protocol Guide](https://ssbc.github.io/scuttlebutt-protocol-guide/)
  - Good place to get started covering the basics of how SSB works
- [Patchfox docs on message types](https://patchfox.org/#/message_types/)
  - Description of a range of message types
- [SSB message types](http://scuttlebot.io/docs/message-types/post.html)
  - Docs from scuttlebot.io on message formats

## Types

It seems like we'll need to create a new message type or types.

The `bookclub` type seems to have the following fields:

- `title` (`string`) - Presumably the book title
- `authors` (`string`) - Presumably the authors of the book
- `description` (`string`) - Seems to be a description of the book (and not a book club)
- `image` - Uses an SSB blob format, presumably a standard (needs more investigation)

There are other fields supported in the books v2 schema which can be seen here: https://github.com/ssbc/ssb-book-schema/blob/master/v2/schema/book.js

Some things to note:

- All fields appear to be top level keys (no nested values like `series.number`)
- Images are a nested object, which contains some kind of SSB link

### Images

According to the (scuttlebot docs](http://scuttlebot.io/docs/message-types/about.html) the `image` nested object contains teh following keys:

- `link` (`string`) - An SSB blob ID
- `width` (`number`) - The image width in pixels
- `height` (`number`) - The image height in pixels
- `name` (`string`) - A filename for the image
- `size` (`number`) - File size in bytes
- `type` (`string`) - Mimetype of the image

## Gift Schema

If we generalise hospitality as the offer (or wish) of a gift, then perhaps a schema could look as follows:

- `giftType` - What type of gift is this (hospitality would be our first use case)
- `title` (`string`) - Some title for the gift
- `description` (`string`) - A Description of the gift

Other information to include (schema to be defined):

- `status` - This is `yes`, `maybe`, or `no` in trustroots
  - Presumably we could drop this, it seems a bit wonky to have "maybe" offers, all offers should be considered as "maybe" offers
- `plusCode` - An open location code (plus code) specifying where the gift is located
- `startDateTime` - A date and time when the gift becomes available
- `expiryDateTime` - A date and time when the gift expires (is no longer available)
- `numberOfPeople` - How many people can be accommodated
  - This makes the most sense around hospitality, if a person has space to host just 1 visitor that's very different than being able to host a group of 5 people.

Other choices the user can make currently which wouldn't map onto the message schema.

- Only show to circles
  - This would control how widely the gift is replicated, which likely can't be set on a message basis but might be possible to signal on a feed basis in SSB.

### Questions

- How to encode dates and times
- How to encode locations (see `../brainstorming/locations.md`)

#### Times

There seem to be multiple time encoding methods used in SSB schemas. From a unix epoch timestamp (as a signed 64 bit integer to support values in the past) to an unusual object including a timestamp, timezone and other data to a JavaScript style date (eg `2018-03-15T03:40:06.222Z`).

There are likely many opinions here. It's doubtful any specific strategy has any massive advantage over the others.

The SSB log format itself only includes a timestamp, without any timezone information.

Question: is it **ever** useful to store the "local" timezone along with a date?

If not, then using the same timestamp format as in the feed itself makes the most sense. Any application which is working with feeds is likely capable of parsing these timestamps already.
