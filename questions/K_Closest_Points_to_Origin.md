# K Closest Points to Origin

_Score_: 3

Given an array of 2D points and an integer `k`, return the **k closest points** to the origin `(0, 0)` based on **Euclidean distance**.

### Distance Formula
The Euclidean distance from point `(x, y)` to origin `(0, 0)` is:
$$\sqrt{x^2 + y^2}$$

For comparison purposes, you can use squared distance `x^2 + y^2` (avoids square root calculation).

### Algorithm Approach
Use a **max heap** to maintain the k closest points:
1. For each point, calculate its distance to origin
2. Add to heap (storing negative distance for max heap behavior)
3. If heap size exceeds k, remove the farthest point
4. Return the k points remaining in the heap

### Example
`points = [[1,3], [-2,2], [5,8], [0,1]]`, `k = 2`
- Distances: 
  - [1,3]: √10 ≈ 3.16
  - [-2,2]: √8 ≈ 2.83
  - [5,8]: √89 ≈ 9.43
  - [0,1]: √1 = 1.00
- 2 closest: `[[0,1], [-2,2]]`

### Input Format
- `points`: List of [x, y] coordinates
- `k`: Number of closest points to find

### Output Format
- List of k closest points (order doesn't matter)

## Placeholder:
```java

public static int[][] solution(int[][] points, int k) {
    // Write your code here
    return new int[0][0];
}
```

## Examples:
```
Input: points=[[1, 3], [-2, 2], [5, 8], [0, 1]], k=2
Output: [[-2, 2], [0, 1]]

Input: points=[[3, 3], [5, -1], [-2, 4], [0, 0], [1, 2]], k=3
Output: [[3, 3], [1, 2], [0, 0]]

Input: points=[[1, 1], [2, 2], [3, 3], [4, 4], [5, 5]], k=2
Output: [[2, 2], [1, 1]]

Input: points=[[1, 2], [3, 4], [0, 0], [1, 1], [2, 3]], k=3
Output: [[1, 2], [1, 1], [0, 0]]

Input: points=[[5, 6], [-1, 2], [0, 0], [2, 3]], k=2
Output: [[-1, 2], [0, 0]]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<Object[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new Object[]{new int[][]{{1, 3}, {-2, 2}, {5, 8}, {0, 1}}, 2});
    testCases.add(new Object[]{new int[][]{{3, 3}, {5, -1}, {-2, 4}, {0, 0}, {1, 2}}, 3});
    testCases.add(new Object[]{new int[][]{{1, 1}, {2, 2}, {3, 3}, {4, 4}, {5, 5}}, 2});
    testCases.add(new Object[]{new int[][]{{1, 2}, {3, 4}, {0, 0}, {1, 1}, {2, 3}}, 3});
    testCases.add(new Object[]{new int[][]{{5, 6}, {-1, 2}, {0, 0}, {2, 3}}, 2});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(6) + 3; // [3,8]
        int[][] points = new int[n][2];
        for (int j = 0; j < n; j++) {
            points[j][0] = random.nextInt(21) - 10; // [-10,10]
            points[j][1] = random.nextInt(21) - 10;
        }
        int k = random.nextInt(n) + 1; // [1,n]
        testCases.add(new Object[]{points, k});
    }

    // Verify
    for (Object[] tc : testCases) {
        int[][] points = (int[][]) tc[0];
        int k = (int) tc[1];

        int[][] result = solution(points, k);
        int[][] expected = correctSolution(points, k);

        List<String> resultList = new ArrayList<>();
        for (int[] p : result) {
            resultList.add(p[0] + "," + p[1]);
        }

        List<String> expectedList = new ArrayList<>();
        for (int[] p : expected) {
            expectedList.add(p[0] + "," + p[1]);
        }

        Collections.sort(resultList);
        Collections.sort(expectedList);

        if (!resultList.equals(expectedList)) {
            throw new AssertionError(
                "Test case failed: points=" + Arrays.deepToString(points) +
                ", k=" + k +
                ", expected=" + expectedList +
                ", got=" + resultList
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static int[][] correctSolution(int[][] points, int k) {
    // Max-heap based on distance (use negative distance or reverse comparator)
    PriorityQueue<int[]> maxHeap = new PriorityQueue<>(
        (a, b) -> Integer.compare(b[0], a[0]) // compare by distance descending
    );

    for (int[] point : points) {
        int x = point[0];
        int y = point[1];
        int distance = x * x + y * y;

        // store {distance, x, y}
        maxHeap.offer(new int[]{distance, x, y});

        if (maxHeap.size() > k) {
            maxHeap.poll();
        }
    }

    int[][] result = new int[maxHeap.size()][2];
    int i = 0;
    for (int[] item : maxHeap) {
        result[i][0] = item[1];
        result[i][1] = item[2];
        i++;
    }

    return result;
}
```
