# Are Sentences Similar

_Score_: 3

Given two sentences and a list of similar word pairs, determine if the sentences are **similar**.

### Similarity Rules
Two sentences are similar if **all** of the following conditions are met:
1. They have the **same length** (same number of words)
2. For each position i, the words at position i in both sentences must be:
   - **Equal**, OR
   - **Similar** according to the given pairs

### Important Notes
- Similarity is **symmetric**: if word1 is similar to word2, then word2 is similar to word1
- Similarity is **not transitive**: if A~B and B~C, it doesn't mean A~C
- Each word must match or be explicitly listed as similar

### Input Format
- `sentence1`: List of words (first sentence)
- `sentence2`: List of words (second sentence)
- `similarPairs`: List of word pairs indicating similarity

### Output Format
- Boolean: `True` if sentences are similar, `False` otherwise

## Placeholder:
```java

public static boolean solution(List<String> sentence1, List<String> sentence2, List<List<String>> similarPairs) {
    // Write your code here
    return false;
}
```

## Examples:
```
Input: sentence1=['great', 'acting', 'skills'], sentence2=['fine', 'drama', 'talent'], similarPairs=[['great', 'fine'], ['drama', 'acting'], ['skills', 'talent']]
Output: True

Input: sentence1=['I', 'am', 'happy', 'with', 'leetcode'], sentence2=['I', 'am', 'content', 'with', 'coding'], similarPairs=[['content', 'happy'], ['leetcode', 'coding']]
Output: True

Input: sentence1=['a', 'b'], sentence2=['b', 'a'], similarPairs=[['a', 'b']]
Output: True

Input: sentence1=['a'], sentence2=['a'], similarPairs=[]
Output: True

Input: sentence1=['strike', 'me'], sentence2=['strike', 'you'], similarPairs=[]
Output: False
```

## Verify code:
```java
public static void verify() {
    Random random = new Random();

    List<Object[]> testCases = new ArrayList<>();

    testCases.add(new Object[]{
            Arrays.asList("great", "acting", "skills"),
            Arrays.asList("fine", "drama", "talent"),
            Arrays.asList(
                    Arrays.asList("great", "fine"),
                    Arrays.asList("drama", "acting"),
                    Arrays.asList("skills", "talent")
            )
    });

    testCases.add(new Object[]{
            Arrays.asList("I", "am", "happy", "with", "leetcode"),
            Arrays.asList("I", "am", "content", "with", "coding"),
            Arrays.asList(
                    Arrays.asList("content", "happy"),
                    Arrays.asList("leetcode", "coding")
            )
    });

    testCases.add(new Object[]{
            Arrays.asList("a", "b"),
            Arrays.asList("b", "a"),
            Arrays.asList(Arrays.asList("a", "b"))
    });

    testCases.add(new Object[]{
            Arrays.asList("a"),
            Arrays.asList("a"),
            new ArrayList<>()
    });

    testCases.add(new Object[]{
            Arrays.asList("a", "b"),
            Arrays.asList("a", "c"),
            Arrays.asList(
                    Arrays.asList("b", "d"),
                    Arrays.asList("c", "e")
            )
    });

    testCases.add(new Object[]{
            Arrays.asList("strike", "me"),
            Arrays.asList("strike", "you"),
            new ArrayList<>()
    });

    testCases.add(new Object[]{
            Arrays.asList("the", "boy", "loves", "the", "girl"),
            Arrays.asList("the", "girl", "loves", "the", "boy"),
            Arrays.asList(Arrays.asList("boy", "girl"))
    });

    testCases.add(new Object[]{
            Arrays.asList("yes"),
            Arrays.asList("yes"),
            new ArrayList<>()
    });

    testCases.add(new Object[]{
            Arrays.asList("cool", "guy"),
            Arrays.asList("nice", "man"),
            Arrays.asList(
                    Arrays.asList("cool", "nice"),
                    Arrays.asList("guy", "man")
            )
    });

    testCases.add(new Object[]{
            Arrays.asList("one", "two", "three"),
            Arrays.asList("one", "two", "four"),
            Arrays.asList(
                    Arrays.asList("three", "fifteen"),
                    Arrays.asList("four", "sixteen"),
                    Arrays.asList("five", "seventeen")
            )
    });

    for (int t = 0; t < 2; t++) {
        int n = random.nextInt(5) + 1;

        List<String> words = new ArrayList<>();
        for (int i = 0; i < n * 2; i++) {
            int len = random.nextInt(4) + 2;
            StringBuilder sb = new StringBuilder();
            for (int j = 0; j < len; j++) {
                sb.append((char) ('a' + random.nextInt(26)));
            }
            words.add(sb.toString());
        }

        List<String> sentence1 = words.subList(0, n);
        List<String> sentence2 = words.subList(n, n * 2);

        int numPairs = random.nextInt(4);
        List<List<String>> pairs = new ArrayList<>();
        for (int i = 0; i < numPairs; i++) {
            String w1 = words.get(random.nextInt(words.size()));
            String w2 = words.get(random.nextInt(words.size()));
            pairs.add(Arrays.asList(w1, w2));
        }

        testCases.add(new Object[]{sentence1, sentence2, pairsairs});
    }

    for (Object[] testCase : testCases) {
        List<String> sentence1 = (List<String>) testCase[0];
        List<String> sentence2 = (List<String>) testCase[1];
        List<List<String>> similarPairs = (List<List<String>>) testCase[2];

        if (solution(sentence1, sentence2, similarPairs)
                != correct_solution(sentence1, sentence2, similarPairs)) {
            throw new AssertionError(
                    "Test case failed: sentence1=" + sentence1 +
                            ", sentence2=" + sentence2 +
                            ", similarPairs=" + similarPairs
            );
        }
    }

    System.out.println("success");
}
```

## Solution:
```java
public static boolean correctSolution(List<String> sentence1, List<String> sentence2, List<List<String>> similarPairs) {
    if (sentence1.size() != sentence2.size()) {
        return false;
    }

    Map<String, Set<String>> similarityMap = new HashMap<>();

    for (List<String> pair : similarPairs) {
        String word1 = pair.get(0);
        String word2 = pair.get(1);

        similarityMap.computeIfAbsent(word1, k -> new HashSet<>()).add(word2);
        similarityMap.computeIfAbsent(word2, k -> new HashSet<>()).add(word1);
    }

    for (int i = 0; i < sentence1.size(); i++) {
        String word1 = sentence1.get(i);
        String word2 = sentence2.get(i);

        if (word1.equals(word2)) {
            continue;
        }

        if (!similarityMap.containsKey(word1) || !similarityMap.get(word1).contains(word2)) {
            return false;
        }
    }

    return true;
}
```
