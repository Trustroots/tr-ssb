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
