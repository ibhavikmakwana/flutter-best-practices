# Best practices in Flutter development

Do's and Don'ts for Flutter development, heavily inspired from the [android-best-practices](https://github.com/futurice/android-best-practices)

## Summary

#### [Add a linting rules firstly when starting app from scratch](#lint-rules)
#### [Use const wherever possible](#use-const)
#### [Create separate class to define the colors](#separate-color-class)
#### [Define theme for your app](#define-theme)
#### [Don't use functional widgets](#avoid-functional-widgets)
#### [Avoid print()](#avoid-print)

----------

### lint-rules
Whenever you start a project from scratch firstly add [Lint](https://pub.dev/packages/lint) package which can help you to statically analyzed your flutter/dart code and can help you to improve your code quality which evantually reduces the bugs and errors.


### use-const

Try using a const keyword wherever possible, to improve the performance of the app. For example: When you use it for some widgets and when `setState` gets called it does not change the `widget` with the `const` keyword.

```dart
const Text(
  "Flutter Best Practices",
  style: const TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
  ),
),
```

### separate-color-class

Try to have all the colors in a single class for your application, do it with the Strings as well if you are not using the localization so whenever you want to add localization you can find all the strings in one place.

Note: You might get the linting error `avoid_classes_with_only_static_members` but it is okay to ignore for this kind of usage.

Quoting the official [documentation](https://dart.dev/guides/language/effective-dart/design#avoid-defining-a-class-that-contains-only-static-members): 
> In idiomatic Dart, classes define kinds of objects. A type that is never instantiated is a code smell.
> However, this isn’t a hard rule. With constants and enum-like types, it may be natural to group them in a class.

Color Example:

```dart
class AppColor {
  static const Color red = Color(0xFFFF0000);
  static const Color green = Color(0xFF4CAF50);
  static const Color errorRed = Color(0xFFFF6E6E);
}
```

### define-theme

Define Theme of the app as well as a first priority to avoid the headache of changing the theme in future updates, Setting up Theme is surely confusring but one time task.
Ask your designer to share all the Theme related data like, Colors, font sizes and weightage.

Example:
```dart
MaterialApp(
  title: appName,
  theme: ThemeData(
    // Define the default brightness and colors.
    brightness: Brightness.dark,
    
    // You can add the color from the seprate class here as well to maintain it well.
    primaryColor: Colors.lightBlue[800],

    // Define the default font family.
    fontFamily: 'Georgia',

    // Define the default `TextTheme`. Use this to specify the default
    // text styling for headlines, titles, bodies of text, and more.
    textTheme: const TextTheme(
      headline1: TextStyle(fontSize: 72.0, fontWeight: FontWeight.bold),
      headline6: TextStyle(fontSize: 36.0, fontStyle: FontStyle.italic),
      bodyText2: TextStyle(fontSize: 14.0, fontFamily: 'Hind'),
    ),
  ),
  home: const MyHomePage(
    title: appName,
  ),
);
```

You can do much more with the Theme, which is defining your custom TextField, Card, Bottom Navigation Bar themes for once and use it straight away in the app. Checkout an example of using Theme over [here](https://github.com/ibhavikmakwana/supabase_playground/blob/master/lib/values/theme.dart)

>In Addition, In a large app/project you might end up having a multipl Themes not just light and dark mode themes but Custom themes for some specific widgets/components as well so you can always create a separate class/file and maintain all the themes over there.

### avoid-functional-widgets

We usually have a situation where we need to separate out UI code from the widget, But we avoid creating a separate widget and use function which returns Widget.
This practice have some benefits, like you don't need to pass all parameters in your new widget, You have less code and less files. But this approach may cause issue when you want to inspect your widget. Let's see this in depth.

When you use functional widget code looks like this.
```dart
Widget functionWidget({ Widget child}) {
  return Container(child: child);
}
```
You can now use it as
```dart
functionWidget(
  child: functionWidget(),
);
```
In this case Widget tree will look something like this
```dart
Container
  Container
```

Instead if we use Widget, Our widget looks like
```dart
class ClassWidget extends StatelessWidget {
  final Widget child;

  const ClassWidget({Key key, this.child}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      child: child,
    );
  }
}
```
You can use it as
```dart
new ClassWidget(
  child: new ClassWidget(),
);
```
And in this case Widget tree looks like this
```dart
ClassWidget
  Container
    ClassWidget
      Container
```
As we can see here, if we use Widgets, framework understands it in better way and UI becomes easy to inspect.
For more info follow [this](https://stackoverflow.com/a/53234826/11445644) answer from stackoverflow

### avoid-print
Avoid using `print()` in your code to view outputs in the console, as if the output is too much at once, then Android sometimes discards some log lines. To avoid this, you can use `debugPrint()`.

You can also use `log()` from `dart:developer` which does not have max length limits like print() or debugPrint(), which also helps in dart dev tools to show formatted logging.
