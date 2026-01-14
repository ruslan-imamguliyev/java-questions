# Icecream Parlor

_Score_: 2

## Problem Description

Two friends visit an ice cream parlor and want to buy exactly two different flavors of ice cream. They have a fixed amount of money `m` to spend, and they want to spend all of it.

Given:
- `m`: the amount of money they have
- `cost`: an array of ice cream prices where `cost[i]` represents the price of the ice cream at index `i`

Find the **1-indexed** positions of the two ice cream flavors they will purchase so that they spend exactly `m` dollars.

### Constraints:
- `2 <= cost.length <= 10^4`
- `1 <= cost[i] <= 10^4`
- `2 <= m <= 10^4`
- There is always exactly one unique solution
- The two flavors must be at different indices

### Example 1:
**Input:** `m = 4, cost = [1, 4, 5, 3, 2]`  
**Output:** `[1, 4]`  
**Explanation:**  
The friends can buy ice cream at positions 1 and 4 (1-indexed).
- Position 1 has cost 1
- Position 4 has cost 3
- Total: 1 + 3 = 4 (exactly m)

### Example 2:
**Input:** `m = 4, cost = [2, 2, 4, 3]`  
**Output:** `[1, 2]`  
**Explanation:**  
The friends can buy ice cream at positions 1 and 2.
- Position 1 has cost 2
- Position 2 has cost 2
- Total: 2 + 2 = 4 (exactly m)

### Example 3:
**Input:** `m = 9, cost = [1, 3, 4, 6, 7]`  
**Output:** `[2, 4]`  
**Explanation:**  
The friends can buy ice cream at positions 2 and 4.
- Position 2 has cost 3
- Position 4 has cost 6
- Total: 3 + 6 = 9 (exactly m)

### Note:
- Return the indices in ascending order
- The indices are 1-indexed (not 0-indexed)
- There is guaranteed to be exactly one valid pair
- You cannot buy the same flavor twice (different indices required)

## Placeholder:
```java

public static int[] solution(int m, int[] cost) {
    // Write your code here
    return new int[0];
}
```

## Examples:
```
Input: m=4, cost=[1, 4, 5, 3, 2]
Output: [1, 4]

Input: m=4, cost=[2, 2, 4, 3]
Output: [1, 2]

Input: m=9, cost=[1, 3, 4, 6, 7]
Output: [2, 4]

Input: m=6, cost=[1, 2, 3, 4, 5]
Output: [2, 4]

Input: m=3, cost=[1, 2, 3]
Output: [1, 2]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    List<Object[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new Object[]{4, new int[]{1, 4, 5, 3, 2}});
    testCases.add(new Object[]{4, new int[]{2, 2, 4, 3}});
    testCases.add(new Object[]{9, new int[]{1, 3, 4, 6, 7}});
    testCases.add(new Object[]{6, new int[]{1, 2, 3, 4, 5}});
    testCases.add(new Object[]{8, new int[]{2, 1, 9, 4, 4, 56, 90, 3}});
    testCases.add(new Object[]{5, new int[]{5, 75, 25}});
    testCases.add(new Object[]{10, new int[]{1,2,3,4,5,6,7,8,9,10}});
    testCases.add(new Object[]{7, new int[]{1, 6, 3, 5, 7, 9}});
    testCases.add(new Object[]{11, new int[]{7, 3, 2, 4, 8}});
    testCases.add(new Object[]{3, new int[]{1, 2, 3}});

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int n = random.nextInt(7) + 2; // [2,8]
        int[] cost = new int[n];
        for (int i = 0; i < n; i++) {
            cost[i] = random.nextInt(10) + 1;
        }

        int i = random.nextInt(n);
        int j;
        do {
            j = random.nextInt(n);
        } while (j == i);

        int m = cost[i] + cost[j];
        testCases.add(new Object[]{m, cost});
    }

    // Verify
    for (Object[] tc : testCases) {
        int m = (int) tc[0];
        int[] cost = (int[]) tc[1];

        int[] result = solution(m, cost);
        int[] expected = correctSolution(m, cost);

        if (!Arrays.equals(result, expected)) {
            throw new AssertionError(
                "Test case failed: m=" + m +
                ", cost=" + Arrays.toString(cost) +
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
public static int[] correctSolution(int m, int[] cost) {
    Map<Integer, Integer> priceToIndex = new HashMap<>();

    for (int i = 0; i < cost.length; i++) {
        int price = cost[i];
        int target = m - price;

        if (priceToIndex.containsKey(target)) {
            int idx1 = priceToIndex.get(target) + 1;
            int idx2 = i + 1;
            return new int[]{
                Math.min(idx1, idx2),
                Math.max(idx1, idx2)
            };
        }

        priceToIndex.put(price, i);
    }

    return new int[0];
}
```
