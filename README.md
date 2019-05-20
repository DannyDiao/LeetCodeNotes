# LeetCode笔记

- 70.爬楼梯 Climbing-Stairs

  - 暴力递归，把所有可能的解法递归出来。

  ```java
  package ClimbStairs;
  
  public class Sol_one {
  
      public static void main(String[] args) {
          int i = climbStairs(44);
          System.out.println(i);
      }
  
      public static int climbStairs(int n) {
          return climb_Stairs(0,n);
      }
      public static int climb_Stairs(int i,int n){
          if(i>n){
              return 0;
          }
          if(i == n){
              return 1;
          }
          return climb_Stairs(i+1,n) + climb_Stairs(i+2,n);
      }
  }
  ```

  - 记忆化递归，用一个memo数组储存每次递归的结果，这样可以大大节省时间复杂度。

    ```java
    public class Sol_two {
       public int climbStairs(int n) {
            int memo[] = new int[n + 1];
            return climb_Stairs(0, n, memo);
        }
    
        public int climb_Stairs(int i, int n, int memo[]) {
            if (i > n) {
                return 0;
            }
            if (i == n) {
                return 1;
            }
    
            if (memo[i] > 0) {
                return memo[i];
            }
            memo[i] = climb_Stairs(i + 1, n, memo) + climb_Stairs(i + 2, n, memo);
            return memo[i];
        }
            
        }
    ```
    
  - 动态规划：
  
    不难发现，这个问题可以被分解为一些包含最优子结构的子问题，即它的最优解可以从其子问题的最优解来有效地构建，我们可以使用动态规划来解决这一问题。
  
    第`i`阶可以由以下两种方法得到：
  
    1. 在第`(i-1)`阶后向上爬一阶。
    2. 在第`(i-2)`阶后向上爬两阶。
  
    所以到达第`i`阶的方法总数就是到第`i-1`阶和第`i-2`阶的方法数之和。
  
    令`solve[i]`表示能到达第`i`阶的方法总数：
  
    `solev[i] = solve[i-1] + solve[i-2]`
  
    ```java
    public class Sol_three {
        public int climbStairs(int n) {
            if (n == 1) {
                return 1;
            }
    
            int[] solve = new int[n + 1];
            solve[1] = 1;
            solve[2] = 2;
            for (int i = 3; i <= n; i++) {
                solve[i] = solve[i - 1] + solve[i - 2];
    
            }
            return solve[n];
        }
    }
    ```
  
- 100.相同的树 Same-Tree
  
  - 递归：最简单的策略是使用递归。检查p和q节点是否不是空，它们的值是否相等。如果所有检查都正常，则递归地为子节点执行相同操作。
  
    ```java
    class Sol_one {
        public boolean isSameTree(TreeNode p, TreeNode q) {
            if (p == null && q == null) {
                return true;
            }
            if (p == null || q == null) {
                return false;
            }
            if (p.val != q.val) {
                return false;
            }
            return isSameTree(q.left, p.left) && isSameTree(q.right, p.right);
        }
    }
    ```
  
    
  
    
