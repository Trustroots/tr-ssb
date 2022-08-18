# Schemas

Brainstorming what schemas might look like.

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

## Metadata

Data like how many people can be accommodated could be stored in a "metadata" object. This object could be used to build a set of available filters. In principle, it would be possible to have arbitrary data in this object so long as it follows some simple rules. The client could use whatever data it has available to build a set of available filters.

It should be possible to define variable type by the field name. So `boolean` values are always prefixed with `is`. `integer` values are always suffixed with `Count`. Anything else should be a `string`. `string`s should never begin with `is` and never end with `Count`. All variables should be named in `camelCase`.

For `boolean` values, the default case should always be `false`.

Examples of possible `meta` objects might be:

```json
{
  "peopleCount": 3,
  "isBikeToolsAvailable": true,
  "isWheelchairAccessible": true
}
```

```json
{
  "isRefrigerated": true,
  "isGlutenFree": true,
  "isVegetarian": false,
  "isVegan": true
}
```

```json
{
  "color": "red"
}
```

```json
{
  "itemCount": 6
}
```

Is metadata the appropriate name for this type of content?

Maybe not. Perhaps it's better called `fields` or `data` or something. Maybe even specify that its purpose is to allow for filtering and call it `filter` or something. Not sure, but I guess it's not really "meta" data, it's actually data. How many people can be accommodated is no more "meta" than the location where the gift is available.

## First step

Having reviewed all the above, here's a more concrete example:

```json
{
  "type": "gift/offering",
  "version": "v1",
  "gift": "hospitality",
  "plusCodes": ["9F4MG700+"],
  "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis.",
  "images": [] // Array of SSB image links
}
```

Required fields would be `type`, `version`, `gift`, `description`.

## Required locations

Would it make sense to have location as a required field?

One can offer "digital" gifts. I wonder if there's political value in forcing things to be considered in physical space rather than virtual space.
