# Flood Fill

_Score_: 3

Given an image represented by a 2D array of pixels, a starting pixel (sr, sc), and a new color, perform a flood fill. A flood fill changes the color of the starting pixel and all adjacent pixels of the same color to the new color (4-directionally: up, down, left, right).

## Placeholder:
```java

public static int[][] solution(int[][] image, int sr, int sc, int color) {
    // Write your code here
    return new int[0][0];
}
```

## Examples:
```
Input: image=[[1, 1, 1], [1, 1, 0], [1, 0, 1]], sr=1, sc=1, color=2
Output: [[2, 2, 2], [2, 2, 0], [2, 0, 1]]

Input: image=[[0, 0, 0], [0, 0, 0]], sr=0, sc=0, color=0
Output: [[0, 0, 0], [0, 0, 0]]

Input: image=[[0, 0, 0], [0, 1, 0]], sr=1, sc=1, color=2
Output: [[0, 0, 0], [0, 2, 0]]

Input: image=[[1]], sr=0, sc=0, color=5
Output: [[5]]

Input: image=[[1, 1], [1, 1]], sr=0, sc=0, color=2
Output: [[2, 2], [2, 2]]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<Object[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new Object[]{new int[][]{{1,1,1},{1,1,0},{1,0,1}}, 1, 1, 2});
    testCases.add(new Object[]{new int[][]{{0,0,0},{0,0,0}}, 0, 0, 0});
    testCases.add(new Object[]{new int[][]{{0,0,0},{0,1,0}}, 1, 1, 2});
    testCases.add(new Object[]{new int[][]{{7,7,7},{7,7,7},{7,7,7}}, 0, 0, 7});
    testCases.add(new Object[]{new int[][]{{1,0,0},{0,0,0},{0,0,1}}, 1, 1, 5});
    testCases.add(new Object[]{new int[][]{{1,2,3},{4,5,6},{7,8,9}}, 0, 0, 10});
    testCases.add(new Object[]{new int[][]{{1}}, 0, 0, 5});
    testCases.add(new Object[]{new int[][]{{1,1},{1,1}}, 0, 0, 2});
    testCases.add(new Object[]{new int[][]{{1,0},{0,1}}, 0, 0, 0});
    testCases.add(new Object[]{new int[][]{{1,1,1},{1,0,1},{1,1,1}}, 1, 1, 1});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int rows = random.nextInt(5) + 1;
        int cols = random.nextInt(5) + 1;
        int[][] image = new int[rows][cols];
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                image[r][c] = random.nextInt(4);
            }
        }
        int sr = random.nextInt(rows);
        int sc = random.nextInt(cols);
        int color = random.nextInt(6);

        testCases.add(new Object[]{image, sr, sc, color});
    }

    // Verify
    for (Object[] tc : testCases) {
        int[][] image = (int[][]) tc[0];
        int sr = (int) tc[1];
        int sc = (int) tc[2];
        int color = (int) tc[3];

        int[][] copy1 = deepCopy(image);
        int[][] copy2 = deepCopy(image);

        int[][] result = solution(copy1, sr, sc, color);
        int[][] expected = correctSolution(copy2, sr, sc, color);

        if (!Arrays.deepEquals(result, expected)) {
            throw new AssertionError(
                "Test case failed: image=" + Arrays.deepToString(image) +
                ", sr=" + sr + ", sc=" + sc + ", color=" + color +
                ", expected=" + Arrays.deepToString(expected) +
                ", got=" + Arrays.deepToString(result)
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}

private static int[][] deepCopy(int[][] image) {
    int[][] copy = new int[image.length][];
    for (int i = 0; i < image.length; i++) {
        copy[i] = Arrays.copyOf(image[i], image[i].length);
    }
    return copy;
}
```

## Solution:
```java
public static int[][] correctSolution(int[][] image, int sr, int sc, int color) {
    int rows = image.length;
    int cols = image[0].length;
    int originalColor = image[sr][sc];

    if (originalColor == color) {
        return image;
    }

    fill(image, sr, sc, originalColor, color, rows, cols);
    return image;
}

private static void fill(int[][] image, int r, int c, int originalColor, int color, int rows, int cols) {
    if (r < 0 || r >= rows || c < 0 || c >= cols || image[r][c] != originalColor) {
        return;
    }

    image[r][c] = color;

    fill(image, r + 1, c, originalColor, color, rows, cols);
    fill(image, r - 1, c, originalColor, color, rows, cols);
    fill(image, r, c + 1, originalColor, color, rows, cols);
    fill(image, r, c - 1, originalColor, color, rows, cols);
}
```
