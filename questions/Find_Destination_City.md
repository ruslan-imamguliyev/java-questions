# Find Destination City

_Score_: 1

Find the **destination city** in a travel path - the city that has **no outgoing path** to any other city.

### Graph Interpretation
- Each path `[cityA, cityB]` is a **directed edge** from cityA to cityB
- Destination city: has **incoming edges** but **no outgoing edges**

### Algorithm
1. Collect all cities that **start** a path (have outgoing edges)
2. Collect **all** cities (start + end)
3. Find the city that appears as an endpoint but never as a starting point

### Example 1
Paths: `[["London","New York"], ["New York","Lima"], ["Lima","Sao Paulo"]]`
```
London → New York → Lima → Sao Paulo
```
- Starting cities: {London, New York, Lima}
- All cities: {London, New York, Lima, Sao Paulo}
- Destination (no outgoing): **"Sao Paulo"**

### Example 2
Paths: `[["B","C"], ["D","B"], ["C","A"]]`
```
D → B → C → A
```
- Starting cities: {B, D, C}
- All cities: {B, D, C, A}
- Destination: **"A"**

### Example 3
Paths: `[["A","Z"]]`
```
A → Z
```
- Starting cities: {A}
- All cities: {A, Z}
- Destination: **"Z"**

### Properties
- Paths form a **chain** (not a cycle)
- Exactly **one** city has no outgoing path
- Guaranteed to have a unique answer

### Alternative Approach
Use a set:
1. Add all starting cities to `outgoing_set`
2. For each path `[a, b]`:
   - If `b` not in `outgoing_set`, it's the destination

### Input Format
- List of paths: `[[cityA, cityB], ...]`
- Each path is a directed edge

### Output Format
- String: the destination city name

## Placeholder:
```java

public static String solution(List<List<String>> paths) {
    // Write your code here
    return "";
}
```

## Examples:
```
Input: paths=[['London', 'New York'], ['New York', 'Lima'], ['Lima', 'Sao Paulo']]
Output: 'Sao Paulo'

Input: paths=[['B', 'C'], ['D', 'B'], ['C', 'A']]
Output: 'A'

Input: paths=[['A', 'Z']]
Output: 'Z'

Input: paths=[['one', 'two']]
Output: 'two'

Input: paths=[['start', 'middle'], ['middle', 'end']]
Output: 'end'
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();
    List<List<List<String>>> testCases = new ArrayList<>();

    // Fixed test cases
    testCases.add(List.of(
            List.of("London", "New York"),
            List.of("New York", "Lima"),
            List.of("Lima", "Sao Paulo")
    ));
    testCases.add(List.of(
            List.of("B", "C"),
            List.of("D", "B"),
            List.of("C", "A")
    ));
    testCases.add(List.of(
            List.of("A", "Z")
    ));
    testCases.add(List.of(
            List.of("qwe", "rty"),
            List.of("rty", "uio"),
            List.of("uio", "pas")
    ));
    testCases.add(List.of(
            List.of("one", "two")
    ));
    testCases.add(List.of(
            List.of("alpha", "beta"),
            List.of("beta", "gamma"),
            List.of("gamma", "delta"),
            List.of("delta", "epsilon")
    ));
    testCases.add(List.of(
            List.of("1", "2"),
            List.of("2", "3")
    ));
    testCases.add(List.of(
            List.of("first", "second"),
            List.of("second", "third"),
            List.of("third", "fourth"),
            List.of("fourth", "fifth"),
            List.of("fifth", "sixth")
    ));
    testCases.add(List.of(
            List.of("x", "y")
    ));
    testCases.add(List.of(
            List.of("start", "middle"),
            List.of("middle", "end")
    ));

    // Random test cases
    String chars = "abcdefghijklmnopqrstuvwxyz";
    for (int t = 0; t < 2; t++) {
        int n = random.nextInt(5) + 1; // [1,5]
        List<String> cities = new ArrayList<>();
        for (int i = 0; i < n + 1; i++) {
            int len = random.nextInt(4) + 3; // [3,6]
            StringBuilder sb = new StringBuilder();
            for (int j = 0; j < len; j++) {
                sb.append(chars.charAt(random.nextInt(chars.length())));
            }
            cities.add(sb.toString());
        }

        List<List<String>> paths = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            paths.add(List.of(cities.get(i), cities.get(i + 1)));
        }
        testCases.add(paths);
    }

    // Verify
    for (List<List<String>> paths : testCases) {
        String result = solution(paths);
        String expected = correctSolution(paths);

        if (!result.equals(expected)) {
            throw new AssertionError(
                    "Test case failed: paths=" + paths +
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
public static String correctSolution(List<List<String>> paths) {
    Set<String> startingCities = new HashSet<>();
    Set<String> allCities = new HashSet<>();

    for (List<String> path : paths) {
        String start = path.get(0);
        String end = path.get(1);

        startingCities.add(start);
        allCities.add(start);
        allCities.add(end);
    }

    for (String city : allCities) {
        if (!startingCities.contains(city)) {
            return city;
        }
    }

    return "";
}
```
