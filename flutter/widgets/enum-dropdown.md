# Dropdown Button

### enum class
```
enum FlamerMode {
  mono,
  bifrost,
  multiLayer,
  dynamicLayer,
}
```

### dropdown button
```
DropdownButton<FlamerMode>(
    value: flamerStore.mode,
    icon: const Icon(Icons.arrow_downward),
    iconSize: 24,
    elevation: 16,
    style: const TextStyle(color: Color(0xFF313131)),
    underline: Container(
        height: 1,
        color: const Color(0xFF313131),
    ),
    onChanged: (FlamerMode? newValue) {
        flamerStore.mode = newValue!;
    },
    items: FlamerMode.values.map((FlamerMode classType) {
        return DropdownMenuItem<FlamerMode>(
        value: classType,
        child: Text(classType.toString().split('.').last),
        );
    }).toList(),
)
```