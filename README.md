# Best practices in Flutter development

Do's and Don'ts for Flutter development, heavily inspired from the [android-best-practices](https://github.com/futurice/android-best-practices)

## Summary

#### [Use const wherever possible](#use-const)

----------

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
