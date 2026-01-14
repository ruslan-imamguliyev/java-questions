# Sum Of Even Numbers

_Score_: 1

Calculate the **sum of all even numbers** in a given list of integers.

### Even Number Definition
An even number is any integer that is **divisible by 2** (i.e., `n % 2 == 0`).

### Task
- Iterate through the list
- Identify all even numbers
- Return their sum

### Examples
- `[1, 2, 3, 4, 5, 6]` → Even numbers: 2, 4, 6 → Sum: **12**
- `[1, 3, 5]` → No even numbers → Sum: **0**
- `[-2, -1, 0, 1, 2]` → Even numbers: -2, 0, 2 → Sum: **0**

### Input Format
- A list of integers (can include negative numbers and zero)

### Output Format
- An integer representing the sum of all even numbers

### Edge Cases
- Empty list returns **0**
- List with no even numbers returns **0**
- Zero is considered even

## Placeholder:
```java

public static int solution(int[] nums) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: [-9, 4, 7, -7]
Output: 4

Input: [-8, 7, -1, 10, 9, 1]
Output: 2

Input: [-4, -8, -9, -3, -1, -8, -3, -7, 2]
Output: -18

Input: [4, 10, 1, -5]
Output: 14

Input: [1, -4, -2, 10, -8]
Output: -4
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    for (int i = 0; i < 10; i++) {
        int size = random.nextInt(101); // [0,100]
        int[] nums = new int[size];

        for (int j = 0; j < size; j++) {
            nums[j] = random.nextInt(2001) - 1000; // [-1000,1000]
        }

        int result = solution(nums);
        int expected = correctSolution(nums);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: " + Arrays.toString(nums) +
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
public static int correctSolution(int[] nums) {
    int sum = 0;
    for (int num : nums) {
        if (num % 2 == 0) {
            sum += num;
        }
    }
    return sum;
}
```
