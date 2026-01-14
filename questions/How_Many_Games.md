# How Many Games

_Score_: 1

A video game tournament costs a certain amount of money initially. For each consecutive game, the price decreases by a discount amount until it reaches the minimum price. Determine how many games can be purchased with a given budget.

## Placeholder:
```java

public static int solution(int p, int d, int m, int s) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: p=20, d=3, m=6, s=80
Output: 6

Input: p=20, d=3, m=6, s=50
Output: 2

Input: p=10, d=2, m=3, s=100
Output: 28

Input: p=5, d=1, m=1, s=20
Output: 10

Input: p=30, d=5, m=10, s=100
Output: 5
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    int[][] testCases = {
        {20, 3, 6, 80},
        {20, 3, 6, 50},
        {10, 2, 3, 100},
        {5, 1, 1, 20},
        {30, 5, 10, 100}
    };

    // Fixed test cases
    for (int[] tc : testCases) {
        int p = tc[0], d = tc[1], m = tc[2], s = tc[3];
        int result = solution(p, d, m, s);
        int expected = correctSolution(p, d, m, s);
        if (result != expected) {
            throw new AssertionError(
                "Test case failed: p=" + p + ", d=" + d + ", m=" + m + ", s=" + s
            );
        }
    }

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int p = random.nextInt(41) + 10; // [10,50]
        int d = random.nextInt(10) + 1;  // [1,10]
        int m = random.nextInt(p) + 1;   // [1,p]
        int s = random.nextInt(191) + 10; // [10,200]

        int result = solution(p, d, m, s);
        int expected = correctSolution(p, d, m, s);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: p=" + p + ", d=" + d + ", m=" + m + ", s=" + s
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static int correctSolution(int p, int d, int m, int s) {
    int gamesCount = 0;
    int currentPrice = p;

    while (s >= currentPrice) {
        s -= currentPrice;
        gamesCount++;
        currentPrice = Math.max(currentPrice - d, m);
    }

    return gamesCount;
}
```
