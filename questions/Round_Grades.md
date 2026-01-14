# Round Grades

_Score_: 2

Round student grades according to university rounding rules.

### Grading Policy
- Any grade less than **40** is a failing grade.

### Rounding Rules
1. If the difference between the `grade` and the next multiple of 5 is less than 3, round `grade` up to the next multiple of 5.
2. If the value of `grade` is less than **38**, no rounding occurs as the result will still be a failing grade.

### Algorithm
```python
# If the grade is less than 38, no rounding.
if grade < 38:
    return grade

# Find the difference to the next multiple of 5.
diff = 5 - (grade % 5)

# If the difference is less than 3, round up.
if diff < 3:
    return grade + diff
else:
    return grade
```

### Examples
- Grade **84**: next multiple = 85, diff = 1 < 3 → Round to **85**
- Grade **29**: < 38 (failing) → Keep **29**
- Grade **57**: next multiple = 60, diff = 3 ≥ 3 → Keep **57**
- Grade **38**: next multiple = 40, diff = 2 < 3 → Round to **40**
- Grade **73**: next multiple = 75, diff = 2 < 3 → Round to **75**

### Input Format
- List of integer grades (0-100)

### Output Format
- List of rounded grades

## Placeholder:
```java

public static int[] solution(int[] grades) {
    // Write your code here
    return new int[0];
}
```

## Examples:
```
Input: [84, 29, 57]
Output: [85, 29, 57]

Input: [73, 67, 38, 33]
Output: [75, 67, 40, 33]

Input: [37, 38, 39, 40]
Output: [37, 40, 40, 40]

Input: [0, 1, 2, 3, 4]
Output: [0, 1, 2, 3, 4]

Input: [88, 89, 90]
Output: [90, 90, 90]
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{84, 29, 57});
    testCases.add(new int[]{73, 67, 38, 33});
    testCases.add(new int[]{37, 38, 39, 40});
    testCases.add(new int[]{99, 100, 98});
    testCases.add(new int[]{0, 1, 2, 3, 4});
    testCases.add(new int[]{55, 56, 57, 58, 59});
    testCases.add(new int[]{61, 62, 63});
    testCases.add(new int[]{75, 76, 77});
    testCases.add(new int[]{88, 89, 90});
    testCases.add(new int[]{33, 34, 35, 36, 37});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int size = random.nextInt(10) + 1; // [1,10]
        int[] grades = new int[size];
        for (int j = 0; j < size; j++) {
            grades[j] = random.nextInt(101); // [0,100]
        }
        testCases.add(grades);
    }

    // Verify
    for (int[] testCase : testCases) {
        int[] result = solution(testCase);
        int[] expected = correctSolution(testCase);

        if (!Arrays.equals(result, expected)) {
            throw new AssertionError(
                "Test case failed: " + Arrays.toString(testCase) +
                ", expected=" + Arrays.toString(expected) +
                ", got=" + Arrays.toString(result)
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static int[] correctSolution(int[] grades) {
    int[] rounded = new int[grades.length];

    for (int i = 0; i < grades.length; i++) {
        int grade = grades[i];

        if (grade < 38) {
            rounded[i] = grade;
        } else {
            int nextMultipleOf5 = ((grade / 5) + 1) * 5;
            if (nextMultipleOf5 - grade < 3) {
                rounded[i] = nextMultipleOf5;
            } else {
                rounded[i] = grade;
            }
        }
    }

    return rounded;
}
```
