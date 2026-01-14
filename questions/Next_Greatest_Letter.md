# Next Greatest Letter

_Score_: 1

Given a **sorted** list of letters and a target letter, find the **smallest letter** in the list that is **greater than** the target. Letters **wrap around** (circular).

### Wrap-Around Rule
If no letter is greater than target, return the **first letter** in the list.

### Binary Search Approach
Since the array is sorted, use binary search:
1. Find the leftmost position where `letter > target`
2. If no such position exists, return `letters[0]`

### Examples
`letters=['c','f','j']`, `target='a'` → **'c'** (first letter > 'a')

`letters=['c','f','j']`, `target='c'` → **'f'** (smallest letter > 'c')

`letters=['c','f','j']`, `target='d'` → **'f'** (smallest letter > 'd')

`letters=['c','f','j']`, `target='j'` → **'c'** (wrap around!)

`letters=['c','f','j']`, `target='k'` → **'c'** (wrap around!)

### Input Format
- `letters`: Sorted list of lowercase letters (may contain duplicates)
- `target`: A single lowercase letter

### Output Format
- Character: the next greatest letter (with wrap-around)

## Placeholder:
```java

public static String solution(String[] letters, String target) {
    // Write your code here
    return "";
}
```

## Examples:
```
Input: letters=['c', 'f', 'j'], target='a'
Output: 'c'

Input: letters=['c', 'f', 'j'], target='c'
Output: 'f'

Input: letters=['c', 'f', 'j'], target='d'
Output: 'f'

Input: letters=['c', 'f', 'j'], target='j'
Output: 'c'

Input: letters=['a', 'b'], target='z'
Output: 'a'
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<Object[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new Object[]{new String[]{"c", "f", "j"}, "a"});
    testCases.add(new Object[]{new String[]{"c", "f", "j"}, "c"});
    testCases.add(new Object[]{new String[]{"c", "f", "j"}, "d"});
    testCases.add(new Object[]{new String[]{"c", "f", "j"}, "g"});
    testCases.add(new Object[]{new String[]{"c", "f", "j"}, "j"});
    testCases.add(new Object[]{new String[]{"c", "f", "j"}, "k"});
    testCases.add(new Object[]{new String[]{"e","e","e","e","e","e","n","n","n","n"}, "e"});
    testCases.add(new Object[]{new String[]{"a", "b"}, "z"});
    testCases.add(new Object[]{new String[]{"a", "c"}, "b"});
    testCases.add(new Object[]{new String[]{"x", "y", "y"}, "x"});

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(9) + 2; // [2,10]
        String[] letters = new String[n];
        for (int j = 0; j < n; j++) {
            letters[j] = String.valueOf(chars.charAt(random.nextInt(chars.length())));
        }
        Arrays.sort(letters);
        String target = String.valueOf(chars.charAt(random.nextInt(chars.length())));
        testCases.add(new Object[]{letters, target});
    }

    // Verify
    for (Object[] tc : testCases) {
        String[] letters = (String[]) tc[0];
        String target = (String) tc[1];

        String result = solution(letters, target);
        String expected = correctSolution(letters, target);

        if (!result.equals(expected)) {
            throw new AssertionError(
                "Test case failed: letters=" + Arrays.toString(letters) +
                ", target='" + target +
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
public static String correctSolution(String[] letters, String target) {
    int left = 0, right = letters.length;

    while (left < right) {
        int mid = (left + right) / 2;
        if (letters[mid].compareTo(target) <= 0) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }

    if (left == letters.length) {
        return letters[0];
    } else {
        return letters[left];
    }
}
```
