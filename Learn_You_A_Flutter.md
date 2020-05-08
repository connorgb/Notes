# Learn You A Flutter

## Projects

### Create Subfolders
* assets\image
  * Holds subfolders such as images, fonts, and config files
* lib\pages
  * UI files: logins, lists of items, charts, and settings
* lib\models
  * Classes for data like customer info, inventory
* lib\utils
  * Helper classes such as date calculations and conversions
* lib\widgets
  * Dart files separating widgets to reuse throughout the app
* lib\services
  * Classes to retrieve data from internet services (Firestore, cloud)

### Structuring Widgets
* The SDK does not automatically create the separate home file, which contains the main presentation page. You must make your own home.dart file in a pages folder. The main.dart file starts the app and calls the `Home` widget in the home file.
* The `main.dart` file has 3 main sections:
  * `import` package/file
  * `main()` function
  * A class that extends the `StatelessWidget` widget and returns the app as a widget

#### Material Design
* A system of best practices for UI design
* Material design components (widgets): 
  * visual
  * behavioural 
  * motion

## The Widget Tree

Some widgets that **cannot** be used with Cupertino:
* `Scaffold`
  * Implements the Material Design visual layout, allows us to use Material Components widgets
  * Can include `AppBar`, `BottomAppBar`, `FloatingActionButton`, etc.
* `AppBar`
  * The toolbar at the top of the screen
  * Usually contains, among others:
    * `title`
      * Typically implemented with a `Text` widget
      * Can be customized with `DropdownButton`, etc.
    * `leading`
      * Displayed before the `title` property
      * An `IconButton` or `Backbutton`
    * `actions`
      * Displayed to the right of the `title` property
      * A list of widgets in the upper right of the `AppBar` widget, with an `IconButton` or `Backbutton`
    * `flexibleSpace`
        * Stacked behind the `Toolbar` or `TabBar` widget
        * Height is usually the same as the `Appbar` widget's height
        * Can be a background, or any widget



* `CircleAvatar` -- Rounded image display

Some widgets that **can** be used with Cupertino:
* `SingleChildScrollview` -- Vertical or horizontal scrolling for a single child widget
* `Padding` -- Adds padding
* `Column` -- Vertical list of child widgets
* `Row` -- Horizontal "                   "
* `Container` -- Can be an empty placeholder, or specify height, width, transform, and many other properties
* `Expanded` -- Fills the available space for the child widget (belonging to a `Column` or `Row`)
* `Text` -- Can be one or multiple lines. Optional `style` argument
* `Stack` -- Lets you stack widgets, with an optional widget to align
* `Positioned` -- Optional for the `Stack` widget to control child position and size

### Refactoring the Widget Tree
#### With a Constant
* Initializes the widget to a `final` variable
* Allows you to separate code into sections
* Widgets rely on the `BuildContext` object of the parent widget when initialized with a constant
  * Will always redraw with the parent widget
```dart
final container = Container(
  color: Colors.yellow,
  height: 40.0,
  width: 40.0,
);
```

#### With a Method
* Returns the widget by calling the method name
  * Can return a value by a general `Widget` or a specific `Container`, `Row`, etc.
* Pretty much identical to constants
```dart
// Return by general Widget name
Widget _buildContainer() {
  return Container(
    color: Colors.yellow,
    height: 40.0,
    width: 40.0,
  );
} 

// Or return by specific widget
Container _buildContainer() {
  return Container(
    color: Colors.yellow,
    height: 40.0,
    width: 40.0,
  );
}
```
#### With a Widget Class
* Subclassing the `StatelessWidget` or `StatefulWidget` class
* `const` allows you to cache and reuse the widget, and will not redraw when the parent widget redraws
* Relies on its own `BuildContext`, not its parent's
* Performance
```dart
class ContainerLeft extends StatelessWidget {
  const ContainerLeft ({
    Key key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container (
      color: Colors.yellow,
      height: 40.0,
      width: 40.0,
    );
  }
}
// Call to initialize the widget. Note the const keyword
const ContainerLeft(),
```

## Widgets

## Animations
## App Navigation
## Scrolling Lists And Effects
## Layouts
## Interactivity
## Platform-Native
