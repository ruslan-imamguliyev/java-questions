# Cells with Odd Values in Matrix

_Score_: 1

Start with an **m × n matrix of zeros**. Apply a series of increment operations specified by indices. For each `[r, c]` in indices, increment **all cells in row r** and **all cells in column c** by 1. Return the count of cells with **odd values** after all operations.

### Operations
For each `[r, c]` pair:
- Increment every cell in row `r` by 1
- Increment every cell in column `c` by 1

### Efficient Approach
Instead of maintaining full matrix:
1. Track increments per row: `row_count[r]`
2. Track increments per column: `col_count[c]`
3. Cell `[i, j]` has value: `row_count[i] + col_count[j]`
4. Count cells where this sum is odd

### Example
`m=2, n=3, indices=[[0,1],[1,1]]`
- Initial: 0 0 0 / 0 0 0
- After [0,1]: row 0 +1, col 1 +1 → 1 2 1 / 0 1 0
- After [1,1]: row 1 +1, col 1 +1 → 1 3 1 / 1 2 1
- Odd cells: positions (0,0), (0,2), (1,0), (1,2), (0,1), (1,1) = **6**

### Input Format
- `m`: Number of rows
- `n`: Number of columns
- `indices`: List of [row, column] pairs

### Output Format
- Integer: count of cells with odd values

## Placeholder:
```java

public static int solution(int m, int n, int[][] indices) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: m=2, n=3, indices=[[0, 1], [1, 1]]
Output: 6

Input: m=2, n=2, indices=[[1, 1], [0, 0]]
Output: 0

Input: m=1, n=1, indices=[[0, 0]]
Output: 0

Input: m=3, n=3, indices=[[1, 2], [0, 1], [2, 0]]
Output: 0

Input: m=2, n=4, indices=[[0, 3], [1, 0]]
Output: 4
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    List<Integer> ms = new ArrayList<>();
    List<Integer> ns = new ArrayList<>();
    List<int[][]> indicesList = new ArrayList<>();

    // Fixed test cases
    ms.add(2); ns.add(3); indicesList.add(new int[][]{{0,1},{1,1}});
    ms.add(2); ns.add(2); indicesList.add(new int[][]{{1,1},{0,0}});
    ms.add(1); ns.add(1); indicesList.add(new int[][]{{0,0}});
    ms.add(3); ns.add(3); indicesList.add(new int[][]{{1,2},{0,1},{2,0}});
    ms.add(4); ns.add(5); indicesList.add(new int[][]{{0,0},{1,2},{2,4},{3,1}});
    ms.add(5); ns.add(3); indicesList.add(new int[][]{{0,2},{4,0},{1,1},{3,2},{2,0}});
    ms.add(2); ns.add(4); indicesList.add(new int[][]{{0,3},{1,0}});
    ms.add(3); ns.add(2); indicesList.add(new int[][]{{0,0},{0,1},{1,0},{1,1},{2,0},{2,1}});
    ms.add(1); ns.add(5); indicesList.add(new int[][]{{0,0},{0,2},{0,4}});
    ms.add(5); ns.add(1); indicesList.add(new int[][]{{0,0},{2,0},{4,0}});

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int m = random.nextInt(5) + 1;
        int n = random.nextInt(5) + 1;
        int numIndices = random.nextInt(8) + 1;

        int[][] indices = new int[numIndices][2];
        for (int i = 0; i < numIndices; i++) {
            indices[i][0] = random.nextInt(m);
            indices[i][1] = random.nextInt(n);
        }

        ms.add(m);
        ns.add(n);
        indicesList.add(indices);
    }

    // Verify
    for (int i = 0; i < ms.size(); i++) {
        int m = ms.get(i);
        int n = ns.get(i);
        int[][] indices = indicesList.get(i);

        int result = solution(m, n, indices);
        int expected = correctSolution(m, n, indices);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: m=" + m +
                ", n=" + n +
                ", indices=" + Arrays.deepToString(indices) +
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
public static int correctSolution(int m, int n, int[][] indices) {
    int[] rows = new int[m];
    int[] cols = new int[n];

    for (int[] idx : indices) {
        int r = idx[0];
        int c = idx[1];
        rows[r]++;
        cols[c]++;
    }

    int oddCount = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if ((rows[i] + cols[j]) % 2 != 0) {
                oddCount++;
            }
        }
    }

    return oddCount;
}
```
