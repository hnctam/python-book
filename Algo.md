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
public List<List<String>> groupAnagrams(String[] strs) {
    HashMap<String, List<String>> map = new HashMap<String, List<String>>();

    for (String st : strs) {
        char[] ar = st.toCharArray();
        Arrays.sort(ar);
        String key = String.valueOf(ar);

        List<String> l = map.getOrDefault(key, new ArrayList<>());
        l.add(st);
        map.put(key, l);
    }

    return new ArrayList<>(map.values());
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



## Longest substring without repeating characters

```txt
Given a string, find the length of the longest substring without repeating characters.

Example 1:

Input: "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
Example 2:

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

```java
public int lengthOfLongestSubstring(String s) {
    int n = s.length(), ans = 0;
    int[] index = new int[128]; // current index of character
    // try to extend the range [i, j]
    for (int j = 0, i = 0; j < n; j++) {
        i = Math.max(index[s.charAt(j)], i);
        ans = Math.max(ans, j - i + 1);
        index[s.charAt(j)] = j + 1;
    }
    return ans;
}
```

## String to Integer

```txt
Implement atoi which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.
```

```Java
int myAtoi(String str) {
    str = str.trim();

    StringBuilder sb = new StringBuilder();

    for (int i = 0; i < str.length(); i++) {
        char c = str.charAt(i);
        boolean isDigit = Character.isDigit(c);

        if (i == 0) {
            if (c != '+' && c != '-' && !isDigit) {
                return 0;
            } else {
                sb.append(c);
                continue;
            }
        }

        if (isDigit) {
            sb.append(c);
        } else {
            break;
        }
    }

    String resultAsString = sb.toString();

    if (resultAsString.isEmpty()) {
        return 0;
    }

    char firstCharacter = resultAsString.charAt(0);

    if (resultAsString.length() == 1 && !Character.isDigit(firstCharacter)) {
        return 0;
    }

    try {
        return Integer.parseInt(resultAsString);
    } catch (NumberFormatException e) {
        return firstCharacter == '-' ? Integer.MIN_VALUE : Integer.MAX_VALUE;
    }
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
Find the pair of integers in an array whose sum is x
```

```Java
boolean hasArrayTwoCandidates(int A[], int arr_size, int x) {
    int l, r;
    /* Sort the elements */
    Arrays.sort(A);
    /* Now look for the two candidates in the sorted array*/
    l = 0;
    r = arr_size - 1;
    while (l < r) {
        if (A[l] + A[r] == x)
            return true;
        else if (A[l] + A[r] < x)
            l++;
        else // A[i] + A[j] > x
            r--;
    }
    return false;
}
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

## 3Sum

```text
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

Example:

Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

```Java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    Arrays.sort(nums);
    for (int i = 0; i < nums.length-2; i++) {
        if(i == 0 || (i > 0 && nums[i]!= nums[i-1])) {
            int low = i + 1;
            int high = nums.length - 1;
            while(low < high) {
                int curr = nums[i] + nums[low] + nums[high];
                if(curr > 0) {
                    high--;
                } else if(curr < 0) {
                    low++;
                } else{
                    List<Integer> ans = new ArrayList<Integer>();
                    ans.add(nums[i]);
                    ans.add(nums[low]);
                    ans.add(nums[high]);
                    result.add(ans);
                    while(low < high && nums[high] == nums[high-1]) high--;
                    while(low < high && nums[low] == nums[low+1]) low++;
                    high--;
                    low++;
                }
            }
        }
    }
    return result;
}
```



## 3Sum Closest

```txt
Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

For example, given array S = {-1 2 1 -4}, and target = 1.
The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

```Java
int threeSumClosest(int[] nums, int target) {
    int min = Integer.MAX_VALUE;
	int result = 0;

	Arrays.sort(nums);

    for (int i = 0; i < nums.length; i++) {
        int j = i + 1;
        int k = nums.length - 1;
        while (j < k) {
            int sum = nums[i] + nums[j] + nums[k];
            int diff = Math.abs(sum - target);
            if(diff == 0) return sum;
            if (diff < min) {
                min = diff;
                result = sum;
            }
            if (sum <= target) {
                j++;
            } else {
                k--;
            }
        }
    }
    return result;
}
```

## Combination Sum

```text
Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

Note:

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
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
List<List<Integer>> res = new ArrayList<>();
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    Arrays.sort(candidates);
    combine(candidates, 0, new ArrayList<Integer>(), 0, target);
    return res;
}

void combine(int[] candidates, int i, List<Integer> list, int sum, int target) {
    if (sum == target) {
        res.add(new ArrayList(list));
        return;
    }
    // add candidates[i]
    if (sum + candidates[i] <= target) {
        list.add(candidates[i]);
        combine(candidates, i, list, sum + candidates[i], target);
        list.remove(list.size() - 1);
    }
    // ignore candidates[i]
    if (i+1 < candidates.length && sum + candidates[i+1] <= target) {
        combine(candidates, i+1, list, sum, target);
    }
}
```

## Pow(x, n)

```txt
Implement pow(x, n), which calculates x raised to the power n (x^n).

Example 1:

Input: 2.00000, 10
Output: 1024.00000
Example 2:

Input: 2.10000, 3
Output: 9.26100
Example 3:

Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

```Java
public double myPow(double x, int n) {
    if (x == (double) 0) return 0;
    if (x == (double) 1) return 1;
    if (n == 0) return 1;
    if (n < 0) {
        return (1/x)*myPow(1/x, -n-1);
    }
    if (n == 1) return x;

    return n % 2 == 0 ? myPow(x*x, n/2) : x*myPow(x*x, n/2);
}
```

## Add two number

```txt
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example:

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

```Java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    int ok = ((l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val));
    l1 = (l1 == null ? null : l1.next);
    l2 = (l2 == null ? null : l2.next);
    ListNode result = new ListNode(ok % 10);
    int temp = ok / 10;
    ListNode root = result;
    while(l1 != null || l2 != null){
        ok = ((l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + temp);
        root.next = new ListNode(ok % 10);
        temp = ok / 10;
        root = root.next;
        l1 = (l1 == null ? null : l1.next);
        l2 = (l2 == null ? null : l2.next);
    }
    if(temp != 0){
        root.next = new ListNode(temp);
    }
    return result;
}
```

## Valid BST

```txt
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

Example 1:

    2
   / \
  1   3

Input: [2,1,3]
Output: true

Example 2:

    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public boolean isValidBST(TreeNode root) {
    if(root == null)
        return true;

    return isValidBST(root.left, - Double.MAX_VALUE, root.val) && isValidBST(root.right, root.val, Double.MAX_VALUE);
}

private boolean isValidBST(TreeNode node, double min, double max)
{
    if(node == null)
        return true;

    if(node.val <= min || node.val >= max)
        return false;

    return isValidBST(node.left, min, node.val) && isValidBST(node.right, node.val, max);
}
```

## Coin Change

```txt
N = 12
Index of Array of Coins:    [0, 1,  2]
Array of coins:             [1, 5, 10]

Comparing 10 cents to each of the index
and making that same comparison, if the
value of the coin is smaller than the value of the
index at the ways array then
ways[j-coins[i]]+ways[j] is the new value of ways[j].
Thus we get the following.


Index of Array of ways:    [0,  1,  2,  3,  4,  5,  6,  7,  8,  9,  10,  11,  12]
Array of  ways:            [1,  1,  1,  1,  1,  2,  2,  2,  2,  2,  4,   4,    4]

So the answer to our example is ways[12] which is 4.
```

```Java
static long getNumberOfWays(long N, long[] Coins) {
    // Create the ways array to 1 plus the amount
    // to stop overflow
    long[] ways = new long[(int)N + 1];

    // Set the first way to 1 because its 0 and
    // there is 1 way to make 0 with 0 coins
    ways[0] = 1;

    // Go through all of the coins
    for (int i = 0; i < Coins.length; i++) {
        // Make a comparison to each index value
        // of ways with the coin value.
        for (int j = 0; j < ways.length; j++) {
            if (Coins[i] <= j) {
                // Update the ways array
                ways[j] += ways[(int)(j - Coins[i])];
            }
        }
    }

    // return the value at the Nth position
    // of the ways array.
    return ways[(int)N];
}
```

## Stair Case

```txt
Count ways to reach the n’th stair
There are n stairs, a person standing at the bottom wants to reach the top. The person can climb either 1 stair or 2 stairs at a time. Count the number of ways, the person can reach the top.
```

![alt](https://media.geeksforgeeks.org/wp-content/uploads/nth-stair.png)

```Java
static int fib(int n) {
    if (n <= 1)
        return n;
    return fib(n-1) + fib(n-2);
}

// Returns number of ways to reach s'th stair
static int countWays(int s) {
    return fib(s + 1);
}

// Dynamic Programming
static int countWaysUtil(int n, int m) {
    int res[] = new int[n];
    res[0] = 1; res[1] = 1;
    for (int i=2; i<n; i++) {
        res[i] = 0;
        for (int j=1; j<=m && j<=i; j++)
            res[i] += res[i-j];
    }
    return res[n-1];
}
```

## Minimum Swaps for Bracket Balancing

```txt
You are given a string of 2N characters consisting of N ‘[‘ brackets and N ‘]’ brackets. A string is considered balanced if it can be represented in the for S2[S1] where S1 and S2 are balanced strings. We can make an unbalanced string balanced by swapping adjacent characters. Calculate the minimum number of swaps necessary to make a string balanced.

Examples:

Input  : []][][
Output : 2
First swap: Position 3 and 4
[][]][
Second swap: Position 5 and 6
[][][]

Input  : [[][]]
Output : 0
String is already balanced.
```

```Java
int minSwap(String s) {
    int countSwap = 0;
    char[] arr = s.toCharArray();
    int countLeft = 0;
    int countRight = 0;
    int countUnbalance = 0;
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == '[') {
            countLeft++;
            if (countUnbalance > 0) {
                countSwap += countUnbalance;
                countUnbalance--;
            }
        } else if (arr[i] == ']'){
            countRight ++;
            countUnbalance = (countRight - countLeft);
        }
    }
    return countSwap;
}
```

## Decode String

```txt
Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

Examples:

s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

```java
public String decodeString(String s) {
    StringBuilder sb = new StringBuilder();
    dfs(s, 0, sb, 0);
    return sb.toString();
}

int dfs(String s, int idx, StringBuilder sb, int num) {
    if (idx == s.length()) {
        return idx;
    }
    char c = s.charAt(idx);
    if (Character.isDigit(c)) {
        num = 10 * num + (c - '0');
        return dfs(s, idx + 1, sb, num);
    } else if (c == '[') {
        StringBuilder nestedSb = new StringBuilder();
        int nextIdx = dfs(s, idx + 1, nestedSb, 0);
        String nested = nestedSb.toString();
        while (num-- > 0) {
            sb.append(nested);
        }
        return dfs(s, nextIdx + 1, sb, 0);
    } else if (s.charAt(idx) == ']') {
        return idx;
    } else {
        sb.append(c);
        return dfs(s, idx + 1, sb, 0);
    }
}
```

## Container With Most Water

```txt
Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

Example:

Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

![alt](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

```java
public int maxArea(int[] height) {
    int maxArea =0;
    int low =0;
    int high=height.length-1;

    while(low<high){
        maxArea=Math.max(maxArea,(high-low)*Math.min(height[low],height[high]));

        if(height[low]<height[high])
            low++;
            else
                high--;
    }
    return maxArea;
}
```

## Decoded String at Index

```txt
An encoded string S is given.  To find and write the decoded string to a tape, the encoded string is read one character at a time and the following steps are taken:

If the character read is a letter, that letter is written onto the tape.
If the character read is a digit (say d), the entire current tape is repeatedly written d-1 more times in total.
Now for some encoded string S, and an index K, find and return the K-th letter (1 indexed) in the decoded string.

Example 1:

Input: S = "leet2code3", K = 10
Output: "o"
Explanation:
The decoded string is "leetleetcodeleetleetcodeleetleetcode".
The 10th letter in the string is "o".
Example 2:

Input: S = "ha22", K = 5
Output: "h"
Explanation:
The decoded string is "hahahaha".  The 5th letter is "h".
Example 3:

Input: S = "a2345678999999999999999", K = 1
Output: "a"
Explanation:
The decoded string is "a" repeated 8301530446056247680 times.  The 1st letter is "a".
```

```java
public String decodeAtIndex(String s, int k) {
    if (s == null || s.trim().length() == 0 || k <= 0) {
        return null;
    }

    long count = 0;
    int index = 0;
    for (index = 0; index < s.length(); index++) {

        char c = s.charAt(index);
        if (c >= 'a' && c <= 'z') {
            count++;
        } else {
            int d = c - 0x30;
            count *= d;
        }

        if (count >= k) {
            break;
        }
    }

    return helper(s, index, k, count);
}

private String helper(String s, int index, long k, long count) {
    char c = s.charAt(index);
    if (c >= 'a' && c <= 'z') {
        if (k == count) {
            return String.valueOf(c);
        }

        count -= 1;
        index -= 1;
    } else {
        int d = c - 0x30;
        count /= d;
        k = k % count == 0 ? count : k % count;
        index -= 1;
    }

    return helper(s, index, k, count);
}
```

## Decode Ways

```txt
A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given a non-empty string containing only digits, determine the total number of ways to decode it.

Example 1:

Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
Example 2:

Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

```Java
public int numDecodings(String s) {
    int dp2 = 1;
    int dp1 = s.charAt(0) == '0' ? 0 : 1;   //first character

    for (int i = 1; i < s.length(); i++) {
        int dp = s.charAt(i) > '0' && s.charAt(i) <= '9' ? dp1 : 0;
        if (s.charAt(i - 1) == '1') dp += dp2;
        else if (s.charAt(i - 1) == '2' && s.charAt(i) <= '6') dp += dp2;

        dp2 = dp1;
        dp1 = dp;
    }

    return dp1;
}
```

## Edit Distance

```txt
Given two strings str1 and str2 and below operations that can performed on str1. Find minimum number of edits (operations) required to convert ‘str1’ into ‘str2’.

Insert
Remove
Replace
All of the above operations are of equal cost.

Examples:

Input:   str1 = "geek", str2 = "gesek"
Output:  1
We can convert str1 into str2 by inserting a 's'.

Input:   str1 = "cat", str2 = "cut"
Output:  1
We can convert str1 into str2 by replacing 'a' with 'u'.

Input:   str1 = "sunday", str2 = "saturday"
Output:  3
Last three and first characters are same.  We basically
need to convert "un" to "atur".  This can be done using
below three operations.
Replace 'n' with 'r', insert t, insert a
```

```java
static int editDist(String str1, String str2, int m, int n)
{
    // If first string is empty, the only option is to
    // insert all characters of second string into first
    if (m == 0)
        return n;

    // If second string is empty, the only option is to
    // remove all characters of first string
    if (n == 0)
        return m;

    // If last characters of two strings are same, nothing
    // much to do. Ignore last characters and get count for
    // remaining strings.
    if (str1.charAt(m - 1) == str2.charAt(n - 1))
        return editDist(str1, str2, m - 1, n - 1);

    // If last characters are not same, consider all three
    // operations on last character of first string, recursively
    // compute minimum cost for all three operations and take
    // minimum of three values.
    return 1 + min(editDist(str1, str2, m, n - 1), // Insert
                    editDist(str1, str2, m - 1, n), // Remove
                    editDist(str1, str2, m - 1, n - 1) // Replace
                    );
}
```

## Knapsack Problem

```txt
Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. In other words, given two integer arrays val[0..n-1] and wt[0..n-1] which represent values and weights associated with n items respectively. Also given an integer W which represents knapsack capacity, find out the maximum value subset of val[] such that sum of the weights of this subset is smaller than or equal to W. You cannot break an item, either pick the complete item, or don’t pick it (0-1 property).
```

![alt](https://www.geeksforgeeks.org/wp-content/uploads/knapsack-problem.png)

```Java
// Returns the maximum value that can be put in a knapsack of capacity W
static int knapSack(int w, int wt[], int val[], int n) {
    // Base Case
    if (n == 0 || w == 0)
        return 0;

    // If weight of the nth item is more than Knapsack capacity W, then
    // this item cannot be included in the optimal solution
    if (wt[n-1] > w)
        return knapSack(w, wt, val, n-1);

    // Return the maximum of two cases:
    // (1) nth item included
    // (2) not included
    else return max( val[n-1] + knapSack(w - wt[n-1], wt, val, n-1),
                        knapSack(w, wt, val, n-1)
                        );
}
```