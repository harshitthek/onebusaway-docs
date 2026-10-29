---
layout: rest_api
title: arrivals-and-departures-for-location Method
description: Get current arrivals and departures near a location
sample_request_url: https://api.pugetsound.onebusaway.org/api/where/arrivals-and-departures-for-location.json?key=TEST&lat=47.653&lon=-122.307&radius=100
example_response_file: arrivals-and-departures-for-location.json
---

Get current arrivals and departures for stops identified by a specific location (using a bounding box or a radius).

## Request Parameters

* `lat` - The latitude coordinate of the search center (required)
* `lon` - The longitude coordinate of the search center (required)
* `radius` - The search radius in meters. Maximum is 20,000 meters (20 km). Either `radius` or both `latSpan` and `lonSpan` is required; if none is provided, the search area has zero size and returns no stops.
* `latSpan`/`lonSpan` - Sets the search bounding box as an alternative to `radius`. Both must be provided together.
* `time` - By default, the method returns the status of the system right now. However, the system can also be queried at a specific time. See [timestamps](/api/where/#timestamps) for details on the format of the `time` parameter.
* `minutesBefore=n` - Include vehicles having arrived or departed in the previous n minutes (default=5)
* `minutesAfter=n` - Include vehicles arriving or departing in the next n minutes (default=35)
* `frequencyMinutesBefore=n` - Include frequency-based vehicles having arrived or departed in the previous n minutes
* `frequencyMinutesAfter=n` - Include frequency-based vehicles arriving or departing in the next n minutes
* `maxCount=n` - The maximum number of stops to search and return in `nearbyStopIds` (default=250, maximum=1000). Note that this limits the stop search and the nearby-stop list, not the count of arrival records returned.
* `routeType` - A comma-delimited list of GTFS route types to filter by (e.g. `0` for tram/light rail, `3` for bus)
* `emptyReturnsNotFound` - When set to `true`, indicates that an empty result should return a `404 Not Found` response instead of `200 OK`. Note: On the production server, this flag currently has no effect and `200 OK` is always returned. The flag also applies only when no stops are found in the search area, not when stops are found with no arrivals.

## Response

The response is primarily composed of [`<arrivalAndDeparture/>` elements](/api/where/elements/arrival-and-departure), so see the element documentation for specific details.

The `nearbyStopIds` list is designed to capture stops that are very close by (like across the street) for quick navigation. This is sorted by distance and limited by `maxCount`.

Trips will not show up in the results if their schedule relationship is SKIPPED or if there is no active service for the given time window.
