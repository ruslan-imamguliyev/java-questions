# Closest Numbers

_Score_: 2

Given an array of integers, find **all pairs** of elements that have the **minimum absolute difference**.

### Task
1. Find the minimum absolute difference between any two elements
2. Return all pairs that achieve this minimum difference
3. Pairs should be in ascending order

### Algorithm
1. **Sort** the array
2. Find minimum difference by checking consecutive elements
3. Collect all pairs with this minimum difference

### Example
Input: `[1, 2, 4, 5, 3]`
- Sorted: [1, 2, 3, 4, 5]
- Differences: |2-1|=1, |3-2|=1, |4-3|=1, |5-4|=1
- Minimum difference: 1
- Pairs: [1,2], [2,3], [3,4], [4,5]
- Output: `[1, 2, 2, 3, 3, 4, 4, 5]`

### Input Format
- An array of unique integers

### Output Format
- A flat list of integers representing pairs (each pair as two consecutive elements)

## Placeholder:
```java

public static int[] solution(int[] arr) {
    // Write your code here
    return new int[0];
}
```

## Examples:
```
Input: [1, 2, 3, 4, 5]
Output: [1, 2, 2, 3, 3, 4, 4, 5]

Input: [10, 20, 30, 40, 50]
Output: [10, 20, 20, 30, 30, 40, 40, 50]

Input: [1, 2, 4, 5, 3]
Output: [1, 2, 2, 3, 3, 4, 4, 5]

Input: [5, 10, 3, 9]
Output: [9, 10]

Input: [1, 1, 1, 1, 1]
Output: [1, 1, 1, 1, 1, 1, 1, 1]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{1, 2, 3, 4, 5});
    testCases.add(new int[]{10, 20, 30, 40, 50});
    testCases.add(new int[]{1, 2, 4, 5, 3});
    testCases.add(new int[]{5, 10, 3, 9});
    testCases.add(new int[]{1, 2, 3, 5, 7, 8});
    testCases.add(new int[]{1, 1, 1, 1, 1});
    testCases.add(new int[]{4, 8, 1, 6, 7});
    testCases.add(new int[]{100, 1, 50, 51});
    testCases.add(new int[]{1, 1, 2, 2, 3});
    testCases.add(new int[]{9, 1, 3, 4, 8});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int size = random.nextInt(9) + 2; // [2,10]
        int[] arr = new int[size];
        for (int j = 0; j < size; j++) {
            arr[j] = random.nextInt(101) - 50; // [-50,50]
        }
        testCases.add(arr);
    }

    // Verify
    for (int[] testCase : testCases) {
        int[] copy1 = Arrays.copyOf(testCase, testCase.length);
        int[] copy2 = Arrays.copyOf(testCase, testCase.length);

        int[] result = solution(copy1);
        int[] expected = correctSolution(copy2);

        if (!Arrays.equals(result, expected)) {
            throw new AssertionError(
                "Test case failed: " + Arrays.toString(testCase) +
                ", expected=" + Arrays.toString(expected) +
                ", got=" + Arrays.toString(result)
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static int[] correctSolution(int[] arr) {
    if (arr == null || arr.length < 2) {
        return new int[0];
    }

    // Sort the array
    Arrays.sort(arr);

    int minDiff = Integer.MAX_VALUE;
    List<Integer> result = new ArrayList<>();

    // Find pairs with the smallest difference
    for (int i = 1; i < arr.length; i++) {
        int diff = arr[i] - arr[i - 1];

        if (diff < minDiff) {
            minDiff = diff;
            result.clear();
            result.add(arr[i - 1]);
            result.add(arr[i]);
        } else if (diff == minDiff) {
            result.add(arr[i - 1]);
            result.add(arr[i]);
        }
    }

    // Convert List<Integer> to int[]
    int[] res = new int[result.size()];
    for (int i = 0; i < result.size(); i++) {
        res[i] = result.get(i);
    }

    return res;
}
```
