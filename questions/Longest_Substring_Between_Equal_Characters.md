# Longest Substring Between Equal Characters

_Score_: 1

Find the **length of the longest substring** between two **equal characters**, excluding the characters themselves. If no such pair exists, return -1.

### Key Points
- Look for pairs of the same character
- Count characters **between** them (not including the pair itself)
- Maximize this distance

### Strategy
Use a hash map to store the first occurrence of each character:
1. Iterate through the string
2. If character seen before: calculate distance between occurrences
3. Track maximum distance found
4. Track maximum distance found
5. Return maximum distance, or -1 if no pairs

### Examples
- `"aa"` → Between two 'a's: 0 characters → **0**
- `"abca"` → Between two 'a's: "bc" (2 chars) → **2**
- `"cbzxy"` → No repeated characters → **-1**
- `"cabbac"` → Between two 'c's: "abba" (4 chars) → **4**
- `"aba"` → Between two 'a's: "b" (1 char) → **1**

### Input Format
- A string `s` containing lowercase letters

### Output Format
- Integer: maximum length of substring between equal characters, or -1

## Placeholder:
```java

public static int solution(String s) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: s='aa'
Output: 0

Input: s='abca'
Output: 2

Input: s='cbzxy'
Output: -1

Input: s='cabbac'
Output: 4

Input: s='abcdefga'
Output: 6
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add("aa");
    testCases.add("abca");
    testCases.add("cbzxy");
    testCases.add("cabbac");
    testCases.add("abcdefga");
    testCases.add("abccba");
    testCases.add("axyza");
    testCases.add("aba");
    testCases.add("zzzzz");
    testCases.add("abcdefghijklmnopqrstuvwxyz");

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < 2; i++) {
        int length = random.nextInt(14) + 2; // 2..15
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
    int maxLength = -1;
    Map<Character, Integer> firstOccurrence = new HashMap<>();

    for (int index = 0; index < s.length(); index++) {
        char ch = s.charAt(index);
        if (firstOccurrence.containsKey(ch)) {
            int length = index - firstOccurrence.get(ch) - 1;
            if (length > maxLength) {
                maxLength = length;
            }
        } else {
            firstOccurrence.put(ch, index);
        }
    }

    return maxLength;
}
```
