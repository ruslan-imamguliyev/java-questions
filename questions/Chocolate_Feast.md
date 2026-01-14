# Chocolate Feast

_Score_: 1

You have **n dollars** to buy chocolates that cost **c dollars** each. After eating a chocolate, you get a wrapper. You can turn in **m wrappers** to get **1 free chocolate**.

### The Game
1. Buy as many chocolates as you can with your money
2. Collect wrappers after eating chocolates
3. Exchange every m wrappers for 1 free chocolate
4. Repeat until you can't exchange anymore

### Example
n = 15, c = 3, m = 2
- Buy: 15 ÷ 3 = **5 chocolates**
- Wrappers: 5
- Exchange: 5 ÷ 2 = 2 free chocolates (1 wrapper left)
- New wrappers: 1 + 2 = 3
- Exchange: 3 ÷ 2 = 1 free chocolate (1 wrapper left)
- New wrappers: 1 + 1 = 2
- Exchange: 2 ÷ 2 = 1 free chocolate (0 wrappers left)
- Total chocolates: 5 + 2 + 1 + 1 = **9**

### Input Format
- `n`: Money available
- `c`: Cost per chocolate
- `m`: Wrappers needed for 1 free chocolate

### Output Format
- Integer: total chocolates you can eat

## Placeholder:
```java

public static int solution(int n, int c, int m) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: n=15, c=3, m=2
Output: 9

Input: n=10, c=2, m=5
Output: 6

Input: n=12, c=4, m=4
Output: 3

Input: n=20, c=4, m=3
Output: 7

Input: n=30, c=5, m=2
Output: 11
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    int[][] testCases = {
        {15, 3, 2},
        {10, 2, 5},
        {12, 4, 4},
        {5, 5, 2},
        {20, 4, 3},
        {9, 2, 3},
        {25, 5, 6},
        {30, 5, 2},
        {7, 3, 3},
        {10, 3, 3}
    };

    // Fixed test cases
    for (int[] tc : testCases) {
        int n = tc[0], c = tc[1], m = tc[2];
        int result = solution(n, c, m);
        int expected = correctSolution(n, c, m);
        if (result != expected) {
            throw new AssertionError(
                "Test case failed: n=" + n + ", c=" + c + ", m=" + m +
                ", expected=" + expected + ", got=" + result
            );
        }
    }

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(91) + 10; // [10,100]
        int c = random.nextInt(10) + 1;  // [1,10]
        int m = random.nextInt(9) + 2;   // [2,10]

        int result = solution(n, c, m);
        int expected = correctSolution(n, c, m);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: n=" + n + ", c=" + c + ", m=" + m +
                ", expected=" + expected + ", got=" + result
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static int correctSolution(int n, int c, int m) {
    int chocolates = n / c;
    int wrappers = chocolates;

    while (wrappers >= m) {
        int extraBars = wrappers / m;
        chocolates += extraBars;
        wrappers = wrappers % m + extraBars;
    }

    return chocolates;
}
```
