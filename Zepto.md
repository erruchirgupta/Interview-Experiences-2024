# ![Zepto logo](https://upload.wikimedia.org/wikipedia/en/7/7d/Logo_of_Zepto.png) <br>Zepto | Software Engineer III (Backend) (June-2024)

## [Round 1] Live coding Round - Solve Sudoku

![Sudoku](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Sudoku_Puzzle_by_L2G-20050714_standardized_layout.svg/1920px-Sudoku_Puzzle_by_L2G-20050714_standardized_layout.svg.png)

### Proposed solution (Using Backtracking):
Follow the steps below to solve the problem:<br>
- Create a function that checks after assigning the current index the grid becomes unsafe or not. Keep Hashmap for a row, column and boxes. If any number has a frequency greater than 1 in the hashMap return false else return true; hashMap can be avoided by using loops.
- Create a recursive function that takes a grid.
- Check for any unassigned location.
  - If present then assigns a number from 1 to 9.
  - Check if assigning the number to current index makes the grid unsafe or not.
  - If safe then recursively call the function for all safe cases from 0 to 9.
  - If any recursive call returns true, end the loop and return true. If no recursive call returns true then return false.
- If there is no unassigned location then return true.

<br>Here's how we can implement this in Java:

```java
public class SolveSudoku {
    public static boolean isSafe(int[][] board, int row, int col, int num) {
        // Row has the unique (row-clash)
        for (int d = 0; d < board.length; d++) {
            // Check if the number we are trying to
            // place is already present in
            // that row, return false;
            if (board[row][d] == num)
                return false;
        }

        // Column has the unique numbers (column-clash)
        for (int r = 0; r < board.length; r++) {

            // Check if the number
            // we are trying to
            // place is already present in
            // that column, return false;
            if (board[r][col] == num)
                return false;
        }

        // Corresponding square has
        // unique number (box-clash)
        int sqrt = (int)Math.sqrt(board.length);
        int boxRowStart = row - row % sqrt;
        int boxColStart = col - col % sqrt;

        for (int r = boxRowStart; r < boxRowStart + sqrt; r++)
            for (int d = boxColStart; d < boxColStart + sqrt; d++)
                if (board[r][d] == num)
                    return false;

        // if there is no clash, it's safe
        return true;
    }

    public static boolean solveSudoku(int[][] board) {
        int row = -1;
        int col = -1;
        boolean isEmpty = true;
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board.length; j++)
                if (board[i][j] == 0) {
                    row = i;
                    col = j;

                    // We still have some remaining
                    // missing values in Sudoku
                    isEmpty = false;
                    break;
                }

            if (!isEmpty)
                break;
        }

        // No empty space left
        if (isEmpty)
            return true;

        // Else for each-row backtrack
        for (int num = 1; num <= board.length; num++)
            if (isSafe(board, row, col, num)) {
                board[row][col] = num;
                if (solveSudoku(board))
                    // print(board, n);
                    return true;
                else
                    // replace it
                    board[row][col] = 0;
            }
        return false;
    }

    public static void print(int[][] board) {
        // We got the answer, just print it
        for (int r = 0; r < board.length; r++) {
            for (int d = 0; d < board.length; d++)
                System.out.print(board[r][d] + " ");
            System.out.print("\n");

            if ((r + 1) % (int)Math.sqrt(board.length) == 0)
                System.out.print("");
        }
    }

    public static void main(String [] args) {
        int[][] board = new int[][] {
                { 5, 3, 0, 0, 7, 0, 0, 0, 0 },
                { 6, 0, 0, 1, 9, 5, 0, 0, 0 },
                { 0, 9, 8, 0, 0, 0, 0, 6, 0 },
                { 8, 0, 0, 0, 6, 0, 0, 0, 3 },
                { 4, 0, 0, 8, 0, 3, 0, 0, 1 },
                { 7, 0, 0, 0, 2, 0, 0, 0, 6 },
                { 0, 6, 0, 0, 0, 0, 2, 8, 0 },
                { 0, 0, 0, 4, 1, 9, 0, 0, 5 },
                { 0, 0, 0, 0, 8, 0, 0, 7, 9 }
        };

        if (solveSudoku(board))
            print(board);
        else
            System.out.println("No solution");
    }
}
```
