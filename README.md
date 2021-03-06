[![Build Status](https://travis-ci.org/donalmurtagh/grails-geocode.svg)](https://travis-ci.org/donalmurtagh/grails-geocode)

This Grails plugin provides a Spring bean named `geocodingService` which uses the
[Google Geocoding API](https://developers.google.com/maps/documentation/geocoding) to perform geocoding and reverse geocoding.

## Geocoding
The service provides two methods for geocoding, i.e. converting an address to latitude/longitude coordinates.
One of these methods returns a single result and the other returns a list of results.

````groovy
// get a single location matching the provided address
Point location = geocodingService.getPoint('Bray, Co. Wicklow, Ireland')
Float latitude = location.latitude
Float longitude = location.longitude

// get a list of locations matching the address 'Birmingham'
List<Point> location = geocodingService.getPoints('Birmingham')
````

### Optional arguments

A second optional Map argument may be provided to either of the above methods. This Map supports the following entries:

* `sensor` a boolean that indicates whether or not the request comes from a device with a location sensor. If omitted, false is assumed.
* `max` an integer that limits the number of results returned. This value is ignored when `getPoint` is called (because this method always returns at most 1 result)
* parameters that are defined as optional in the [API docs](https://developers.google.com/maps/documentation/geocoding/#geocoding)

````groovy
// geocode the address 'Newcastle' with the results biased to Ireland.
// A maximum of 2 locations should be returned
List<Point> results = geocodingService.getPoint('Newcastle', [max: 2, region: 'ie'])
````

## Reverse Geocoding
The service provides two methods for reverse geocoding, i.e. converting a latitude/longitude coordinate to an address.
One of these methods returns a single result and the other returns a list of results.

````groovy
// convert a coordinate to an address
Point coordinate = new Point(latitude: 40.714224, longitude: -73.961452)
Address address = geocodingService.getAddress(coordinate)
println address.formattedAddress

// convert a coordinate to a list of addresses
List<Address> addresses = geocodingService.getAddresses(coordinate)
````

### Optional arguments

A second optional Map argument may be provided to either of the above methods. This Map supports the following entries:

* `sensor` a boolean that indicates whether or not the request comes from a device with a location sensor. If omitted, false is assumed.
* `max` an integer that limits the number of results returned. This value is ignored when `getAddress` is called (because this method always returns at most 1 result)
* parameters that are defined as optional in the [API docs](https://developers.google.com/maps/documentation/geocoding/#ReverseGeocoding)

````groovy
// convert a coordinate to a list of addresses in the French language.
// A maximum of 2 results should be returned
Point coordinate = new Point(latitude: 40.714224, longitude: -73.961452)
List<Point> addresses = service.getAddresses(coordinate, [language: 'fr', max: 2])
````

## Configuration

### HTTP/HTTPS
By default requests are submitted to the Google Geocoding API over HTTPS. To use HTTP instead, add the following to `Config.groovy`

````groovy
geocode.useHttp = true
````

## Release History

* 0.3 - Uses HTTPS by default
* 0.2 - Added support for HTTPS
* 0.1 - Initial release
