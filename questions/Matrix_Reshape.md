# Matrix Reshape

_Score_: 2

**Reshape** a matrix from dimensions m×n to r×c while preserving the order of elements. If the reshape is impossible, return the original matrix.

### Reshape Conditions
- Reshaping is only possible if: `m × n = r × c`
- Elements must maintain their **row-major order** (left-to-right, top-to-bottom)

### Algorithm
1. Check if reshape is possible: total elements must match
2. Flatten the matrix into a 1D array (row by row)
3. Rebuild as r×c matrix by taking c elements at a time

### Examples
`mat=[[1,2],[3,4]]`, `r=1, c=4`
- Original: 2×2 matrix (4 elements)
- Target: 1×4 matrix (4 elements) ✓
- Flatten: [1,2,3,4]
- Reshape: `[[1,2,3,4]]`

`mat=[[1,2],[3,4]]`, `r=2, c=4`
- Original: 2×2 (4 elements)
- Target: 2×4 (8 elements) ✗
- Impossible! Return original: `[[1,2],[3,4]]`

### Input Format
- `mat`: 2D matrix (m×n)
- `r`: Target number of rows
- `c`: Target number of columns

### Output Format
- 2D matrix: reshaped matrix (r×c) or original if impossible

## Placeholder:
```java

public static int[][] solution(int[][] mat, int r, int c) {
    // Write your code here
    return new int[0][0];
}
```

## Examples:
```
Input: mat=[[1, 2], [3, 4]], r=1, c=4
Output: [[1, 2, 3, 4]]

Input: mat=[[1, 2], [3, 4]], r=2, c=4
Output: [[1, 2], [3, 4]]

Input: mat=[[1, 2, 3], [4, 5, 6]], r=2, c=3
Output: [[1, 2, 3], [4, 5, 6]]

Input: mat=[[1, 2, 3], [4, 5, 6]], r=3, c=2
Output: [[1, 2], [3, 4], [5, 6]]

Input: mat=[[1]], r=1, c=1
Output: [[1]]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    List<int[][]> mats = new ArrayList<>();
    List<Integer> rs = new ArrayList<>();
    List<Integer> cs = new ArrayList<>();

    // Fixed test cases
    mats.add(new int[][]{{1, 2}, {3, 4}}); rs.add(1); cs.add(4);
    mats.add(new int[][]{{1, 2}, {3, 4}}); rs.add(2); cs.add(4);
    mats.add(new int[][]{{1, 2, 3}, {4, 5, 6}}); rs.add(2); cs.add(3);
    mats.add(new int[][]{{1, 2, 3}, {4, 5, 6}}); rs.add(3); cs.add(2);
    mats.add(new int[][]{{1}}); rs.add(1); cs.add(1);

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int rows = random.nextInt(4) + 1;
        int cols = random.nextInt(4) + 1;

        int[][] mat = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                mat[i][j] = random.nextInt(20) + 1;
            }
        }

        int newR = random.nextInt(rows * cols) + 1;
        int newC = random.nextInt(rows * cols) + 1;

        mats.add(mat);
        rs.add(newR);
        cs.add(newC);
    }

    // Verify
    for (int i = 0; i < mats.size(); i++) {
        int[][] mat = mats.get(i);
        int r = rs.get(i);
        int c = cs.get(i);

        int[][] result = solution(copyMatrix(mat), r, c);
        int[][] expected = correctSolution(copyMatrix(mat), r, c);

        if (!Arrays.deepEquals(result, expected)) {
            throw new AssertionError(
                "Test case failed: mat=" + Arrays.deepToString(mat) +
                ", r=" + r +
                ", c=" + c +
                ", expected=" + Arrays.deepToString(expected) +
                ", got=" + Arrays.deepToString(result)
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}

private static int[][] copyMatrix(int[][] mat) {
    int[][] copy = new int[mat.length][];
    for (int i = 0; i < mat.length; i++) {
        copy[i] = Arrays.copyOf(mat[i], mat[i].length);
    }
    return copy;
}
```

## Solution:
```java
public static int[][] correctSolution(int[][] mat, int r, int c) {
    int m = mat.length;
    int n = mat[0].length;

    // Check if reshape is possible
    if (m * n != r * c) {
        return mat;
    }

    // Flatten the matrix
    int[] flat = new int[m * n];
    int idx = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            flat[idx++] = mat[i][j];
        }
    }

    // Reshape the matrix
    int[][] reshaped = new int[r][c];
    idx = 0;
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            reshaped[i][j] = flat[idx++];
        }
    }

    return reshaped;
}
```
