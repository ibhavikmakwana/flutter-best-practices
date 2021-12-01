# Best practices in Flutter development

Do's and Don'ts for Flutter development, heavily inspired from the [android-best-practices](https://github.com/futurice/android-best-practices)

## Summary

#### [Add a linting rules firstly when starting app from scratch](#lint-rules)
#### [Use const wherever possible](#use-const)
#### [Create separate class to define the colors](#separate-color-class)

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
