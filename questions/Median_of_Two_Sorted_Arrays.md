# Median of Two Sorted Arrays

_Score_: 3

Given two sorted arrays `nums1` and `nums2` of size m and n respectively, return the **median** of the two sorted arrays.

The overall run time complexity should be **O(log (m+n))**.

### Problem Details
- Both arrays are sorted in ascending order
- You need to find the median of the merged sorted array without actually merging them
- The median is the middle value in a sorted list
  - If the total length is odd, return the middle element
  - If the total length is even, return the average of the two middle elements

### Input Format
- `nums1`: List of integers (sorted in ascending order)
- `nums2`: List of integers (sorted in ascending order)

### Output Format
- Float: The median of the two sorted arrays

### Constraints
- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-10^6 <= nums1[i], nums2[i] <= 10^6`

## Placeholder:
```java

public static double solution(int[] nums1, int[] nums2) {
        // Write your code here
        return 0.0;
    }
```

## Examples:
```
Input: nums1=[1, 3], nums2=[2]
Output: 2.00000

Input: nums1=[1, 2], nums2=[3, 4]
Output: 2.50000

Input: nums1=[0, 0], nums2=[0, 0]
Output: 0.00000

Input: nums1=[], nums2=[1]
Output: 1.00000

Input: nums1=[2], nums2=[]
Output: 2.00000
```

## Verify code:
```java
public static void verify() {
        Random random = new Random();
        List<Object[]> testCases = new ArrayList<>();
        
        testCases.add(new Object[]{new int[]{1, 3}, new int[]{2}});
        testCases.add(new Object[]{new int[]{1, 2}, new int[]{3, 4}});
        testCases.add(new Object[]{new int[]{0, 0}, new int[]{0, 0}});
        testCases.add(new Object[]{new int[]{}, new int[]{1}});
        testCases.add(new Object[]{new int[]{2}, new int[]{}});
        testCases.add(new Object[]{new int[]{1, 3, 5, 7, 9}, new int[]{2, 4, 6, 8, 10}});
        testCases.add(new Object[]{new int[]{1, 2, 3, 4, 5}, new int[]{6, 7, 8, 9, 10}});
        testCases.add(new Object[]{new int[]{1}, new int[]{2, 3, 4, 5, 6}});
        testCases.add(new Object[]{new int[]{1, 2, 3}, new int[]{4, 5}});
        testCases.add(new Object[]{new int[]{-5, -3, -1}, new int[]{0, 2, 4}});
        testCases.add(new Object[]{new int[]{1, 3}, new int[]{2, 7}});
        testCases.add(new Object[]{new int[]{1, 2}, new int[]{1, 2}});
        testCases.add(new Object[]{new int[]{100000}, new int[]{100001}});
        testCases.add(new Object[]{new int[]{-1000000, 1000000}, new int[]{0}});
        
        // Add random test cases
        for (int i = 0; i < 5; i++) {
            int m = random.nextInt(11);
            int n = (m == 0) ? random.nextInt(10) + 1 : random.nextInt(11);
            
            int[] nums1 = new int[m];
            for (int j = 0; j < m; j++) {
                nums1[j] = random.nextInt(201) - 100;
            }
            Arrays.sort(nums1);
            
            int[] nums2 = new int[n];
            for (int j = 0; j < n; j++) {
                nums2[j] = random.nextInt(201) - 100;
            }
            Arrays.sort(nums2);
            
            testCases.add(new Object[]{nums1, nums2});
        }
        
        for (Object[] testCase : testCases) {
            int[] nums1 = (int[]) testCase[0];
            int[] nums2 = (int[]) testCase[1];
            double result = solution(nums1, nums2);
            double expected = correctSolution(nums1, nums2);
            if (Math.abs(result - expected) >= 1e-5) {
                throw new AssertionError(String.format(
                    "Test case failed: nums1=%s, nums2=%s. Expected %.5f, got %.5f",
                    Arrays.toString(nums1), Arrays.toString(nums2), expected, result
                ));
            }
        }
        
        System.out.println("success");
        System.out.flush();
    }

```

## Solution:
```java
public static double correctSolution(int[] nums1, int[] nums2) {
        // Ensure nums1 is the smaller array for optimization
        if (nums1.length > nums2.length) {
            int[] temp = nums1;
            nums1 = nums2;
            nums2 = temp;
        }
        
        int m = nums1.length;
        int n = nums2.length;
        int low = 0;
        int high = m;
        
        while (low <= high) {
            int partition1 = (low + high) / 2;
            int partition2 = (m + n + 1) / 2 - partition1;
            
            // Handle edge cases where partition is at the beginning or end
            double maxLeft1 = (partition1 == 0) ? Double.NEGATIVE_INFINITY : nums1[partition1 - 1];
            double minRight1 = (partition1 == m) ? Double.POSITIVE_INFINITY : nums1[partition1];
            
            double maxLeft2 = (partition2 == 0) ? Double.NEGATIVE_INFINITY : nums2[partition2 - 1];
            double minRight2 = (partition2 == n) ? Double.POSITIVE_INFINITY : nums2[partition2];
            
            // Check if we found the correct partition
            if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
                // If total length is even
                if ((m + n) % 2 == 0) {
                    return (Math.max(maxLeft1, maxLeft2) + Math.min(minRight1, minRight2)) / 2.0;
                }
                // If total length is odd
                else {
                    return Math.max(maxLeft1, maxLeft2);
                }
            } else if (maxLeft1 > minRight2) {
                // Move towards left in nums1
                high = partition1 - 1;
            } else {
                // Move towards right in nums1
                low = partition1 + 1;
            }
        }
        
        throw new IllegalArgumentException("Input arrays are not sorted");
    }
```
