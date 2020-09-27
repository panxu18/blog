> 在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。
>
> 来源：力扣（LeetCode）

暴力解法就是枚举每个起点i，从i开始能否完成一周，这里很多重复计算。假设从起点i开始最多能够到达某一点j，那么i+1最多也只能到达j。很多起点都可以直接跳过，需要考虑的点应该是从i无法到达的点。

```java
public class LeetCode134 {
    public static void main(String[] args) {
        int[] gas = {3,1,1};
        int[] cost = {1,2,2};
        System.out.println(new LeetCode134().canCompleteCircuit(gas, cost));
    }
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int need = 0;
        int sum = 0;
        int start = 0;
        for (int i = 0; i < gas.length; i++) {
            sum += gas[i] - cost[i];
            if (sum < 0) {
                need += sum;
                sum = 0;
                start = i + 1;
            }
        }
        return need + sum >= 0 ? start : -1;
    }
}
```

