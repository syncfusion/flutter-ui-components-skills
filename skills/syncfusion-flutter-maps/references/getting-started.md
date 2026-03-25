# Getting Started with Syncfusion Flutter Maps

This guide covers installation, setup, and basic implementation of both MapShapeLayer and MapTileLayer in your Flutter application.

## Installation

### Step 1: Add Dependency

Add the Syncfusion Flutter Maps package to your `pubspec.yaml` file:

```bash
# Always installs the latest compatible version automatically
flutter pub add syncfusion_flutter_maps
```
> Always install the package via terminal — do **not** edit `pubspec.yaml` directly.
> Run this command from the Flutter project root and wait for it to complete successfully before proceeding.

### Step 2: Import Package

Import the Maps package in your Dart file:

```dart
import 'package:syncfusion_flutter_maps/maps.dart';
```

## Basic MapShapeLayer Implementation

MapShapeLayer renders GeoJSON data as geographical shapes.

### Prepare GeoJSON File

1. Download or create a GeoJSON file (e.g., `world_map.json`, `usa_states.json`)
2. Place it in your `assets` folder
3. Declare it in `pubspec.yaml`:

```yaml
flutter:
  assets:
    - assets/world_map.json
```

### Simple Shape Layer

```dart
import 'package:flutter/material.dart';
import 'package:syncfusion_flutter_maps/maps.dart';

class BasicShapeMap extends StatefulWidget {
  @override
  _BasicShapeMapState createState() => _BasicShapeMapState();
}

class _BasicShapeMapState extends State<BasicShapeMap> {
  late MapShapeSource _dataSource;
  
  @override
  void initState() {
    super.initState();
    _dataSource = MapShapeSource.asset(
      'assets/world_map.json',
      shapeDataField: 'name',
    );
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('World Map')),
      body: Padding(
        padding: EdgeInsets.all(15),
        child: SfMaps(
          layers: [
            MapShapeLayer(
              source: _dataSource,
            ),
          ],
        ),
      ),
    );
  }
}
```

### Shape Layer with Styling

```dart
MapShapeLayer(
  source: _dataSource,
  color: Colors.blue[100],
  strokeColor: Colors.blue[700],
  strokeWidth: 0.5,
  showDataLabels: true,
)
```

## Loading GeoJSON from Different Sources

### From Asset Bundle

Most common approach for offline maps:

```dart
_dataSource = MapShapeSource.asset(
  'assets/australia.json',
  shapeDataField: 'STATE_NAME',
);
```

**Requirements:**
- Add file to `assets/` folder
- Declare in `pubspec.yaml`

### From Network

Load GeoJSON from a URL:

```dart
_dataSource = MapShapeSource.network(
  'https://example.com/api/map-data.json',
  shapeDataField: 'name',
);
```

**Note:** Requires internet connectivity. Handle loading states appropriately.

### From Memory (Uint8List)

Load GeoJSON as bytes:

```dart
@override
Widget build(BuildContext context) {
  return FutureBuilder(
    future: _fetchJsonData(),
    builder: (BuildContext context, snapshot) {
      if (snapshot.hasData) {
        Uint8List bytesData = snapshot.data as Uint8List;
        return SfMaps(
          layers: [
            MapShapeLayer(
              source: MapShapeSource.memory(
                bytesData,
                shapeDataField: 'STATE_NAME',
              ),
            ),
          ],
        );
      } else {
        return CircularProgressIndicator();
      }
    },
  );
}

Future<Uint8List> _fetchJsonData() async {
  return (await rootBundle.load('assets/map.json'))
      .buffer
      .asUint8List();
}
```

## Basic MapTileLayer Implementation

MapTileLayer renders tiles from web map services.

### OpenStreetMap (Free)

```dart
import 'package:flutter/material.dart';
import 'package:syncfusion_flutter_maps/maps.dart';

class BasicTileMap extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('OpenStreetMap')),
      body: SfMaps(
        layers: [
          MapTileLayer(
            urlTemplate: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
          ),
        ],
      ),
    );
  }
}
```

### Tile Layer with Custom Location

```dart
MapTileLayer(
  urlTemplate: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
  initialFocalLatLng: MapLatLng(37.7749, -122.4194), // San Francisco
  initialZoomLevel: 10,
)
```

### URL Template Format

The `urlTemplate` uses WMTS (Web Map Tile Service) format:
- `{z}` - Zoom level
- `{x}` - Tile X coordinate
- `{y}` - Tile Y coordinate

**Common formats:**
- `https://provider.com/{z}/{x}/{y}.png`
- `https://provider.com/z={z}/x={x}/y={y}.png`
- `https://provider.com/{z}/{x}/{y}.png?key=API_KEY`

## Understanding shapeDataField

The `shapeDataField` is a crucial property that maps your data to GeoJSON shapes.

### How It Works

1. **GeoJSON Structure:**
```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "name": "California",
        "STATE_CODE": "CA"
      },
      "geometry": { ... }
    }
  ]
}
```

2. **Set shapeDataField:**
```dart
MapShapeSource.asset(
  'assets/usa_states.json',
  shapeDataField: 'name', // Matches 'name' property in GeoJSON
);
```

3. **Match with Your Data:**
```dart
List<StateData> data = [
  StateData('California', 100),
  StateData('Texas', 85),
];

MapShapeSource.asset(
  'assets/usa_states.json',
  shapeDataField: 'name',
  dataCount: data.length,
  primaryValueMapper: (index) => data[index].stateName,
  // 'California' from data matches 'California' in GeoJSON
);
```

## Complete Example: Map with Data

```dart
import 'package:flutter/material.dart';
import 'package:syncfusion_flutter_maps/maps.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MapExample(),
    );
  }
}

class MapExample extends StatefulWidget {
  @override
  _MapExampleState createState() => _MapExampleState();
}

class _MapExampleState extends State<MapExample> {
  late List<CountryData> _data;
  late MapShapeSource _dataSource;
  
  @override
  void initState() {
    super.initState();
    
    _data = [
      CountryData('India', 280, Colors.orange),
      CountryData('United States of America', 190, Colors.blue),
      CountryData('Brazil', 120, Colors.green),
    ];
    
    _dataSource = MapShapeSource.asset(
      'assets/world_map.json',
      shapeDataField: 'name',
      dataCount: _data.length,
      primaryValueMapper: (int index) => _data[index].country,
      shapeColorValueMapper: (int index) => _data[index].color,
    );
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Maps Example'),
      ),
      body: Center(
        child: Container(
          height: 400,
          padding: EdgeInsets.all(15),
          child: SfMaps(
            layers: [
              MapShapeLayer(
                source: _dataSource,
                strokeColor: Colors.white,
                strokeWidth: 2,
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class CountryData {
  CountryData(this.country, this.value, this.color);
  
  final String country;
  final double value;
  final Color color;
}
```

## Next Steps

Now that you have basic maps working:

1. **Add Markers** - See `markers.md` for adding location pins
2. **Customize Colors** - See `color-mapping.md` for choropleth maps
3. **Add Legends** - See `legend.md` for legend configuration
4. **Enable Zoom/Pan** - See `zoom-pan.md` for interactive features
5. **Add Tooltips** - See `tooltip.md` for hover information

## Common Issues

### Issue: Map Not Displaying

**Cause:** GeoJSON file not loaded or path incorrect

**Solution:**
1. Verify file is in `assets/` folder
2. Check `pubspec.yaml` has correct asset path
3. Run `flutter pub get` after updating pubspec
4. Use correct file name (case-sensitive)

### Issue: Tiles Not Loading (MapTileLayer)

**Cause:** No internet connection or incorrect URL

**Solution:**
1. Check internet connectivity
2. Verify URL template format
3. Test URL in browser (replace {z}/{x}/{y} with actual values)
4. Check for API key requirements

### Issue: Shapes Not Matching Data

**Cause:** `shapeDataField` doesn't match GeoJSON property

**Solution:**
1. Open GeoJSON file and check property names
2. Set `shapeDataField` to exact property name
3. Ensure `primaryValueMapper` returns matching values
