# C++ Sorting Methods: std::less vs std::greater Behavior

| Sorting Method | Default Comparator | With std::less | With std::greater | Usage Pattern | Notes |
|----------------|-------------------|----------------|------------------|---------------|--------|
| **std::sort** | `std::less` | Ascending order (1,2,3,4) | Descending order (4,3,2,1) | `std::sort(begin, end, comp)` | Modifies container in-place |
| **std::stable_sort** | `std::less` | Ascending order (1,2,3,4) | Descending order (4,3,2,1) | `std::stable_sort(begin, end, comp)` | Preserves relative order of equal elements |
| **std::partial_sort** | `std::less` | First N elements in ascending | First N elements in descending | `std::partial_sort(begin, middle, end, comp)` | Only sorts first N elements |
| **std::nth_element** | `std::less` | Nth element as if ascending sorted | Nth element as if descending sorted | `std::nth_element(begin, nth, end, comp)` | Partitions around nth element |
| **std::make_heap** | `std::less` | **Max-heap** (largest at front) | **Min-heap** (smallest at front) | `std::make_heap(begin, end, comp)` | **OPPOSITE behavior like priority_queue!** |
| **std::sort_heap** | `std::less` | Ascending order (1,2,3,4) | Descending order (4,3,2,1) | `std::sort_heap(begin, end, comp)` | Converts heap to sorted range |
| **std::push_heap** | `std::less` | Maintains max-heap property | Maintains min-heap property | `std::push_heap(begin, end, comp)` | Adds element to heap |
| **std::pop_heap** | `std::less` | Removes largest (max-heap) | Removes smallest (min-heap) | `std::pop_heap(begin, end, comp)` | Removes top of heap |
| **std::is_sorted** | `std::less` | Checks ascending order | Checks descending order | `std::is_sorted(begin, end, comp)` | Returns bool for sorted check |
| **std::binary_search** | `std::less` | Searches in ascending sorted range | Searches in descending sorted range | `std::binary_search(begin, end, value, comp)` | Requires pre-sorted range |
| **std::lower_bound** | `std::less` | First position where value could be inserted (ascending) | First position where value could be inserted (descending) | `std::lower_bound(begin, end, value, comp)` | Returns iterator |
| **std::upper_bound** | `std::less` | First position after value (ascending) | First position after value (descending) | `std::upper_bound(begin, end, value, comp)` | Returns iterator |
| **std::equal_range** | `std::less` | Range of equal elements (ascending context) | Range of equal elements (descending context) | `std::equal_range(begin, end, value, comp)` | Returns pair of iterators |

## Detailed Behavior Examples

### Basic Sorting Algorithms
```cpp
std::vector<int> v1 {3, 1, 4, 2};
std::vector<int> v2 {3, 1, 4, 2};

// std::less (default) - ASCENDING order
std::sort(v1.begin(), v1.end());
// Result: {1, 2, 3, 4}

// std::greater - DESCENDING order
std::sort(v2.begin(), v2.end(), std::greater<int>());
// Result: {4, 3, 2, 1}
```

### Heap Operations (OPPOSITE behavior!)
```cpp
std::vector<int> v1 {3, 1, 4, 2};
std::vector<int> v2 {3, 1, 4, 2};

// std::less (default) - MAX-HEAP (largest at front)
std::make_heap(v1.begin(), v1.end());
// v1.front() = 4 (largest element)

// std::greater - MIN-HEAP (smallest at front)
std::make_heap(v2.begin(), v2.end(), std::greater<int>());
// v2.front() = 1 (smallest element)
```

### Stable Sort (preserves relative order)
```cpp
struct Person { std::string name; int age; };
std::vector<Person> people = {{"Alice", 25}, {"Bob", 25}, {"Charlie", 30}};

// Ascending by age, Alice and Bob maintain their relative order
std::stable_sort(people.begin(), people.end(),
    [](const Person& a, const Person& b) { return a.age < b.age; });

// Descending by age, Alice and Bob maintain their relative order
std::stable_sort(people.begin(), people.end(),
    [](const Person& a, const Person& b) { return a.age > b.age; });
```

### Binary Search on Sorted Ranges
```cpp
std::vector<int> ascending {1, 2, 3, 4, 5};
std::vector<int> descending {5, 4, 3, 2, 1};

// Search in ascending sorted range
bool found1 = std::binary_search(ascending.begin(), ascending.end(), 3);
// found1 = true

// Search in descending sorted range (must specify comparator!)
bool found2 = std::binary_search(descending.begin(), descending.end(), 3, std::greater<int>());
// found2 = true
```

### Partial Sort Example
```cpp
std::vector<int> v {5, 2, 8, 1, 9, 3};

// Sort only first 3 elements in ascending order
std::partial_sort(v.begin(), v.begin() + 3, v.end());
// Result: {1, 2, 3, ...} (rest may be in any order)

std::vector<int> v2 {5, 2, 8, 1, 9, 3};
// Sort only first 3 elements in descending order
std::partial_sort(v2.begin(), v2.begin() + 3, v2.end(), std::greater<int>());
// Result: {9, 8, 5, ...} (rest may be in any order)
```

## Key Insights

1. **Heap operations behave OPPOSITE** to other sorting methods (like `std::priority_queue`)
   - `std::less` creates max-heap (largest at top)
   - `std::greater` creates min-heap (smallest at top)

2. **All other sorting methods behave consistently**:
   - `std::less` → ascending order
   - `std::greater` → descending order

3. **Binary search functions require matching comparator** with the sorted order of the range

4. **Stable sort preserves relative order** of equal elements regardless of comparator direction
