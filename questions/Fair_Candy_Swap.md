# Fair Candy Swap

_Score_: 2

Alice and Bob have different total amounts of candy. Find **one candy box** to swap so they both end up with **equal totals**.

### Goal
After swapping:
- Alice's new total = Bob's new total

### Mathematical Approach
Let:
- `sum_A` = Alice's total candy
- `sum_B` = Bob's total candy
- `x` = candy size Alice gives to Bob
- `y` = candy size Bob gives to Alice

After swap:
- Alice: `sum_A - x + y`
- Bob: `sum_B - y + x`

For equality:
```
sum_A - x + y = sum_B - y + x
2y = sum_B - sum_A + 2x
y = x + (sum_B - sum_A) / 2
```

### Algorithm
1. Calculate `diff = (sum_B - sum_A) / 2`
2. Store Bob's candy sizes in a set for O(1) lookup
3. For each candy `x` that Alice has:
   - Check if `y = x + diff` exists in Bob's candy
   - If found, return `[x, y]`

### Example 1
- Alice: `[1, 1]`, total = 2
- Bob: `[2, 2]`, total = 4
- Need equal total: 3 each
- diff = (4-2)/2 = 1
- Alice gives 1, Bob gives 2 → `[1, 2]`
- Result: Alice=[1,2], Bob=[2,1] ✓

### Example 2
- Alice: `[1, 2]`, total = 3
- Bob: `[2, 3]`, total = 5
- Need equal total: 4 each
- diff = (5-3)/2 = 1
- Alice gives 1, Bob gives 2 → `[1, 2]`
- Result: Alice=[2,2], Bob=[3,1] ✓

### Example 3
- Alice: `[2]`, total = 2
- Bob: `[1, 3]`, total = 4
- Need equal total: 3 each
- diff = (4-2)/2 = 1
- Alice gives 2, Bob gives 3 → `[2, 3]`
- Result: Alice=[3], Bob=[1,2] ✓

### Guarantees
- A valid answer always exists
- Only one pair needs to be found

### Input Format
- `aliceSizes`: Array of Alice's candy box sizes
- `bobSizes`: Array of Bob's candy box sizes

### Output Format
- `[x, y]`: sizes to swap (x from Alice, y from Bob)

## Placeholder:
```java

public static int[] solution(int[] aliceSizes, int[] bobSizes) {
    // Write your code here
    return new int[0];
}
```

## Examples:
```
Input: aliceSizes=[1, 1], bobSizes=[2, 2]
Output: [1, 2.0]

Input: aliceSizes=[1, 2], bobSizes=[2, 3]
Output: [1, 2.0]

Input: aliceSizes=[2], bobSizes=[1, 3]
Output: [2, 3.0]

Input: aliceSizes=[1, 2, 5], bobSizes=[2, 4]
Output: [5, 4.0]

Input: aliceSizes=[10, 5], bobSizes=[8, 12]
Output: None
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[][]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[][]{{1, 1}, {2, 2}});
    testCases.add(new int[][]{{1, 2}, {2, 3}});
    testCases.add(new int[][]{{2}, {1, 3}});
    testCases.add(new int[][]{{1, 2, 5}, {2, 4}});
    testCases.add(new int[][]{{10, 20}, {5, 15}});
    testCases.add(new int[][]{{30, 15}, {10, 25}});

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int aLen = random.nextInt(6) + 1;
        int bLen = random.nextInt(6) + 1;

        List<Integer> alice = new ArrayList<>();
        List<Integer> bob = new ArrayList<>();

        for (int i = 0; i < aLen; i++) alice.add(random.nextInt(10) + 1);
        for (int i = 0; i < bLen; i++) bob.add(random.nextInt(10) + 1);

        int sumA = alice.stream().mapToInt(Integer::intValue).sum();
        int sumB = bob.stream().mapToInt(Integer::intValue).sum();

        if (sumB == sumA) bob.add(2);
        if (sumB < sumA) {
            List<Integer> tmp = alice; alice = bob; bob = tmp;
            sumA = alice.stream().mapToInt(Integer::intValue).sum();
            sumB = bob.stream().mapToInt(Integer::intValue).sum();
        }

        int diff = sumB - sumA;
        if (diff % 2 != 0) {
            alice.add(1);
            diff++;
        }

        int x = random.nextInt(diff) + 1;
        alice.add(x);
        bob.add(diff - x);

        testCases.add(new int[][]{
            alice.stream().mapToInt(Integer::intValue).toArray(),
            bob.stream().mapToInt(Integer::intValue).toArray()
        });
    }

    // Verify
    for (int[][] tc : testCases) {
        int[] aliceSizes = tc[0];
        int[] bobSizes = tc[1];

        int[] result = solution(aliceSizes, bobSizes);
        int[] expected = correctSolution(aliceSizes, bobSizes);

        if (!Arrays.equals(result, expected)) {
            throw new AssertionError(
                "Test case failed: aliceSizes=" + Arrays.toString(aliceSizes) +
                ", bobSizes=" + Arrays.toString(bobSizes) +
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
public static int[] correctSolution(int[] aliceSizes, int[] bobSizes) {
    int Sa = 0, Sb = 0;
    for (int x : aliceSizes) Sa += x;
    for (int x : bobSizes) Sb += x;

    Set<Integer> setB = new HashSet<>();
    for (int x : bobSizes) {
        setB.add(x);
    }

    int diff = (Sb - Sa) / 2;

    for (int x : aliceSizes) {
        int y = x + diff;
        if (setB.contains(y)) {
            return new int[]{x, y};
        }
    }

    return new int[0];
}
```
