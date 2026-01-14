# Is Majority Element

_Score_: 3

## Problem Description

Given a **sorted** array `nums` of integers and a `target` value, determine if the `target` is a **majority element**.

A majority element is an element that appears **more than** `n/2` times in the array, where `n` is the length of the array.

Return `True` if `target` is a majority element, otherwise return `False`.

### Constraints:
- `1 <= nums.length <= 1000`
- `1 <= nums[i], target <= 10^9`
- `nums` is sorted in non-decreasing order

### Example 1:
**Input:** `nums = [2,4,5,5,5,5,5,6,6], target = 5`  
**Output:** `True`  
**Explanation:**  
- Array length n = 9
- n/2 = 4.5, so we need more than 4 occurrences
- Target 5 appears 5 times
- 5 > 4, so return True

### Example 2:
**Input:** `nums = [10,100,101,101], target = 101`  
**Output:** `False`  
**Explanation:**  
- Array length n = 4
- n/2 = 2, so we need more than 2 occurrences
- Target 101 appears 2 times
- 2 is not > 2, so return False

### Example 3:
**Input:** `nums = [2,4,5,5,5,5,5,6,6], target = 6`  
**Output:** `False`  
**Explanation:**  
- Array length n = 9
- n/2 = 4.5, so we need more than 4 occurrences
- Target 6 appears 2 times
- 2 is not > 4, so return False

### Example 4:
**Input:** `nums = [1,1,2,2,2,2], target = 2`  
**Output:** `True`  
**Explanation:**  
- Array length n = 6
- n/2 = 3, so we need more than 3 occurrences
- Target 2 appears 4 times
- 4 > 3, so return True

### Example 5:
**Input:** `nums = [1], target = 1`  
**Output:** `True`  
**Explanation:**  
- Array length n = 1
- n/2 = 0.5, so we need more than 0 occurrences
- Target 1 appears 1 time
- 1 > 0, so return True

### Note:
- The array is already sorted, which can help optimize the solution
- A majority element must appear **strictly more than** n/2 times (not equal to)
- Use integer division: n//2 for comparison
- If target doesn't exist in the array, return False

## Placeholder:
```java

public static boolean solution(int[] nums, int target) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: nums=[2, 4, 5, 5, 5, 5, 5, 6, 6], target=5
Output: True

Input: nums=[10, 100, 101, 101], target=101
Output: False

Input: nums=[2, 4, 5, 5, 5, 5, 5, 6, 6], target=6
Output: False

Input: nums=[1, 1, 2, 2, 2, 2], target=2
Output: True

Input: nums=[1], target=1
Output: True
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> numsList = new ArrayList<>();
    List<Integer> targets = new ArrayList<>();

    // Fixed test cases
    numsList.add(new int[]{2,4,5,5,5,5,5,6,6}); targets.add(5);
    numsList.add(new int[]{10,100,101,101});    targets.add(101);
    numsList.add(new int[]{2,4,5,5,5,5,5,6,6}); targets.add(6);
    numsList.add(new int[]{1,1,2,2,2,2});       targets.add(2);
    numsList.add(new int[]{1,2,3});             targets.add(2);
    numsList.add(new int[]{1});                 targets.add(1);
    numsList.add(new int[]{1,1});               targets.add(1);
    numsList.add(new int[]{1,2});               targets.add(1);
    numsList.add(new int[]{1,2});               targets.add(2);
    numsList.add(new int[]{3,3,4});             targets.add(3);

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(10) + 1;
        int target = random.nextInt(10) + 1;
        int[] nums;

        if (random.nextBoolean()) {
            // Make target a majority element
            int count = n / 2 + 1;
            nums = new int[n];
            for (int j = 0; j < count; j++) {
                nums[j] = target;
            }
            for (int j = count; j < n; j++) {
                nums[j] = random.nextInt(20) + 1;
            }
            Arrays.sort(nums);
        } else {
            nums = new int[n];
            for (int j = 0; j < n; j++) {
                nums[j] = random.nextInt(20) + 1;
            }
            Arrays.sort(nums);
        }

        numsList.add(nums);
        targets.add(target);
    }

    // Verify
    for (int i = 0; i < numsList.size(); i++) {
        int[] nums = numsList.get(i);
        int target = targets.get(i);

        boolean result = solution(nums, target);
        boolean expected = correctSolution(nums, target);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: nums=" + Arrays.toString(nums) +
                ", target=" + target +
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
public static boolean correctSolution(int[] nums, int target) {
    int n = nums.length;
    if (n == 0) {
        return false;
    }

    int firstOccurrence = -1;
    int low = 0, high = n - 1;

    // Find first occurrence
    while (low <= high) {
        int mid = (low + high) / 2;
        if (nums[mid] == target) {
            firstOccurrence = mid;
            high = mid - 1;
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    if (firstOccurrence == -1) {
        return false;
    }

    int lastOccurrence = -1;
    low = 0;
    high = n - 1;

    // Find last occurrence
    while (low <= high) {
        int mid = (low + high) / 2;
        if (nums[mid] == target) {
            lastOccurrence = mid;
            low = mid + 1;
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    int count = lastOccurrence - firstOccurrence + 1;
    return count > n / 2;
}
```
