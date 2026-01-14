# Make the String Good

_Score_: 2

Make a string **"good"** by repeatedly removing adjacent characters that are the **same letter** in **opposite case** (one uppercase, one lowercase).

### Removal Rule
Remove pairs like:
- `"aA"` or `"Aa"` → remove both
- `"bB"` or `"Bb"` → remove both
- Continue until no more such pairs exist

### Stack-Based Approach
1. Use a stack to process characters
2. For each character:
   - If stack top and current char form a bad pair → pop stack
   - Otherwise → push to stack
3. Return remaining characters in stack

### Examples
- `"leEeetcode"` 
  - Remove "eE" → "leetcode"
  - Remove "Ee" → "letcode"
  - No more pairs → **"letcode"**

- `"abBAcC"`
  - Remove "Bb" → "aBAcC"
  - Remove "Aa" → "BcC"
  - Remove "Cc" → "B"
  - Result: **"B"**

- `"s"` → No pairs to remove → **"s"**

### Input Format
- A string `s` of uppercase and lowercase letters

### Output Format
- String: the "good" string after all removals

## Placeholder:
```java

public static String solution(String s) {
    // Write your code here
    return "";
}
```

## Examples:
```
Input: s='leEeetcode'
Output: 'leetcode'

Input: s='abBAcC'
Output: ''

Input: s='s'
Output: 's'

Input: s='Pp'
Output: ''

Input: s='AaBbCcDdEe'
Output: ''
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add("leEeetcode");
    testCases.add("abBAcC");
    testCases.add("s");
    testCases.add("Pp");
    testCases.add("pP");
    testCases.add("AaBbCcDdEe");
    testCases.add("aABBccdDEe");
    testCases.add("mCmc");
    testCases.add("baA");
    testCases.add("cAacCcBbAab");

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
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
        String result = solution(s);
        String expected = correctSolution(s);

        if (!result.equals(expected)) {
            throw new AssertionError(
                "Test case failed: s='" + s +
                "', expected='" + expected +
                "', got='" + result + "'"
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static String correctSolution(String s) {
    Deque<Character> stack = new ArrayDeque<>();

    for (char c : s.toCharArray()) {
        if (!stack.isEmpty()) {
            char top = stack.peekLast();
            if (Character.toLowerCase(top) == Character.toLowerCase(c) && top != c) {
                stack.pollLast();
                continue;
            }
        }
        stack.addLast(c);
    }

    StringBuilder result = new StringBuilder();
    for (char c : stack) {
        result.append(c);
    }

    return result.toString();
}
```
