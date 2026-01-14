# Longest Duration Key Press

_Score_: 1

Given a sequence of key presses and their release times, find the key that was pressed for the **longest duration**. If multiple keys have the same longest duration, return the **lexicographically largest** one.

### Key Press Duration
- First key's duration = `releaseTimes[0]` (pressed from time 0)
- Subsequent key's duration = `releaseTimes[i] - releaseTimes[i-1]`

### Tie-Breaking Rule
When multiple keys have the same maximum duration, choose the lexicographically largest character (e.g., 'z' > 'a').

### Algorithm
1. Calculate duration for first key
2. For each subsequent key, calculate duration as difference
3. Track the maximum duration and corresponding key
4. On ties, update if current key is lexicographically larger

### Example
`keysPressed="cbacaba"`, `releaseTimes=[9,23,33,36,46,62,68]`
- 'c': duration = 9
- 'b': duration = 23-9 = 14
- 'a': duration = 33-23 = 10
- ... (continue)
- Longest duration key with tie-breaking

### Input Format
- `keysPressed`: String of keys in press order
- `releaseTimes`: List of release times (sorted, increasing)

### Output Format
- Character: the key with longest press duration

## Placeholder:
```java

public static char solution(String keysPressed, int[] releaseTimes) {
    // Write your code here
    return '\0';
}
```

## Examples:
```
Input: keysPressed='cbacaba', releaseTimes=[9, 23, 33, 36, 46, 62, 68]
Output: 'b'

Input: keysPressed='spuda', releaseTimes=[1, 5, 8, 12, 16]
Output: 'p'

Input: keysPressed='ab', releaseTimes=[2, 5]
Output: 'b'

Input: keysPressed='ba', releaseTimes=[2, 5]
Output: 'a'

Input: keysPressed='abca', releaseTimes=[5, 9, 10, 15]
Output: 'a'
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<Object[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new Object[]{"cbacaba", new int[]{9, 23, 33, 36, 46, 62, 68}});
    testCases.add(new Object[]{"spuda", new int[]{1, 5, 8, 12, 16}});
    testCases.add(new Object[]{"ab", new int[]{2, 5}});
    testCases.add(new Object[]{"ba", new int[]{2, 5}});
    testCases.add(new Object[]{"abca", new int[]{5, 9, 10, 15}});
    testCases.add(new Object[]{"zzzz", new int[]{2, 3, 4, 5}});
    testCases.add(new Object[]{"axyz", new int[]{10, 20, 30, 40}});
    testCases.add(new Object[]{"zyxa", new int[]{10, 20, 30, 40}});
    testCases.add(new Object[]{"mnbvcxz", new int[]{1, 2, 3, 4, 5, 6, 7}});
    testCases.add(new Object[]{"abc", new int[]{5, 10, 15}});

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < 2; i++) {
        int length = random.nextInt(9) + 2; // [2,10]

        StringBuilder keys = new StringBuilder();
        for (int j = 0; j < length; j++) {
            keys.append(chars.charAt(random.nextInt(chars.length())));
        }

        int[] releaseTimes = new int[length];
        for (int j = 0; j < length; j++) {
            releaseTimes[j] = random.nextInt(100) + 1; // [1,100]
        }
        Arrays.sort(releaseTimes);

        testCases.add(new Object[]{keys.toString(), releaseTi
```

## Solution:
```java
public static char correctSolution(String keysPressed, int[] releaseTimes) {
    int n = keysPressed.length();

    int maxDuration = releaseTimes[0];
    char longestKey = keysPressed.charAt(0);

    for (int i = 1; i < n; i++) {
        int duration = releaseTimes[i] - releaseTimes[i - 1];

        if (duration > maxDuration) {
            maxDuration = duration;
            longestKey = keysPressed.charAt(i);
        } else if (duration == maxDuration) {
            if (keysPressed.charAt(i) > longestKey) {
                longestKey = keysPressed.charAt(i);
            }
        }
    }

    return longestKey;
}
```
