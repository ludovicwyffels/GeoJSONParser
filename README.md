# GeoJSONParser

You can specify a callback function as an option third parameter.

```javascript
GeoJSONParser.parse(data, {Point: ['lat', 'lng']}, function(geojson){
  console.log(JSON.stringify(geojson));
});
```

## Parameters

#### include/exclude

Depending on which makes more sense for the data that is being parsed, either specify an array of attributes to include or exclude in `properties` for each feature. If neither `include` nor `exclude` is set, all the attributes (besides the attributes containing the geometry data) will be added to feature `properties`.

- `include` - Array of attributes to include in `properties` for each feature. All other fields will be ignored.
- `exclude` - Array of attributes that shouldn't be included in feature `properties`. All other attributes will be added (besides geometry attributes)

#### Geometry

The geometry parameters specify which attribute(s) contain(s) the geographic/geometric data. A geometry parameter must be specified for each type of geometry object that is present in the data that is being parsed. For example, if the data contains both points and polygons, specify both the `Point` and `Polygon` parameters. **Note that geometry parameters must be in proper case.** See the [GeoJSON spec](http://geojson.org/geojson-spec.html) for details on each geometry type. The structure of the geometry parameter is:

    ParameterName: 'attributeName'

Except for `Point`, which can be specified with a field name or an array of field names, i.e:

    data = [{ name: 'location', x: 34, y: 85 }];

    GeoJSON.parse(data, {Point: ['lat', 'lng']});

or

    data = [{ name: 'location', coords: [85, 34] }];

    GeoJSON.parse(data, {Point: 'coords'});

The valid geometry types are

- `Point`
- `MultiPoint`
- `LineString`
- `MultiLineString`
- `Polygon`
- `MultiPolygon`

To parse already encoded GeoJSON use

`GeoJSON`

    var data = [{name: 'Location A', geo: {"type": "Point", "coordinates": [125.6, 10.1]}}];

    GeoJSON.parse(data, {GeoJSON: 'geo'});

	"type": "FeatureCollection",
	"features": [{
		"type": "Feature",
		"geometry": {
			"type": "Point",
			"coordinates": [125.6, 10.1]
		},
		"properties": {
			"name": "Location A"
		}
	}]
}

#### bbox, crs

geojson.js also supports the optional GeoJSON properties `bbox` and `crs`.

- `crs` - An object identifying a coordinate reference system. [More information](http://geojson.org/geojson-spec.html#coordinate-reference-system-objects)
- `bbox` - A bounding box for the feature collection. An array with the following format: `[y1, x1, y2, x2]`. [More information](http://geojson.org/geojson-spec.html#bounding-boxes)

#### extra

You can add arbitrary properties to features using the `extra` param. The value for `extra` must be an object. For example, using the original sample data:

    GeoJSON.parse(data, {
      Point: ['lat', 'lng'],
      extra: {
        style: {
          "color": "#ff7800",
          "weight": 5,
          "opacity": 0.65
        }
      }
    });

    {
      "type": "FeatureCollection",
      "features": [
        { "type": "Feature",
          "geometry": {"type": "Point", "coordinates": [-75.343, 39.984]},
          "properties": {
            "name": "Location A",
            "category": "Store",
            "style": {
              "color": "#ff7800",
              "weight": 5,
              "opacity": 0.65
            }
          }
        },
      ...
    }    

#### extraGlobal

You can also add dataset properties using the `extraGlobal` param. The value for `extraGlobal` must be an object.

    GeoJSON.parse(data, {
      Point: ['lat', 'lng'],
      extraGlobal: {
        'Creator': 'Mr. Example',
        'records': data.length,
        'summary': 'A few example points'
      }
    });

      {
        "type": "FeatureCollection",
        "features": [
          { "type": "Feature",
            "geometry": {"type": "Point", "coordinates": [-75.343, 39.984]},
            "properties": {
              "name": "Location A"
            }
          },
          ...
          { "type": "Feature",
            "geometry": {"type": "Point", "coordinates": [ -75.534, 39.123]},
            "properties": {
              "name": "Location C"
            }
          }
        ],
        "properties": {
          "Creator": "Mr. Example",
          "records": 2,
          "summary": "A few example points"
        }
      }

## Tests

For node, `$ npm test`.

## License

Licensed under the MIT License. See `LICENSE` for details.