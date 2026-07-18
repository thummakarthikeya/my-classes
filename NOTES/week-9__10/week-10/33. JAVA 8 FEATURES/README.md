
| Requirement              | Stream API                                                    |
| ------------------------ | ------------------------------------------------------------- |
| Ascending                | `.sorted()`                                                   |
| Descending               | `.sorted(Comparator.reverseOrder())`                          |
| By String Length         | `.sorted(Comparator.comparingInt(String::length))`            |
| By String Length Desc    | `.sorted(Comparator.comparingInt(String::length).reversed())` |
| Using `compareTo()`      | `.sorted((a, b) -> a.compareTo(b))`                           |
| Using `compareTo()` Desc | `.sorted((a, b) -> b.compareTo(a))`                           |
