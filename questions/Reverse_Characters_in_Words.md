# Reverse Characters in Words

_Score_: 1

Reverse the characters of **each word** in a sentence while maintaining the **original word order**.

### Rules
- The input string does not contain any leading or trailing spaces.
- All words are separated by a single space.
- Reverse the characters within each word individually.
- Keep the word order unchanged.

### Examples
- `"Let's take LeetCode contest"` → `"s'teL ekat edoCteeL tsetnoc"`
- `"Mr Ding"` → `"rM gniD"`

### Input Format
- A string `s` containing words separated by single spaces.

### Output Format
- A string with each word's characters reversed.

### Constraints
- `1 <= s.length <= 5 * 10^4`
- `s` contains printable ASCII characters.
- `s` does not contain any leading or trailing spaces.
- There is at least one word in `s`.
- All the words in `s` are separated by a single space.

## Placeholder:
```java

public static String solution(String s) {
    // Write your code here
    return "";
}
```

## Examples:
```
Input: 'hello world'
Output: 'olleh dlrow'

Input: 'Python is fun'
Output: 'nohtyP si nuf'

Input: 'Reverse each word'
Output: 'esreveR hcae drow'

Input: 'a b c'
Output: 'a b c'

Input: 'one'
Output: 'eno'
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add("hello world");
    testCases.add("Python is fun");
    testCases.add("Reverse each word");
    testCases.add("a b c");
    testCases.add("one");
    testCases.add("The quick brown fox jumps over the lazy dog");
    testCases.add("Hello World");
    testCases.add("OpenAI GPT");
    testCases.add("Programming in Python");
    testCases.add("Data Structures and Algorithms");

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    for (int i = 0; i < 2; i++) {
        int wordCount = random.nextInt(4) + 2; // [2,5]
        StringBuilder sb = new StringBuilder();
        for (int w = 0; w < wordCount; w++) {
            int len = random.nextInt(6) + 3; // [3,8]
            StringBuilder word = new StringBuilder();
            for (int j = 0; j < len; j++) {
                word.append(chars.charAt(random.nextInt(chars.length())));
            }
            if (w > 0) sb.append(" ");
            sb.append(word);
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
    String[] words = s.split("\\s+");
    StringBuilder result = new StringBuilder();

    for (int i = 0; i < words.length; i++) {
        String word = words[i];
        result.append(new StringBuilder(word).reverse());
        if (i < words.length - 1) {
            result.append(" ");
        }
    }

    return result.toString();
}
```
