# Find the Town Judge

_Score_: 2

In a town, there are `n` people labeled from `1` to `n`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:
1. The town judge trusts nobody.
2. Everybody (except for the town judge) trusts the town judge.
3. There is exactly one person that satisfies properties 1 and 2.

You are given an array `trust` where `trust[i] = [a, b]` representing that the person labeled `a` trusts the person labeled `b`.

Return the label of the town judge if the town judge exists and can be identified, or return **-1** otherwise.

### Example 1
`n = 2, trust = [[1, 2]]`
- Person 1 trusts person 2.
- Person 2 trusts nobody and is trusted by person 1.
- Output: **2**

### Example 2
`n = 3, trust = [[1, 3], [2, 3]]`
- Persons 1 and 2 trust person 3.
- Person 3 trusts nobody and is trusted by 2 people (n-1).
- Output: **3**

### Example 3
`n = 3, trust = [[1, 3], [2, 3], [3, 1]]`
- Person 3 trusts person 1, so they cannot be the judge.
- No one satisfies both conditions.
- Output: **-1**

### Constraints
- `1 <= n <= 1000`
- `0 <= trust.length <= 10^4`
- `trust[i].length == 2`
- All pairs in `trust` are unique.

## Placeholder:
```java

public static int solution(int n, int[][] trust) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: n=2, trust=[[1, 2]]
Output: 2

Input: n=3, trust=[[1, 3], [2, 3]]
Output: 3

Input: n=3, trust=[[1, 3], [2, 3], [3, 1]]
Output: -1

Input: n=1, trust=[]
Output: 1

Input: n=4, trust=[[2, 1], [3, 1], [4, 1]]
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
        int len1 = random.nextInt(9);
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
public static int correctSolution(int n, int[][] trust) {
    // Special case: only one person and no trust relationships
    if (n == 1 && (trust == null || trust.length == 0)) {
        return 1;
    }

    int[] trusts = new int[n + 1];
    int[] trustedBy = new int[n + 1];

    for (int[] pair : trust) {
        int a = pair[0];
        int b = pair[1];
        trusts[a]++;
        trustedBy[b]++;
    }

    for (int i = 1; i <= n; i++) {
        if (trusts[i] == 0 && trustedBy[i] == n - 1) {
            return i;
        }
    }

    return -1;
}
```
