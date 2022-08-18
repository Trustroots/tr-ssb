# Locations

Trying to capture locations is tricky. With your home, one likely doesn't want to publish the precise location. Yet with other things, like furniture identified as being gifted on the street, the precise location is useful. Open location codes (called Plus Codes) are an interesting method of encoding areas. As most things being described have physical mass, they occupy areas in space and not points. But if one wants to specify "central Paris" as a location, it can be difficult to achieve with a single Plus Code.

One neat feature is that a Plus Code can be shortened to increase the size of the area it represents. So if I know the Plus Code of my home, I can remove some trailing digits (replace them with `0` padding) and now I have a location which includes my home, but is less accurate.

As Plus Codes can be encoded and decoded offline, without any online lookup, they can work well in the scuttleverse where some operations can be expected to be performed offline.

## Location scenarios

People may want to express different types of locations in different scenarios.

- I am inviting people to stay with me at my home, I want to share the approximate location so people know where I am based, but not so close they could drop by unannounced.
- I spotted an item being given away on the street, I want to share it's location as precisely as possible.
- I'm planning a bike trip and I want to share my general route plan so people could offer to meet or host me along the way.
- I'm driving from Berlin to Paris and I want to share my route so people could hitchhike with me.
- I live very rurally and I'm inviting people to stay with me at my home, I want to share the precise location including some directions of how to reach my place.
- I've discovered a few great dumpster diving spots and I want to share with friends including precise locations and a detailed instruction on how to reach the spots.

## One field or many

There are competing perspectives. From one view, all location related data should be in a single `location` field. From another view, each type of location should have its own field.

### One field to rule them all

A single large area:

```json
{
  ...
  "location": {
    "plusCode": "9F4MFC00+"
  }
}
```

A collection of areas:

```json
{
  ...
  "location": {
    "plusCodes": ["9F4MG700+", "9F4MG800+", "9F4MG900+", "9F4MGC00+", "9F4MGF00+", "9F4MF800+", "9F4MF900+", "9F4MFC00+", "9F4MFF00+"],
  }
}
```

A point:

```json
{
  ...
  "location": {
    "latitude": 12.1913,
    "longitude": 10.1992
  }
}
```

A route:

```json
{
  ...
  "location": {
    "line": [
      {
        "latitude": 37.778259000,
        "longitude": -122.391386000
      },
      {
        "latitude": 37.778194000,
        "longitude": -122.391226000
      },
      {
        "latitude": 37.778297000,
        "longitude": -122.391174000
      },
      {
        "latitude": 37.778378000,
        "longitude": -122.391117000
      },
      {
        "latitude": 37.778449000,
        "longitude": -122.391039000
      },
      {
        "latitude": 37.778525000,
        "longitude": -122.390942000
      },
    ]
  }
}
```

## Plus Codes

In theory, a route could be generally expressed by choosing a series of contiguous regions on the map. Those could be expressed with plus codes.

Instead of the typical GPX file approach of listing points and drawing a line between them, which is inherently very precise, a collection of areas could be used. This might provide a more "privacy friendly" approach to sharing a "general location" without having to express which road you will travel on which day at which time.

All plus codes resolve to an area on the map. If we use plus codes as the only standard initially, then all data which can be displayed on the map gets linked onto areas.

## Naming

Perhaps the only field required at first is `plusCodes`. It must always contain an array of `string`s if it exists. Those `string`s must be valid plus codes. It can contain only 1, or multiple.

## Links

- [Open Location Code](https://github.com/google/open-location-code/)
  - [Comparison of similar systems](https://github.com/google/open-location-code/wiki/Evaluation-of-Location-Encoding-Systems)
