# ![BitGo logo](https://logowik.com/content/uploads/images/bitgo4658.jpg) <br>BitGo | Staff Backend Engineer (June-2024)

## [Round 1] CoderByte coding assessment Round
### [Coding] Problem Statement 1 - Valid password

Have the function **StringChallenge(str)** take the **str** parameter being passed and determine if it passes as a valid password that follows the list of constraints:
<br>
1. It must have a capital letter
2. It must contain at least one number
3. It must contain a punctuation mark or mathematical symbol.
4. It cannot have the work "password" in the string.
5. It must be longer than 7 characters and shorter than 31 characters.
<br>
if all above constraints are met then output as true otherwise false.

eg:
| input |   output    |
| :-:   |:-:|
| apple!M7|true|
| passWord123!!!!|false|
| turkey90AAA=|false|

### Solution
<br>Here's how we can implement this in Java:

```java
import java.util.regex.*;

public class Main {

    public static boolean StringChallenge(String str) {
        // Check length constraint
        if (str.length() <= 7 || str.length() >= 31)
            return false;

        // Check for "password" in lowercase
        if (str.toLowerCase().contains("password"))
            return false;

        // Check for at least one capital letter
        if (!str.matches(".*[A-Z].*"))
            return false;

        // Check for at least one digit
        if (!str.matches(".*\\d.*"))
            return false;

        // Check for at least one punctuation or mathematical symbol
        Pattern p = Pattern.compile("[^a-zA-Z0-9]");
        Matcher m = p.matcher(str);
        if (!m.find())
            return false;

        return true;
    }

    public static void main(String[] args) {
        System.out.println(StringChallenge("apple!M7")); // true
        System.out.println(StringChallenge("passWord123!!!!")); // false
        System.out.println(StringChallenge("turkey90AAA=")); // false
    }
}
```

### [Coding] Problem Statement 2 - Largest Rectangle in Histogram

Given an array of integers heights representing the histogram's bar **height** where the width of each bar is **1**, return the **area** of the **largest rectangle** in the histogram.
### [refer leetcode](https://leetcode.com/problems/largest-rectangle-in-histogram/description)
eg:
| input(heights) |   output    |
| :-:   |:-:|
| [2,1,3,4,1]|6|
| [6,3,1,4,12,4]|12|
| [5,6,7,4,1]|16|

### Solution
<br>Here's how we can implement this in Java:

```java
class Solution {
    public int largestRectangleArea(int[] height) {
    if (height == null || height.length == 0) {
        return 0;
    }
    int[] lessFromLeft = new int[height.length]; // idx of the first bar the left that is lower than current
    int[] lessFromRight = new int[height.length]; // idx of the first bar the right that is lower than current
    lessFromRight[height.length - 1] = height.length;
    lessFromLeft[0] = -1;

    for (int i = 1; i < height.length; i++) {
        int p = i - 1;

        while (p >= 0 && height[p] >= height[i]) {
            p = lessFromLeft[p];
        }
        lessFromLeft[i] = p;
    }

    for (int i = height.length - 2; i >= 0; i--) {
        int p = i + 1;

        while (p < height.length && height[p] >= height[i]) {
            p = lessFromRight[p];
        }
        lessFromRight[i] = p;
    }

    int maxArea = 0;
    for (int i = 0; i < height.length; i++) {
        maxArea = Math.max(maxArea, height[i] * (lessFromRight[i] - lessFromLeft[i] - 1));
    }

    return maxArea;
    }
}
```


## [Round 2] CoderByte DSA (60Mins) On-Site virtual coding Round
### [Coding] Problem Statement - Longest Substring With K Distinct Characters

You are given a string **'S'** of length **'N'** consisting of lowercase English alphabet letters. You are also given a positive integer **'K'**.
<br>
Now, a substring of this string is good if it contains at most **'K'** distinct characters. A string **'X'** is a substring of string **'Y'** if it can be obtained by deletion of several continuous elements(possibly zero) from the beginning and the end from the string **'Y'**.
<br>
Your task is to return the maximum size of any good substring of the string **'S'**.
<br>
Example:
**‘S’ = “bacda” and ‘K’ = 3**
<br>
So, the substrings having at most ‘3’ distinct characters are called good substrings. Some possible good substrings are:
1. **“bac”**
2. **“acd”**
3. **“acda”**

The substring **"acda"** is the largest possible good substring, as we cannot get any other substring of length **5** or more having distinct characters less than or equal to **‘3’**. Thus, you should return **"acda"** as the answer.
<br>

### [refer naukri code360](https://www.naukri.com/code360/problems/longest-sub-string-with-k-distinct-characters_920521)

eg:
| input (s, k) |   output    |
| :-:   |:-:|
| s = bacda, k = 3|acda|
| s = aabbcbbbadef, k = 2|bbcbbb|

### Solution
<br>Here's how we can implement this in Java:

```java
import java.util.HashMap;

public class MaximumGoodSubstring {

    public static String maximumGoodSubstring(String S, int K) {
        int n = S.length();
        int maxLen = 0;
        int start = 0;
        int maxStart = 0;
        HashMap<Character, Integer> charCount = new HashMap<>();

        for (int end = 0; end < n; end++) {
            char endChar = S.charAt(end);
            charCount.put(endChar, charCount.getOrDefault(endChar, 0) + 1);

            while (charCount.size() > K) {
                char startChar = S.charAt(start);
                charCount.put(startChar, charCount.get(startChar) - 1);
                if (charCount.get(startChar) == 0) {
                    charCount.remove(startChar);
                }
                start++;
            }

            // Now charCount has at most K distinct characters from S[start...end]
            int currentLen = end - start + 1;
            if (currentLen > maxLen) {
                maxLen = currentLen;
                maxStart = start;
            }
        }

        // maxStart is the starting index of the longest substring with at most K distinct characters
        return S.substring(maxStart, maxStart + maxLen);
    }

    public static void main(String[] args) {
        String s = "bacda";
        int k = 3;
        System.out.println("Maximum Good Substring: " + s + " is " + maximumGoodSubstring(s, k)); // Output: acda

        s = "aabbcbbbadef";
        k = 2;
        System.out.println("Maximum Good Substring: " + s + " is " + maximumGoodSubstring(s, k)); // Output: bbcbbb
    }
}
```
