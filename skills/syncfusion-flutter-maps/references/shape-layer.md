# MapShapeLayer Overview

The MapShapeLayer renders GeoJSON data as geographical shapes with extensive customization and data visualization capabilities. It's ideal for choropleth maps, statistical visualizations, and offline mapping.

## Table of Contents
- [Overview](#overview)
- [Basic Configuration](#basic-configuration)
- [Loading GeoJSON Data](#loading-geojson-data)
- [Shape Appearance](#shape-appearance)
- [Loading Progress Indicator](#loading-progress-indicator)
- [Shape Data Field Mapping](#shape-data-field-mapping)
- [Hover Effects (Web)](#hover-effects-web)
- [When to Use Shape Layer](#when-to-use-shape-layer)

## Overview

MapShapeLayer provides:
- **GeoJSON rendering** for any geographical data
- **Data binding** to display statistical information
- **Color mapping** for choropleth visualizations
- **Bubbles** for proportional symbol maps
- **Selection** for interactive exploration
- **Legends** for data interpretation
- **Markers** for point data
- **Sublayers** for hierarchical visualization
- **Offline support** (no internet required)

## Basic Configuration

### Minimal Setup

```dart
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
  return SfMaps(
    layers: [
      MapShapeLayer(
        source: _dataSource,
      ),
    ],
  );
}
```

### With Basic Styling

```dart
MapShapeLayer(
  source: _dataSource,
  color: Colors.blue[100],
  strokeColor: Colors.blue[700],
  strokeWidth: 0.5,
)
```

## Loading GeoJSON Data

### MapShapeSource Constructors

#### 1. Asset (Most Common)

Load GeoJSON from asset bundle:

```dart
_dataSource = MapShapeSource.asset(
  'assets/maps/australia.json',
  shapeDataField: 'STATE_NAME',
);
```

**Requirements:**
```yaml
# pubspec.yaml
flutter:
  assets:
    - assets/maps/australia.json
```

#### 2. Network

Load GeoJSON from URL:

```dart
_dataSource = MapShapeSource.network(
  'https://api.example.com/geojson/world',
  shapeDataField: 'name',
);
```

**Best Practices:**
- Cache data locally for offline access
- Show loading indicator
- Handle network errors

```dart
FutureBuilder(
  future: _loadMapData(),
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return CircularProgressIndicator();
    }
    if (snapshot.hasError) {
      return Text('Error loading map');
    }
    return SfMaps(layers: [MapShapeLayer(source: _dataSource)]);
  },
)
```

#### 3. Memory (Uint8List)

Load GeoJSON as bytes:

```dart
Future<Uint8List> _loadGeoJsonBytes() async {
  return (await rootBundle.load('assets/map.json'))
      .buffer
      .asUint8List();
}

@override
Widget build(BuildContext context) {
  return FutureBuilder<Uint8List>(
    future: _loadGeoJsonBytes(),
    builder: (context, snapshot) {
      if (snapshot.hasData) {
        return SfMaps(
          layers: [
            MapShapeLayer(
              source: MapShapeSource.memory(
                snapshot.data!,
                shapeDataField: 'name',
              ),
            ),
          ],
        );
      }
      return CircularProgressIndicator();
    },
  );
}
```

**Use cases:**
- Loading compressed GeoJSON
- Fetching from secure storage
- Processing GeoJSON before display

## Shape Appearance

### Basic Colors

```dart
MapShapeLayer(
  source: _dataSource,
  color: Colors.lightBlue[50],        // Fill color
  strokeColor: Colors.blue[700],       // Border color
  strokeWidth: 1.0,                    // Border width
)
```

### Using SfMapsTheme

More comprehensive theming approach:

```dart
import 'package:syncfusion_flutter_core/theme.dart';

SfMapsTheme(
  data: SfMapsThemeData(
    layerColor: Colors.blue[100],
    layerStrokeColor: Colors.blue[700],
    layerStrokeWidth: 1.5,
  ),
  child: SfMaps(
    layers: [
      MapShapeLayer(source: _dataSource),
    ],
  ),
)
```

**SfMapsThemeData Properties:**
- `layerColor` - Shape fill color
- `layerStrokeColor` - Shape border color
- `layerStrokeWidth` - Border width
- `shapeHoverColor` - Hover fill color (web)
- `shapeHoverStrokeColor` - Hover border color (web)
- `shapeHoverStrokeWidth` - Hover border width (web)

### Data-Driven Colors

Apply colors based on data values:

```dart
late List<CountryData> _data;
late MapShapeSource _dataSource;

@override
void initState() {
  super.initState();
  
  _data = [
    CountryData('India', Colors.orange),
    CountryData('USA', Colors.blue),
    CountryData('Brazil', Colors.green),
  ];
  
  _dataSource = MapShapeSource.asset(
    'assets/world_map.json',
    shapeDataField: 'name',
    dataCount: _data.length,
    primaryValueMapper: (index) => _data[index].country,
    shapeColorValueMapper: (index) => _data[index].color,
  );
}

MapShapeLayer(
  source: _dataSource,
  strokeColor: Colors.white,
  strokeWidth: 2,
)
```

## Loading Progress Indicator

Show a loading widget while GeoJSON loads:

```dart
MapShapeLayer(
  source: _dataSource,
  loadingBuilder: (BuildContext context) {
    return Container(
      height: 25,
      width: 25,
      child: CircularProgressIndicator(
        strokeWidth: 3,
      ),
    );
  },
)
```

### Custom Loading Widget

```dart
loadingBuilder: (context) {
  return Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      CircularProgressIndicator(),
      SizedBox(height: 10),
      Text('Loading map data...'),
    ],
  );
}
```

## Shape Data Field Mapping

The `shapeDataField` connects GeoJSON properties to your data.

### Understanding the Connection

**GeoJSON Structure:**
```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "name": "California",
        "STATE_CODE": "CA",
        "population": 39538223
      },
      "geometry": { ... }
    }
  ]
}
```

**Mapping Configuration:**
```dart
MapShapeSource.asset(
  'assets/usa.json',
  shapeDataField: 'name',  // Must match GeoJSON property
  dataCount: stateData.length,
  primaryValueMapper: (index) => stateData[index].stateName,
  // Links your data's 'stateName' with GeoJSON's 'name'
);
```

### Complete Data Binding Example

```dart
class StateData {
  StateData(this.name, this.population, this.gdp);
  final String name;
  final double population;
  final double gdp;
}

late List<StateData> _data;
late MapShapeSource _dataSource;

@override
void initState() {
  super.initState();
  
  _data = [
    StateData('California', 39.5, 3.4),
    StateData('Texas', 29.1, 2.0),
    StateData('Florida', 21.5, 1.2),
  ];
  
  _dataSource = MapShapeSource.asset(
    'assets/usa_states.json',
    shapeDataField: 'name',                    // GeoJSON property
    dataCount: _data.length,
    primaryValueMapper: (index) => _data[index].name,  // Your data
    dataLabelMapper: (index) => _data[index].name,     // Label text
    shapeColorValueMapper: (index) => _data[index].gdp, // Color data
  );
}
```

### Multiple Property Options

If your GeoJSON has multiple identifier fields:

```json
{
  "properties": {
    "name": "California",
    "STATE_CODE": "CA",
    "FIPS": "06"
  }
}
```

You can use any of them:
```dart
// Option 1
shapeDataField: 'name',
primaryValueMapper: (index) => data[index].name,

// Option 2
shapeDataField: 'STATE_CODE',
primaryValueMapper: (index) => data[index].code,

// Option 3
shapeDataField: 'FIPS',
primaryValueMapper: (index) => data[index].fipsCode,
```

## Hover Effects (Web)

For web platforms, customize hover appearance:

```dart
SfMapsTheme(
  data: SfMapsThemeData(
    layerColor: Colors.grey[200],
    shapeHoverColor: Colors.blue[300],
    shapeHoverStrokeColor: Colors.blue[900],
    shapeHoverStrokeWidth: 3,
  ),
  child: SfMaps(
    layers: [
      MapShapeLayer(source: _dataSource),
    ],
  ),
)
```

**Hover Properties:**
- `shapeHoverColor` - Fill color on hover
- `shapeHoverStrokeColor` - Border color on hover
- `shapeHoverStrokeWidth` - Border width on hover

**Note:** Hover effects only work on web platform (mouse pointer).

## When to Use Shape Layer

### Best Use Cases

✅ **Use MapShapeLayer for:**
- **Choropleth maps** (election results, census data, disease spread)
- **Statistical maps** with data bound to regions
- **Offline maps** that don't need internet
- **Custom geographical areas** (sales territories, districts)
- **Proportional symbol maps** with bubbles
- **Data exploration** with selection and filtering
- **Hierarchical visualizations** with sublayers
- **Maps with legends** showing data categories

### Example Scenarios

1. **Election Results:**
```dart
// Color states by winning party
shapeColorValueMapper: (index) => 
  results[index].winner == 'Party A' ? Colors.blue : Colors.red
```

2. **Sales Performance:**
```dart
// Color regions by sales achievement
shapeColorMappers: [
  MapColorMapper(from: 0, to: 50, color: Colors.red),
  MapColorMapper(from: 51, to: 100, color: Colors.green),
]
```

3. **Population Density:**
```dart
// Use bubbles for population
bubbleSizeMapper: (index) => data[index].population
```

## Common Patterns

### Pattern 1: Basic Map with All Features

```dart
MapShapeLayer(
  source: _dataSource,
  color: Colors.grey[200],
  strokeColor: Colors.grey[700],
  strokeWidth: 0.5,
  showDataLabels: true,
  legend: MapLegend(MapElement.shape),
  selectionSettings: MapSelectionSettings(
    color: Colors.yellow,
    strokeColor: Colors.black,
    strokeWidth: 2,
  ),
  dataLabelSettings: MapDataLabelSettings(
    textStyle: TextStyle(fontSize: 10, fontWeight: FontWeight.bold),
  ),
)
```

### Pattern 2: Loading with Error Handling

```dart
class MapWidget extends StatefulWidget {
  @override
  _MapWidgetState createState() => _MapWidgetState();
}

class _MapWidgetState extends State<MapWidget> {
  late Future<MapShapeSource> _dataSourceFuture;
  
  @override
  void initState() {
    super.initState();
    _dataSourceFuture = _loadMapData();
  }
  
  Future<MapShapeSource> _loadMapData() async {
    try {
      return MapShapeSource.asset(
        'assets/map.json',
        shapeDataField: 'name',
      );
    } catch (e) {
      throw Exception('Failed to load map: $e');
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return FutureBuilder<MapShapeSource>(
      future: _dataSourceFuture,
      builder: (context, snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return Center(child: CircularProgressIndicator());
        }
        if (snapshot.hasError) {
          return Center(child: Text('Error: ${snapshot.error}'));
        }
        return SfMaps(
          layers: [MapShapeLayer(source: snapshot.data!)],
        );
      },
    );
  }
}
```

## Troubleshooting

### Issue: Shapes Not Rendering

**Check:**
1. GeoJSON file path is correct
2. File is declared in `pubspec.yaml`
3. `shapeDataField` matches a property in GeoJSON
4. GeoJSON is valid (test with online validators)

### Issue: Data Not Matching Shapes

**Check:**
1. Values from `primaryValueMapper` exactly match GeoJSON property values
2. String case matches (e.g., "california" ≠ "California")
3. Spelling is identical
4. Special characters match

### Issue: Colors Not Applying

**Check:**
1. `shapeColorValueMapper` is returning correct type
2. If using `shapeColorMappers`, values fall within defined ranges
3. Colors aren't being overridden by theme
4. Layer `color` property isn't conflicting
