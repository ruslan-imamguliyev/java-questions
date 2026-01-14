# Average Salary Excluding Minimum and Maximum

_Score_: 1

Calculate the **average salary** of employees, but **exclude** both the minimum and maximum salaries from the calculation.

### Steps
1. Identify the **minimum** salary
2. Identify the **maximum** salary
3. Remove **one** occurrence of min and **one** occurrence of max
4. Calculate the average of the remaining salaries

### Important Notes
- If there are multiple occurrences of min/max, remove only one of each
- At least 3 salaries will be given (so at least 1 remains after exclusions)
- Return the average as a floating-point number

### Input Format
- An array of integers representing salaries

### Output Format
- A float representing the average (excluding min and max)

## Placeholder:
```java

public static double solution(int[] salary) {
    // Write your code here
    return 0.0;
}
```

## Examples:
```
Input: salary=[4000, 3000, 1000, 2000]
Output: 2500.0

Input: salary=[1000, 2000, 3000]
Output: 2000.0

Input: salary=[6000, 5000, 4000, 3000, 2000, 1000]
Output: 3500.0

Input: salary=[8000, 9000, 2000, 3000, 6000, 1000]
Output: 4750.0

Input: salary=[100, 101, 102]
Output: 101.0
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<int[]> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(new int[]{4000, 3000, 1000, 2000});
    testCases.add(new int[]{1000, 2000, 3000});
    testCases.add(new int[]{6000, 5000, 4000, 3000, 2000, 1000});
    testCases.add(new int[]{8000, 9000, 2000, 3000, 6000, 1000});
    testCases.add(new int[]{100, 101, 102});
    testCases.add(new int[]{500, 100, 200});
    testCases.add(new int[]{16000, 12000, 14000, 18000});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(8) + 3; // [3,10]
        Set<Integer> set = new HashSet<>();

        while (set.size() < n) {
            set.add((random.nextInt(99) + 1) * 1000);
        }

        int[] salary = new int[n];
        int idx = 0;
        for (int s : set) {
            salary[idx++] = s;
        }

        testCases.add(salary);
    }

    // Verify
    for (int[] salary : testCases) {
        double result = solution(salary);
        double expected = correctSolution(salary);

        if (Double.compare(result, expected) != 0) {
            throw new AssertionError(
                "Test case failed: salary=" + Arrays.toString(salary) +
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
public static double correctSolution(int[] salary) {
    int n = salary.length;
    if (n <= 2) {
        return 0.0;
    }
    int minn = salary[0];
    int maxx = salary[0];
    int summ = 0;
    for (int num: salary) {
        if (num > maxx) {
            maxx = num;
        }

        if (num < minn) {
            minn = num;
        }
        summ += num;
    }

    return (summ - minn - maxx) / (n - 2);
}
```

