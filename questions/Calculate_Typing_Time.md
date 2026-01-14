# Calculate Typing Time

_Score_: 2

Calculate the total time needed to type a word on a special keyboard.

### Keyboard Layout
- The keyboard has **all keys in a single row**
- The keyboard layout is given as a string of length 26 containing each English lowercase letter exactly once in some order
- Each character's position is its index in the keyboard string (indexed from 0 to 25)
- Initially, the finger is at index **0**

### Time Calculation
- **Moving** the finger from index `i` to index `j` takes `|i - j|` time units
- **Typing** a character takes `0` time (instantaneous)
- Total time = sum of all movements

### Task
Given a keyboard layout and a word, calculate the minimum time to type it.

### Input Format
- `keyboard`: String of length 26 indicating the layout (indexed from 0 to 25), containing each English lowercase letter exactly once
- `word`: The word to type (English lowercase letters)

### Output Format
- Integer: total time to type the word

## Placeholder:
```java

public static int solution(String keyboard, String word) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: keyboard='abcdefghijklmnopqrstuvwxyz', word='cba'
Output: 4

Input: keyboard='pqrstuvwxyzabcdefghijklmno', word='leetcode'
Output: 73

Input: keyboard='qwertyuiopasdfghjklzxcvbnm', word='apple'
Output: 36

Input: keyboard='abcdefghijklmnopqrstuvwxyz', word=''
Output: 0

Input: keyboard='abcdefghijklmnopqrstuvwxyz', word='ace'
Output: 4
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new String[]{"abcdefghijklmnopqrstuvwxyz", "cba"});
    testCases.add(new String[]{"pqrstuvwxyzabcdefghijklmno", "leetcode"});
    testCases.add(new String[]{"qwertyuiopasdfghjklzxcvbnm", "apple"});
    testCases.add(new String[]{"abcdefghijklmnopqrstuvwxyz", ""});
    testCases.add(new String[]{"zyxwvutsrqponmlkjihgfedcba", "zyx"});
    testCases.add(new String[]{"mnbvcxzlkjhgfdsapoiuytrewq", "mnb"});
    testCases.add(new String[]{"poiuytrewqasdfghjklmnbvcxz", "poi"});
    testCases.add(new String[]{"abcdefghijklmnopqrstuvwxyz", "a"});
    testCases.add(new String[]{"abcdefghijklmnopqrstuvwxyz", "zzz"});
    testCases.add(new String[]{"abcdefghijklmnopqrstuvwxyz", "ace"});

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < 2; i++) {
        // random keyboard permutation
        List<Character> kb = new ArrayList<>();
        for (char c : chars.toCharArray()) {
            kb.add(c);
        }
        Collections.shuffle(kb);
        StringBuilder keyboard = new StringBuilder();
        for (char c : kb) {
            keyboard.append(c);
        }

        int wordLength = random.nextInt(9); // 0..8
        StringBuilder word = new StringBuilder();
        for (int j = 0; j < wordLength; j++) {
            word.append(chars.charAt(random.nextInt(chars.length())));
        }

        testCases.add(new String[]{keyboard.toString(), word.toString()});
    }

    // Verify
    for (String[] tc : testCases) {
        String keyboard = tc[0];
        String word = tc[1];

        int result = solution(keyboard, word);
        int expected = correctSolution(keyboard, word);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: keyboard='" + keyboard +
                "', word='" + word +
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
public static int correctSolution(String keyboard, String word) {
    Map<Character, Integer> keyPositions = new HashMap<>();

    for (int i = 0; i < keyboard.length(); i++) {
        keyPositions.put(keyboard.charAt(i), i);
    }

    int currentPosition = 0;
    int totalTime = 0;

    for (int i = 0; i < word.length(); i++) {
        char ch = word.charAt(i);
        int targetPosition = keyPositions.get(ch);
        totalTime += Math.abs(targetPosition - currentPosition);
        currentPosition = targetPosition;
    }

    return totalTime;
}
```
