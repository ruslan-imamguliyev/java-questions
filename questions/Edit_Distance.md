# Edit Distance

_Score_: 3

Calculate the **minimum number of operations** needed to convert one string to another. Also known as **Levenshtein Distance**.

### Allowed Operations
1. **Insert** a character
2. **Delete** a character
3. **Replace** a character

Each operation counts as **1** edit.

### Example Transformation
Convert "horse" → "ros":
1. Replace 'h' with 'r' → "rorse"
2. Delete 'r' → "rose"
3. Delete 'e' → "ros"
**Total: 3 operations**

### Dynamic Programming Solution
Use a 2D table `dp[i][j]` = minimum edits to convert `word1[0...i-1]` to `word2[0...j-1]`

**Base cases:**
- `dp[0][j] = j` (insert j characters)
- `dp[i][0] = i` (delete i characters)

**Recurrence:**
```
If word1[i-1] == word2[j-1]:
    dp[i][j] = dp[i-1][j-1]  (no operation needed)
Else:
    dp[i][j] = 1 + min(
        dp[i-1][j],     # Delete from word1
        dp[i][j-1],     # Insert into word1
        dp[i-1][j-1]    # Replace in word1
    )
```

### Example: "horse" → "ros"
DP Table:
```
    ""  r  o  s
""   0  1  2  3
h    1  1  2  3
o    2  2  1  2
r    3  2  2  2
s    4  3  3  2
e    5  4  4  3
```
Result: `dp[5][3]` = **3**

### Example 2: "intention" → "execution"
- Delete 'i', 'n', 't'
- Replace 'e' with 'e' (match)
- Insert 'x', 'e', 'c'
- More operations...
**Total: 5 operations**

### Edge Cases
- `"" → "abc"`: 3 insertions
- `"abc" → ""`: 3 deletions
- `"abc" → "abc"`: 0 operations
- `"a" → "b"`: 1 replacement

### Complexity
- Time: O(m × n) where m, n are string lengths
- Space: O(m × n) for DP table

### Input Format
- `word1`: Source string
- `word2`: Target string

### Output Format
- Integer: minimum number of edit operations

## Placeholder:
```java

public static int solution(String word1, String word2) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: word1='horse', word2='ros'
Output: 3

Input: word1='intention', word2='execution'
Output: 5

Input: word1='abc', word2='yabd'
Output: 2

Input: word1='a', word2='a'
Output: 0

Input: word1='a', word2='b'
Output: 1
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<String[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new String[]{"horse", "ros"});
    testCases.add(new String[]{"intention", "execution"});
    testCases.add(new String[]{"abc", "yabd"});
    testCases.add(new String[]{"", ""});
    testCases.add(new String[]{"a", "a"});
    testCases.add(new String[]{"a", "b"});
    testCases.add(new String[]{"abcdef", "azced"});
    testCases.add(new String[]{"abc", "def"});
    testCases.add(new String[]{"sunday", "saturday"});
    testCases.add(new String[]{"", "abc"});

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < 2; i++) {
        int len1 = random.nextInt(9); // 0..8
        int len2 = random.nextInt(9);

        StringBuilder w1 = new StringBuilder();
        StringBuilder w2 = new StringBuilder();

        for (int j = 0; j < len1; j++) {
            w1.append(chars.charAt(random.nextInt(chars.length())));
        }
        for (int j = 0; j < len2; j++) {
            w2.append(chars.charAt(random.nextInt(chars.length())));
        }

        testCases.add(new String[]{w1.toString(), w2.toString()});
    }

    // Verify
    for (String[] tc : testCases) {
        String word1 = tc[0];
        String word2 = tc[1];

        int result = solution(word1, word2);
        int expected = correctSolution(word1, word2);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: word1='" + word1 +
                "', word2='" + word2 +
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
public static int correctSolution(String word1, String word2) {
    int m = word1.length();
    int n = word2.length();

    int[][] dp = new int[m + 1][n + 1];

    for (int i = 0; i <= m; i++) {
        for (int j = 0; j <= n; j++) {
            if (i == 0) {
                dp[i][j] = j;
            } else if (j == 0) {
                dp[i][j] = i;
            } else if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = 1 + Math.min(
                        dp[i - 1][j],                 // delete
                        Math.min(dp[i][j - 1],        // insert
                                 dp[i - 1][j - 1])    // replace
                );
            }
        }
    }

    return dp[m][n];
}
```
