# Algorithm Review

## Permutation
```text
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

```Java
List<List<Integer>> res = new ArrayList<>();

public List<List<Integer>> permute(int[] nums) {
    backtrack(nums, new ArrayList<Integer>());
    return res;
}

public void backtrack(int[] nums, List<Integer> al) {
    if (al.size() == nums.length) {
        res.add(new ArrayList<Integer>(al));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (!al.contains(nums[i])) {
            al.add(nums[i]);
            backtrack(nums, al);
            al.remove(al.size() - 1);
        }
    }
}
```

```Java
private static List<String> permute(String str) {
        StringBuilder chars = new StringBuilder(str);
        List<String> results = new LinkedList<>();
        permuteHelper(chars, new LinkedHashSet<>(), results);
        return results;
    }

    private static void permuteHelper(StringBuilder chars, Set<Character> permute, List<String> results) {
        if (permute.size() == chars.length()) {
            final StringBuilder sb = new StringBuilder();
            permute.forEach(entry -> {
                sb.append(entry);
            });
            results.add(sb.toString());
            return;
        }

        for (int index = 0; index < chars.length(); index++) {
            char val = chars.charAt(index);
            if (!permute.contains(val)) {
                permute.add(val);
                permuteHelper(chars, permute, results);
                permute.remove(val);
            }
        }
    }
```

## Next Permutation

> Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.
> If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).
> The replacement must be in-place and use only constant extra memory.
Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

```Java
public void nextPermutation(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }
    int n = nums.length;
    int p = -1;
    for (int i = n - 2; i >= 0; i--) {
        if (nums[i] < nums[i + 1]) {
            int pivot = i + 1;
            for (int j = i + 1; j < n; j++) {
                if (nums[j] <= nums[i]) {
                    break;
                }
                pivot = j;
            }
            int temp = nums[pivot];
            nums[pivot] = nums[i];
            nums[i] = temp;
            p = i;
            break;
        }
    }
    Arrays.sort(nums, p + 1, n);
}
```

## Combination Sum

```text
Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.
The same repeated number may be chosen from candidates unlimited number of times.
Note:
•	All numbers (including target) will be positive integers.
•	The solution set must not contain duplicate combinations.

Example 1:
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
Example 2:
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

```Java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    backtrack(res, candidates, target, new ArrayList<Integer>(), 0, 0);
    return res;
}

public void backtrack(List<List<Integer>> res, int[] nums, int target, List<Integer> al, int sum, int index) {
    if (sum == target) {
        res.add(new ArrayList<Integer>(al));
    } else if (sum < target) {
        for (int i = index; i < nums.length; i++) {
            sum += nums[i];
            al.add(nums[i]);

            backtrack(res, nums, target, al, sum, i);

            sum -= nums[i];
            al.remove(al.size() - 1);
        }
    }
}
```

## Group Anagrams

```text
Given an array of strings, group anagrams together.
Example:
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
Note:
•	All inputs will be in lowercase.
•	The order of your output does not matter.
```

```Java
```

## Longest Palindromic Substring

```text
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
```

```Java
public String longestPalindrome(String s) {
    if (s == null || s.length() < 1) {
        return "";
    }
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    int i = left, j = right;
    while (i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j)) {
        i--;
        j++;
    }
    return j - i - 1;
}
```

## Valid Palindrome

```
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Note: For the purpose of this problem, we define empty string as valid palindrome.
```

```Java
boolean isPalindrome(String s) {
    if (null == s || s.isEmpty()) {
        return true;
    }
    int left = 0, right = s.length() - 1;
    while (left < right) {
        char leftChar = s.charAt(left);
        char rightChar = s.charAt(right);
        if (!Character.isLetterOrDigit(leftChar)) {
            left++;
        } else if (!Character.isLetterOrDigit(rightChar)) {
            right--;
        } else if (Character.toLowerCase(leftChar) != Character.toLowerCase(rightChar)) {
            return false;
        } else {
            left++;
            right--;
        }
    }
    return true;
}
```

## Palindrome Number

```text
Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.
```

```Java
boolean isPalindrome(int x) {
    if (x == 0) {
        return true;
    }
    if (x < 0 || x % 10 == 0) {
        return false;
    }
    int value = x;
    int reverseValue = 0;
    while (value > 0) {
        reverseValue = reverseValue * 10 + value % 10;
        value = value / 10;
    }
    if (x == reverseValue) {
        return true;
    }
    return false;
}
```

## Number of Islands

```text
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
Example 1:
Input:
11110
11010
11000
00000
Output: 1
Example 2:
Input:
11000
11000
00100
00011

Output: 3

```
```Java
public int numIslands(char[][] grid) {
    int countOfIsland = 0;
    for(int i = 0; i <grid.length;i++)
        for(int j =0; j < grid[0].length ; j++){
            if(grid[i][j] == '1'){
                fillWater(grid,i,j);
                countOfIsland++;
            }
        }
    return countOfIsland;
}

public void fillWater(char[][] grid, int i, int j){
     if(i < 0 || i > grid.length-1 || j < 0 || j > grid[0].length-1)
        return;
    if(grid[i][j] == '0')
        return;
    grid[i][j] ='0';
    fillWater(grid,i+1,j);
    fillWater(grid,i-1,j);
    fillWater(grid,i,j+1);
    fillWater(grid,i,j-1);
}
```

## Find the pair of integers in an array whose sum is x.

```text
```

```Java
```

## Kth Largest Element in an Array

```text
Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Example 1:

Input: [3,2,1,5,6,4] and k = 2
Output: 5
Example 2:

Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
Note:
You may assume k is always valid, 1 ≤ k ≤ array's length.
```

```Java
public int findKthLargest(int[] nums, int k) {
    Arrays.sort(nums);
    int len = nums.length;
    int j = 1;
    for (int i = 0; i < len; i++) {
        if (j == k) return nums[len - i - 1];
        else j++;
    }
    return -1;
}
```

## Find the second largest number is an array

```text
Find the second largest number is an array
```

```Java
int find2ndLatestElement(int arr[]) {
    if (null == arr || arr.length < 2) {
        return Integer.MIN_VALUE;
    }

    int firstVal = Integer.MIN_VALUE;
    int secondVal = Integer.MIN_VALUE;
    for (int index = 0; index < arr.length; index++) {
        int currentVal = arr[index];
        if (currentVal > firstVal) {
            secondVal = firstVal;
            firstVal = currentVal;
        } else if (currentVal > secondVal && currentVal != firstVal) {
            secondVal = currentVal;
        }
    }
    return secondVal;
}
```

## Two Sum

```text
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

```Java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap();
    for (int i = 0; i < nums.length; i++) {
        int a = nums[i];
        // note that the numbers in the array can be negative as well
        if (map.get(target-a) != null) {
            return new int[]{map.get(target-a), i};
        }
        // The following should be after the check above,
        // otherwise it will fail for the case where target = 6 and there's a 3 in the original array.
        map.put (a, i);
    }
    return null;
}
```

## Number of Islands

```text
```

```Java
```

## Number of Islands

```text
```

```Java
```