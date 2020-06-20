Prepare the Bunnies' Escape
===========================

You're awfully close to destroying the LAMBCHOP doomsday device and freeing Commander Lambda's bunny prisoners, but once they're free of the prison blocks, the bunnies are going to need to escape Lambda's space station via the escape pods as quickly as possible. Unfortunately, the halls of the space station are a maze of corridors and dead ends that will be a deathtrap for the escaping bunnies. Fortunately, Commander Lambda has put you in charge of a remodeling project that will give you the opportunity to make things a little easier for the bunnies. Unfortunately (again), you can't just remove all obstacles between the bunnies and the escape pods - at most you can remove one wall per escape pod path, both to maintain structural integrity of the station and to avoid arousing Commander Lambda's suspicions. 

You have maps of parts of the space station, each starting at a prison exit and ending at the door to an escape pod. The map is represented as a matrix of 0s and 1s, where 0s are passable space and 1s are impassable walls. The door out of the prison is at the top left (0,0) and the door into an escape pod is at the bottom right (w-1,h-1). 

Write a function solution(map) that generates the length of the shortest path from the prison door to the escape pod, where you are allowed to remove one wall as part of your remodeling plans. The path length is the total number of nodes you pass through, counting both the entrance and exit nodes. The starting and ending positions are always passable (0). The map will always be solvable, though you may or may not need to remove a wall. The height and width of the map can be from 2 to 20. Moves can only be made in cardinal directions; no diagonal moves are allowed.

Languages
=========

To provide a Python solution, edit solution.py
To provide a Java solution, edit Solution.java

Test cases
==========
Your code should pass the following test cases.
Note that it may also be run against hidden test cases not shown here.

-- Python cases --
Input:
solution.solution([[0, 1, 1, 0], [0, 0, 0, 1], [1, 1, 0, 0], [1, 1, 1, 0]])
Output:
    7

Input:
solution.solution([[0, 0, 0, 0, 0, 0], [1, 1, 1, 1, 1, 0], [0, 0, 0, 0, 0, 0], [0, 1, 1, 1, 1, 1], [0, 1, 1, 1, 1, 1], [0, 0, 0, 0, 0, 0]])
Output:
    11

-- Java cases --
Input:
Solution.solution({{0, 1, 1, 0}, {0, 0, 0, 1}, {1, 1, 0, 0}, {1, 1, 1, 0}})
Output:
    7

Input:
Solution.solution({{0, 0, 0, 0, 0, 0}, {1, 1, 1, 1, 1, 0}, {0, 0, 0, 0, 0, 0}, {0, 1, 1, 1, 1, 1}, {0, 1, 1, 1, 1, 1}, {0, 0, 0, 0, 0, 0}})
Output:
    11

Use verify [file] to test your solution and see how it does. When you are finished editing your code, use submit [file] to submit your answer. If your solution passes the test cases, it will be removed from your home folder.
```java
/*
迷宫求解,但是可以穿过一个堵墙.

解题思路:
深搜实现,到达每个点有两种方式:
	1. 破墙过来的
	2. 未破墙过来
对于破墙过来的步数少于未破墙,也需要继续搜索,因为后面可能需要破墙才能到达终点.
*/
public class Solution {

    public static int solution(int[][] map) {
        int x = map[0].length - 1;
        int y = map.length - 1;
        int[][] steps = new int[map.length][];
        for (int i = 0; i < steps.length; i++) {
            steps[i] = new int[map[0].length];
            for (int j = 0; j < steps[i].length; j++) {
                steps[i][j] = Integer.MAX_VALUE;
            }
        }
        boolean[][] canBreak = new boolean[map.length][];
        for (int i = 0; i < canBreak.length; i++) {
            canBreak[i] = new boolean[map[0].length];
            for (int j = 0; j < canBreak[i].length; j++) {
                canBreak[i][j] = true;
            }
        }

        int dfs = dfs(map, x, y, 1, true/*, pathName(x, y)*/, steps, canBreak);
        // for (int i = 0; i < steps.length; i++) {
        //     for (int j = 0; j < steps[i].length; j++) {
        //         System.out.print(String.format("%d-%02d[%d], ", map[i][j] ,steps[i][j] == Integer.MAX_VALUE ? 0 : steps[i][j], canBreak[i][j] ? 1 : 0));
        //     }
        //     System.out.println();
        // }
        return dfs;
    }

    public static int dfs(int[][] map, int x, int y, int step, boolean canBreak, int[][] steps, boolean[][] canBreaks) {
        if (x == 0 && y == 0) {
//            System.out.print(step + ", ");
                       steps[0][0] = Math.min(steps[0][0],step);

            return step;
        }
        if (x < 0 || y < 0 || x >= map[0].length || y >= map.length) return Integer.MAX_VALUE;
       if (step >= steps[0][0]) return Integer.MAX_VALUE;
//        if (step == steps[y][x] && (!canBreak || canBreaks[y][x])) {
//            return Integer.MAX_VALUE;
//        }
        if (step >= steps[y][x]) {
            if (canBreak == canBreaks[y][x])
                return Integer.MAX_VALUE;
            if (canBreaks[y][x] && !canBreak) return Integer.MAX_VALUE;
        }
        canBreaks[y][x] = canBreak;
        if (map[y][x] == 1) {
            if (canBreak) {
                canBreaks[y][x] = false;
//                path += "[break:" + pathName(x, y) + "]";
            } else return Integer.MAX_VALUE;
        }
        steps[y][x] = step;
        steps[0][0] = Math.min(steps[0][0], dfs(map, x - 1, y, step + 1, canBreaks[y][x], steps, canBreaks));
        steps[0][0] = Math.min(steps[0][0], dfs(map, x, y - 1, step + 1, canBreaks[y][x], steps, canBreaks));
        steps[0][0] = Math.min(steps[0][0], dfs(map, x, y + 1, step + 1, canBreaks[y][x], steps, canBreaks));
        steps[0][0] = Math.min(steps[0][0], dfs(map, x + 1, y, step + 1, canBreaks[y][x], steps, canBreaks));
        return steps[0][0];
    }
}
/*
测试数据(95步)
{
                {0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
                {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0},
                {1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
                {1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
                {1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0},
                {1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
                {1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0},
                {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
                {0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1}, 
                {0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
                {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
                {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0},
                {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
                {0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1},
                {0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1},
                {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
                {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0},
                {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
                {0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
                {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
                }
*/
```