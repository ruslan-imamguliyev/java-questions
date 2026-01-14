# Replace Elements with Greatest Element on Right

_Score_: 1

Replace every element in an array with the **greatest element among the elements to its right**, and replace the **last element with -1**.

### Task
For each position `i`:
- Replace `arr[i]` with `max(arr[i+1], arr[i+2], ..., arr[n-1])`
- The last element becomes `-1`

### Efficient Approach (Right to Left)
Traverse from right to left, tracking the maximum seen so far:
```python
greatest_right = -1
for i from n-1 down to 0:
    temp = arr[i]
    arr[i] = greatest_right
    greatest_right = max(greatest_right, temp)
```

### Examples
`[17, 18, 5, 4, 6, 1]`
- Index 0 (17): max of right = 18 → **18**
- Index 1 (18): max of right = 6 → **6**
- Index 2 (5): max of right = 6 → **6**
- Index 3 (4): max of right = 6 → **6**
- Index 4 (6): max of right = 1 → **1**
- Index 5 (1): last element → **-1**
- Result: **[18, 6, 6, 6, 1, -1]**

`[400]` → **[-1]**

### Input Format
- Array of integers

### Output Format
- Modified array with replacements

## Placeholder:
```java

public static int[] solution(int[] arr) {
    // Write your code here
    return new int[0];
}
```

## Examples:
```
Input: arr=[17, 18, 5, 4, 6, 1]
Output: [18, 6, 6, 6, 1, -1]

Input: arr=[400]
Output: [-1]

Input: arr=[5, 2, 8, 1, 9]
Output: [9, 9, 9, 9, -1]

Input: arr=[1, 2, 3, 4, 5]
Output: [5, 5, 5, 5, -1]

Input: arr=[5, 4, 3, 2, 1]
Output: [4, 3, 2, 1, -1]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{17, 18, 5, 4, 6, 1});
    testCases.add(new int[]{400});
    testCases.add(new int[]{5, 2, 8, 1, 9});
    testCases.add(new int[]{1, 2, 3, 4, 5});
    testCases.add(new int[]{5, 4, 3, 2, 1});
    testCases.add(new int[]{10});
    testCases.add(new int[]{1, 1, 1, 1});
    testCases.add(new int[]{100, 0, 50, 10, 75});
    testCases.add(new int[]{0, -1, -5, 10});
    testCases.add(new int[]{});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(11); // [0,10]
        int[] arr = new int[n];
        for (int j = 0; j < n; j++) {
            arr[j] = random.nextInt(61) - 10; // [-10,50]
        }
        testCases.add(arr);
    }

    // Verify
    for (int[] arr : testCases) {
        int[] copy1 = Arrays.copyOf(arr, arr.length);
        int[] copy2 = Arrays.copyOf(arr, arr.length);

        int[] result = solution(copy1);
        int[] expected = correctSolution(copy2);

        if (!Arrays.equals(result, expected)) {
            throw new AssertionError(
                "Test case failed: arr=" + Arrays.toString(arr) +
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
    int n = arr.length;
    if (n == 0) {
        return new int[0];
    }

    int greatestRight = -1;

    for (int i = n - 1; i >= 0; i--) {
        int currentElement = arr[i];
        arr[i] = greatestRight;
        if (currentElement > greatestRight) {
            greatestRight = currentElement;
        }
    }

    return arr;
}
```
