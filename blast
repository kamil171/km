import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

class DrillHole {
  final int id;
  final double x;
  final double y;
  final double z;
  
  DrillHole(this.id, this.x, this.y, this.z);
}

Future<List<DrillHole>> loadDrillHoles(String filePath) async {
  final data = await rootBundle.loadString(filePath);
  final lines = data.split('\n');
  
  return lines.map((line) {
    final parts = line.split(',').map((e) => e.trim()).toList();
    return DrillHole(
      int.parse(parts[0]),
      double.parse(parts[1]),
      double.parse(parts[2]),
      double.parse(parts[3]),
    );
  }).toList();
}

class NavigationScreen extends StatefulWidget {
  @override
  _NavigationScreenState createState() => _NavigationScreenState();
}

class _NavigationScreenState extends State<NavigationScreen> {
  List<DrillHole> _holes = [];
  DrillHole? _currentHole;

  @override
  void initState() {
    super.initState();
    loadDrillHoles('assets/drill_holes.txt').then((holes) {
      setState(() => _holes = holes);
    });
  }

  void _setCurrentHole(int holeId) {
    setState(() => _currentHole = _holes.firstWhere((h) => h.id == holeId));
    // Здесь запускаем отслеживание перемещений через датчики
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Навигатор скважин')),
      body: Column(
        children: [
          Expanded(
            child: ArcgisMapView( // Пример интеграции с ArcGIS
              initialCameraPosition: CameraPosition(
                target: _holes.isNotEmpty 
                    ? LatLng(_holes.first.y, _holes.first.x) 
                    : LatLng(0, 0),
              ),
              markers: _holes.map((hole) => Marker(
                id: hole.id.toString(),
                position: LatLng(hole.y, hole.x),
              )).toList(),
            ),
          ),
          ElevatedButton(
            onPressed: () => _setCurrentHole(1),
            child: Text('Я у скважины №1'),
          ),
        ],
      ),
    );
  }
}
