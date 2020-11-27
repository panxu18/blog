> 给定一个大小为 n 的整数数组，找出其中所有出现超过 ⌊ n/3 ⌋ 次的元素。
>
> 进阶：尝试设计时间复杂度为 O(n)、空间复杂度为 O(1)的算法解决此问题。
>
>  
>
> 示例 1：
>
> 输入：[3,2,3]
> 输出：[3]
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/majority-element-ii
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> res = new ArrayList<>();
        int cand1 = nums[0], count1 = 0;
        int cand2 = nums[0], count2 = 0;

        for (int n : nums) {
            if (cand1 == n) {
                count1++;
            } else if (cand2 == n) {
                count2++;
            } else if (count1 == 0) {
                cand1 = n;
                count1++;
            } else if (count2 == 0) {
                cand2 = n;
                count2++;
            } else {
                count1--;
                count2--;
            }
        }
        count1 = count2 = 0;
        for (int n : nums) {
            if (n == cand1) {
                count1++;
            }else if (n == cand2) {
                count2++;
            }
        }
        if (count1 > (nums.length / 3)) {
            res.add(cand1);
        }
        if (count2 > (nums.length / 3)) {
            res.add(cand2);
        }
        return res;

    }
}
```

