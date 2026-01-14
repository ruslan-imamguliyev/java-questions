# Count Negative Numbers in Sorted Matrix

_Score_: 1

Count the **number of negative numbers** in a grid where the grid is sorted in **non-increasing** order both row-wise and column-wise.

### Grid Properties
- Each row is sorted from **largest to smallest** (non-increasing)
- Example row: `[5, 3, 1, -2, -5]`
- Once you find a negative, all subsequent elements in that row are also negative

### Efficient Algorithm (O(m+n))
Instead of checking every element:

```python
For each row:
    1. Find first negative number (binary search or linear scan)
    2. Count all remaining elements in that row
    3. Add to total count
```

### Example
Grid:
```
[4,  3,  2, -1]
[3,  2,  1, -1]
[1,  1, -1, -2]
[-1, -1, -2, -3]
```

Row analysis:
- Row 0: First negative at index 3 → count = 4-3 = **1**
- Row 1: First negative at index 3 → count = 4-3 = **1**
- Row 2: First negative at index 2 → count = 4-2 = **2**
- Row 3: First negative at index 0 → count = 4-0 = **4**
- Total: 1+1+2+4 = **8**

### Optimization
Since rows are sorted non-increasingly:
- Use binary search to find first negative
- Or scan left-to-right, break once negative found

### Examples
`[[3, 2], [1, 0]]` → **0** negatives

`[[1, -1], [-1, -1]]` → **3** negatives

`[[-1]]` → **1** negative

`[[5, 1, 0], [-5, -5, -5]]` → **3** negatives

### Input Format
- 2D grid (list of lists) of integers
- Each row sorted in non-increasing order

### Output Format
- Integer: total count of negative numbers

## Placeholder:
```java

public static int solution(int[][] grid) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: grid=[[4, 3, 2, -1], [3, 2, 1, -1], [1, 1, -1, -2], [-1, -1, -2, -3]]
Output: 8

Input: grid=[[3, 2], [1, 0]]
Output: 0

Input: grid=[[1, -1], [-1, -1]]
Output: 3

Input: grid=[[-1]]
Output: 1

Input: grid=[[5, 1, 0], [-5, -5, -5]]
Output: 3
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[][]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[][]{
        {4, 3, 2, -1},
        {3, 2, 1, -1},
        {1, 1, -1, -2},
        {-1, -1, -2, -3}
    });
    testCases.add(new int[][]{{3, 2}, {1, 0}});
    testCases.add(new int[][]{{1, -1}, {-1, -1}});
    testCases.add(new int[][]{{-1}});
    testCases.add(new int[][]{{5, 1, 0}, {-5, -5, -5}});
    testCases.add(new int[][]{{10, 8, 6, 4}, {2, 0, -2, -4}});
    testCases.add(new int[][]{{1}});
    testCases.add(new int[][]{{0, 0}, {0, 0}});
    testCases.add(new int[][]{{5, 4, 3}, {2, 1, 0}, {-1, -2, -3}});
    testCases.add(new int[][]{{10, 5, 0, -5}, {8, 3, -1, -6}});

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int rows = random.nextInt(5) + 1;
        int cols = random.nextInt(5) + 1;
        int[][] grid = new int[rows][cols];

        for (int i = 0; i < rows; i++) {
            int current = random.nextInt(31) - 10; // [-10,20]
            for (int j = 0; j < cols; j++) {
                grid[i][j] = current;
                current -= random.nextInt(6); // keep non-increasing
            }
        }
        testCases.add(grid);
    }

    // Verify
    for (int[][] grid : testCases) {
        int result = solution(copyGrid(grid));
        int expected = correctSolution(copyGrid(grid));

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: grid=" + Arrays.deepToString(grid) +
                ", expected=" + expected +
                ", got=" + result
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}

private static int[][] copyGrid(int[][] grid) {
    int[][] copy = new int[grid.length][];
    for (int i = 0; i < grid.length; i++) {
        copy[i] = Arrays.copyOf(grid[i], grid[i].length);
    }
    return copy;
}
```

## Solution:
```java
public static int correctSolution(int[][] grid) {
    int count = 0;
    int rows = grid.length;
    int cols = rows > 0 ? grid[0].length : 0;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (grid[i][j] < 0) {
                // since the row is sorted in non-increasing order,
                // all elements to the right are also negative
                count += (cols - j);
                break;
            }
        }
    }

    return count;
}
```
