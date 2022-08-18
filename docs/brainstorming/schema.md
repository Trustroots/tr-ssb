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
