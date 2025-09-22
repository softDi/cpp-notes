# C++ Containers: std::less vs std::greater Behavior

| Container | Default Comparator | With std::less | With std::greater | Access Pattern | Notes |
|-----------|-------------------|----------------|------------------|----------------|--------|
| **std::set** | `std::less` | Ascending order (1,2,3,4) | Descending order (4,3,2,1) | Iterator traversal | Sorted associative container |
| **std::multiset** | `std::less` | Ascending order (1,2,3,4) | Descending order (4,3,2,1) | Iterator traversal | Allows duplicates |
| **std::map** | `std::less` | Keys in ascending order | Keys in descending order | Iterator traversal | Key-value pairs sorted by key |
| **std::multimap** | `std::less` | Keys in ascending order | Keys in descending order | Iterator traversal | Multiple values per key allowed |
| **std::priority_queue** | `std::less` | **Max-heap** (largest at top) | **Min-heap** (smallest at top) | `top()` access only | **OPPOSITE behavior!** |

## Detailed Behavior Examples

### Regular Sorted Containers (set, multiset, map, multimap)
```cpp
// std::less (default) - ASCENDING order
std::set<int> s1 {3, 1, 4, 2};
// Iteration: 1, 2, 3, 4

// std::greater - DESCENDING order
std::set<int, std::greater<int>> s2 {3, 1, 4, 2};
// Iteration: 4, 3, 2, 1
