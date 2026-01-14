# Kids With the Greatest Number of Candies

_Score_: 1

Each kid has some candies, and you have extra candies to distribute. For each kid, determine if they could have the **greatest number of candies** among all kids if you give them **all** the extra candies.

### Key Points
- Each kid is evaluated independently
- Each kid gets ALL extra candies (not split)
- Check if `kid's candies + extraCandies >= max candies among all kids`
- Note that multiple kids can have the greatest number of candies.

### Algorithm
1. Find the maximum number of candies any kid currently has
2. For each kid, check: `candies[i] + extraCandies >= max_candies`
3. Return boolean array

### Examples
`candies = [2, 3, 5, 1, 3]`, `extraCandies = 3`
- Max candies = 5
- Kid 0: 2+3=5 ≥ 5 → **True** ✓
- Kid 1: 3+3=6 ≥ 5 → **True** ✓
- Kid 2: 5+3=8 ≥ 5 → **True** ✓
- Kid 3: 1+3=4 < 5 → **False** ✗
- Kid 4: 3+3=6 ≥ 5 → **True** ✓
- Result: `[True, True, True, False, True]`

### Input Format
- `candies`: List of integers (candies each kid has)
- `extraCandies`: Integer (extra candies available)

### Output Format
- Boolean list indicating if each kid could have the most candies

## Placeholder:
```java

public static boolean[] solution(int[] candies, int extraCandies) {
    // Write your code here
    return new boolean[0];
}
```

## Examples:
```
Input: candies=[2, 3, 5, 1, 3], extraCandies=3
Output: [True, True, True, False, True]

Input: candies=[4, 2, 1, 1, 2], extraCandies=1
Output: [True, False, False, False, False]

Input: candies=[12, 1, 12], extraCandies=10
Output: [True, False, True]

Input: candies=[1, 2, 3, 4, 5], extraCandies=0
Output: [False, False, False, False, True]

Input: candies=[100], extraCandies=50
Output: [True]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<Object[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new Object[]{new int[]{2, 3, 5, 1, 3}, 3});
    testCases.add(new Object[]{new int[]{4, 2, 1, 1, 2}, 1});
    testCases.add(new Object[]{new int[]{12, 1, 12}, 10});
    testCases.add(new Object[]{new int[]{1, 2, 3, 4, 5}, 0});
    testCases.add(new Object[]{new int[]{10, 10, 10}, 5});
    testCases.add(new Object[]{new int[]{0, 0, 0}, 1});
    testCases.add(new Object[]{new int[]{100}, 50});
    testCases.add(new Object[]{new int[]{5, 2, 8, 1, 9}, 2});
    testCases.add(new Object[]{new int[]{1, 1, 1, 1, 1}, 0});
    testCases.add(new Object[]{new int[]{10, 5, 8, 3, 12}, 7});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(10) + 1; // [1,10]
        int[] candies = new int[n];
        for (int j = 0; j < n; j++) {
            candies[j] = random.nextInt(21); // [0,20]
        }
        int extraCandies = random.nextInt(16); // [0,15]
        testCases.add(new Object[]{candies, extraCandies});
    }

    // Verify
    for (Object[] tc : testCases) {
        int[] candies = (int[]) tc[0];
        int extraCandies = (int) tc[1];

        boolean[] result = solution(candies, extraCandies);
        boolean[] expected = correctSolution(candies, extraCandies);

        if (!Arrays.equals(result, expected)) {
            throw new AssertionError(
                "Test case failed: candies=" + Arrays.toString(candies) +
                ", extraCandies=" + extraCandies +
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
public static boolean[] correctSolution(int[] candies, int extraCandies) {
    int n = candies.length;
    boolean[] result = new boolean[n];

    int maxCandies = 0;
    for (int candy : candies) {
        if (candy > maxCandies) {
            maxCandies = candy;
        }
    }

    for (int i = 0; i < n; i++) {
        if (candies[i] + extraCandies >= maxCandies) {
            result[i] = true;
        }
    }

    return result;
}
```
