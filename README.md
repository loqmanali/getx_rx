# GetX RX - Reactive State Management

A powerful and lightweight reactive state management library for Flutter applications, providing reactive programming capabilities with minimal boilerplate code.

## Features

- **Reactive Variables**: Transform any data type into reactive observables
- **Stream-based Architecture**: Built on Dart streams for efficient state management
- **Worker Functions**: Advanced reactive listeners with conditions and timing controls
- **Type Safety**: Full type safety with generic implementations
- **Memory Efficient**: Automatic disposal and memory management
- **Zero Dependencies**: Minimal external dependencies for better performance
- **Extension Methods**: Rich extension methods for common data types

### Core Components

- **Reactive Types**: `RxString`, `RxInt`, `RxDouble`, `RxBool`, `RxList`, `RxMap`, `RxSet`
- **Worker Functions**: `ever()`, `once()`, `interval()`, `everAll()`
- **Stream Management**: Built-in stream controllers and subscription handling
- **Debouncing**: Built-in debouncing utilities for performance optimization

## Getting Started

### Installation

Add this to your package's `pubspec.yaml` file:

```yaml
dependencies:
  getx_rx:
    git:
      url: https://github.com/loqmanali/getx_rx.git
```

### Basic Usage

#### Creating Reactive Variables

```dart
import 'package:getx_rx/getx_rx.dart';

// Basic reactive types
final count = 0.obs;                    // RxInt
final name = 'Flutter'.obs;             // RxString
final isLoading = false.obs;            // RxBool
final items = <String>[].obs;           // RxList
final userMap = <String, dynamic>{}.obs; // RxMap
```

#### Updating Values

```dart
// Direct assignment
count.value = 10;
name.value = 'GetX RX';
isLoading.value = true;

// Using call method
count(15);
name('Updated Name');

// List operations
items.add('New Item');
items.addAll(['Item 1', 'Item 2']);
items.removeAt(0);
```

#### Listening to Changes

```dart
// Basic listener
count.listen((value) {
  print('Count changed to: $value');
});

// Listen with error handling
name.listen(
  (value) => print('Name: $value'),
  onError: (error) => print('Error: $error'),
  onDone: () => print('Stream completed'),
);
```

## Advanced Usage

### Worker Functions

#### ever() - Listen to every change

```dart
final counter = 0.obs;

// Listen to every change
final worker = ever(counter, (value) {
  print('Counter value: $value');
});

// With condition
final conditionalWorker = ever(
  counter,
  (value) => print('Counter is greater than 5: $value'),
  condition: () => counter.value > 5,
);

// Dispose when done
worker.dispose();
```

#### once() - Execute only once

```dart
final status = 'loading'.obs;

// Execute only once when condition is met
once(status, (value) {
  print('Status changed to: $value');
}, condition: () => status.value == 'completed');
```

#### interval() - Debounced execution

```dart
final searchQuery = ''.obs;

// Execute after 1 second of inactivity
interval(
  searchQuery,
  (query) => performSearch(query),
  time: Duration(seconds: 1),
);
```

#### everAll() - Listen to multiple observables

```dart
final firstName = ''.obs;
final lastName = ''.obs;

// Listen to changes in multiple observables
everAll([firstName, lastName], (value) {
  print('Name changed: ${firstName.value} ${lastName.value}');
});
```

### Custom Reactive Types

```dart
class User {
  String name;
  int age;
  
  User({required this.name, required this.age});
  
  @override
  String toString() => 'User(name: $name, age: $age)';
}

// Create reactive custom object
final user = User(name: 'John', age: 25).obs;

// Update custom object
user.update((user) {
  user?.name = 'Jane';
  user?.age = 30;
});

// Listen to changes
user.listen((user) {
  print('User updated: $user');
});
```

### Stream Integration

```dart
final dataStream = Stream.periodic(
  Duration(seconds: 1),
  (count) => count,
);

final reactiveData = 0.obs;

// Bind stream to reactive variable
reactiveData.bindStream(dataStream);
```

### Extension Methods

#### String Extensions

```dart
final text = 'Hello World'.obs;

// Use string methods directly
print(text.toUpperCase());     // HELLO WORLD
print(text.substring(0, 5));   // Hello
print(text.contains('World')); // true
print(text.split(' '));        // [Hello, World]
```

#### List Extensions

```dart
final numbers = <int>[].obs;

// Conditional additions
numbers.addIf(true, 42);
numbers.addAllIf(numbers.length < 5, [1, 2, 3]);

// Replace all items
numbers.assignAll([10, 20, 30]);
```

## Best Practices

### Memory Management

```dart
class MyController {
  final count = 0.obs;
  late Worker _worker;
  
  void onInit() {
    _worker = ever(count, (value) {
      // Handle changes
    });
  }
  
  void onClose() {
    _worker.dispose(); // Always dispose workers
    count.close();     // Close reactive variables
  }
}
```

### Performance Optimization

```dart
// Use interval for expensive operations
final searchTerm = ''.obs;

interval(
  searchTerm,
  (term) => expensiveSearchOperation(term),
  time: Duration(milliseconds: 500),
);

// Use conditions to limit executions
ever(
  dataList,
  (list) => updateUI(list),
  condition: () => dataList.isNotEmpty,
);
```

### Error Handling

```dart
final apiData = ''.obs;

apiData.listen(
  (data) => processData(data),
  onError: (error) {
    print('Error processing data: $error');
    // Handle error appropriately
  },
);
```

## API Reference

### Core Classes

- `RxInterface<T>`: Base interface for all reactive types
- `RxObjectMixin<T>`: Mixin providing reactive functionality
- `_RxImpl<T>`: Base implementation for reactive types

### Reactive Types

- `RxString`: Reactive string with string-specific methods
- `RxInt`, `RxDouble`: Reactive numbers with arithmetic operations
- `RxBool`: Reactive boolean
- `RxList<T>`: Reactive list with list operations
- `RxMap<K, V>`: Reactive map with map operations
- `RxSet<T>`: Reactive set with set operations

 Worker Functions

- `ever<T>()`: Listen to every change
- `once<T>()`: Execute only once
- `interval<T>()`: Debounced execution
- `everAll()`: Listen to multiple observables

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
