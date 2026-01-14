# Migratory Birds

_Score_: 2

Track bird sightings and determine which bird type is spotted **most frequently**. If multiple types are tied for most frequent, return the one with the **smallest ID**.

### Task
- Count frequency of each bird type ID
- Find the type(s) with maximum frequency
- If tied, choose the smallest ID

### Tie-Breaking Rule
When multiple bird types have the same maximum frequency, always return the **smallest type ID**.

### Examples
`[1, 1, 2, 2, 3]`
- Frequencies: 1→2, 2→2, 3→1
- Tied at max (2): types 1 and 2
- Return: **1** (smaller ID)

`[1, 4, 4, 4, 5, 3]`
- Frequencies: 1→1, 3→1, 4→3, 5→1
- Maximum: type 4 with count 3
- Return: **4**

### Input Format
- Array of integers representing bird type IDs

### Output Format
- Integer: the bird type ID that appears most frequently (smallest if tied)

### Algorithm
1. Count frequency of each bird type
2. Find maximum frequency
3. Among types with max frequency, return smallest ID

## Placeholder:
```java

public static int solution(int[] arr) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: [1, 1, 2, 2, 3]
Output: 1

Input: [1, 4, 4, 4, 5, 3]
Output: 4

Input: [3, 3, 1, 2, 1, 2, 1]
Output: 1

Input: [5, 5, 5, 5]
Output: 5

Input: [1, 2, 3, 4, 5]
Output: 1
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{1, 1, 2, 2, 3});
    testCases.add(new int[]{1, 4, 4, 4, 5, 3});
    testCases.add(new int[]{3, 3, 1, 2, 1, 2, 1});
    testCases.add(new int[]{5, 5, 5, 5});
    testCases.add(new int[]{1, 2, 3, 4, 5});
    testCases.add(new int[]{2, 2, 2, 3, 3, 3, 1, 1});
    testCases.add(new int[]{7, 7, 7, 7, 2, 2, 2});
    testCases.add(new int[]{6, 6, 6, 5, 5, 5, 4, 4});
    testCases.add(new int[]{9, 9, 9, 9, 8, 8, 8, 8});
    testCases.add(new int[]{10, 20, 20, 10, 30, 30});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int size = random.nextInt(16) + 5; // [5,20]
        int[] arr = new int[size];
        for (int j = 0; j < size; j++) {
            arr[j] = random.nextInt(10) + 1;
        }
        testCases.add(arr);
    }

    // Verify
    for (int[] testCase : testCases) {
        int result = solution(testCase);
        int expected = correctSolution(testCase);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: " + Arrays.toString(testCase) +
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
public static int correctSolution(int[] arr) {
    Map<Integer, Integer> frequency = new HashMap<>();

    for (int birdId : arr) {
        frequency.put(birdId, frequency.getOrDefault(birdId, 0) + 1);
    }

    int maxCount = 0;
    int minId = Integer.MAX_VALUE;

    for (Map.Entry<Integer, Integer> entry : frequency.entrySet()) {
        int birdId = entry.getKey();
        int count = entry.getValue();

        if (count > maxCount) {
            maxCount = count;
            minId = birdId;
        } else if (count == maxCount && birdId < minId) {
            minId = birdId;
        }
    }

    return minId;
}
```
