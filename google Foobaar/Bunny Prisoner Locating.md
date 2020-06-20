Bunny Prisoner Locating
=======================

Keeping track of Commander Lambda's many bunny prisoners is starting to get tricky. You've been tasked with writing a program to match bunny prisoner IDs to cell locations.

The LAMBCHOP doomsday device takes up much of the interior of Commander Lambda's space station, and as a result the prison blocks have an unusual layout. They are stacked in a triangular shape, and the bunny prisoners are given numerical IDs starting from the corner, as follows:

| 7
| 4 8
| 2 5 9
| 1 3 6 10

Each cell can be represented as points (x, y), with x being the distance from the vertical wall, and y being the height from the ground. 

For example, the bunny prisoner at (1, 1) has ID 1, the bunny prisoner at (3, 2) has ID 9, and the bunny prisoner at (2,3) has ID 8. This pattern of numbering continues indefinitely (Commander Lambda has been taking a LOT of prisoners). 

Write a function solution(x, y) which returns the prisoner ID of the bunny at location (x, y). Each value of x and y will be at least 1 and no greater than 100,000. Since the prisoner ID can be very large, return your solution as a string representation of the number.

Languages
=========

To provide a Java solution, edit Solution.java
To provide a Python solution, edit solution.py

Test cases
==========
Your code should pass the following test cases.
Note that it may also be run against hidden test cases not shown here.

-- Java cases --
Input:
Solution.solution(3, 2)
Output:
    9

Input:
Solution.solution(5, 10)
Output:
    96

-- Python cases --
Input:
solution.solution(5, 10)
Output:
    96

Input:
solution.solution(3, 2)
Output:
    9

Use verify [file] to test your solution and see how it does. When you are finished editing your code, use submit [file] to submit your answer. If your solution passes the test cases, it will be removed from your home folder.

```java
/**
解题思路:
等差数列求和,然后加上偏移坐标.
斜线可以看成等差数列,然后第一数列值则为其等差数列的和,每个斜线数量依次为1(1),2(2,3),3(4,5,6)

详细思路:
坐标开始(1,1)值为1, 横坐标为x,竖坐标为y.
该等差数列为1,2,3,4,5... 
其求和公式为:Sn=n*(1+n)/2
(1,y)的值=S(n-1)+1
(x,y)=(1,y)+(x-1)
/*
public class Solution {

    public static String solution(long x, long y) {
//        long h = x < y ? x + y : x;
        long l = height(x+y-1)+x-1;
        return "" + l;
    }

    public static long height(long h) {
        h -= 1;
        long number = h * (1 + h) / 2 + 1;
//        System.out.println("height=" + number);
        return number;
    }


    public static void main(String[] args) {
        System.out.println("id=" + solution(3, 2));
        System.out.println("id=" + solution(5, 10));
        System.out.println("id=" + solution(1, 1));
        System.out.println("id=" + solution(3, 1));
        System.out.println("id=" + solution(10000, 10000));
    }

    private static void print(int[] test) {
        for (int integer : test) {
            System.out.print(integer + ", ");
        }
        System.out.println();
    }
}
```