# Maximum Sum Less Than K

_Score_: 1

Find the **maximum sum** of any two **distinct** elements from an array such that the sum is **less than** k. Return -1 if no such pair exists.

### Requirements
- Must choose exactly 2 different elements
- Elements must be at different indices (even if values are same)
- Sum must be **strictly less than** k (not equal)
- Maximize the sum among all valid pairs

### Brute Force Approach
1. Try all pairs (i, j) where i < j
2. Calculate sum = nums[i] + nums[j]
3. If sum < k, track maximum
4. Return maximum found, or -1 if none

### Examples
`nums=[34,23,1,24,75,33,54,8]`, `k=60`
- Many pairs possible
- 34+23=57 < 60 ✓
- 33+24=57 < 60 ✓
- Best pair: 33+23=56 or 34+24=58... 
- Maximum: **58**

`nums=[10,20,30]`, `k=15`
- 10+20=30 ≥ 15 ✗
- All pairs exceed k
- Result: **-1**

### Input Format
- `nums`: Array of integers
- `k`: Upper bound (exclusive)

### Output Format
- Integer: maximum sum less than k, or -1

## Placeholder:
```java

public static int solution(int[] nums, int k) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: nums=[34, 23, 1, 24, 75, 33, 54, 8], k=60
Output: 58

Input: nums=[10, 20, 30], k=15
Output: -1

Input: nums=[5, 5, 5], k=10
Output: -1

Input: nums=[1, 1, 1, 1], k=3
Output: 2

Input: nums=[1, 10, 5, 2], k=8
Output: 7
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<Object[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new Object[]{new int[]{34, 23, 1, 24, 75, 33, 54, 8}, 60});
    testCases.add(new Object[]{new int[]{10, 20, 30}, 15});
    testCases.add(new Object[]{new int[]{5, 5, 5}, 10});
    testCases.add(new Object[]{new int[]{2, 2, 2}, 3});
    testCases.add(new Object[]{new int[]{-1, 0, 1}, 0});
    testCases.add(new Object[]{new int[]{1, 1, 1, 1}, 3});
    testCases.add(new Object[]{new int[]{1, 2, 3, 4, 5}, 10});
    testCases.add(new Object[]{new int[]{50, 25, 75, 100}, 120});
    testCases.add(new Object[]{new int[]{1, 10, 5, 2}, 8});
    testCases.add(new Object[]{new int[]{7, 8, 9}, 10});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(9) + 2; // [2,10]
        int[] nums = new int[n];
        for (int j = 0; j < n; j++) {
            nums[j] = random.nextInt(61) - 10; // [-10,50]
        }
        int k = random.nextInt(101); // [0,100]
        testCases.add(new Object[]{nums, k});
    }

    // Verify
    for (Object[] tc : testCases) {
        int[] nums = (int[]) tc[0];
        int k = (int) tc[1];

        int result = solution(nums, k);
        int expected = correctSolution(nums, k);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: nums=" + Arrays.toString(nums) +
                ", k=" + k +
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
public static int correctSolution(int[] nums, int k) {
    int maxSum = -1;
    int n = nums.length;

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int currentSum = nums[i] + nums[j];
            if (currentSum < k) {
                maxSum = Math.max(maxSum, currentSum);
            }
        }
    }

    return maxSum;
}
```
