# Buddy Strings

_Score_: 2

Given two strings `s` and `goal`, return `True` if you can swap two letters in `s` so the result is equal to `goal`, otherwise, return `False`.

### Rules
- Swapping letters is defined as taking two indices `i` and `j` (0-indexed) such that `i != j` and swapping the characters at `s[i]` and `s[j]`
- You can only swap **at most once** (either swap exactly once or not at all)
- The two indices must be **different** (i != j)

### Cases to Consider

**Case 1: Strings are different**
- Find exactly 2 positions where characters differ
- Swapping those 2 positions should make `s` equal to `goal`
- Example: `s = "ab", goal = "ba"` → Swap indices 0 and 1 → **True**

**Case 2: Strings are already equal**
- Can you perform a swap that keeps them equal?
- Only possible if `s` has at least one duplicate character
- Example: `s = "aa", goal = "aa"` → Can swap the two 'a's (indices 0 and 1) → **True**
- Example: `s = "ab", goal = "ab"` → No duplicate characters, any swap changes the string → **False**

### Input Format
- Two strings: `s` and `goal`

### Output Format
- Boolean: `True` if buddy strings, `False` otherwise

### Constraints
- 1 <= s.length, goal.length <= 2 × 10⁴
- `s` and `goal` consist of lowercase letters

## Placeholder:
```java

public static boolean solution(String s, String goal) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: s = "ab", goal = "ba"
Output: True

Input: s = "ab", goal = "ab"
Output: False

Input: s = "aa", goal = "aa"
Output: True

Input: s = "abca", goal = "abca"
Output: True

Input: s = "ac", goal = "ca"
Output: True

Input: s = "abc", goal = "adc"
Output: False
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new String[]{"ab", "ba"});
    testCases.add(new String[]{"ab", "ab"});
    testCases.add(new String[]{"aa", "aa"});
    testCases.add(new String[]{"abca", "abca"});
    testCases.add(new String[]{"aabb", "aabb"});
    testCases.add(new String[]{"", ""});
    testCases.add(new String[]{"a", "a"});
    testCases.add(new String[]{"ac", "ca"});
    testCases.add(new String[]{"abcd", "badc"});
    testCases.add(new String[]{"kelb", "kelb"});
    testCases.add(new String[]{"abc", "ab"});
    testCases.add(new String[]{"abc", "adc"});

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int t = 0; t < 2; t++) {
        int length = random.nextInt(8) + 1; // [1,8]

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < length; i++) {
            sb.append(chars.charAt(random.nextInt(chars.length())));
        }
        String s = sb.toString();

        String goal;
        if (random.nextDouble() > 0.5 && length >= 2) {
            char[] arr = s.toCharArray();
            int i = random.nextInt(length);
            int j;
            do {
                j = random.nextInt(length);
            } while (j == i);
            char tmp = arr[i];
            arr[i] = arr[j];
            arr[j] = tmp;
            goal = new String(arr);
        } else {
            StringBuilder sb2 = new StringBuilder();
            for (int i = 0; i < length; i++) {
                sb2.append(chars.charAt(random.nextInt(chars.length())));
            }
            goal = sb2.toString();
        }

        testCases.add(new String[]{s, goal});
    }

    // Verify
    for (String[] tc : testCases) {
        String s = tc[0];
        String goal = tc[1];

        boolean result = solution(s, goal);
        boolean expected = correctSolution(s, goal);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: s='" + s + "', goal='" + goal +
                "', expected=" + expected + ", got=" + result
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static boolean correctSolution(String s, String goal) {
    if (s.length() != goal.length()) {
        return false;
    }

    if (s.equals(goal)) {
        Set<Character> seen = new HashSet<>();
        for (char c : s.toCharArray()) {
            if (seen.contains(c)) {
                return true;
            }
            seen.add(c);
        }
        return false;
    }

    List<Integer> diff = new ArrayList<>();
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) != goal.charAt(i)) {
            diff.add(i);
        }
    }

    if (diff.size() == 2) {
        int i = diff.get(0);
        int j = diff.get(1);
        char[] chars = s.toCharArray();
        char temp = chars[i];
        chars[i] = chars[j];
        chars[j] = temp;
        return new String(chars).equals(goal);
    }

    return false;
}
```
