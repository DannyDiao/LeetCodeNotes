# LeetCode笔记

- 70.爬楼梯 Climbing-Stairs

  - 暴力递归，把所有可能的解法递归出来。

    ```java
    public class Sol_one {
      public int climbStairs(int n) {
          return climb_Stairs(0, n);
      }
    
      public int climb_Stairs(int i, int n) {
          if (i > n) {
              return 0;
          }
          if (i == n) {
              return 1;
          }
          return climb_Stairs(i + 1, n) + climb_Stairs(i + 2, n);
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

- 101.对称二叉树

  - 递归：对称是指一个二叉树的左子树和右子树镜像对称。

    那么符合什么条件的时候可以视为镜像对称的呢？

    - 它们的两个根结点具有相同的值

    - 每个树的右子树都与另一个树的左子树镜像对称
    ```java
  public boolean isSymmetric(TreeNode root) {
            return isMirror(root, root);
        }
    
        public boolean isMirror(TreeNode a, TreeNode b) {
            if (a == null && b == null) {
                return true;
            }
            if (a == null || b == null) {
                return false;
            }
            return (a.val == b.val) 
                && isMirror(a.right, b.left)
                && isMirror(a.left, b.right);
        }
    ```
    
    
  
- 104.二叉树的最大深度 Maximum-Depth-Of-Binary-Tree

  - 递归：使用DFS（深度优先搜索）

    ```java
    class Solution {
        public int maxDepth(TreeNode root) {
            if (root == null) {
                return 0;
            }
            int max = Math.max(maxDepth(root.left), maxDepth(root.right));
            return max + 1;
        }
    }
    ```


- 617.合并二叉树 merge-two-binary-trees

  - 递归：先序遍历

    ```java
    class Solution {
        public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
    
            if (t1 == null) {
                return t2;
            }
            if (t2 == null) {
                return t1;
            }
            t1.val = t1.val + t2.val;
            t1.left = mergeTrees(t1.left, t2.left);
            t1.right = mergeTrees(t1.right, t2.right);
            return t1;
        }
    }
    ```


- 226.翻转二叉树 invert-binary-tree

  - 递归：DFS 深度优先遍历

    `遇树就递归，遇空就返回～`

    ```java
    class Solution {
        public TreeNode invertTree(TreeNode root) {
            if(root != null){
                TreeNode left = root.left;
                TreeNode right = root.right;
                root.left = invertTree(right);
                root.right = invertTree(left);
            }
            return root;
        }
    }
    ```

- 169.求众数

  - 暴力法：两重循环，一重循环遍历数组，另一重循环统计每个数字出现的次数。

    ```java
    class Solution {
        public int majorityElement(int[] nums) {
            int major = nums.length/2;
        
            for(int num:nums){
                int count = 0;
                for(int elem:nums){
                    if(elem == num){
                        count++;
                    }
                }
                if (count > major){
                    return num;
                }
            }
                return -1;
            }
        }
    ```

  - 投票法：遇到众数计为+1，其他数计为-1，最后和会大于0。当和为0的时候，下一个数当作候选众数。

    ```java
    class Solution {
        public int majorityElement(int[] nums) {
            int count = 0;
            Integer major = null;
            
            for(int num:nums){
                if(count == 0){
                    major = num;
                }
                
                count += (major == num) ? 1 : -1;
            }
            
            return major;
        }
    }
    ```

    - 三目运算符：

      ```java
      if(a<b){
        min = a;
      }else{
        min = b;
      }
      ```

      可以用三目运算符转写为：

      ```java
      min = (a < b) ? a : b
      ```

      表示对(a < b)作判断，若为真执行冒号前，若为假执行冒号后。

- 88.合并两个有效数组

  - 合并后排序：最简单直观

    ```java
    class Solution {
        public void merge(int[] nums1, int m, int[] nums2, int n) {
            System.arraycopy(nums2, 0, nums1, m, n);
            Arrays.sort(nums1);
        }
    }
    ```

  - 从头到尾归并：拷贝num1数组到num3，把num2和num3逐个比较，小的就归并进num1中。

    ```java
    class Solution {
        public void merge(int[] nums1, int m, int[] nums2, int n) {
            int nums3[] = new int[m+n];
            int length = m + n;
            int nums2_flag = 0;
            int nums3_flag = 0;
            
            System.arraycopy(nums1, 0, nums3, 0, m);
            
            for(int i=0;i<length;i++){
              //在这里要先对nums2和nums3做越界判断
                if(nums3_flag == m){
                    nums1[i] = nums2[nums2_flag];
                    nums2_flag++;
                }else if(nums2_flag == n){
                    nums1[i] = nums3[nums3_flag];
                    nums3_flag++;
                }else if(nums3[nums3_flag] < nums2[nums2_flag]){
                    nums1[i] = nums3[nums3_flag] ;
                    nums3_flag++;
                }else{
                    nums1[i] = nums2[nums2_flag] ;
                    nums2_flag++;
                }
            }
        }
    }
    ```

- 28.实现strStr()

  - 库函数法：直接调用内置的indexOf函数。

    ```java
    class Solution {
        public int strStr(String haystack, String needle) {
            int pos = haystack.indexOf(needle);
            return pos;
        }
    }
    ```

- 27.移除元素

  - 拷贝覆盖：遍历数组nums，同时设置一个下标flag = 0。当取出的数组元素与val不同时，nums[flag] =    num，无损覆盖，同时flag+1；当取出的数组元素与val相同时，跳过，flag不动，这样下一个元素如果与val不同就会覆盖掉之前的这个元素。

    ```java
    class Solution {
        public int removeElement(int[] nums, int val) {
            int flag = 0;    
            for(int num:nums){
                if(num != val){
                    nums[flag] = num;
                    flag++;
                }
            }
            return flag;
        }
    }
    ```

- 14.最长公共前缀

  - 水平扫描法：

    ```java
    class Solution {
        public String longestCommonPrefix(String[] strs) {
       if (strs.length == 0) return "";
       String prefix = strs[0];
       for (int i = 1; i < strs.length; i++)
           while (strs[i].indexOf(prefix) != 0) {
               prefix = prefix.substring(0, prefix.length() - 1);
               if (prefix.isEmpty()) return "";
           }        
       return prefix;
    }
    }
    ```

- 860.柠檬水找零

  - 模拟场景：

    - 当客户给5元零钱时：最好情况
    - 当客户给10元钱时：如果至少有一张5元零钱，则`true`，否则`false`
    - 当客户给20元钱时：先检查有没有一张10元和一张5元可以找零，若没有再检查有没有三张5元（贪心算法思想，优先不使用适应性最强的方法）

    ```java
    class Solution {
        public boolean lemonadeChange(int[] bills) {
            int bill_5 = 0;
            int bill_10 = 0;
    
            for (int cur : bills) {
                if (cur == 5) {
                    bill_5++;
                }
                if (cur == 10) {
                    if (bill_5 > 0) {
                        bill_5--;
                        bill_10++;
                    } else {
                        bill_10++;
                        return false;
                    }
                }
    
                if (cur == 20) {
                    if (bill_10 > 0 && bill_5 > 0) {
                        bill_10--;
                        bill_5--;
    
                    } else if (bill_5 > 2) {
                        bill_5 = bill_5 - 3;
                    } else {
                        return false;
                    }
                }
            }
            return true;
        }
    }
    ```

- 455.分发饼干

  - 贪心算法：尽量让小朋友拿到和自己的胃口相近大小的饼干，所以先对胃口数组和饼干数组进行升序排序。

    ```java
    import java.util.Arrays;
    
    class Solution {
        public int findContentChildren(int[] g, int[] s) {
            Arrays.sort(g);
            Arrays.sort(s);
    
            int index1 = 0;
            int index2 = 0;
            int count = 0;
    
            while (index1 < g.length && index2 < s.length) {
                if (g[index1] <= s[index2]) {
                    index1++;
                    index2++;
                    count++;
                } else {
                    index2++;
                }
            }
            return count;
        }
    }
    ```

    