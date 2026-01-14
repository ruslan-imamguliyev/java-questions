# Diet Plan Performance

_Score_: 1

Calculate a **performance score** based on daily calorie intake over sliding k-day windows.

### Scoring Rules
For each **k-day window**:
- If total calories **< lower**: **-1 point** (too few calories)
- If total calories **> upper**: **+1 point** (too many calories)
- If **lower ≤ total ≤ upper**: **0 points** (perfect!)

### Task
Sum up the points for all possible k-day windows in the array.

### Sliding Window Approach
```python
1. Calculate sum of first k days
2. Score it
3. Slide window: add next day, remove first day
4. Score each window
5. Return total score
```

### Example
`calories = [1,2,3,4,5], k=1, lower=3, upper=3`
- Day 1: 1 < 3 → **-1**
- Day 2: 2 < 3 → **-1**
- Day 3: 3 = 3 → **0**
- Day 4: 4 > 3 → **+1**
- Day 5: 5 > 3 → **+1**
- Total score: -1-1+0+1+1 = **0**

### Example 2
`calories = [6,5,4,3,2,1], k=2, lower=5, upper=10`
- Window [6,5]: 11 > 10 → **+1**
- Window [5,4]: 9 ✓ → **0**
- Window [4,3]: 7 ✓ → **0**
- Window [3,2]: 5 ✓ → **0**
- Window [2,1]: 3 < 5 → **-1**
- Total score: 1+0+0+0-1 = **0**

### Algorithm Efficiency
- **Naive**: O(n×k) - recalculate sum for each window
- **Optimized**: O(n) - sliding window technique
  ```
  new_sum = old_sum + calories[i] - calories[i-k]
  ```

### Input Format
- `calories`: Array of daily calories (integers)
- `k`: Window size (consecutive days)
- `lower`: Lower calorie limit
- `upper`: Upper calorie limit

### Output Format
- Integer: total performance score (can be negative, zero, or positive)

## Placeholder:
```java

public static int solution(int[] calories, int k, int lower, int upper) {
        // Write your code here
        return 0;
    }
```

## Examples:
```
Input: calories=[1, 2, 3, 4, 5], k=1, lower=3, upper=3
Output: 0

Input: calories=[6, 5, 4, 3, 2, 1], k=2, lower=5, upper=10
Output: 0

Input: calories=[6, 5, 4, 3, 2, 1], k=1, lower=5, upper=5
Output: -3

Input: calories=[1, 2, 3, 4, 5], k=3, lower=3, upper=7
Output: 2

Input: calories=[1, 2, 3], k=2, lower=0, upper=4
Output: 1
```

## Verify code:
```java
public static void verify() {
        Random random = new Random();
        List<Object[]> testCases = new ArrayList<>();
        
        testCases.add(new Object[]{new int[]{1, 2, 3, 4, 5}, 1, 3, 3});
        testCases.add(new Object[]{new int[]{6, 5, 4, 3, 2, 1}, 2, 5, 10});
        testCases.add(new Object[]{new int[]{6, 5, 4, 3, 2, 1}, 1, 5, 5});
        testCases.add(new Object[]{new int[]{1, 2, 3, 4, 5}, 3, 3, 7});
        testCases.add(new Object[]{new int[]{1, 2, 3}, 2, 0, 4});
        testCases.add(new Object[]{new int[]{100, 200, 300, 400}, 2, 150, 350});
        testCases.add(new Object[]{new int[]{1, 1, 1, 1, 1}, 2, 1, 1});
        testCases.add(new Object[]{new int[]{1, 1, 1, 1, 1}, 3, 0, 5});
        testCases.add(new Object[]{new int[]{5, 5, 5, 5, 5}, 3, 10, 14});
        testCases.add(new Object[]{new int[]{0, 0, 0, 0, 0}, 2, -1, 1});
        
        // Add random test cases
        for (int i = 0; i < 2; i++) {
            int n = random.nextInt(8) + 3;
            int k = random.nextInt(n) + 1;
            int[] calories = new int[n];
            for (int j = 0; j < n; j++) {
                calories[j] = random.nextInt(21);
            }
            int lower = random.nextInt(16);
            int upper = random.nextInt(16) + lower;
            testCases.add(new Object[]{calories, k, lower, upper});
        }
        
        for (Object[] testCase : testCases) {
            int[] calories = (int[]) testCase[0];
            int k = (int) testCase[1];
            int lower = (int) testCase[2];
            int upper = (int) testCase[3];
            int result = solution(calories, k, lower, upper);
            int expected = correctSolution(calories, k, lower, upper);
            if (result != expected) {
                throw new AssertionError(String.format(
                    "Test case failed: calories=%s, k=%d, lower=%d, upper=%d, expected=%d, got=%d",
                    Arrays.toString(calories), k, lower, upper, expected, result
                ));
            }
        }
        System.out.println("success");
        System.out.flush();
    }
```

## Solution:
```java
public static int correctSolution(int[] calories, int k, int lower, int upper) {
        int points = 0;
        int n = calories.length;
        if (n < k) {
            return 0;
        }
        int windowSum = 0;
        for (int i = 0; i < k; i++) {
            windowSum += calories[i];
        }
        if (windowSum < lower) {
            points -= 1;
        } else if (windowSum > upper) {
            points += 1;
        }
        for (int i = k; i < n; i++) {
            windowSum += calories[i] - calories[i - k];
            if (windowSum < lower) {
                points -= 1;
            } else if (windowSum > upper) {
                points += 1;
            }
        }
        return points;
    }
```
