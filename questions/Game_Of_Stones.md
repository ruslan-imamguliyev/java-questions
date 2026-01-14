# Game Of Stones

_Score_: 3

Two players play a game with a pile of n stones. On each turn, a player can remove 2, 3, or 5 stones. The player who cannot make a move loses. Assuming both players play optimally, determine who wins. Return 'First' if the first player wins, otherwise 'Second'.

## Placeholder:
```java

public static String solution(int n) {
    // Write your code here
    return "";
}
```

## Examples:
```
Input: 1
Output: Second

Input: 2
Output: First

Input: 3
Output: First

Input: 4
Output: First

Input: 5
Output: First
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<Integer> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(1);
    testCases.add(2);
    testCases.add(3);
    testCases.add(4);
    testCases.add(5);
    testCases.add(6);
    testCases.add(7);
    testCases.add(8);
    testCases.add(9);
    testCases.add(10);

    // Random test cases
    for (int i = 0; i < 2; i++) {
        testCases.add(random.nextInt(100) + 1); // [1,100]
    }

    // Verify
    for (int n : testCases) {
        String result = solution(n);
        String expected = correctSolution(n);

        if (!result.equals(expected)) {
            throw new AssertionError(
                "Test case failed: n=" + n +
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
public static String correctSolution(int n) {
    boolean[] dp = new boolean[n + 1];

    dp[0] = false;
    if (n >= 1) dp[1] = false;
    if (n >= 2) dp[2] = true;
    if (n >= 3) dp[3] = true;
    if (n >= 4) dp[4] = true;
    if (n >= 5) dp[5] = true;

    for (int i = 6; i <= n; i++) {
        if (!dp[i - 2] || !dp[i - 3] || !dp[i - 5]) {
            dp[i] = true;
        } else {
            dp[i] = false;
        }
    }

    return dp[n] ? "First" : "Second";
}
```
