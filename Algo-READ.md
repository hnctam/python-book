# Algorithm Review

## Permutation
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