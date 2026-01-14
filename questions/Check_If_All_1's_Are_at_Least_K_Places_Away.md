# Check If All 1's Are at Least K Places Away

_Score_: 1

Given a **binary array** (containing only 0s and 1s) and an integer `k`, determine if **all 1's** are at least `k` places away from each other.

### Distance Requirement
The **distance** between two indices i and j is `|i - j|`.

For all pairs of 1's at consecutive positions i and j (where i < j):
- The number of 0s between them must be **>= k**
- This is equivalent to an index difference of `j - i >= k + 1`

### Special Cases
- If there are **0 or 1** occurrences of '1', return `True`
- If k = 0, any adjacent 1's are allowed

### Example
`nums = [1,0,0,0,1,0,0,1], k = 2`
- 1's are at positions: 0, 4, 7
- Distance between position 0 and 4: 4 - 0 = 4 > 2 ✓
- Distance between position 4 and 7: 7 - 4 = 3 > 2 ✓
- Output: **True**

### Input Format
- `nums`: Binary array (list of 0s and 1s)
- `k`: Minimum distance required

### Output Format
- Boolean: `True` if all 1's satisfy the distance requirement, `False` otherwise

## Placeholder:
```java

public static boolean solution(int[] nums, int k) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: nums=[1, 0, 0, 0, 1, 0, 0, 1], k=2
Output: True

Input: nums=[1, 0, 0, 1, 0, 0, 0, 1], k=2
Output: True

Input: nums=[1, 1, 1, 1, 1], k=0
Output: True

Input: nums=[0, 1, 0, 1], k=1
Output: True

Input: nums=[1], k=5
Output: True
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> numsList = new ArrayList<>();
    List<Integer> kList = new ArrayList<>();

    // Fixed test cases
    numsList.add(new int[]{1,0,0,0,1,0,0,1}); kList.add(2);
    numsList.add(new int[]{1,0,0,1,0,0,0,1}); kList.add(2);
    numsList.add(new int[]{1,1,1,1,1});       kList.add(0);
    numsList.add(new int[]{0,1,0,1});         kList.add(1);
    numsList.add(new int[]{1,0,0,0});         kList.add(3);
    numsList.add(new int[]{0,0,0,0});         kList.add(3);
    numsList.add(new int[]{1});               kList.add(5);
    numsList.add(new int[]{0,0,1});           kList.add(2);
    numsList.add(new int[]{1,0,1});           kList.add(2);
    numsList.add(new int[]{1,0,0,0,0,0,0,0,1}); kList.add(3);

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(15) + 1;
        int[] nums = new int[n];
        for (int j = 0; j < n; j++) {
            nums[j] = random.nextBoolean() ? 1 : 0;
        }
        int k = random.nextInt(6);
        numsList.add(nums);
        kList.add(k);
    }

    // Verify
    for (int i = 0; i < numsList.size(); i++) {
        int[] nums = numsList.get(i);
        int k = kList.get(i);

        boolean result = solution(nums, k);
        boolean expected = correctSolution(nums, k);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: nums=" + Arrays.toString(nums) + ", k=" + k
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static boolean correctSolution(int[] nums, int k) {
    int previousOne = Integer.MIN_VALUE;

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 1) {
            if (i - previousOne <= k) {
                return false;
            }
            previousOne = i;
        }
    }
    return true;
}
```
