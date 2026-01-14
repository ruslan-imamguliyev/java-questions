# Check if Points Make Straight Line

_Score_: 2

Given an array of 2D coordinates, determine if all points lie on the **same straight line**.

### Mathematical Approach
Points lie on the same line if they have the same **slope** between any two pairs of points.

For points (x₁, y₁) and (x₂, y₂), slope = (y₂ - y₁) / (x₂ - x₁)

### Important Considerations
- **Avoid division** to prevent floating-point errors
- Use cross multiplication: instead of checking if `(y₂-y₁)/(x₂-x₁) == (y₃-y₁)/(x₃-x₁)`, check if `(y₂-y₁)*(x₃-x₁) == (y₃-y₁)*(x₂-x₁)`
- Handle **vertical lines** (same x-coordinates)

### Example
Points: `[[1,2], [2,3], [3,4], [4,5], [5,6], [6,7]]`
- All points lie on the same line.
- Output: **True**

Points: `[[1,1], [2,2], [3,4], [4,5], [5,6], [7,7]]`
- The points do not form a single straight line.
- Output: **False**

### Input Format
- Array of 2D coordinates: `[[x₁, y₁], [x₂, y₂], ...]`

### Output Format
- Boolean: `True` if all points are collinear, `False` otherwise

## Placeholder:
```java

public static boolean solution(int[][] coordinates) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: coordinates=[[1, 2], [2, 3], [3, 4], [4, 5], [5, 6], [6, 7]]
Output: True

Input: coordinates=[[1, 1], [2, 2], [3, 4], [4, 5], [5, 6], [7, 7]]
Output: False

Input: coordinates=[[0, 0], [0, 1], [0, -1]]
Output: True

Input: coordinates=[[1, 1], [2, 2], [3, 3]]
Output: True

Input: coordinates=[[0, 0]]
Output: True
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[][]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[][]{{1,2},{2,3},{3,4},{4,5},{5,6},{6,7}});
    testCases.add(new int[][]{{1,1},{2,2},{3,4},{4,5},{5,6},{7,7}});
    testCases.add(new int[][]{{0,0},{0,1},{0,-1}});
    testCases.add(new int[][]{{1,1},{2,2},{3,3}});
    testCases.add(new int[][]{{1,2},{1,3},{1,4}});
    testCases.add(new int[][]{{2,1},{3,1},{4,1}});
    testCases.add(new int[][]{{0,0},{1,2},{2,4}});
    testCases.add(new int[][]{{0,0}});
    testCases.add(new int[][]{{-4,-3},{1,0},{3,-1},{0,-2},{-2,-2}});
    testCases.add(new int[][]{{2,3},{3,3},{4,3},{5,3},{6,3}});

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int n = random.nextInt(5) + 2; // [2,6]
        int[][] coords = new int[n][2];

        if (random.nextDouble() > 0.5) {
            int x0 = random.nextInt(21) - 10;
            int y0 = random.nextInt(21) - 10;
            int dx = random.nextInt(7) - 3;
            int dy = random.nextInt(7) - 3;

            for (int i = 0; i < n; i++) {
                coords[i][0] = x0 + i * dx;
                coords[i][1] = y0 + i * dy;
            }
        } else {
            for (int i = 0; i < n; i++) {
                coords[i][0] = random.nextInt(21) - 10;
                coords[i][1] = random.nextInt(21) - 10;
            }
        }

        testCases.add(coords);
    }

    // Verify
    for (int[][] coordinates : testCases) {
        boolean result = solution(coordinates);
        boolean expected = correctSolution(coordinates);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: coordinates=" + Arrays.deepToString(coordinates) +
                ", expected=" + expected +
                ", got=" + result
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static boolean correctSolution(int[][] coordinates) {
    int n = coordinates.length;
    if (n <= 2) {
        return true;
    }

    int x0 = coordinates[0][0];
    int y0 = coordinates[0][1];
    int x1 = coordinates[1][0];
    int y1 = coordinates[1][1];

    for (int i = 2; i < n; i++) {
        int xi = coordinates[i][0];
        int yi = coordinates[i][1];

        // (yi - y0) * (x1 - x0) == (y1 - y0) * (xi - x0)
        if ((yi - y0) * (x1 - x0) != (y1 - y0) * (xi - x0)) {
            return false;
        }
    }

    return true;
}
```
