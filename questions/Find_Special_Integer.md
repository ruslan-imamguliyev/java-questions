# Find Special Integer

_Score_: 1

## Problem Description

You are given an array `nums` of non-negative integers. `nums` is considered **special** if there exists a number `x` such that there are exactly `x` numbers in `nums` that are greater than or equal to `x`.

Notice that `x` does not have to be an element in `nums`.

Return `x` if the array is special, otherwise, return `-1`. It can be proven that if `nums` is special, the value for `x` is unique.

### Constraints:
- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

### Example 1:
**Input:** `nums = [3,5]`
**Output:** `2`
**Explanation:** There are 2 values (3 and 5) that are greater than or equal to 2.

### Example 2:
**Input:** `nums = [0,0]`
**Output:** `-1`
**Explanation:** No numbers fit the criteria for x.
- If x = 0, there should be 0 numbers >= x, but there are 2.
- If x = 1, there should be 1 number >= x, but there are 0.
- If x = 2, there should be 2 numbers >= x, but there are 0.

### Example 3:
**Input:** `nums = [0,4,3,0,4]`
**Output:** `3`
**Explanation:** There are 3 values that are greater than or equal to 3.

### Note:
- The special integer `x` must satisfy: count(numbers >= x) == x
- The valid range for `x` is from 0 to the length of the array.

## Placeholder:
```java

public static int solution(int[] nums) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: nums=[3, 5, 9]
Output: 3

Input: nums=[0, 0]
Output: -1

Input: nums=[0, 4, 3, 0, 4]
Output: 3

Input: nums=[3, 6, 7, 7, 0]
Output: -1

Input: nums=[1]
Output: 1
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{3, 5, 9});
    testCases.add(new int[]{0, 0});
    testCases.add(new int[]{0, 4, 3, 0, 4});
    testCases.add(new int[]{3, 6, 7, 7, 0});
    testCases.add(new int[]{1, 2, 3, 4, 5});
    testCases.add(new int[]{5, 5, 5, 5, 5});
    testCases.add(new int[]{1});
    testCases.add(new int[]{2});
    testCases.add(new int[]{0});
    testCases.add(new int[]{1, 1, 1});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(10) + 1; // [1,10]
        int[] nums = new int[n];
        for (int j = 0; j < n; j++) {
            nums[j] = random.nextInt(16); // [0,15]
        }
        testCases.add(nums);
    }

    // Verify
    for (int[] nums : testCases) {
        int result = solution(nums);
        int expected = correctSolution(nums);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: nums=" + Arrays.toString(nums) +
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
public static int correctSolution(int[] nums) {
    int n = nums.length;

    for (int x = 0; x <= n; x++) {
        int count = 0;
        for (int num : nums) {
            if (num >= x) {
                count++;
            }
        }
        if (count == x) {
            return x;
        }
    }

    return -1;
}
```
