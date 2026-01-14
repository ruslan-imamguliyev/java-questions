# Valid Mountain Array

_Score_: 2

Determine if an array represents a **valid mountain array**.

### Mountain Array Definition
A mountain array must satisfy ALL of these conditions:
1. Length ≥ 3 elements
2. **Strictly increases** to a peak (no plateaus)
3. **Strictly decreases** after the peak (no plateaus)
4. Peak cannot be at the **first** or **last** position

### Algorithm
1. Walk up: move right while elements are strictly increasing
2. Check: peak must not be at start (index 0) or end (index n-1)
3. Walk down: move right while elements are strictly decreasing
4. Valid if we reach the end

### Examples
- `[0, 3, 2, 1]` → Peak at index 1 → **Valid** ✓
- `[0, 1, 2, 3, 2, 1, 0]` → Peak at index 3 → **Valid** ✓
- `[3, 5, 5]` → Plateau (not strictly increasing) → **Invalid** ✗
- `[0, 1, 2, 3]` → Only ascending → **Invalid** ✗
- `[9, 8, 7]` → Only descending → **Invalid** ✗

### Input Format
- An array of integers

### Output Format
- Boolean: `True` if valid mountain array, `False` otherwise

## Placeholder:
```java

public static boolean solution(int[] arr) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: arr=[2, 1]
Output: False

Input: arr=[3, 5, 5]
Output: False

Input: arr=[0, 3, 2, 1]
Output: True

Input: arr=[0, 1, 2, 3, 2, 1, 0]
Output: True

Input: arr=[1, 3, 2]
Output: True
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{2, 1});
    testCases.add(new int[]{3, 5, 5});
    testCases.add(new int[]{0, 3, 2, 1});
    testCases.add(new int[]{0, 1, 2, 3, 4, 5, 6, 7, 8, 9});
    testCases.add(new int[]{9, 8, 7, 6, 5, 4, 3, 2, 1, 0});
    testCases.add(new int[]{0, 1, 2, 3, 2, 1, 0});
    testCases.add(new int[]{0, 2, 3, 4, 5, 2, 1, 0});
    testCases.add(new int[]{0, 1, 2, 3, 4, 3, 2, 1});
    testCases.add(new int[]{1, 2, 3, 4, 5, 4, 3, 2, 1});
    testCases.add(new int[]{1, 3, 2});

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int n = random.nextInt(9) + 2; // [2,10]
        int[] arr;

        if (random.nextDouble() > 0.5 && n >= 3) {
            int peak = random.nextInt(n - 2) + 1; // [1, n-2]
            List<Integer> list = new ArrayList<>();

            for (int i = 0; i <= peak; i++) {
                list.add(random.nextInt(30) + 1);
            }
            Collections.sort(list);

            List<Integer> down = new ArrayList<>();
            for (int i = 0; i < n - peak - 1; i++) {
                down.add(random.nextInt(30) + 1);
            }
            down.sort(Collections.reverseOrder());

            list.addAll(down);
            arr = list.stream().mapToInt(Integer::intValue).toArray();
        } else {
            arr = new int[n];
            for (int i = 0; i < n; i++) {
                arr[i] = random.nextInt(30) + 1;
            }
        }

        testCases.add(arr);
    }

    // Verify
    for (int[] arr : testCases) {
        boolean result = solution(arr);
        boolean expected = correctSolution(arr);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: arr=" + Arrays.toString(arr) +
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
public static boolean correctSolution(int[] arr) {
    int n = arr.length;
    if (n < 3) {
        return false;
    }

    int i = 0;

    // Walk up
    while (i + 1 < n && arr[i] < arr[i + 1]) {
        i++;
    }

    // Peak can't be the first or the last element
    if (i == 0 || i == n - 1) {
        return false;
    }

    // Walk down
    while (i + 1 < n && arr[i] > arr[i + 1]) {
        i++;
    }

    return i == n - 1;
}
```
