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
* Some widgets that **cannot** be used with Cupertino:
  *  `Scaffold` -- Implements the Material Design visual layout, allows us to use Material Components widgets
  * `AppBar` -- The toolbar at the top of the screen
  * `CircleAvatar` -- Rounded image display
* Some widgets that **can** be used with Cupertino:
  * `SingleChildScrollview` -- Vertical or horizontal scrolling for a single child widget
  * `Padding` -- Adds padding
  * `Column` -- Vertical list of child widgets
  * `Row` -- Horizontal "                   "
  * `Container` -- Can be an empty placeholder, or specify height, width, transform, and many other properties
  * `Expanded` -- Fills the available space for the child widget (belonging to a `Column` or `Row`)
  * `Text` -- Can be one or multiple lines. Optional `style` argument
  * `Stack` -- Lets you stack widgets, with an optional widget to align
  * `Positioned` -- Optional for the `Stack` widget to control child position and size




## Widgets
## Animations
## App Navigation
## Scrolling Lists And Effects
## Layouts
## Interactivity
## Platform-Native
