# ![Park+ logo](https://images.crunchbase.com/image/upload/c_pad,f_auto,q_auto:eco,dpr_1/wjnjtvyqowi0b6lbcnr9) Park+ | Engineering Manager (June-2024)

## [Round 1] Park+ coding Round
### Problem Statement - Shadow balls

The goal is to count the number of "shadow balls" for each ball. A "shadow ball" for a given ball is any ball to its right in the array that is smaller than the given ball. The total number of shadows for all balls is the sum of these counts.
<br>

### Approach 1 (Brute force)
To achieve this, we need to:<br>
1. Iterate through each ball.
2. For each ball, count how many of the subsequent balls are smaller.
3. Sum these counts.

<br>Here's how we can implement this in Java:

```java
import java.util.*;

public class ShadowBalls {
    public static int totalShadows(int[] balls) {
        int totalShadowCount = 0;
        
        // Iterate through each ball
        for (int i = 0; i < balls.length; i++) {
            // For each ball, count how many of the subsequent balls are smaller
            for (int j = i + 1; j < balls.length; j++) {
                if (balls[j] < balls[i]) {
                    totalShadowCount += 1;
                }
            }
        }
        
        return totalShadowCount;
    }

    public static void main(String[] args) {
        // Example usage
        int[] arr = {3, 5, 1, 4};
        System.out.println(totalShadows(arr));  // Output: 2
    }
}
```
| input (arr) |   output    |
| :---:   |:------------------------:|
| [3, 5, 1, 4] |     2     |

#### Explanation
1. **Outer Loop**: The outer loop iterates through each ball in the array.
2. **Inner Loop**: For each ball (indexed by **i**), the inner loop iterates through the balls to its right (indexed by **j** from **i+1** to the end of the array).
3. **Comparison**: Inside the inner loop, we compare the current ball (**balls[i]**) with the subsequent ball (**balls[j**]). If **balls[j]** is smaller, it counts as a shadow ball.
4. **Count Shadows**:  We increment **totalShadowCount** for each shadow ball found.
5. **Return Result**: Finally, we return the total count of shadow balls.

#### Efficiency
- The time complexity is **O(n^2)** due to the nested loops where **n** is the number of balls in the array.
- The space complexity is **O(1)** since no additional space proportional to the input size is used, only a few integer variables.

### Approach 2 (Using merge sort)
To optimize the solution, we can use a more efficient approach such as a modified merge sort. This method will allow us to count the number of smaller elements to the right of each element in **O(n log n)** time<br>

<br>Here's how we can implement this in Java:

```java
import java.util.Arrays;

public class ShadowBalls {

    public static int totalShadows(int[] balls) {
        int n = balls.length;
        int[] count = new int[n];
        int[] indexes = new int[n];

        for (int i = 0; i < n; i++) {
            indexes[i] = i;
        }

        mergeSort(balls, indexes, count, 0, n - 1);
        int totalShadowCount = 0;
        for (int c : count) {
            totalShadowCount += c;
        }
        return totalShadowCount;
    }

    private static void mergeSort(int[] balls, int[] indexes, int[] count, int left, int right) {
        if (left >= right) return;

        int mid = left + (right - left) / 2;
        mergeSort(balls, indexes, count, left, mid);
        mergeSort(balls, indexes, count, mid + 1, right);
        merge(balls, indexes, count, left, mid, right);
    }

    private static void merge(int[] balls, int[] indexes, int[] count, int left, int mid, int right) {
        int[] temp = new int[right - left + 1];
        int i = left, j = mid + 1, k = 0;

        while (i <= mid && j <= right) {
            if (balls[indexes[i]] <= balls[indexes[j]]) {
                temp[k++] = indexes[j++];
            } else {
                count[indexes[i]] += right - j + 1;
                temp[k++] = indexes[i++];
            }
        }

        while (i <= mid) {
            temp[k++] = indexes[i++];
        }

        while (j <= right) {
            temp[k++] = indexes[j++];
        }

        for (i = left, k = 0; i <= right; i++, k++) {
            indexes[i] = temp[k];
        }
    }

    public static void main(String[] args) {
        // Example usage
        int[] arr = {3, 5, 1, 4};
        System.out.println(totalShadows(arr));  // Output: 2
    }
}
```
| input (arr) |   output    |
| :---:   |:------------------------:|
| [3, 5, 1, 4] |     2     |


#### Explanation
1. **Initialization**:
   - **count**: Array to keep track of the count of smaller elements to the right for each element.
   - **indexes**: Array to keep track of the original indices of the elements.
2. **Modified Merge Sort**:
   - **mergeSort**: Recursively splits the array into halves.
   - **merge**: Merges the two halves while counting the number of smaller elements to the right.
3. **Merge Function**:
   - The **merge** function compares the elements of the two halves.
   - If an element from the left half is greater than an element from the right half, it means that all remaining elements in the right half are smaller than this element.
   - The count of these smaller elements is added to the **count** array for the element from the left half.
4. **Result Calculation**:
   - After the merge sort completes, the **count** array contains the number of smaller elements to the right for each element.
   - The total number of shadows is the sum of all values in the **count** array.

#### Efficiency
- The time complexity is **O(n log n)** due to the merge sort algorithm.
- The space complexity is **O(n)** because of the additional arrays used for indexing and counting.
- This optimized solution is much faster for large input sizes compared to the **O(n^2)** approach.
