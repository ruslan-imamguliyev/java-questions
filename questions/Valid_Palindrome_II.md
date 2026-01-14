# Valid Palindrome II

_Score_: 2

Determine if a string can become a **palindrome** by deleting **at most one character**.

### Definition
- **Palindrome**: reads the same forwards and backwards (e.g., "racecar", "aba")
- You can delete 0 or 1 character to make it a palindrome

### Algorithm (Two Pointers)
1. Start with two pointers: left=0, right=len(s)-1
2. Compare characters at left and right:
   - If equal: move both pointers inward (left++, right--)
   - If different: try two options:
     - Skip left character: check if s[left+1:right+1] is palindrome
     - Skip right character: check if s[left:right] is palindrome
     - If either option is palindrome, return True
3. If all characters match, return True

### Examples
**Example 1: "aba"**
- Already a palindrome
- No deletion needed
- Output: **True**

**Example 2: "abca"**
- Compare: a==a ✓, b==c ✗
- Try deleting 'b': "aca" → palindrome ✓
- Try deleting 'c': "aba" → palindrome ✓
- Output: **True**

**Example 3: "abc"**
- Compare: a==c ✗
- Try deleting 'a': "bc" → not palindrome ✗
- Try deleting 'c': "ab" → not palindrome ✗
- Output: **False**

**Example 4: "deeee"**
- Compare: d==e ✗
- Try deleting 'd': "eeee" → palindrome ✓
- Output: **True**

**Example 5: "racecar"**
- Already a perfect palindrome
- Output: **True**

### Detailed Walkthrough
For "abccdba":
```
left=0, right=6: a==a ✓ → move to left=1, right=5
left=1, right=5: b==b ✓ → move to left=2, right=4
left=2, right=4: c==d ✗
  Try skip left: "cdba" → not palindrome ✗
  Try skip right: "bcc" → not palindrome ✗
Result: False
```

For "abccba":
```
left=0, right=5: a==a ✓ → move to left=1, right=4
left=1, right=4: b==b ✓ → move to left=2, right=3
left=2, right=3: c==c ✓
Result: True (already palindrome)
```

### Edge Cases
- Empty string: **True**
- Single character: **True**
- Two same characters "aa": **True**
- Two different characters "ab": **True** (delete one)

### Input Format
- String of lowercase letters

### Output Format
- Boolean: True if can become palindrome by deleting ≤1 character

## Placeholder:
```java

public static boolean solution(String s) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: 'aba'
Output: True

Input: 'abca'
Output: True

Input: 'abc'
Output: False

Input: 'deeee'
Output: True

Input: 'racecar'
Output: True
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add("aba");
    testCases.add("abca");
    testCases.add("abc");
    testCases.add("deeee");
    testCases.add("racecar");
    testCases.add("abcdefgfedcba");
    testCases.add("a");
    testCases.add("");
    testCases.add("abccba");
    testCases.add("abccdba");

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < 2; i++) {
        int halfLen = random.nextInt(5) + 2; // 2..6
        StringBuilder half = new StringBuilder();
        for (int j = 0; j < halfLen; j++) {
            half.append(chars.charAt(random.nextInt(chars.length())));
        }

        String palindrome = half.toString() + half.reverse().toString();

        // Sometimes insert one extra character
        if (random.nextBoolean()) {
            int pos = random.nextInt(palindrome.length() + 1);
            char extra = chars.charAt(random.nextInt(chars.length()));
            palindrome = palindrome.substring(0, pos) + extra + palindrome.substring(pos);
        }

        testCases.add(palindrome);
    }

    // Verify
    for (String s : testCases) {
        boolean result = solution(s);
        boolean expected = correctSolution(s);

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
public static boolean correctSolution(String s) {
    int left = 0;
    int right = s.length() - 1;

    while (left < right) {
        if (s.charAt(left) == s.charAt(right)) {
            left++;
            right--;
        } else {
            return isPalindrome(s, left, right - 1) ||
                   isPalindrome(s, left + 1, right);
        }
    }
    return true;
}

private static boolean isPalindrome(String s, int left, int right) {
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```
