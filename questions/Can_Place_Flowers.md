# Can Place Flowers

_Score_: 1

You have a flowerbed represented as an array with `0` (empty) and `1` (flower planted). You want to plant `n` **new flowers** without violating the planting rule.

### Planting Rule
Flowers cannot be planted in **adjacent** plots. There must be at least one empty plot between any two flowers. The input `flowerbed` is guaranteed to follow this rule.

### Valid Planting Position
You can plant at position `i` if:
1. `flowerbed[i] == 0` (empty)
2. `flowerbed[i-1] == 0` or `i == 0` (left is empty or at start)
3. `flowerbed[i+1] == 0` or `i == len-1` (right is empty or at end)

### Task
Determine if you can plant `n` new flowers while maintaining the no-adjacent-flowers rule.

### Input Format
- `flowerbed`: Array of 0s and 1s
- `n`: Number of flowers to plant

### Output Format
- Boolean: `True` if possible, `False` otherwise

### Example
`flowerbed = [1,0,0,0,1], n = 1`
- Can plant at index 2
- Output: **True**

## Placeholder:
```java

public static boolean solution(int[] flowerbed, int n) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: flowerbed=[1, 0, 0, 0, 1], n=1
Output: True

Input: flowerbed=[1, 0, 0, 0, 1], n=2
Output: False

Input: flowerbed=[0, 0, 1, 0, 0], n=2
Output: True

Input: flowerbed=[0, 0, 0, 0, 0], n=3
Output: True

Input: flowerbed=[0], n=1
Output: True
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<Object[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new Object[]{new int[]{1,0,0,0,1}, 1});
    testCases.add(new Object[]{new int[]{1,0,0,0,1}, 2});
    testCases.add(new Object[]{new int[]{0,0,1,0,0}, 2});
    testCases.add(new Object[]{new int[]{0,0,0,0,0}, 3});
    testCases.add(new Object[]{new int[]{1,0,0,0,0,1}, 2});
    testCases.add(new Object[]{new int[]{1,0,0,0,0,0,1}, 2});
    testCases.add(new Object[]{new int[]{1,0,1,0,1,0,1}, 1});
    testCases.add(new Object[]{new int[]{0}, 1});
    testCases.add(new Object[]{new int[]{0}, 0});
    testCases.add(new Object[]{new int[]{1,0,1,0,1,0,0,0,1}, 2});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int length = random.nextInt(12) + 1; // [1,12]
        int[] flowerbed = new int[length];
        for (int j = 0; j < length; j++) {
            flowerbed[j] = random.nextBoolean() ? 1 : 0;
        }
        int n = random.nextInt(6); // [0,5]
        testCases.add(new Object[]{flowerbed, n});
    }

    // Verify
    for (Object[] tc : testCases) {
        int[] flowerbed = (int[]) tc[0];
        int n = (int) tc[1];

        int[] fbCopy1 = Arrays.copyOf(flowerbed, flowerbed.length);
        int[] fbCopy2 = Arrays.copyOf(flowerbed, flowerbed.length);

        boolean result = solution(fbCopy1, n);
        boolean expected = correctSolution(fbCopy2, n);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: flowerbed=" + Arrays.toString(flowerbed) +
                ", n=" + n +
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
public static boolean correctSolution(int[] flowerbed, int n) {
    int i = 0;
    int length = flowerbed.length;

    while (i < length) {
        if (flowerbed[i] == 0) {
            boolean emptyPrev = (i == 0) || (flowerbed[i - 1] == 0);
            boolean emptyNext = (i == length - 1) || (flowerbed[i + 1] == 0);

            if (emptyPrev && emptyNext) {
                flowerbed[i] = 1;
                n--;
                if (n == 0) {
                    return true;
                }
                i += 2;
                continue;
            }
        }
        i++;
    }

    return n <= 0;
}
```
