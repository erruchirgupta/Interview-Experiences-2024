# ![Komprise logo](https://www.komprise.com/wp-content/uploads/2018/01/komprise-logo-e1517517089312.jpg) <br>Komprise | Senior Backend Engineer (June-2024)

## [Round 1] DSA Coding Round (60 Mins)
### [Coding] Problem Statement - Task Scheduler (Least Intervals)

You are given an array of CPU `tasks`, each represented by letters A to Z, and a cooling time, `n`. Each cycle or interval allows the completion of one task. Tasks can be completed in any order, but there's a constraint: **identical** tasks must be separated by at least `n` intervals due to cooling time.
<br><br> 
​Return the minimum number of intervals required to complete all tasks.
<br> 

**Example 1:**
> **Input:** tasks = ["A","A","A","B","B","B"], n = 2<br><br>
> **Output:** 8<br><br>
> **Explanation:** A possible sequence is: A -> B -> idle -> A -> B -> idle -> A -> B.<br><br>
> After completing task A, you must wait two cycles before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th cycle, you can do A again as 2 intervals have passed.
<br>

**Example 2:**
> **Input:** tasks = ["A","C","A","B","D","B"], n = 1<br><br>
> **Output:** 6<br><br>
> **Explanation:** A possible sequence is: A -> B -> C -> D -> A -> B.<br><br>
> With a cooling interval of 1, you can repeat a task after just one other task.

eg:
| input (tasks, n) |   output    |
| :-:   |:-:|
| tasks = ["A","A","A","B","B","B"], n = 2|8|
| tasks = ["A","C","A","B","D","B"], n = 1|6|

#### Solution
<br>Here's how we can implement this in Java:

```java
import java.util.Arrays;

class Task {
    public int func(char[] tasks, int n) {
        // Step 1: Count frequency of each task
        int[] fr = new int[26];
        for (int i = 0; i < tasks.length; i++)
            fr[tasks[i] - 'A']++;

        // Step 2: Sort the frequencies in ascending order
        Arrays.sort(fr);

        // Step 3: Calculate the basic frame based on the highest frequency
        int space = fr[25] - 1;
        int gap = space * n;

        // Step 4: Fill the gaps with other tasks
        for (int i = 24; i >= 0; i--) {
            gap -= Math.min(space, fr[i]);
        }

        // Step 5: Calculate the total intervals required
        return gap < 0 ? tasks.length : gap + tasks.length;
    }

    public static void main(String[] args) {
        Task scheduler = new Task();
        char[] tasks1 = {'A', 'A', 'A', 'B', 'B', 'B'};
        int n1 = 2;
        System.out.println(scheduler.func(tasks1, n1)); // Output: 8

        char[] tasks2 = {'A', 'C', 'A', 'B', 'D', 'B'};
        int n2 = 1;
        System.out.println(scheduler.func(tasks2, n2)); // Output: 6
    }
}
```

<details>
<summary>More info</summary>
 
 #### Explanation
1. Count Frequency of Tasks:
   - The array `fr` of size 26 is used to store the frequency of each task (assuming tasks are represented by uppercase English letters 'A' to 'Z').
   - Iterate over the `tasks` array and increment the corresponding index in the `fr` array.
2. Sort the Frequencies:
   - Sort the `fr` array in ascending order. This makes it easier to identify the task with the maximum frequency and handle the scheduling.
3. Calculate the Basic Frame:
   - The variable `space` is calculated as `fr[25] - 1`, where `fr[25]` is the maximum frequency of any task.
   - The `gap` is calculated as `space * n`. This represents the total number of idle slots needed if we only consider the most frequent task.
4. Fill the Gaps with Other Tasks:
   - Iterate over the remaining frequencies in descending order (from `fr[24]` to `fr[0]`).
   - Reduce the `gap` by the lesser of `space` or the current frequency. This step essentially tries to fill the idle slots with other tasks as much as possible.
5. Calculate the Total Intervals:
   - If `gap` is less than 0, it means that all the slots can be filled without any idle times, so the result is the length of the `tasks` array.
   - Otherwise, the result is the total number of slots needed (`gap + tasks.length`).

#### Dry Run of Example 1
**Example 1:** `tasks = ["A", "A", "A", "B", "B", "B"], n = 2`

1. Count Frequency of Tasks:
   - `fr = [3, 3, 0, 0, ..., 0]` (frequencies of A and B are 3, others are 0)
2. Sort the Frequencies:
  - `fr = [0, 0, ..., 0, 3, 3]`
3. Calculate the Basic Frame:
  - `space = fr[25] - 1 = 3 - 1 = 2`
  - `gap = space * n = 2 * 2 = 4`
4. Fill the Gaps with Other Tasks:
  - `gap -= Math.min(space, fr[24]) = gap - Math.min(2, 3) = 4 - 2 = 2`
  - Continue filling gaps: `gap -= Math.min(space, fr[23]) = gap - Math.min(2, 0) = 2`
  - (No more tasks to fill, so `gap` remains 2)
5. Calculate the Total Intervals:
  - Since `gap` is not less than 0, the result is `gap + tasks.length = 2 + 6 = 8`

**Output:** 8
<br>
#### Dry Run of Example 2
**Example 2:** `tasks = ["A", "C", "A", "B", "D", "B"], n = 1`

1. Count Frequency of Tasks:
  - `fr = [2, 2, 1, 1, 0, ..., 0]` (frequencies of A, B are 2, C and D are 1)
2. Sort the Frequencies:
  - `fr = [0, 0, ..., 1, 1, 2, 2]`
3. Calculate the Basic Frame:
  - `space = fr[25] - 1 = 2 - 1 = 1`
  - `gap = space * n = 1 * 1 = 1`
4. Fill the Gaps with Other Tasks:
  - `gap -= Math.min(space, fr[24]) = gap - Math.min(1, 2) = 1 - 1 = 0`
  - Continue filling gaps: `gap -= Math.min(space, fr[23]) = gap - Math.min(1, 1) = 0`
  - (No more gaps to fill, so `gap` remains 0)
5. Calculate the Total Intervals:
- Since `gap` is less than 0, the result is `tasks.length = 6`

**Output:** 6
<br>
#### Complexity
- Time complexity:O(n)
- Space complexity:O(1)
</details>

