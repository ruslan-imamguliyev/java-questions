# Unplaced Fruits

_Score_: 2

Place fruits into baskets following a **greedy first-fit** strategy. Each fruit has a certain quantity, and each basket has a capacity. A fruit can only be placed in a basket if the basket's capacity is sufficient and the basket is not already occupied.

### Placement Rules
- Process fruits in the given order
- For each fruit, find the **first available basket** that can hold it
- A basket can hold at most **one fruit**
- If a fruit is placed, that basket becomes occupied
- Count how many fruits **cannot** be placed

### Algorithm
1. Iterate through each fruit in order
2. For each fruit, scan baskets from left to right
3. Place fruit in first unoccupied basket with sufficient capacity
4. If no suitable basket found, fruit remains unplaced

### Examples
- `fruits=[10,20,5]`, `baskets=[15,30,5]`
  - Fruit 10 → basket 0 (capacity 15) ✓
  - Fruit 20 → basket 1 (capacity 30) ✓
  - Fruit 5 → basket 2 (capacity 5) ✓
  - Unplaced: **0**

- `fruits=[10,20,5]`, `baskets=[5,10,15]`
  - Fruit 10 → basket 1 (capacity 10) ✓
  - Fruit 20 → no suitable basket ✗
  - Fruit 5 → basket 0 (capacity 5) ✓
  - Unplaced: **1**

### Input Format
- `fruits`: List of fruit quantities
- `baskets`: List of basket capacities

### Output Format
- Integer: number of fruits that cannot be placed

## Placeholder:
```java

public static int solution(int[] fruits, int[] baskets) {
    // Write your code here
    return 0;
}
```

## Examples:
```
Input: fruits=[10, 20, 5], baskets=[15, 30, 5]
Output: 0

Input: fruits=[10, 20, 5], baskets=[5, 10, 15]
Output: 1

Input: fruits=[10, 20, 5], baskets=[30, 10, 20]
Output: 0

Input: fruits=[5, 5, 5], baskets=[10, 10, 10]
Output: 0

Input: fruits=[10, 10, 10], baskets=[5, 5, 5]
Output: 3
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    List<int[]> fruitsList = new ArrayList<>();
    List<int[]> basketsList = new ArrayList<>();

    // Fixed test cases
    fruitsList.add(new int[]{10, 20, 5}); basketsList.add(new int[]{15, 30, 5});
    fruitsList.add(new int[]{10, 20, 5}); basketsList.add(new int[]{5, 10, 15});
    fruitsList.add(new int[]{10, 20, 5}); basketsList.add(new int[]{30, 10, 20});
    fruitsList.add(new int[]{5, 5, 5});   basketsList.add(new int[]{10, 10, 10});
    fruitsList.add(new int[]{10, 10, 10}); basketsList.add(new int[]{5, 5, 5});
    fruitsList.add(new int[]{10, 5, 20}); basketsList.add(new int[]{20, 10, 5});
    fruitsList.add(new int[]{1, 2, 3, 4, 5}); basketsList.add(new int[]{5, 4, 3, 2, 1});
    fruitsList.add(new int[]{5, 4, 3, 2, 1}); basketsList.add(new int[]{1, 2, 3, 4, 5});
    fruitsList.add(new int[]{100}); basketsList.add(new int[]{50});
    fruitsList.add(new int[]{50});  basketsList.add(new int[]{100});

    // Random test cases
    for (int t = 0; t < 2; t++) {
        int n = random.nextInt(6) + 1;
        int[] fruits = new int[n];
        int[] baskets = new int[n];

        for (int i = 0; i < n; i++) {
            fruits[i] = random.nextInt(30) + 1;
            baskets[i] = random.nextInt(30) + 1;
        }

        fruitsList.add(fruits);
        basketsList.add(baskets);
    }
```

## Solution:
```java
public static int correctSolution(int[] fruits, int[] baskets) {
    int n = fruits.length;
    boolean[] basketOccupied = new boolean[n];
    int placedCount = 0;

    for (int fruitQuantity : fruits) {
        boolean placed = false;
        for (int i = 0; i < n; i++) {
            if (!basketOccupied[i] && baskets[i] >= fruitQuantity) {
                basketOccupied[i] = true;
                placed = true;
                break;
            }
        }
        if (placed) {
            placedCount++;
        }
    }

    return n - placedCount;
}
```
