# Beautiful Days

_Score_: 1

A day number is considered **beautiful** if the absolute difference between the number and its reverse is **divisible by k**.

### Task
Count how many beautiful days exist in the range from day `i` to day `j` (inclusive).

### Beautiful Day Definition
For a day number `d`:
1. Calculate `reverse(d)` - the number with digits reversed
2. Calculate `|d - reverse(d)|`
3. If this difference is divisible by `k`, the day is beautiful

### Example
If k = 6 and day = 20:
- reverse(20) = 02 = 2
- |20 - 2| = 18
- 18 % 6 = 0 → **Beautiful day!**

### Input Format
- `i`: Starting day number
- `j`: Ending day number
- `k`: Divisor

### Output Format
- Integer: count of beautiful days in range [i, j]

## Placeholder:
```java

public static int solution(int i, int j, int k) {
        // Write your code here
        return 0;
    }
```

## Examples:
```
Input: i=20, j=23, k=6
Output: 2

Input: i=1, j=10, k=1
Output: 10

Input: i=13, j=45, k=3
Output: 33

Input: i=123, j=123, k=1
Output: 1

Input: i=1, j=100, k=9
Output: 100
```

## Verify code:
```java
public static void verify() {
        Random random = new Random();
        List<int[]> testCases = new ArrayList<>();
        
        testCases.add(new int[]{20, 23, 6});
        testCases.add(new int[]{1, 10, 1});
        testCases.add(new int[]{13, 45, 3});
        testCases.add(new int[]{10, 20, 2});
        testCases.add(new int[]{100, 200, 10});
        testCases.add(new int[]{1, 1000, 7});
        testCases.add(new int[]{123, 123, 1});
        testCases.add(new int[]{120, 130, 7});
        testCases.add(new int[]{1, 100, 9});
        testCases.add(new int[]{987, 1000, 2});
        
        // Add random test cases
        for (int idx = 0; idx < 2; idx++) {
            int start = random.nextInt(100) + 1;
            int end = random.nextInt(51) + start;
            int k = random.nextInt(10) + 1;
            testCases.add(new int[]{start, end, k});
        }
        
        for (int[] testCase : testCases) {
            int i = testCase[0];
            int j = testCase[1];
            int k = testCase[2];
            int result = solution(i, j, k);
            int expected = correctSolution(i, j, k);
            if (result != expected) {
                throw new AssertionError(String.format(
                    "Test case failed: i=%d, j=%d, k=%d, expected=%d, got=%d",
                    i, j, k, expected, result
                ));
            }
        }
        System.out.println("success");
        System.out.flush();
    }
```

## Solution:
```java
public static int correctSolution(int i, int j, int k) {
        int count = 0;
        for (int day = i; day <= j; day++) {
            int reversed = 0;
            int num = day;
            while (num > 0) {
                reversed = reversed * 10 + num % 10;
                num /= 10;
            }
            int reverseDay = reversed;
            if (Math.abs(day - reverseDay) % k == 0) {
                count++;
            }
        }
        return count;
    }
```
