# Game of Thrones - I

_Score_: 1

Dothraki are planning an attack to usurp King Robert's throne. To lock the door, he needs a key that is an anagram of a palindrome. Given a string, determine if it can be rearranged into a palindrome.

The function to complete is `gameOfThrones`, but in this environment, you will implement the `solution` function.

### Palindrome Properties
For a string to be rearrangeable into a palindrome:
- If its length is **even**, all characters must appear an **even** number of times.
- If its length is **odd**, exactly **one** character must appear an **odd** number of times.

### Key Insight
This means we only need to count the frequency of each character. Based on the counts, we can apply the rules:
- If the string's length is even, there should be **zero** characters with an odd frequency.
- If the string's length is odd, there should be exactly **one** character with an odd frequency.

A simpler way to combine these is to check that the number of characters with an **odd frequency** is no more than 1.

### Examples
- `"aab"` → Can form "aba" → **YES**
- `"abc"` → All chars appear once (3 odd frequencies) → **NO**
- `"aaabbbb"` → Can form "bbaaabb" → **YES**

### Input Format
- A string `s`

### Output Format
- A string: `"YES"` if it can form a palindrome, `"NO"` otherwise.

## Placeholder:
```java

public static String solution(String s) {
    // Write your code here
    return "";
}
```

## Examples:
```
Input: 'aabb'
Output: YES

Input: 'racecar'
Output: YES

Input: 'abc'
Output: NO

Input: 'aabbcc'
Output: YES

Input: 'a'
Output: YES
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add("aabb");
    testCases.add("racecar");
    testCases.add("abc");
    testCases.add("aabbcc");
    testCases.add("aaabbbb");
    testCases.add("civic");
    testCases.add("ivicc");
    testCases.add("abcd");
    testCases.add("a");
    testCases.add("");

    // Random test cases
    String chars = "abcde";
    for (int i = 0; i < 2; i++) {
        int length = random.nextInt(11); // [0,10]
        StringBuilder sb = new StringBuilder();
        for (int j = 0; j < length; j++) {
            sb.append(chars.charAt(random.nextInt(chars.length())));
        }
        testCases.add(sb.toString());
    }

    // Verify
    for (String testCase : testCases) {
        String result = solution(testCase);
        String expected = correctSolution(testCase);

        if (!result.equals(expected)) {
            throw new AssertionError(
                "Test case failed: '" + testCase +
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
public static String correctSolution(String s) {
    Map<Character, Integer> freq = new HashMap<>();

    for (char c : s.toCharArray()) {
        freq.put(c, freq.getOrDefault(c, 0) + 1);
    }

    int oddCount = 0;
    for (int count : freq.values()) {
        if (count % 2 != 0) {
            oddCount++;
        }
    }

    return oddCount > 1 ? "NO" : "YES";
}
```
