# ![KickDrum logo](https://images.squarespace-cdn.com/content/v1/587fc3e546c3c40b37363b71/1486656837457-218DC3CYXY1CA4WTC1RW/kickdrum-social.png) <br>KickDrum | Technical Lead (June-2024)

## [Round 1] KickDrum HackerEarth coding Round
### [Coding] Problem Statement 1 - The largest Number

You are given **9** digits that are numbered from **1** to **9** and each digit has a cost. The cost is denoted by an array **X** where **X<sub>i</sub>** denotes the cost of **i<sup>th</sup>** digit. You have **N** units of money to spend. You are required to find the largest number possible that can be formed by using the **N** units of money. If it is possible to form such a number, then print **-1**.
<br>

### Input format
- First line: **T** denoting the number of the cases
- For each test case:
  - First line: **N** denoting the amount of money.
  - Second line: Nine space-separated integers **X<sub>1</sub>**, **X<sub>2</sub>**, **X<sub>3</sub>** .... **X<sub>9</sub>** denoting the cost of each digit.

| input |   output    |
| :-:   |:-:|
| <p align="left">3<br>5<br>9 4 2 2 6 3 2 2 1<br>2<br>5 11 2 2 5 8 9 10 19<br>5<br>9 9 9 9 9 9 9 9 9</p>|<p align="left"> <br>99999<br>4<br>-1</p>|


<br>Here's how we can implement this in Java:

```java
import java.util.*;

public class LargestNumber {
   public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
       int T = scanner.nextInt(); // number of test cases

       for (int t = 0; t < T; t++) {
           int N = scanner.nextInt(); // amount of money
           int[] X = new int[9]; // costs of each digit
           for (int i = 0; i < 9; i++) {
               X[i] = scanner.nextInt();
           }
           System.out.println(getLargestNumber(N, X));
       }
       scanner.close();
   }

   public static String getLargestNumber(int N, int[] X) {
       StringBuilder result = new StringBuilder();

       // Start from the highest cost digit
       for (int digit = 9; digit >= 1; digit--) {
           int count = Math.min(N / X[digit - 1], 9); // Calculate how many times the current digit can be bought
           if (count > 0) {
               result.append(String.valueOf(digit).repeat(count)); // Append the digit count times
               N -= count * X[digit - 1]; // Update the remaining money
           }
       }

       // If no number could be formed within the budget, return -1
       if (result.length() == 0) {
           return "-1";
       }
       return result.toString();
   }
}
```

### [SQL] Problem Statement 2 - Maximum runs
Write a query to find the maximum runs scored by a batsman in the month of January 2016 among all the batsmen who played during that month.<br>

#### Table name: **CricketMatch**
| Field |   Type    |
| :-:   |:-:|
| BatsmanName|Text|
| RunsMade|Integer|
| MatchDate|Text|


#### Sample
| BatsmanName |   RunsMade    |   MatchDate    |
| :-: | :-: | :-: |
| Colin|100|1-Jan-2016|
|Colin|50|5-Jan-2016|
|Colin|150|25-Jan-2016|
|Mike|120|20-Jan-2016|
|Mike|100|28-Jan-2016|
|Chris|50|28-Jan-2016|
|Chris|75|02-Jan-2016|
|Chris|100|05-Jan-2016|
|Chris|100|05-Feb-2016|



```mysql
SELECT MAX(best) FROM 
(SELECT BatsmanName, SUM(RunsMade) as best
  FROM CRICKETMATCH
  WHERE STR_TO_DATE(MatchDate, '%d-%b-%Y') >= '2016-01-01' 
  AND STR_TO_DATE(MatchDate, '%d-%b-%Y') <= '2016-02-01'
GROUP BY BatsmanName) as X
```
#### Output
| MAX(best)    |
| :-: |
| 300|

### [onecompiler link](https://onecompiler.com/mysql/42fu3ednp)
