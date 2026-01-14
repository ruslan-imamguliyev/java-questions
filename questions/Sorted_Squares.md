# Sorted Squares

_Score_: 3

Given a **sorted** integer array, return an array of the **squares of each number**, also sorted. Must solve in **O(n)** time.

### Challenge
Input is sorted, but squares may not be:
- `[-4, -1, 0, 3, 10]` → squares: `[16, 1, 0, 9, 100]` (not sorted!)

### Two-Pointer Approach (O(n))
Since input is sorted, the largest squares are at the **ends** (either very negative or very positive numbers):

1. Use two pointers: `left=0`, `right=n-1`
2. Compare `nums[left]²` vs `nums[right]²`
3. Place larger square at end of result array
4. Move corresponding pointer inward
5. Fill result array from right to left

### Example
`[-4, -1, 0, 3, 10]`
- Compare: 16 vs 100 → place 100
- Compare: 16 vs 9 → place 16
- Compare: 1 vs 9 → place 9
- Compare: 1 vs 0 → place 1
- Place: 0
- Result: **[0, 1, 9, 16, 100]**

### Input Format
- Sorted array of integers (ascending)

### Output Format
- Sorted array of squares (ascending)

## Placeholder:
```java

public static int[] solution(int[] nums) {
    // Write your code here
    return new int[0];
}
```

## Examples:
```
Input: nums=[-4, -1, 0, 3, 10]
Output: [0, 1, 9, 16, 100]

Input: nums=[-7, -3, 2, 3, 11]
Output: [4, 9, 9, 49, 121]

Input: nums=[-1]
Output: [1]

Input: nums=[0]
Output: [0]

Input: nums=[1, 2, 3, 4, 5]
Output: [1, 4, 9, 16, 25]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{-4, -1, 0, 3, 10});
    testCases.add(new int[]{-7, -3, 2, 3, 11});
    testCases.add(new int[]{-1});
    testCases.add(new int[]{0});
    testCases.add(new int[]{5});
    testCases.add(new int[]{-5, -2, -1});
    testCases.add(new int[]{1, 2, 3, 4, 5});
    testCases.add(new int[]{-10, -9, -8, -7, -6});
    testCases.add(new int[]{-3, -3, -2, -1, 0});
    testCases.add(new int[]{0, 0, 3, 10, 11});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(12) + 1; // [1,12]
        int negCount = random.nextInt(n + 1);

        List<Integer> nums = new ArrayList<>();

        for (int j = 0; j < negCount; j++) {
            nums.add(-random.nextInt(20) - 1);
        }
        for (int j = 0; j < n - negCount; j++) {
            nums.add(random.nextInt(21));
        }

        Collections.sort(nums);

        int[] arr = nums.stream().mapToInt(Integer::intValue).toArray();
        testCases.add(arr);
    }

    // Verify
    for (int[] nums : testCases) {
        int[] result = solution(Arrays.copyOf(nums, nums.length));
        int[] expected = correctSolution(Arrays.copyOf(nums, nums.length));

        if (!Arrays.equals(result, expected)) {
            throw new AssertionError(
                "Test case failed: nums=" + Arrays.toString(nums) +
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
public static int[] correctSolution(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];

    int left = 0;
    int right = n - 1;
    int index = n - 1;

    while (left <= right) {
        int leftSquare = nums[left] * nums[left];
        int rightSquare = nums[right] * nums[right];

        if (leftSquare > rightSquare) {
            result[index] = leftSquare;
            left++;
        } else {
            result[index] = rightSquare;
            right--;
        }
        index--;
    }

    return result;
}
```
