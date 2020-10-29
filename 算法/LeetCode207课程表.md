> 你这个学期必须选修 numCourse 门课程，记为 0 到 numCourse-1 。
>
> 在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：[0,1]
>
> 给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/course-schedule

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        ArrayList<ArrayList<Integer>> head = new ArrayList<>(numCourses);
        for (int i = 0; i < numCourses; i++) {
            head.add(new ArrayList<Integer>());
        }
        int[] inDegrees = new int[numCourses];
        for (int i = 0; i < prerequisites.length; i++) {
            int from = prerequisites[i][0];
            int to = prerequisites[i][1];
            inDegrees[to]++;
            head.get(from).add(to);
        }
        Queue<Integer> que = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegrees[i] == 0) {
                que.add(i);
            } 
        }
        while (!que.isEmpty()) {
            numCourses--;
            int from = que.remove();
            for (int to : head.get(from)) {
                if (--inDegrees[to] == 0) {
                    que.add(to);
                }
            }
        }
        return numCourses == 0;
    }

}
```

