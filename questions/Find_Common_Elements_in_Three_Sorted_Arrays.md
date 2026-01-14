# Find Common Elements in Three Sorted Arrays

_Score_: 2

Find all **common elements** that appear in **all three** arrays, which are sorted in **strictly increasing order**. Return the result in sorted order.

### Three-Pointer Approach
Since all arrays are already sorted, use three pointers to efficiently find common elements:
1. Start with a pointer at the beginning of each array.
2. If all three elements are equal → add to the result and advance all pointers.
3. If elements differ → advance the pointer pointing to the smallest element.
4. Continue until any pointer reaches the end of its array.

### Algorithm Complexity
- **Time**: O(n1 + n2 + n3), where n1, n2, and n3 are the lengths of the arrays.
- **Space**: O(1) excluding the output array.

### Examples
- `arr1=[1,2,3,4,5]`, `arr2=[1,2,5,7,9]`, `arr3=[1,3,4,5,8]`
  - Common: `[1, 5]`

- `arr1=[197,418,523,876,1356]`, `arr2=[501,880,1593,1710,1870]`, `arr3=[521,682,1337,1395,1764]`
  - Common: `[]` (no common elements)

### Input Format
- Three sorted integer arrays: `arr1`, `arr2`, `arr3`

### Output Format
- A sorted list of common elements (may be empty)

## Placeholder:
```java

public static int[] solution(int[] arr1, int[] arr2, int[] arr3) {
    // Write your code here
    return new int[0];
}
```

## Examples:
```
Input: arr1=[1, 2, 3, 4, 5], arr2=[1, 2, 5, 7, 9], arr3=[1, 3, 4, 5, 8]
Output: [1, 5]

Input: arr1=[1, 5, 10, 20, 40, 80], arr2=[6, 7, 20, 80, 100], arr3=[3, 4, 15, 20, 30, 70, 80, 120]
Output: [20, 80]

Input: arr1=[1, 2, 3], arr2=[4, 5, 6], arr3=[7, 8, 9]
Output: []

Input: arr1=[1, 2, 3], arr2=[1, 2, 3], arr3=[1, 2, 3]
Output: [1, 2, 3]

Input: arr1=[1], arr2=[1], arr3=[1]
Output: [1]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[][]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[][]{{1,2,3,4,5}, {1,2,5,7,9}, {1,3,4,5,8}});
    testCases.add(new int[][]{{1,5,10,20,40,80}, {6,7,20,80,100}, {3,4,15,20,30,70,80,120}});
    testCases.add(new int[][]{{1,2,3}, {4,5,6}, {7,8,9}});
    testCases.add(new int[][]{{1,2,3}, {1,2,3}, {1,2,3}});
    testCases.add(new int[][]{{1}, {1}, {1}});
    testCases.add(new int[][]{{1,2}, {1}, {1,2}});
    testCases.add(new int[][]{{}, {}, {}});
    testCases.add(new int[][]{{1,1,2}, {1,2,2}, {1,2}});
    testCases.add(new int[][]{{1,2,3,4,5}, {2,4}, {2,4,6}});
    testCases.add(new int[][]{{1,2,3}, {1,2,3,4}, {1,2}});

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int commonSize = random.nextInt(4); // [0,3]
        int[] common = new int[commonSize];
        for (int i = 0; i < commonSize; i++) {
            common[i] = random.nextInt(30) + 1;
        }
        Arrays.sort(common);

        int[] arr1 = buildArray(common, random, 40);
        int[] arr2 = buildArray(common, random, 40);
        int[] arr3 = buildArray(common, random, 40);

        testCases.add(new int[][]{arr1, arr2, arr3});
    }

    // Verify
    for (int[][] tc : testCases) {
        int[] arr1 = tc[0];
        int[] arr2 = tc[1];
        int[] arr3 = tc[2];

        int[] result = solution(arr1, arr2, arr3);
        int[] expected = correctSolution(arr1, arr2, arr3);

        if (!Arrays.equals(result, expected)) {
            throw new AssertionError(
                "Test case failed: arr1=" + Arrays.toString(arr1) +
                ", arr2=" + Arrays.toString(arr2) +
                ", arr3=" + Arrays.toString(arr3)
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}

private static int[] buildArray(int[] common, Random random, int maxVal) {
    int extraSize = random.nextInt(6); // [0,5]
    int[] arr = new int[common.length + extraSize];
    System.arraycopy(common, 0, arr, 0, common.length);
    for (int i = common.length; i < arr.length; i++) {
        arr[i] = random.nextInt(maxVal) + 1;
    }
    Arrays.sort(arr);
    return arr;
}
```

## Solution:
```java
public static int[] correctSolution(int[] arr1, int[] arr2, int[] arr3) {
    int ptr1 = 0, ptr2 = 0, ptr3 = 0;
    List<Integer> result = new ArrayList<>();

    while (ptr1 < arr1.length && ptr2 < arr2.length && ptr3 < arr3.length) {
        if (arr1[ptr1] == arr2[ptr2] && arr2[ptr2] == arr3[ptr3]) {
            result.add(arr1[ptr1]);
            ptr1++;
            ptr2++;
            ptr3++;
        } else if (arr1[ptr1] < arr2[ptr2]) {
            ptr1++;
        } else if (arr2[ptr2] < arr3[ptr3]) {
            ptr2++;
        } else {
            ptr3++;
        }
    }

    int[] res = new int[result.size()];
    for (int i = 0; i < result.size(); i++) {
        res[i] = result.get(i);
    }
    return res;
}
```
