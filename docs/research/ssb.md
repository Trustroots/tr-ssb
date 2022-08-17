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

It seems like times and locations don't have a particularly clear standard way of being stored in the scuttleverse. It might be helpful to specify that. There are a couple of date formats in use.

### Brainstorming

Some examples of different schemas.

```json
{
  "type": "gift/offering",
  "version": "v1",
  "gift": "hospitality",
  "plusCode": "9F4MFC00+",
  "title": "Space for up to 3 people",
  "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis.",
  "people": 3
}
```

A wish rather than an offer:

```json
{
  "type": "gift/seeking",
  "version": "v1",
  "gift": "hospitality",
  "plusCode": "9F4M0000+",
  "title": "Hitchhiking to Berlin with 2 friends",
  "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis.",
  "people": 3,
  "startsAt": "2022-09-15T00:00:00.000Z",
  "endsAt": "2022-09-18T00:00:00.000Z",
}
```

Using `plusCodes` plural instead of `plusCode` singular:

```json
{
  "type": "gift/seeking",
  "version": "v1",
  "gift": "hospitality",
  "plusCodes": ["9F4MG700+", "9F4MG800+", "9F4MG900+", "9F4MGC00+", "9F4MGF00+", "9F4MF800+", "9F4MF900+", "9F4MFC00+", "9F4MFF00+"],
  "title": "Hitchhiking to Berlin with 2 friends",
  "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis.",
  "people": 3
}
```

It could also be an option to specify location as a sub schema like so:

```json
{
  "type": "gift/offering",
  "version": "v1",
  "gift": "hospitality",
  "location": {
    "plusCode": "9F4MFC00+"
  },
  "title": "Space for up to 3 people",
  "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis.",
  "people": 3
}
```

Which would allow for alternate location formats like:

```json
{
  "type": "gift/offering",
  "version": "v1",
  "gift": "hospitality",
  "location": {
    "latitude": 12.1913,
    "longitude": 10.1992,
    "radius": 200
  },
  "title": "Space for up to 3 people",
  "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis.",
  "people": 3
}
```

I think it's important to remember that there's no way to enforce schemas. So users may publish messages of any format in the network, and software needs to either accept them or skip them. Therefore, with fields like `location`, maybe it's important to put all the location options into a single object. Perhaps it's easier to simply accept different location types, without any means of ensuring that only 1 location is present for any given message.

### Locations

Trying to capture locations is tricky. With your home, one likely doesn't want to publish the precise location. Yet with other things, like furniture identified as being gifted on the street, the precise location is useful. Open location codes (called Plus Codes) are an interesting method of encoding areas. As most things being described have physical mass, they occupy areas in space and not points. But if one wants to specify "central Paris" as a location, it can be difficult to achieve with a single Plus Code.

One neat feature is that a Plus Code can be shortened to increase the size of the area it represents. So if I know the Plus Code of my home, I can remove some trailing digits (replace them with `0` padding) and now I have a location which includes my home, but is less accurate.

As Plus Codes can be encoded and decoded offline, without any online lookup, they can work well in the scuttleverse where some operations can be expected to be performed offline.
