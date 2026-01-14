# Consecutive Characters Power

_Score_: 1

Calculate the **power** of a string, which is defined as the **length of the longest consecutive substring** containing only one unique character.

### Power Definition
The power is the maximum number of consecutive identical characters appearing in the string.

### Algorithm Approach
1. Iterate through the string
2. Count consecutive occurrences of each character
3. Track the maximum count encountered

### Examples
- `"leetcode"` → Longest: "ee" (2 chars) → Power: **2**
- `"abbcccddddeeeeedcba"` → Longest: "eeeee" (5 chars) → Power: **5**
- `"ccccccc"` → All same character → Power: **7**
- `"abcdef"` → No consecutive duplicates → Power: **1**

### Input Format
- A string `s` (can be empty or contain any characters)

### Output Format
- An integer representing the power (length of longest consecutive character sequence)

### Edge Cases
- Empty string returns **0**
- Single character returns **1**

## Placeholder:
```java

public static int solution(String s) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: s='leetcode'
Output: 2

Input: s='abbcccddddeeeeedcba'
Output: 5

Input: s='hooraayyyyy'
Output: 5

Input: s='tourist'
Output: 1

Input: s='ccccccc'
Output: 7
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add("leetcode");
    testCases.add("abbcccddddeeeeedcba");
    testCases.add("hooraayyyyy");
    testCases.add("tourist");
    testCases.add("ccccccc");
    testCases.add("a");
    testCases.add("");
    testCases.add("aabbcc");
    testCases.add("aaaaabbbbbaaaaa");
    testCases.add("xyzyzyzyz");

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < 2; i++) {
        int length = random.nextInt(16); // [0,15]
        StringBuilder sb = new StringBuilder();
        for (int j = 0; j < length; j++) {
            sb.append(chars.charAt(random.nextInt(chars.length())));
        }
        testCases.add(sb.toString());
    }

    // Verify
    for (String s : testCases) {
        int result = solution(s);
        int expected = correctSolution(s);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: s='" + s +
                "', expected=" + expected +
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
public static int correctSolution(String s) {
    if (s == null || s.isEmpty()) {
        return 0;
    }

    int maxPower = 0;
    int currentPower = 0;
    char currentChar = '\0';

    for (char c : s.toCharArray()) {
        if (c == currentChar) {
            currentPower++;
        } else {
            maxPower = Math.max(maxPower, currentPower);
            currentChar = c;
            currentPower = 1;
        }
    }

    maxPower = Math.max(maxPower, currentPower);
    return maxPower;
}
```
