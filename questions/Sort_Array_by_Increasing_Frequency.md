# Sort Array by Increasing Frequency

_Score_: 3

Sort an array by **frequency** (ascending), with a **tie-breaker** by value (descending).

### Sorting Rules
**Primary**: Sort by frequency (least frequent first)
**Secondary**: If frequencies are equal, sort by value (largest first)

### Two-Level Sort
1. Count frequency of each number
2. Sort by: `(frequency_ascending, value_descending)`

### Examples
`[1,1,2,2,2,3]`
- Frequencies: 1→2, 2→3, 3→1
- Sort by freq: 3(1), 1(2), 2(3)
- Result: **[3, 1, 1, 2, 2, 2]**

`[2,3,1,3,2]`
- Frequencies: 1→1, 2→2, 3→2
- Sort by freq: 1(1), then 2 vs 3 (both freq 2, 3>2)
- Result: **[1, 3, 3, 2, 2]**

`[-1,1,-6,4,5,-6,1,4,1]`
- Frequencies: -1→1, 5→1, 1→3, 4→2, -6→2
- Sort: 5(1), -1(1), then 4(2), -6(2), then 1(3)
- With tie-break: **[5, -1, 4, 4, -6, -6, 1, 1, 1]**

### Input Format
- Array of integers

### Output Format
- Sorted array by frequency then value

## Placeholder:
```java

public static int[] solution(int[] nums) {
    // Write your code here
    return new int[0];
}
```

## Examples:
```
Input: nums=[1, 1, 2, 2, 2, 3]
Output: [3, 1, 1, 2, 2, 2]

Input: nums=[2, 3, 1, 3, 2]
Output: [1, 3, 3, 2, 2]

Input: nums=[-1, 1, -6, 4, 5, -6, 1, 4, 1]
Output: [5, -1, 4, 4, -6, -6, 1, 1, 1]

Input: nums=[5, 5, 5, 2, 2, 8]
Output: [8, 2, 2, 5, 5, 5]

Input: nums=[1, 2, 3, 4, 5]
Output: [5, 4, 3, 2, 1]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{1, 1, 2, 2, 2, 3});
    testCases.add(new int[]{2, 3, 1, 3, 2});
    testCases.add(new int[]{-1, 1, -6, 4, 5, -6, 1, 4, 1});
    testCases.add(new int[]{5, 5, 5, 2, 2, 8});
    testCases.add(new int[]{7, 7, 7, 2, 2, 2, 9});
    testCases.add(new int[]{6, 6, 6, 1, 1, 1, 2, 2});
    testCases.add(new int[]{1, 2, 3, 4, 5});
    testCases.add(new int[]{5, 4, 3, 2, 1});
    testCases.add(new int[]{1, 1, 1, 1, 1});
    testCases.add(new int[]{1});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int length = random.nextInt(10) + 3; // [3,12]
        int[] nums = new int[length];
        for (int j = 0; j < length; j++) {
            nums[j] = random.nextInt(21) - 10; // [-10,10]
        }
        testCases.add(nums);
    }

    // Verify
    for (int[] nums : testCases) {
        int[] copy1 = Arrays.copyOf(nums, nums.length);
        int[] copy2 = Arrays.copyOf(nums, nums.length);

        int[] result = solution(copy1);
        int[] expected = correctSolution(copy2);

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
    // Frequency map
    Map<Integer, Integer> frequency = new HashMap<>();
    for (int num : nums) {
        frequency.put(num, frequency.getOrDefault(num, 0) + 1);
    }

    // Convert to list of entries
    List<Map.Entry<Integer, Integer>> uniqueNums = new ArrayList<>(frequency.entrySet());

    // Custom sort:
    // 1) increasing frequency
    // 2) if same frequency, decreasing number
    uniqueNums.sort((e1, e2) -> {
        int freqCompare = Integer.compare(e1.getValue(), e2.getValue());
        if (freqCompare != 0) {
            return freqCompare;
        }
        return Integer.compare(e2.getKey(), e1.getKey());
    });

    // Build result array
    int[] result = new int[nums.length];
    int index = 0;
    for (Map.Entry<Integer, Integer> entry : uniqueNums) {
        int num = entry.getKey();
        int freq = entry.getValue();
        for (int i = 0; i < freq; i++) {
            result[index++] = num;
        }
    }

    return result;
}
```
