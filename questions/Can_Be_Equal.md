# Can Be Equal

_Score_: 2

You are given two integer arrays of equal length `target` and `arr`. In one step, you can select any **non-empty subarray** of `arr` and reverse it. You are allowed to make any number of steps.

Return `true` if you can make `arr` equal to `target` or `false` otherwise.

### Operations Allowed
- Select any non-empty subarray from `arr` array
- Reverse that subarray
- Repeat as many times as you want

### Key Insight
If you can reverse any subarray any number of times, you can essentially **rearrange** the array in any order you want.

### Solution Approach
Two arrays can be made equal if and only if they contain the **same elements with the same frequencies**.

### Input Format
- `target`: Target integer array
- `arr`: Integer array to transform

### Output Format
- Boolean: `True` if `arr` can be made equal to `target`, `False` otherwise

### Example
- `target = [1,2,3,4]`, `arr = [2,4,1,3]` → **True** (same elements)
- `target = [1,2,3,4]`, `arr = [1,2,3,5]` → **False** (different elements)

## Placeholder:
```java

public static boolean solution(int[] target, int[] arr) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: target=[1, 2, 3, 4], arr=[2, 4, 1, 3]
Output: True

Input: target=[7], arr=[7]
Output: True

Input: target=[1, 12], arr=[12, 1]
Output: True

Input: target=[3, 7, 9], arr=[3, 7, 11]
Output: False

Input: target=[1, 2, 3, 4, 5], arr=[5, 4, 3, 2, 1]
Output: True
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    List<int[]> targets = new ArrayList<>();
    List<int[]> arrs = new ArrayList<>();

    // Fixed test cases
    targets.add(new int[]{1, 2, 3, 4}); arrs.add(new int[]{2, 4, 1, 3});
    targets.add(new int[]{7});          arrs.add(new int[]{7});
    targets.add(new int[]{1, 12});      arrs.add(new int[]{12, 1});
    targets.add(new int[]{3, 7, 9});    arrs.add(new int[]{3, 7, 11});
    targets.add(new int[]{1, 1, 1, 1}); arrs.add(new int[]{1, 1, 1, 1});
    targets.add(new int[]{1, 2, 2, 3}); arrs.add(new int[]{1, 1, 2, 3});
    targets.add(new int[]{1, 2, 3});    arrs.add(new int[]{1, 2, 3});
    targets.add(new int[]{});            arrs.add(new int[]{});
    targets.add(new int[]{5, 5, 5});    arrs.add(new int[]{5, 5, 5});
    targets.add(new int[]{1, 2, 3, 4, 5}); arrs.add(new int[]{5, 4, 3, 2, 1});

    // Random test cases
    for (int i = 0; i < 2; i++) {
        int n = random.nextInt(11);
        int[] target = new int[n];
        for (int j = 0; j < n; j++) {
            target[j] = random.nextInt(20) + 1;
        }

        int[] arr;
        if (random.nextBoolean()) {
            arr = Arrays.copyOf(target, n);
            // shuffle arr
            for (int j = 0; j < n; j++) {
                int k = random.nextInt(n);
                int tmp = arr[j];
                arr[j] = arr[k];
                arr[k] = tmp;
            }
        } else {
            arr = new int[n];
            for (int j = 0; j < n; j++) {
                arr[j] = random.nextInt(20) + 1;
            }
        }

        targets.add(target);
        arrs.add(arr);
    }

    // Verify
    for (int i = 0; i < targets.size(); i++) {
        int[] target = targets.get(i);
        int[] arr = arrs.get(i);

        boolean result = solution(target, arr);
        boolean expected = correctSolution(target, arr);

        if (result != expected) {
            throw new AssertionError(
                "Test case failed: target=" + Arrays.toString(target) +
                ", arr=" + Arrays.toString(arr)
            );
        }
    }

    System.out.println("success");
    System.out.flush();
}
```

## Solution:
```java
public static boolean correctSolution(int[] target, int[] arr) {
    if (target.length != arr.length) {
        return false;
    }

    Map<Integer, Integer> arrCounts = new HashMap<>();
    Map<Integer, Integer> targetCounts = new HashMap<>();

    for (int x : arr) {
        arrCounts.put(x, arrCounts.getOrDefault(x, 0) + 1);
    }

    for (int x : target) {
        targetCounts.put(x, targetCounts.getOrDefault(x, 0) + 1);
    }

    return arrCounts.equals(targetCounts);
}
```
