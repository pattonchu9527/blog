---
title: OD Coding
---

```javascript
/**
   单调栈
 * @param nums int一维数组 
 * @return int二维数组
 */

function foundMonotoneStack( nums ) {
    let n = nums.length;
    let res = Array.from(new Array(n), () => new Array(2).fill(-1));
    let stack = [];
    for (let i = 0; i < n; i++) {
        while (stack.length && nums[stack[stack.length - 1]] >= nums[i]) {
            stack.pop();
        }
        if (!stack.length) {
            res[i][0] = -1;
        } else {
            res[i][0] = stack[stack.length - 1];
        }
        stack.push(i);
    }
    stack = [];
    for (let i = nums.length - 1; i >= 0; i--) {
        while (stack.length && nums[stack[stack.length - 1]] >= nums[i]) {
            stack.pop();
        }
        if (!stack.length) {
            res[i][1] = -1;
        } else {
            res[i][1] = stack[stack.length - 1];
        }
        stack.push(i);
    }
    return res;
}

/**
   双指针
 * @param nums int一维数组 
 * @return int二维数组
 */
function foundMonotoneStack( nums ) {
    let n = nums.length;
    let res = [];
    for (let i = 0; i < n; i++) {
        let left = i, right = i;
        let sub = [-1, -1];
        while (--left > -1) {
            if (nums[left] < nums[i]) {
                sub[0] = left;
                break;
            }
        }
        while (++right < nums.length) {
            if (nums[right] < nums[i]) {
                sub[1] = right;
                break;
            }
        }
        res.push(sub);
    }
    return res;
}
module.exports = {
    foundMonotoneStack : foundMonotoneStack
};
```
```javascript 
/**
 * 二分查找
 *
 * 如果目标值存在返回下标，否则返回 -1
 * @param nums int整型一维数组 
 * @param target int整型 
 * @return int整型
 */
function search( nums ,  target ) {
    // write code here
	let index = -1;
    let left = 0;
    let right = nums.length - 1;
    while(left <= right) {
        let mid = left + Math.floor((right - left) / 2);
        if (nums[mid] === target) {
            index = mid;
            right = mid - 1;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return index;
}
module.exports = {
    search : search
};

```

```javascript
'AABC-AABB-CC'.replace(/([a-z]*)([A-Z]*)/g, function(str, sub1, sub2) {
	return sub1.toLocaleUpperCase() + sub2.toLocaleLowerCase();
});

function countChar(str, A) {
	let count = 0;
	str.replace(new RegExp(`${A}`, 'gi'), val => count++);
	return count;
}

字符分段
function strLenSplit(str, k) {
	let i = 0;
	let arr = [];
	while(i < str.length) {
	  arr.push(str.substring(i, i+k));
	  i += k;
	}
	return arr;
}
```

# 机考一星期

http://www.amoscloud.com/?cat=57

https://blog.csdn.net/weixin_41010318/article/month/2021/10/2

## 题目86 射击比赛成绩单
```java
package com.amoscloud.newcoder.easy;

import java.util.*;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/12/22
 * Time: 11:18
 * Description: 100%
 */
public class Main105 {
  /*
  给定一个射击比赛成绩单
  包含多个选手若干次射击的成绩分数
  请对每个选手按其最高三个分数之和进行降序排名
  输出降序排名后的选手id序列
  条件如下
    1. 一个选手可以有多个射击成绩的分数，且次序不固定
    2. 如果一个选手成绩少于3个，则认为选手的所有成绩无效，排名忽略该选手
    3. 如果选手的成绩之和相等，则相等的选手按照其id降序排列

   输入描述:
     输入第一行
         一个整数N
         表示该场比赛总共进行了N次射击
         产生N个成绩分数
         2<=N<=100

     输入第二行
       一个长度为N整数序列
       表示参与每次射击的选手id
       0<=id<=99

     输入第三行
        一个长度为N整数序列
        表示参与每次射击选手对应的成绩
        0<=成绩<=100

   输出描述:
      符合题设条件的降序排名后的选手ID序列

   示例一
      输入:
        13
        3,3,7,4,4,4,4,7,7,3,5,5,5
        53,80,68,24,39,76,66,16,100,55,53,80,55
      输出:
        5,3,7,4
      说明:
        该场射击比赛进行了13次
        参赛的选手为{3,4,5,7}
        3号选手成绩53,80,55 最高三个成绩的和为188
        4号选手成绩24,39,76,66  最高三个成绩的和为181
        5号选手成绩53,80,55  最高三个成绩的和为188
        7号选手成绩68,16,100  最高三个成绩的和为184
        比较各个选手最高3个成绩的和
        有3号=5号>7号>4号
        由于3号和5号成绩相等  且id 5>3
        所以输出5,3,7,4
   */

  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int n = Integer.parseInt(in.nextLine());
    List<Integer> ids = toIntList(in.nextLine());
    List<Integer> scores = toIntList(in.nextLine());
    in.close();

    HashMap<Integer, List<Integer>> map = new HashMap<>();

    for (int i = 0; i < n; i++) {
      Integer id = ids.get(i);
      Integer score = scores.get(i);
      List<Integer> list = map.getOrDefault(id, new LinkedList<>());
      list.add(score);
      map.put(id, list);
    }
    StringBuilder builder = new StringBuilder();

    map.entrySet()
        .stream()
        .filter(x -> x.getValue().size() >= 3)
        .sorted((o1, o2) -> {
          Integer sum1 = sumT3(o1.getValue());
          Integer sum2 = sumT3(o2.getValue());
          if (sum1.equals(sum2)) {
            return o2.getKey() - o1.getKey();
          } else {
            return sum2 - sum1;
          }
        })
        .map(Map.Entry::getKey)
        .forEach(x -> builder.append(x).append(","));

    System.out.println(builder.substring(0, builder.length() - 1));

  }

  private static Integer sumT3(List<Integer> list) {
    list.sort(Integer::compareTo);
    int sum = 0;
    for (int i = list.size() - 1; i >= list.size() - 3; i--) {
      sum += list.get(i);
    }
    return sum;
  }

  private static List<Integer> toIntList(String str) {
    return Arrays.stream(str.split(","))
        .map(Integer::parseInt)
        .collect(Collectors.toList());
  }
}
```
### javascript 版本 
```javascript
// 射击比赛成绩单
// 输入:  
// 13
// 3,3,7,4,4,4,4,7,7,3,5,5,5
// 53,80,68,24,39,76,66,16,100,55,53,80,55
// 输出：
// 5,3,7,4
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let n = 0;
let ids = [];
let scores = [];

rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);
        
        sign = 1;
    } else if (sign == 1) {
        ids = line.split(',').map(i => parseInt(i));
        
        sign = 2;
    } else if (sign == 2) {
        scores = line.split(',').map(i => parseInt(i));

        sign = 3;
    }

    if (sign == 3) {
        solution(n, ids, scores);

        sign = -1;
        n = 0;
        ids = [];
        scores = [];
    }
});

function solution(n, ids, scores) {
    let map = new Map();

    for (let i = 0; i < n; i++) {
        let id = ids[i];
        let score = scores[i];
        let list = map.get(id) || [];
        list.push(score);
        map.set(id, list);
    }

    let arr = Array.from(map).filter(i => i[1].length >= 3);
    let keys = arr.sort((a, b) => {
        let sumA = sumT3(a[1]);
        let sumB = sumT3(b[1]);
        if (sumA == sumB) {
            return b[0] - a[0];
        } else {
            return sumB - sumA;
        }
    }).map(i => {
        return i[0]
    });
    
    console.log(keys.join(','));
}

function sumT3(list) {
    list.sort((a, b) => a - b);
    let sum = 0;
    for (let i = list.length - 1; i >= list.length - 3; i--) {
        sum += list[i];
    }
    return sum;
}
```


## 题目85 特殊的五键键盘
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/12/22
 * Time: 9:56
 * Description: 95%
 */
public class Main104 {
  /*
    有一个特殊的五键键盘
    上面有A、Ctrl-C、Ctrl-X、Ctrl-V、Ctrl-A
    A键在屏幕上输出一个字母A
    Ctrl-C将当前所选的字母复制到剪贴板
    Ctrl-X将当前选择的字母复制到剪贴板并清空所选择的字母
    Ctrl-V将当前剪贴板的字母输出到屏幕
    Ctrl-A选择当前屏幕中所有字母
    注意：
      1. 剪贴板初始为空
      2. 新的内容复制到剪贴板会覆盖原有内容
      3. 当屏幕中没有字母时,Ctrl-A无效
      4. 当没有选择字母时Ctrl-C、Ctrl-X无效
      5. 当有字母被选择时A和Ctrl-V这两个输出功能的键,
         会先清空所选的字母再进行输出

    给定一系列键盘输入,
    输出最终屏幕上字母的数量

    输入描述:
       输入为一行
       为简化解析用数字12345分别代替A、Ctrl-C、Ctrl-X、Ctrl-V、Ctrl-A的输入
       数字用空格分割

    输出描述:
        输出一个数字为屏幕上字母的总数量

    示例一:
        输入:
          1 1 1
        输出:
          3

   示例二:
        输入:
          1 1 5 1 5 2 4 4
        输出:
          2

   */
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    String ops = in.nextLine();
    in.close();
    String[] list = ops.split(" ");

    StringBuilder builder = new StringBuilder();

    String choose = "";
    String tab = "";

    for (String op : list) {
      switch (op) {
        case "1":
          choose = reset(builder, choose);
          builder.append('A');
          break;
        case "2":
          if (!choose.isEmpty()) {
            tab = choose;
          }
          break;
        case "3":
          if (!choose.isEmpty()) {
            tab = choose;
            choose = "";
            builder = new StringBuilder();
          }
          break;
        case "4":
          choose = reset(builder, choose);
          builder.append(tab);
          break;
        case "5":
          if (builder.length() != 0) {
            choose = builder.toString();
          }
          break;
        default:
          break;
      }

      System.out.println(builder);
      System.out.println(builder.length());
    }

    System.out.println(builder.length());
  }

  private static String reset(StringBuilder builder, String choose) {
    if (!choose.isEmpty()) {
      builder.replace(0, choose.length(), "");
      choose = "";
    }
    return choose;
  }
}
```
### javascript 版本
```javascript
// 特殊的五键键盘
// 5键盘
// 输入: 1 1 5 1 5 2 4 4
// 输出: 2    
// 输入: 1 1 1
// 输出:  3  
          
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let arr = line.split(' ');
    solution(arr);
});

function solution(arr) {
    let screen = [];
    let choose = '';
    let tab = '';

    for (let key of arr) {
        switch (key) {
            case "1":
                if (choose) {
                    screen = [];
                    choose = '';
                }
                screen.push('A')
                break;
              case "2":
                if (choose) {
                  tab = choose;
                }
                break;
              case "3":
                if (choose) {
                  tab = choose;
                  choose = "";
                  screen = [];
                }
                break;
              case "4":
                if (choose) {
                    screen = [];
                    choose = '';
                }
                screen.push(tab);
                break;
              case "5":
                let str = screen.join('')
                if (str.length != 0) {
                  choose = str;
                }
                break;
              default:
                break;
        }
    }
    console.log(screen.join('').length);
}
```

## 题目70 水仙花数
```java
package com.amoscloud.newcoder.easy;

import java.util.LinkedList;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/8/18
 * Time: 17:42
 * Description:100%
 */
public class Main78 {
    /*
    所谓的水仙花数是指一个n位的正整数其各位数字的n次方的和等于该数本身，
    例如153=1^3+5^3+3^3,153是一个三位数
    输入描述
        第一行输入一个整数N，
        表示N位的正整数N在3-7之间包含3,7
        第二行输入一个正整数M，
        表示需要返回第M个水仙花数
    输出描述
        返回长度是N的第M个水仙花数，
        个数从0开始编号，
        若M大于水仙花数的个数返回最后一个水仙花数和M的乘积，
        若输入不合法返回-1

    示例一：

        输入
         3
         0
        输出
         153
        说明：153是第一个水仙花数
     示例二：
        输入
        9
        1
        输出
        -1
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        int N = Integer.parseInt(in.nextLine());
        int M = Integer.parseInt(in.nextLine());
        in.close();

        if (N < 3 || N > 7) {
            System.out.println(-1);
            return;
        }

        LinkedList<Integer> res = new LinkedList<>();

        int start = (int) Math.pow(10, N - 1);
        int end = (int) Math.pow(10, N);

        for (int i = start; i < end; i++) {
            int sum = 0;
            int bit = start;
            while (bit != 1) {
                sum += Math.pow(i / bit % 10, N);
                bit /= 10;
            }
            sum += Math.pow(i % 10, N);
            if (sum == i) {
                res.add(i);
            }
            if (res.size() == M + 1) {
                System.out.println(i);
                return;
            }
        }

        if (M > res.size()) {
            System.out.println(M * res.getLast());
        }

    }
}
```
### javascript
```javascript
// 水仙花数 返回长度是N的第M个水仙花数
// 输入
// 3
// 0
// 输出
// 153
// 输入
// 9
// 1
// 输出
// -1

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let N = 0;
let M = 0;

rl.on('line', line => {
    if (sign == -1) {
        N = parseInt(line);

        sign = 1;
    } else if (sign == 1) {
        M = parseInt(line);

        if (N < 3 || N > 7) {
            console.log(-1);
            return;
        }

        solution(N, M);
        sign = -1;
        N = 0;
        M = 0;    
    }
});

function solution(N, M) {
    let res = [];
    let start = parseInt(Math.pow(10, N - 1));
    let end = parseInt(Math.pow(10, N));

    for (let i = start; i < end; i++) {
        let sum = 0;
        let carry = start;
        while (carry != 1) {
            sum += parseInt(Math.pow(parseInt(i / carry % 10), N));
            carry = parseInt(carry / 10);
        }
        sum += parseInt(Math.pow(i % 10, N));
        if (sum == i) {
            res.push(i);
        }
        if (res.length == M + 1) {
            console.log(i);
            return;
        }
    }
    if (M > res.length) {
        console.log(M * res[res.length - 1]);
    }
}
```

## 题目81  计费表
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/12/1
 * Time: 18:25
 * Description:
 */
public class Main95 {
  /*
    程序员小明打了一辆出租车去上班。出于职业敏感，他注意到这辆出租车的计费表有点问题，总是偏大。
  出租车司机解释说他不喜欢数字4，所以改装了计费表，任何数字位置遇到数字4就直接跳过，其余功能都正常。
  比如：
    1. 23再多一块钱就变为25；
    2. 39再多一块钱变为50；
    3. 399再多一块钱变为500；
    小明识破了司机的伎俩，准备利用自己的学识打败司机的阴谋。
    给出计费表的表面读数，返回实际产生的费用。

    输入描述:
      只有一行，数字N，表示里程表的读数。
      (1<=N<=888888888)。
    输出描述:
      一个数字，表示实际产生的费用。以回车结束。
    示例1：
    输入
      5
    输出
      4
    说明
      5表示计费表的表面读数。
      表示实际产生的费用其实只有4块钱。

    示例2：
    输入
      17
    输出
      15
    说明
      17表示计费表的表面读数。
      15表示实际产生的费用其实只有15块钱。
    示例3：
    输入
      100
    输出
      81
    说明：100表示计费表的表面读数，81表示实际产生的费用其实只有81块钱
   */
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int N = in.nextInt();
    int ans = N, temp = 0, k = 0, j = 1;
    while (N > 0) {
      //先判断个位上是否跳了4，如果个位上是5~9，就先temp=1。
      if (N % 10 > 4) {
        temp += (N % 10 - 1) * k + j;
      } else {
        temp += (N % 10) * k;
      }
      k = k * 9 + j;//k代表跳了多少次4，多收了多少个1元
      j *= 10;//j代表位数，1代表个位，10代表十位
      N /= 10;//相当于将N整体右移一位
    }
    System.out.println(ans - temp);
  }
}
```

### javascript
```javascript
// 计费表
// 输入： 5
// 输出： 4
// 输入： 17
// 输出： 15
// 输入： 100
// 输出： 81

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let num = parseInt(line);
    solution(num);
});

function solution(N) {
    let  ans = N, temp = 0, k = 0, j = 1;
    while (N > 0) {
        if (N % 10 > 4) {
            temp += (N % 10 - 1) * k + j;
        } else {
            temp += (N % 10) * k;
        }
        k = k * 9 + j;
        j *= 10;
        N = parseInt( N / 10 ); //!!!! 特别重要 有坑  java里默认 /= 取整
    }
    console.log(ans - temp);
}
```

## 题目74 统计停车场最少可以停多少辆车
```java

package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/10/27
 * Time: 14:45
 * Description:
 */
public class Main85 {
  /*
    特定大小的停车场 数组cars表示
    其中1表示有车  0表示没车
    车辆大小不一，小车占一个车位(长度1)
    货车占两个车位(长度2)
    卡车占三个车位(长度3)
    统计停车场最少可以停多少辆车
    返回具体的数目

    输入描述：
      整型字符串数组cars
      其中1表示有车0表示没车
      数组长度<1000

    输出描述：
      整型数字字符串
      表示最少停车数

    示例1：
      输入
        1,0,1
      输出
        2
      说明：
        一个小车占第一个车位
        第二个车位空，一个小车占第三个车位
        最少有两辆车

     示例2:
       输入：
         1,1,0,0,1,1,1,0,1
       输出：
         3
       说明：
         一个货车占第1,2个车位
         第3,4个车位空
         一个卡车占第5,6,7个车位
         第8个车位空
         一个小车占第9个车位
         最少3俩个车
     */

  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    String cars = in.nextLine()
        .replaceAll(",", "");
    in.close();

    int count = 0;

    String[] split = cars.split("[0]+");
    for (String car : split) {
      int len = car.length();
      while (len > 3) {
        count++;
        len -= 3;
      }
      if (len != 0) {
        count++;
      }
    }

    System.out.println(count);
  }
}
```

### javascript
```javascript
// 统计停车场最少可以停多少辆车
// 输入：
// 1,0,1
// 输出：
// 2
// 输入：
// 1,1,0,0,1,1,1,0,1
// 输出：
// 3

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let carsStr = line.replace(/\,/g, '')
    solution(carsStr);
});

function solution(str) {
    let cars = str.split(/[0]+/);
    let count = 0;

    for (let car of cars) {
        let len = car.length;
        while (len > 3) {
            count++;
            len -= 3;
        }
        if (len != 0) {
            count++;
        }
    }
    console.log(count);
}
```

## 题目53 停车场 最大距离
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/7
 * Time: 19:57
 * Description: 90%
 */
public class Main41 {
    /*
       停车场有一横排车位0代表没有停车,1代表有车.
       至少停了一辆车在车位上,也至少有一个空位没有停车.
       为防止刮蹭,需为停车人找到一个车位
       使得停车人的车最近的车辆的距离是最大的
       返回此时的最大距离

       输入描述:
       1. 一个用半角逗号分割的停车标识字符串,停车标识为0或1,
        0为空位,1为已停车
       2. 停车位最多有100个

       输出描述
       1. 输出一个整数记录最大距离

       示例一:
       输入
       1,0,0,0,0,1,0,0,1,0,1

        0,0,1,1,0,0
       输出
       2

       说明
       当车停在第三个位置上时,离其最近的车距离为2(1~3)
       当车停在第四个位置上时,离其最近的车距离为2(4~6)
       其他位置距离为1
       因此最大距离为2
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String line = in.nextLine()
                .replaceAll(",", "");
        char[] sites = line.toCharArray();
        in.close();

        int max = 0;

        for (int i = 0; i < sites.length; i++) {
            char cur_site = sites[i];
            if (cur_site == '0') {
                int pre = line.indexOf('1', i);
                int suf = line.lastIndexOf('1', i);
                if (pre == -1) pre = 100;
                if (suf == -1) suf = line.length() - 1;
                int min = Math.min(pre - i, i - suf);
                if (min > max) max = min;
            }
        }

        System.out.println(max);

    }
}
```
### javascript
```javascript
// 停车场 间隔 最大距离
// 输入 逗号分割停车标识
// 输出 最大距离

const { listenerCount } = require('process');
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let str = line.replace(/\,/g, '');
    solution(str);
});

function solution(str) {
    let max = 0;
    let cars = str.split('');

    for (let i = 0; i < cars.length; i++) {
        let car = cars[i];
        if (car == '0') {
            let left = str.indexOf('1', i);
            let right = str.lastIndexOf('1', i);
            if (left == -1) {
                left = 100;
            }
            if (right == -1) {
                right = str.length - 1;
            }
            let min = Math.min(left - i, i - right);
            if (min > max) {
                max = min;
            }
        }
    }
    console.log(max);
}
```

## 题目80 给定n和k,返回第k个排列
```java
package com.amoscloud.newcoder.easy;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/11/16
 * Time: 17:42
 * Description:
 */
public class Main92 {

  /*
        给定参数n,从1到n会有n个整数:1,2,3,...,n,
        这n个数字共有n!种排列.
      按大小顺序升序列出所有排列的情况,并一一标记,
      当n=3时,所有排列如下:
      "123" "132" "213" "231" "312" "321"
      给定n和k,返回第k个排列.

      输入描述:
        输入两行，第一行为n，第二行为k，
        给定n的范围是[1,9],给定k的范围是[1,n!]。
      输出描述：
        输出排在第k位置的数字。

      实例1：
        输入:
          3
          3
        输出：
          213
        说明
          3的排列有123,132,213...,那么第三位置就是213

      实例2：
        输入
          2
          2
        输出：
          21
        说明
          2的排列有12,21，那么第二位置的为21
   */
  public static void main(String[] args) {

    Scanner in = new Scanner(System.in);
    int n = Integer.parseInt(in.nextLine());
    int k = Integer.parseInt(in.nextLine());
    StringBuilder sb = new StringBuilder();

    List<Integer> candidates = new ArrayList<>();

    int[] factorials = new int[n + 1];
    factorials[0] = 1;
    int fact = 1;
    for (int i = 1; i <= n; ++i) {
      candidates.add(i);
      fact *= i;
      factorials[i] = fact;
    }
    k -= 1;
    for (int i = n - 1; i >= 0; --i) {
      // 计算候选数字的index
      int index = k / factorials[i];
      sb.append(candidates.remove(index));
      k -= index * factorials[i];
    }
    System.out.println(sb);
  }
}
```
### javascript
```javascript
// n个数字 n!排列 按大小顺序升序排列 给定n和k,返回第k个排列
// 输入 
// 2
// 2
// 输出 21
// 输入 
// 3
// 3
// 输出 213
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let n = 0;
let k = 0;
rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);

        sign = 1;
    } else if (sign == 1) {
        k = parseInt(line);
        
        solution(n, k);
        
        sign = -1;
        n = 0;
        k = 0;
    }
});

function solution(n, k) {
    let res = [];
    let list = [];

    let dp = new Array(n + 1);
    dp[0] = 1;
    let fact = 1;
    for (let i = 1; i <= n; ++i) {
        list.push(i);
        fact *= i;
        dp[i] = fact;
    }

    k -= 1;
    for (let i = n - 1; i >= 0; --i) {
        let index = k / dp[i];
        res.push(list.splice(index, 1));
        k -= index * dp[i];
    }
    console.log(res.join(''));
}
```
## 题目79 运动会 身高体重排序
```java
package com.amoscloud.newcoder.easy;

import java.util.LinkedList;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/12/1
 * Time: 18:07
 * Description: 100%
 */
public class Main93 {

  /*
   某学校举行运动会,学生们按编号（1、2、3.....n)进行标识,
   现需要按照身高由低到高排列，
   对身高相同的人，按体重由轻到重排列，
   对于身高体重都相同的人，维持原有的编号顺序关系。
   请输出排列后的学生编号
   输入描述：
      两个序列，每个序列由N个正整数组成，(0<n<=100)。
      第一个序列中的数值代表身高，第二个序列中的数值代表体重，
   输出描述：
      排列结果，每个数据都是原始序列中的学生编号，编号从1开始，
   实例一：
      输入:
       4
       100 100 120 130
       40 30 60 50
      输出:
       2 1 3 4
       输入
        3
        90 110 90
        45 60 45
        输出
        1 3 2
   */
  static class Stu {
    int id;
    int h;
    int w;

    public Stu(int id, int h, int w) {
      this.id = id;
      this.h = h;
      this.w = w;
    }
  }

  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int n = Integer.parseInt(in.nextLine());
    String[] h = in.nextLine().split(" ");
    String[] w = in.nextLine().split(" ");
    in.close();
    LinkedList<Stu> stus = new LinkedList<>();
    for (int i = 0; i < n; i++) {
      Stu stu = new Stu(i + 1, Integer.parseInt(h[i]), Integer.parseInt(w[i]));
      stus.add(stu);
    }
    stus.sort((o1, o2) -> o1.h == o2.h ? (o1.w - o2.w) : o1.h - o2.h);
    StringBuilder builder = new StringBuilder();
    stus.forEach(x -> builder.append(x.id).append(" "));
    System.out.println(builder.substring(0, builder.length() - 1));
  }
}
```
### Javascript

```java
// 运动会  按身高 体重升序排序 输出学生编号
// 输入：
// 4
// 100 100 120 130
// 40 30 60 50
// 输出:
// 2 1 3 4

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let n = 0;
let heights = [];
let weights = [];

rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);
        
        sign = 1;
    } else if (sign == 1) {
        heights = line.split(' ').map(i => parseInt(i));

        sign = 2;
    } else if (sign == 2) {
        weights = line.split(' ').map(i => parseInt(i));

        solution(n, heights, weights);

        sign = -1;
        n = 0;
        heights = [];
        weights = [];
    }
});

function solution(n, heights, weights) {
    let studs = [];
    for (let i = 0; i < n; i++) {
        let obj = {id: i + 1, h: heights[i], w: weights[i]};
        studs.push(obj);
    }
    studs.sort((a, b) => a.h == b.h ? (a.w - b.w) : a.h - b.h);

    let res = studs.map(i => { return i.id; });
    console.log(res.join(' '));
}
```


## 题目7 和小明身高差绝对值比较
```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/7
 * Time: 14:40
 * Description:
 */
public class Demo7 {
    public static void main(String[] args) {
        /*
         小明今年升学到了小学1年纪
         来到新班级后，发现其他小朋友身高参差不齐
         然后就想基于各小朋友和自己的身高差，对他们进行排序
         请帮他实现排序
         输入描述
          第一行为正整数 h和n
          0<h<200 为小明的身高
          0<n<50 为新班级其他小朋友个数
          第二行为n各正整数
           h1 ~ hn分别是其他小朋友的身高
         取值范围0<hi<200
         且n个正整数各不相同

         输出描述
          输出排序结果，各正整数以空格分割
          和小明身高差绝对值最小的小朋友排在前面
          和小明身高差绝对值最大的小朋友排在后面
          如果两个小朋友和小明身高差一样
          则个子较小的小朋友排在前面

          示例一
          输入
          100 10
          95 96 97 98 99 101 102 103 104 105
          输出
           99 101 98 102 97 103 96 104 95 105

          说明  小明身高100
          班级学生10个  身高分别为
         */

        Scanner in = new Scanner(System.in);
        String[] split1 = in.nextLine().split(" ");
        int h = Integer.parseInt(split1[0]);
        String[] split2 = in.nextLine().split(" ");
        ArrayList<Integer> list = new ArrayList<>();
        for (String s : split2) {
            list.add(Integer.parseInt(s));
        }

        list.sort(new Comparator<Integer>() {
            @Override
            public int compare(Integer h1, Integer h2) {
                int d1 = h - h1;
                int d2 = h - h2;
               if ((d1 >0?d1:-d1)==(d2 >0?d2:-d2)){
                   return h1-h2;
               }else return (d1 >0?d1:-d1)-(d2 >0?d2:-d2);
            }
        });

        StringBuilder builder = new StringBuilder();
        for (Integer integer : list) {
            builder.append(integer).append(" ");
        }
        System.out.println(builder.toString().trim());

        in.close();
    }

}

```
### javascript
```javascript
// 和小明升高绝对值比较
// 然后就想基于各小朋友和自己的身高差，对他们进行排序
// 输入 第一行为正整数 h和n 第二行为n各正整数
// 输出描述
// 输出排序结果，各正整数以空格分割
// 和小明身高差绝对值最小的小朋友排在前面
// 和小明身高差绝对值最大的小朋友排在后面
// 如果两个小朋友和小明身高差一样
// 则个子较小的小朋友排在前面

// 示例一
// 输入
// 100 10
// 95 96 97 98 99 101 102 103 104 105
// 输出
// 99 101 98 102 97 103 96 104 95 105

// 说明  小明身高100
// 班级学生10个  身高分别为

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1; 
let h = 0;
let n = 0;
rl.on('line', line => {
    if (sign == -1) {
        let tmp = line.split(' ').map(i => parseInt(i));
        h = tmp[0];
        n = tmp[1];

        sign = 1;
    } else  {
        let arr = line.split(' ').map(i => parseInt(i));
        arr.sort((a, b) => {
            let d1 = h - a;
            let d2 = h - b;
            if ((d1 > 0 ? d1 : -d1) == (d2 > 0 ? d2 : -d2)) {
                return a - b;
            } else {
                return (d1 > 0 ? d1 : -d1) - (d2 > 0 ? d2 : -d2);
            }
        });
        console.log(arr.join(' '));
        // rl.close();
    }
});
```

## 题目76 N个小朋友的好朋友的位置
```java
package com.amoscloud.newcoder.easy;

import java.util.Arrays;
import java.util.LinkedList;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/10/27
 * Time: 17:29
 * Description:
 */
public class Main88 {
  /*
  在学校中
  N个小朋友站成一队
  第i个小朋友的身高为height[i]
  第i个小朋友可以看到第一个比自己身高更高的小朋友j
  那么j是i的好朋友
  (要求：j>i)
  请重新生成一个列表
  对应位置的输出是每个小朋友的好朋友的位置
  如果没有看到好朋友
  请在该位置用0代替
  小朋友人数范围 0~40000

  输入描述：
    第一行输入N
    N表示有N个小朋友

    第二行输入N个小朋友的身高height[i]
    都是整数

  输出描述：
    输出N个小朋友的好朋友的位置

  示例1：
     输入：
       2
       100 95
      输出
       0 0
     说明
       第一个小朋友身高100站在队伍末尾
       向队首看 没有比他身高高的小朋友
       所以输出第一个值为0
       第二个小朋友站在队首前面也没有比他身高高的小朋友
       所以输出第二个值为0

   示例2：
      输入
        8
        123 124 125 121 119 122 126 123
      输出
        1 2 6 5 5 6 0 0
       说明：
       123的好朋友是1位置上的124
       124的好朋友是2位置上的125
       125的好朋友是6位置上的126
        依此类推

   */

  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int n = Integer.parseInt(in.nextLine());

    if (n == 0) {
      System.out.println(0);
      return;
    }
    String[] strs = in.nextLine().split(" ");

    List<Integer> height = Arrays.stream(strs)
        .map(Integer::parseInt)
        .collect(Collectors.toList());

    LinkedList<Integer> res = new LinkedList<>();

    for (int i = 0; i < height.size(); i++) {
      int pos = 0;
      for (int j = i + 1; j < height.size(); j++) {
        if (height.get(j) > height.get(i)) {
          pos = j;
          break;
        }
      }
      res.add(pos);
    }

    StringBuilder builder = new StringBuilder();
    res.forEach(x -> builder.append(x).append(" "));
    if (builder.length() > 1) {
      String substring = builder.substring(0, builder.length() - 1);
      System.out.println(substring);
    }

  }
}
```
### javascript
```javascript
// 输出N个小朋友的好朋友的位置
// 输入
// 8
// 123 124 125 121 119 122 126 123
// 输出
// 1 2 6 5 5 6 0 0
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let n = 0;
let heights = [];

rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);
        if (n == 0) {
            console.log(0);
            return;
        }

        sign = 1;
    } else if (sign == 1) {
        heights = line.split(' ').map(i => parseInt(i));

        solution(heights);
        sign = -1;
        heights = [];
        n = 0;
    }
});

function solution(heights) {
    let len = heights.length;
    let res = [];

    for (let i = 0; i < len; i++) {
        let pos = 0;
        for (let j = i + 1; j < len; j++) {
            if (heights[j] > heights[i]) {
                pos = j;
                break;
            }
        }
        res.push(pos);
    }
    console.log(res.join(' '));
}
```

## 题目40 小朋友排队是否同班
```java
import java.util.ArrayList;
import java.util.Scanner;
import java.util.TreeSet;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/26
 * Time: 15:03
 * Description:
 */
public class Main19 {
    public static void main(String[] args) {
        /*
        幼儿园两个班的小朋友排队时混在了一起
        每个小朋友都知道自己跟前面一个小朋友是不是同班
        请你帮忙把同班的小朋友找出来
        小朋友的编号为整数
        与前面一个小朋友同班用Y表示
        不同班用N表示
        输入描述：
        输入为空格分开的小朋友编号和是否同班标志
        比如 6/N 2/Y 3/N 4/Y
        表示一共有4位小朋友
        2和6是同班 3和2不同班 4和3同班
        小朋友总数不超过999
         0< 每个小朋友编号 <999
         不考虑输入格式错误

         输出两行
         每一行记录一班小朋友的编号  编号用空格分开
         并且
         1. 编号需要按照大小升序排列，分班记录中第一个编号小的排在第一行
         2. 如果只有一个班的小朋友 第二行为空
         3. 如果输入不符合要求输出字符串ERROR

         示例：
         输入
         1/N 2/Y 3/N 4/Y
         输出
         1 2
         3 4
         说明：2的同班标记为Y因此和1同班
              3的同班标记位N因此和1,2不同班
              4的同班标记位Y因此和3同班
         */

        Scanner in = new Scanner(System.in);
        String[] stus = in.nextLine().split(" ");
        in.close();

        try {

            TreeSet<Integer> c1 = new TreeSet<>();
            TreeSet<Integer> c2 = new TreeSet<>();

            boolean is1 = true;
            for (int i = 0; i < stus.length; i++) {
                String[] split = stus[i].split("/");
                String id = split[0];
                String same = split[1];
                if (i == 0) {
                    c1.add(Integer.parseInt(id));
                    continue;
                }
                if ("N".equals(same)) is1 = !is1;
                (is1 ? c1 : c2).add(Integer.parseInt(id));
            }

            StringBuilder b1 = new StringBuilder();
            for (Integer id : c1) b1.append(id).append(" ");

            if (c2.size() > 0) {
                StringBuilder b2 = new StringBuilder();
                for (Integer id : c2) b2.append(id).append(" ");
                if (c1.first() < c2.first()) {
                    System.out.println(b1.toString().trim());
                    System.out.println(b2.toString().trim());
                } else {
                    System.out.println(b2.toString().trim());
                    System.out.println(b1.toString().trim());
                }
            } else {
                System.out.println(b1.toString().trim());
            }

        } catch (Exception e) {
            System.out.println("ERROR");
        }

    }

}
```
### javascript
``` javascript
// 小朋友排队是否同班
// 与前面一个小朋友同班用Y表示
// 不同班用N表示
// 输入：空格分开 是否同班
// 输出：两行 每一行记录一班小朋友编号 空格分开 升序排列 只有一班 第二行为空 不符合 ERROR
// 输入
// 1/N 2/Y 3/N 4/Y
// 输出
// 1 2
// 3 4
// 说明：2的同班标记为Y因此和1同班
//      3的同班标记位N因此和1,2不同班
//      4的同班标记位Y因此和3同班

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
   let students = line.split(' ');

   try {
        solution(students);
   } catch(e) {
       console.log('ERROR');
   }
   
   rl.close();
});

function solution(students) {
    let c1 = new Set();
    let c2 = new Set();
    let isOne = true;

    for (let i = 0; i < students.length; i++) {
        let chars = students[i].split('/');
        let id = chars[0];
        let same = chars[1];
        if (i == 0) {
           c1.add(parseInt(id));
           continue;
        }
        if ('N' == same) isOne = !isOne;
        (isOne ? c1 : c2).add(parseInt(id));
    }

    let arr1 = Array.from(c1);
    let arr2 = Array.from(c2);
    if (arr2.length > 0) {
        if (arr1[0] < arr2[0]) {
            console.log(arr1.join(' '));
            console.log(arr2.join(' '));
        } else {
            console.log(arr2.join(' '));
            console.log(arr1.join(' '));
        }
    } else {
        console.log(arr1.join(' '));
    }
}
```


## 题目71 小朋友高矮高矮排队 最小移动距离
```java
package com.amoscloud.newcoder.easy;

import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/8/27
 * Time: 10:15
 * Description: 100%
 */
public class Main80 {
    /*
    现在有一队小朋友，他们高矮不同，
    我们以正整数数组表示这一队小朋友的身高，如数组{5,3,1,2,3}。
    我们现在希望小朋友排队，以“高”“矮”“高”“矮”顺序排列，
    每一个“高”位置的小朋友要比相邻的位置高或者相等；
    每一个“矮”位置的小朋友要比相邻的位置矮或者相等；
    要求小朋友们移动的距离和最小，第一个从“高”位开始排，输出最小移动距离即可。
    例如，在示范小队{5,3,1,2,3}中，{5, 1, 3, 2, 3}是排序结果。
    {5, 2, 3, 1, 3} 虽然也满足“高”“矮”“高”“矮”顺序排列，
    但小朋友们的移动距离大，所以不是最优结果。
    移动距离的定义如下所示：
    第二位小朋友移到第三位小朋友后面，移动距离为1，
    若移动到第四位小朋友后面，移动距离为2；

    输入描述:
        排序前的小朋友，以英文空格的正整数：
        4 3 5 7 8
        注：小朋友<100个
    输出描述:
        排序后的小朋友，以英文空格分割的正整数：
        4 3 7 5 8
    备注：4（高）3（矮）7（高）5（矮）8（高），
    输出结果为最小移动距离，只有5和7交换了位置，移动距离都是1.

     示例一：
     输入
       4 1 3 5 2
     输出
       4 1 5 2 3

     示例二：
     输入
       1 1 1 1 1
     输出
       1 1 1 1 1
     说明：相邻位置可以相等

     示例三：
     输入：
       xxx
     输出
       []
     说明：出现非法参数情况，返回空数组
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        List<Integer> high = null;
        try {
            high = Arrays.stream(in.nextLine().split(" "))
                    .map(Integer::parseInt).collect(Collectors.toList());
        } catch (Exception e) {
            System.out.println("[]");
            return;
        } finally {
            in.close();
        }

        for (int i = 0; i < high.size() - 1; i++) {
            if (i % 2 == 0 && high.get(i) < high.get(i + 1)) {
                swap(high, i, i + 1);
            }
            if (i % 2 == 1 && high.get(i) > high.get(i + 1)) {
                swap(high, i, i + 1);
            }
        }
        StringBuilder builder = new StringBuilder();
        high.forEach(x -> builder.append(x).append(" "));
        String res = builder.substring(0, builder.length() - 1);
        System.out.println(res);

    }

    static void swap(List<Integer> list, int x, int y) {
        Integer tmp = list.get(x);
        list.set(x, list.get(y));
        list.set(y, tmp);
    }
}
```

### javascript
```javascript
// 小朋友 高矮高矮顺序排队 输出最小移动距离
// 输入：
// 4 1 3 5 2
// 输出：
// 4 1 5 2 3
// 输入：
// 1 1 1 1 1
// 输出：
// 1 1 1 1 1

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let heights = line.split(' ').map(i => parseInt(i));
    if (!heights || !heights.length) {
        console.log('[]');
        return;
    }

    solution(heights);
});

function solution(heights) {
    for (let i = 0; i < heights.length; i++) {
        if (i % 2 == 0 && heights[i] < heights[i + 1]) {
            dealMove(heights, i, i + 1);
        }

        if (i % 2 == 1 && heights[i] > heights[i + 1]) {
            dealMove(heights, i, i + 1);
        }
    }

    console.log(heights.join(' '));
}
function dealMove(arr, left, right) {
    let tmp = arr[left];
    arr[left] = arr[right];
    arr[right] = tmp;
}
```

## 题目78  对日志进行排序
```java
package com.amoscloud.newcoder.easy;

import java.util.Comparator;
import java.util.LinkedList;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/11/2
 * Time: 17:12
 * Description:
 */
public class Main90 {
  /*
  运维工程师采集到某产品线网运行一天产生的日志n条
  现需根据日志时间先后顺序对日志进行排序
  日志时间格式为H:M:S.N
  H表示小时(0~23)
  M表示分钟(0~59)
  S表示秒(0~59)
  N表示毫秒(0~999)
  时间可能并没有补全
  也就是说
  01:01:01.001也可能表示为1:1:1.1

  输入描述
     第一行输入一个整数n表示日志条数
     1<=n<=100000
     接下来n行输入n个时间

   输出描述
     按时间升序排序之后的时间
     如果有两个时间表示的时间相同
     则保持输入顺序

   示例：
     输入：
      2
      01:41:8.9
      1:1:09.211
     输出
       1:1:09.211
       01:41:8.9
   示例
      输入
       3
       23:41:08.023
       1:1:09.211
       08:01:22.0
      输出
        1:1:09.211
        08:01:22.0
        23:41:08.023

    示例
      输入
        2
        22:41:08.023
        22:41:08.23
      输出
        22:41:08.023
        22:41:08.23
      时间相同保持输入顺序
       */
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int n = Integer.parseInt(in.nextLine());
    LinkedList<String> times = new LinkedList<>();
    for (int i = 0; i < n; i++) {
      times.add(in.nextLine());
    }
    in.close();

    times.sort(Comparator.comparingLong(Main90::getTime));

    times.forEach(System.out::println);
  }

  private static long getTime(String str) {
    String[] t1 = str.split(":");
    String[] t2 = t1[2].split("\\.");
    int h = Integer.parseInt(t1[0]) * 60 * 60 * 1000;
    int m = Integer.parseInt(t1[1]) * 60 * 1000;
    int s = Integer.parseInt(t2[0]) * 1000;
    int n = Integer.parseInt(t2[1]);
    return h + m + s + n;
  }

}
```

### Javascript

```javascript
// 日志排序
// 输入
// 3
// 23:41:08.023
// 1:1:09.211
// 08:01:22.0
// 输出
// 1:1:09.211
// 08:01:22.0
// 23:41:08.023

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let n = 0;
let times = [];

rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);
        
        sign = 1;
    } else {
        times.push(line);
    }

    if (times.length == n) {
        solution(times);

        sign = -1;
        n = 0;
        times = [];
    }
});

function solution(times) {
    times.sort((a, b) => getTime(a) - getTime(b));
    times.forEach(i => console.log(i));
}

function getTime(str) {
    let t1 = str.split(":");
    let t2 = t1[2].split('.');
    let h = parseInt(t1[0]) * 60 * 60 * 1000;
    let m = parseInt(t1[1]) * 60 * 1000;
    let s = parseInt(t2[0]) * 1000;
    let nn = parseInt(t2[1]);
    return h + m + s + nn;
}
```
## 题目10 从字符串2中找出字符串1中的所有字符
```java
import java.util.*;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/13
 * Time: 15:54
 * Description:
 */
public class Demo10 {
    public static void main(String[] args) {
        /*
        给定两个字符串
        从字符串2中找出字符串1中的所有字符
        去重并按照ASCII码值从小到大排列
        输入字符串1长度不超过1024
        字符串2长度不超过100

        字符范围满足ASCII编码要求，按照ASCII由小到大排序

        输入描述：
         bach
         bbaaccddfg
         输出
          abc

          2
          输入
          fach
          bbaaccedfg
          输出
          acf

         */

        Scanner in = new Scanner(System.in);
        TreeSet<String> res = new TreeSet<>();
        String[] split = in.nextLine().split("");
        String str2 = in.nextLine();
        for (String s : split) {
            if (str2.contains(s)) res.add(s);
        }
        for (String re : res) System.out.print(re);
        in.close();
    }
}

```

## 题目75  字符串中相同字符连续出现的最大次数
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/10/27
 * Time: 17:11
 * Description:
 */
public class Main87 {
  /*
  输入一串字符串
  字符串长度不超过100
  查找字符串中相同字符连续出现的最大次数

  输入描述
    输入只有一行，包含一个长度不超过100的字符串

  输出描述
    输出只有一行，输出相同字符串连续出现的最大次数

   说明：
   示例1：
     输入
       hello
     输出
       2

    示例2：
      输入
       word
      输出
       1

     示例3：
      输入
        aaabbc
       输出
        3

    字符串区分大小写
   */

  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    String str = in.nextLine();
    in.close();

    char[] chars = str.toCharArray();

    int maxLen = 0;

    for (int i = 0; i < chars.length; i++) {
      int index = i;
      int len = 1;
      while (index + 1 < chars.length && chars[index + 1] == chars[index]) {
        len++;
        index++;
      }
      if (len > maxLen) maxLen = len;
    }

    System.out.println(maxLen);

  }

}
```

### javascript
```javascript
// 查找字符串中相同字符连续出现的最大次数
// 输入: 
// hello
// 输出:
// 2
// 输入
// aaabbc
// 输出
// 3

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let chars = line.split('');
    solution(chars);
});

function solution(chars) {
    let maxLen = 0;

    for (let i = 0; i < chars.length; i++) {
        let start = i;
        let len = 1;
        while (start + 1 < chars.length && chars[start + 1] == chars[start]) {
            len++;
            start++;
        }
        if (len > maxLen) {
            maxLen = len;
        }
    }
    console.log(maxLen);
}
```

## 题目57 大写字母 连续出现次数第k多的字母的次数
```java
package com.amoscloud.newcoder.easy;

import java.util.*;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/13
 * Time: 17:44
 * Description:
 */
public class Main50 {
    public static void main(String[] args) {

/*
    给定一个字符串
    只包含大写字母
    求在包含同一字母的子串中
    长度第K长的子串
    相同字母只取最长的子串

    输入
     第一行 一个子串 1<len<=100
     只包含大写字母
     第二行为k的值

     输出
     输出连续出现次数第k多的字母的次数

     例子：
     输入
             AABAAA
             2
     输出
             1
       同一字母连续出现最多的A 3次
       第二多2次  但A出现连续3次

    输入

    AAAAHHHBBCDHHHH
    3

    输出
    2

//如果子串中只包含同一字母的子串数小于k

则输出-1

 */

        Scanner in = new Scanner(System.in);
        String line = in.nextLine();
        int k = in.nextInt();
        HashMap<Character, Integer> map = new HashMap<>();

        char[] chars = line.toCharArray();
        if (chars.length == 0) {
            System.out.println(-1);
            return;
        }

        char cur = chars[0];
        int count = 1;
        map.put(cur, count);

        for (int i = 1; i < chars.length; i++) {
            char c = chars[i];
            if (c == cur) count++;
            else {
                cur = c;
                count = 1;
            }
            map.put(cur, map.containsKey(cur) ?
                    map.get(cur) > count ? map.get(cur) : count :
                    count);
        }

        ArrayList<String> list = new ArrayList<>();

        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            list.add(entry.getKey() + "-" + entry.getValue());
        }

        list.sort(new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o2.split("-")[1].compareTo(o1.split("-")[1]);
            }
        });

        if (k > list.size()) {
            System.out.println(-1);
        } else {
            System.out.println(list.get(k - 1).split("-")[1]);
        }

        in.close();

    }
}
```
### javascript
```javascript
// 只包含大写字母 求长度第k长的子串 连续出现次数第k多的字母的次数
// 输入：
// 第一行大写字幕
// 第二行 k
// 输出: 连续出现次数第k多的字母的次数
// 输入：
// AABAAA
// 2
// 输出：
// 1
// 输入：
// AAAAHHHBBCDHHHH
// 3

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let str = '';
let k = 0;

rl.on('line', line => {
    if (sign == -1) {
        str = line;

        sign = 1;
    } else if(sign == 1) {
        k = parseInt(line);

        solution(str, k);
        sign = -1;
        str = '';
        k = 0;
    }
});

function solution(str, k) {
    let igore = str.replace(/[A-Z]/g, '').length;
    if (!str || igore > 0) {
        console.log(-1);
        return;
    }

    let chars = str.split('');
    let map = new Map();
    let curr = chars[0];
    let count = 1;

    for (let i = 1; i < chars.length; i++) {
        let c = chars[i];
        if (c == curr) {
            count++;
        } else {
            curr = c;
            count = 1;
        }
        let v = count;
        if (map.has(curr)) {
            v = map.get(curr) > count ? map.get(curr) : count;
        }
        map.set(curr, v);
    }

    let arr = [];
    for (let [key, val] of map.entries()) {
        arr.push(key + '-' + val);
    }
    arr.sort((a, b) => {
        return parseInt(b.split('-')[1]) - parseInt(a.split('-')[1]);
    });
    if (k > arr.length) {
        console.log(-1);
    } else {
        let target = arr[k - 1];
        console.log(target.split('-')[1]);
    }
}
```

## 题目73 按照字母出现次数从大到小的顺序输出各个字母和字母次数
```java
package com.amoscloud.newcoder.easy;

import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/9/7
 * Time: 15:30
 * Description: 85%
 */
public class Main82 {
    /*

    给出一个只包含字母的字符串,
    不包含空格,统计字符串中各个子字母(区分大小写)出现的次数,
    并按照字母出现次数从大到小的顺序输出各个字母及其出现次数
    如果次数相同,按照自然顺序排序,且小写字母在大写字母之前

    输入描述:
      输入一行仅包含字母的字符串

    输出描述:
      按照字母出现次数从大到小的顺序输出各个字母和字母次数,
      用英文分号分割,
      注意末尾的分号
      字母和次数中间用英文冒号分隔

    示例:
        输入: xyxyXX
        输出:x:2;y:2;X:2;
    说明:每个字符出现的次数为2 故x排在y之前
    而小写字母x在大写X之前

    示例2:
        输入:
         abababb
        输出:
            b:4;a:3
        说明:b的出现个数比a多 故排在a前
     */
    public static void main(String[] args) {

        Scanner in = new Scanner(System.in);
        String str = in.nextLine();
        in.close();

        HashMap<Character, Integer> map = new HashMap<>();
        for (char c : str.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }

        print(map.entrySet().stream().filter(e -> e.getKey() >= 'a'));
        print(map.entrySet().stream().filter(e -> e.getKey() <= 'Z'));

    }

    private static void print(Stream<Map.Entry<Character, Integer>> stream) {
        List<Map.Entry<Character, Integer>> list = stream
                .sorted((o1, o2) -> {
                    int v1 = o1.getValue();
                    char k1 = o1.getKey();
                    int v2 = o2.getValue();
                    char k2 = o2.getKey();
                    if (v1 != v2) {
                        return v2 - v1;
                    } else {
                        return k1 - k2;
                    }
                }).collect(Collectors.toList());

        StringBuilder builder = new StringBuilder();
        for (Map.Entry<Character, Integer> entry : list) {
            builder.append(entry.getKey()).append(":")
                    .append(entry.getValue()).append(";");
        }

        System.out.print(builder);
    }

}
```
### javascript
```javascript
// 统计字符串中各个子字母(区分大小写)出现的次数
// 按照字母出现次数从大到小的顺序输出各个字母和字母次数
// 输入: xyxyXX
// 输出:x:2;y:2;X:2;
// 输入:
// abababb
// 输出:
// b:4;a:3

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    solution(line);
});

function solution(str) {
    let map = new Map();
    let chars = str.split('');
    chars.forEach(char => {
        let count = (map.get(char) || 0) + 1;
        map.set(char, count);
    });

    let res = [];
    let minChars = Array.from(map).filter(i => i[0] >= 'a');
    let maxChars = Array.from(map).filter(i => i[0] <= 'Z');
    log(minChars, res);
    log(maxChars, res);
    console.log(res.join(''));
}

function log(arr, res) {
    arr.sort((a, b) => {
        let key1 = a[0];
        let val1 = a[1];
        let key2 = b[0];
        let val2 = b[1];
        if (val1 != val2) {
            return val2 - val1;
        } else {
            return key1 - key2;
        }
    }).forEach(i => {
        res.push(i[0] + ':' + i[1] + ';');
    });
}
```

## 题目19 删除字符串中出现次数最少的字符
```java
import java.util.*;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/12/14
 * Time: 17:01
 * Description:
 */
public class Demo19 {
    public static void main(String[] args) {

    /*
    删除字符串中出现次数最少的字符
    如果多个字符出现次数一样则都删除

    例子：
    输入
      abcdd
      字符串中只
     输出
      dd

    输入
      aabbccdd

    输出
      empty

      如果都被删除  则换为empty

     */

        Scanner in = new Scanner(System.in);

        String line = in.nextLine();
        in.close();
        HashMap<Character, Long> map = new HashMap<>();
        for (char c : line.toCharArray()) {
            map.put(c, map.containsKey(c) ? map.get(c) + 1 : 1L);
        }

        Long[] counts = new Long[map.values().size()];
        Long[] longs = map.values().toArray(counts);
        Arrays.sort(longs);
        Long min = longs[0];
        for (Map.Entry<Character, Long> entry : map.entrySet()) {
            if (entry.getValue().equals(min)) {
                line = line.replaceAll(entry.getKey() + "", "");
            }
        }

        System.out.println(line.length() == 0 ? "empty" : line);
    }
}

```
### javascript
```javascript
// 解法一：
const readline = require('readline');
const rl =readline.createInterface({
    input:  process.stdin,
    output: process.stdout
});

rl.on('line', function (line) {
    let map = {} // 每个字符出现的数量统计
    line.split('').forEach(i => map[i] = (map[i] || 0) + 1);
    
    // 字符出现次数排序
    const keySort = Object.keys(map).sort((a, b) => map[a] - map[b])
    // 统计出现次数最少的字符
    const minStrArr = keySort.reduce((pre, cur) => {
        if (map[cur] === map[keySort[0]]) pre.push(cur)
        return pre
    }, [])
    const reg = new RegExp(`[${minStrArr.join('|')}]`, 'g')
   	let str = line.replace(reg, '');
    console.log(str.length == 0 ? 'empty' : str);
})

// 解法二：
// 去重
function delRepeat(str) {
    return str.replace(/(\w)\1+/g, item => {return item[0]});
}
// 字符串出现次数
function strLength(str1, str2) {
    return str1.match(new RegExp(str2, 'ig')).length;
}

function solution(line) {
    const strN = delRepeat(line);
    const arr = strN.split('').map(v => [strLength(line,v), v])
              .sort((a,b) => a[0]-b[0]);
    const replaceStr = arr.filter((v) => arr[0][0] === v[0])
                                .map(v => v[1]).join('');
    return line.replace(new RegExp(`[${replaceStr}]`, 'g'), '') ;
}
```

## 题目69 消消乐 字符串中的俩个字母如果相邻且相同，就可以消除

```java
package com.amoscloud.newcoder.easy;

import java.util.LinkedList;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/8/17
 * Time: 18:42
 * Description:100%
 */
public class Main77 {
    public static void main(String[] args) {
        /*
        游戏规则：输入一个只包含英文字母的字符串，
        字符串中的俩个字母如果相邻且相同，就可以消除。
        在字符串上反复执行消除的动作，
        直到无法继续消除为止，
        此时游戏结束。
        输出最终得到的字符串长度。

        输出：原始字符串str只能包含大小写英文字母，字母的大小写敏感，长度不超过100，
        输出游戏结束后字符串的长度

        备注：输入中包含非大小写英文字母是均为异常输入，直接返回0。
	    事例：mMbccbc输出为3
        输入
             gg
        输出
             0

        输入：
        mMbccbc
        0123456
        输出
        3
         */

        Scanner in = new Scanner(System.in);
        String str = in.nextLine();
        in.close();

        int len = str.replaceAll("[A-Z]", "")
                .replaceAll("[a-z]", "")
                .length();
        if (len != 0) {
            System.out.println(0);
            return;
        }

        LinkedList<Character> characters = new LinkedList<>();
        for (char c : str.toCharArray()) {
            characters.add(c);
        }

        int count = 0;
        while (characters.size() != count) {
            count = characters.size();
            for (int i = 0; i < characters.size() - 1; i++) {
                if (characters.get(i) == characters.get(i + 1)) {
                    characters.remove(i);
                    characters.remove(i);
                    i--;
                }
            }
        }

        System.out.println(characters.size());

    }
}
```
### javascript
```javascript
// 字符串消除 消消乐 英文字母的字符串 俩个字母如果相邻且相同，就可以消除
// 输入中包含非大小写英文字母是均为异常输入，直接返回0
// 字母的大小写敏感
// 输出游戏结束后字符串的长度
// 输入：mMbccbc
// 输出: 3

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let str = line;
    // let len = str.replace(/[A-Z]/g, '').replace(/[a-z]/g, '').length;
    let len = str.replace(/[a-zA-Z]/g, '').length;
    if (len) {
        console.log(0);
        return;
    }

    solution2(line);
});

function solution(str) {
    let chars = str.split('');
    let count = 0;
    while (chars.length != count) {
        count = chars.length;

        for (let i = 0; i < chars.length - 1; i++) {
            if (chars[i] == chars[i + 1]) {
                chars.splice(i, 1);
                chars.splice(i, 1);
                i--;
            }
        }
    }
    console.log(chars.length);
}

function solution2(str) {
    let chars = str.split('');
    for (let i = 0; i < chars.length - 1;) {
        if (chars[i] == chars[i + 1]) {
            chars.splice(i, 1);
            chars.splice(i, 1);
            i--;
            i = Math.max(i, 0);
        } else {
            i++;
        }
    }
    console.log(chars.length);
}
```

## 题目84 卡片 计算组成的最大数字
```java
package com.amoscloud.newcoder.easy;

import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/12/22
 * Time: 9:07
 * Description:  95%
 */
public class Main103 {
  /*
  小组中每位都有一张卡片
  卡片是6位以内的正整数
  将卡片连起来可以组成多种数字
  计算组成的最大数字

  输入描述：
    ","分割的多个正整数字符串
    不需要考虑非数字异常情况
    小组种最多25个人

   输出描述：
     最大数字字符串

   示例一
     输入
      22,221
     输出
      22221

    示例二
      输入
        4589,101,41425,9999
      输出
        9999458941425101
   */
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    String nums = in.nextLine();
    in.close();

    StringBuilder builder = new StringBuilder();

    Arrays.stream(nums.split(","))
        .sorted((s1, s2) -> {
          char[] v1 = s1.toCharArray();
          char[] v2 = s2.toCharArray();
          int len1 = v1.length;
          int len2 = v2.length;

          if (len1 == len2) {
            return s2.compareTo(s1);
          }

          int min = Math.min(len1, len2);
          for (int i = 0; i < min; i++) {
            char c1 = v1[i];
            char c2 = v2[i];
            if (c1 != c2) {
              return c2 - c1;
            }
          }

          if (len1 > len2) {
            return v1[0] - v1[min];
          } else {
            return v2[min] - v2[0];
          }
        })
        .forEach(builder::append);

    System.out.println(builder);

  }
}
```
### Javascript 版本
```javascript
// 卡片 最大数字字符串 
// 输入 22,221
// 输出 22221
// 输入 4589,101,41425,9999
// 输出 9999458941425101

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let arr = line.split(',');
    solution(arr);
});

function solution(arr) {
    arr.sort((a, b) => {
        let char1 = a.split('');
        let char2 = b.split('');
        let len1 = char1.length;
        let len2 = char2.length;
        if (len1 == len2) {
            return b - a;
        }
        let min = Math.min(len1, len2);
        for (let i = 0; i < min; i++) {
            let c1 = char1[i];
            let c2 = char2[i];
            if (c1 != c2) {
                return c2 - c1;
            }
        }
        if (len1 > len2) {
            return char1[0] - char1[min];
        } else {
            return char2[min] - char2[0];
        }
    })
    console.log(arr.join(''));
}
```
## 题目82 包含数字的字符串 所有整数的最小和
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/12/1
 * Time: 18:36
 * Description: 100%
 */
public class Main96 {
  /*
  1.输入字符串s输出s中包含所有整数的最小和，
  说明：1字符串s只包含a~z,A~Z,+,-，
  2.合法的整数包括正整数，一个或者多个0-9组成，如：0,2,3,002,102
  3.负整数，负号开头，数字部分由一个或者多个0-9组成，如-2,-012,-23,-00023
  输入描述：包含数字的字符串
  输出描述：所有整数的最小和
  示例：
    输入：
      bb1234aa
  　输出
      10
  　输入：
      bb12-34aa
  　输出：
      -31
  说明：1+2-(34)=-31
   */
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    String line = in.nextLine();
    in.close();

    char[] chars = line.toCharArray();
    int sum = 0;

    for (int i = 0; i < chars.length; i++) {
      char c = chars[i];
      if (c == '-') {
        i++;
        int start = i;
        while (i < chars.length && Character.isDigit(chars[i])) {
          i++;
        }
        String substring = line.substring(start, i);
        if (substring.length() > 0) {
          sum -= Integer.parseInt(substring);
        }
        i--;
        continue;
      }

      if (Character.isDigit(c)) {
        sum += Character.digit(c, 10);
      }
    }

    System.out.println(sum);

  }
}
```
### javascript 版本

```javascript 
// 包含数字字符串中 所有整数的最小和
// 输入： bb1234aa
// 输出： 10
// 输入： bb12-34aa
// 输出： -31   说明：1+2-(34)=-31

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    solution(line);
});

function solution(lineStr) {
    let arr = lineStr.split('');
    let sum = 0;
    for (let i = 0; i < arr.length; i++) {
        let char = arr[i];
        if (char == '-') {
            i++;
            let start = i;
            // !isNaN(str) 判读是否数字型字符串
            while (i < arr.length && !isNaN(arr[i])) {
                i++;
            }
            let sub = lineStr.substring(start, i);
            if (sub.length > 0) {
                sum = sum - parseInt(sub);
            }
            i--;
            continue;
        }
        if (!isNaN(char)) {
            sum += parseInt(char);
        }
    }
    console.log(sum);
}
```

## 题目68 两个数和两数之和绝对值 最小值
```java
package com.amoscloud.newcoder.easy;

import java.util.*;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/8/4
 * Time: 15:17
 * Description:100%
 */

public class Main76 {
/*
给定一个随机的整数数组(可能存在正整数和负整数)nums,
请你在该数组中找出两个数，其和的绝对值(|nums[x]+nums[y]|)为最小值
并返回这两个数(按从小到大返回)以及绝对值。
每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

输入描述：
 一个通过空格空格分割的有序整数序列字符串，最多1000个整数，
 且整数数值范围是[-65535,65535]

输出描述：
  两个数和两数之和绝对值

 示例一：
  输入
  -1 -3 7 5 11 15
  输出
  -3 5 2

说明：
因为|nums[0]+nums[2]|=|-3+5|=2最小，
所以返回-3 5 2

 */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String[] nums = in.nextLine().split(" ");

        in.close();

        ArrayList<Integer> list = Arrays.stream(nums)
                .map(Integer::parseInt)
                .distinct()
                .collect(Collectors.toCollection(ArrayList::new));

        int min = Integer.MAX_VALUE;

        TreeSet<Integer> resSet = new TreeSet<>();

        for (int i = 0; i < list.size() - 1; i++) {
            for (int j = i; j < list.size(); j++) {
                Integer a = list.get(i);
                Integer b = list.get(j);
                int sum = Math.abs(a + b);
                if (sum < min && a != b) {
                    min = sum;
                    resSet.clear();
                    resSet.add(a);
                    resSet.add(b);
                }
            }
        }

        if (resSet.size() != 0) {
            for (Integer integer : resSet) {
                System.out.print(integer + " ");
            }
            System.out.println(min);
        }

//-1 -3 7 5 11 15

    }
}
```
### javascript
```javascript
// 数组中找出两个数 和的绝对值最小，输出两个数和绝对值 |nums[x]+nums[y]|)为最小值
// 输入
// -1 -3 7 5 11 15
// 输出
// -3 5 2

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let arr = line.split(' ').map(i => parseInt(i));
    let set = new Set(arr);

    solution(Array.from(set));
});

function solution(arr) {
    let min = Infinity;
    let set = new Set();
    for (let i = 0; i < arr.length - 1; i++) {
        for (let j = i; j < arr.length; j++) {
            let a = arr[i];
            let b = arr[j];
            let sum = Math.abs(a + b);
            if (sum < min && a != b) {
                min = sum;
                set.clear();
                set.add(a);
                set.add(b);
            }
        }
    }

    if (set.size != 0) {
        console.log(Array.from(set).join(" "));
        console.log(min);
    }
}
```
## 题目28 最大N个数和最小N个数的和
```java
import java.util.ArrayList;
import java.util.Scanner;
import java.util.TreeSet;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/8
 * Time: 16:47
 * Description:
 */
public class Main5 {
    public static void main(String[] args) {
        /*
            给定一个数组
            编写一个函数
            来计算他的最大N个数和最小N个数的和
            需要对数组进行去重

            说明
            第一行输入M
            M表示数组大小
            第二行输入M个数
            表示数组内容
            第三行输入N表示需要计算的最大最小N的个数

            输出描述
            输出最大N个数和最小N个数的和

            例一：
                输入
                5
                95 88 83 64 100
                2

                输出
                342

                说明
                最大2个数[100 95] 最小2个数[83 64]
                输出342

             例二
                输入
                5
                3 2 3 4 2
                2

                输出
                 -1
                 说明
                 最大两个数是[4 3]最小2个数是[3 2]
                 有重叠输出为-1

         */

        Scanner in = new Scanner(System.in);
        int m = Integer.parseInt(in.nextLine());
        String[] numsStr = in.nextLine().split(" ");
        int n = Integer.parseInt(in.nextLine());
        in.close();

        TreeSet<Integer> ints = new TreeSet<>();
        for (String s : numsStr) {
            ints.add(Integer.parseInt(s));
        }

        int res = -1;

        if (ints.size() >= 2 * n) {
            res = 0;
            ArrayList<Integer> list = new ArrayList<>(ints);
            for (int i = 0; i < list.size(); i++) {
                if (i < n || i > list.size()-1 - n) {
                    res += list.get(i);
                }
            }
        }
        System.out.println(res);
    }
}
```
### javascript
```javascript
// 给定一个数组
// 计算他的最大N个数和最小N个数的和
// 需要对数组进行去重
// 第一行输入M
// M表示数组大小
// 第二行输入M个数
// 表示数组内容
// 第三行输入N表示需要计算的最大最小N的个数

// 输出描述
// 输出最大N个数和最小N个数的和
// 输入
// 5
// 95 88 83 64 100
// 2

// 输出
// 342

// 说明
// 最大2个数[100 95] 最小2个数[83 64]
// 输出342
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let m = 0; // 数组大小
let data = [];
let n = 0;


rl.on('line', line => {
    if (sign == -1) {
        m = parseInt(line);

        sign = 1;
    } else if(sign == 1) {
        let input = line.split(' ').map(i => parseInt(i));
        input.sort((a, b) => a - b); //!!注意这里有坑Java TreeSet默认会升序排序
        data = new Set(input);

        sign = 2;
    } else {
        n = parseInt(line);

        solution(data, n);
        sign = -1;
        // rl.close();
    }
});

function solution(data, n) {
    let res = -1;
    if (data.size >= 2 * n) {
        res = 0;
        let arr = Array.from(data);
        for (let i = 0; i < arr.length; i++) {
            if (i < n || i > arr.length - 1 - n) {
                res += arr[i];
            }
        }
    }
    console.log(res);
}
```

## 题目67 按照要求变换得到最小字符串
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/7/8
 * Time: 13:44
 * Description: 90
 */
public class Main74 {
    public static void main(String[] args) {
        /*
        给定一个字符串S

        变化规则:
         交换字符串中任意两个不同位置的字符

        输入描述：
         一串小写字母组成的字符串
        输出描述：
         按照要求变换得到最小字符串

        实例1：
         输入：、
         abcdef
        输出
         abcdef

        实例2：
         输入
         bcdefa
         输出
         acdefb

        s都是小写字符组成
        1<=s.length<=1000
         */

        Scanner in = new Scanner(System.in);
        String str = in.nextLine();
        in.close();

        char[] chars = str.toCharArray();
        char tmp = chars[0];
        int pos = 0;
        for (int i = 1; i < chars.length; i++) {
            char cur = chars[i];
            if (cur <= tmp) {
                tmp = cur;
                pos = i;
            }
        }

        if (pos != 0) {
            chars[pos] = chars[0];
            chars[0] = tmp;
        }

        System.out.println(new String(chars));

    }
}
```
### javascript
```javascript
// 按照要求变换位置得到最小字符串
// 输入：
// abcdef
// 输出
// abcdef
// 输入
// bcdefa
// 输出
// acdefb

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let chars = line.split('');    

    solution(chars);
});

function solution(chars) {
    let tmp = chars[0];
    let pos = 0;
    for (let i = 1; i < chars.length; i++) {
        let curr = chars[i];
        if (curr <= tmp) {
            tmp = curr;
            pos = i;
        }
    }

    if (pos != 0) {
        chars[pos] = chars[0];
        chars[0] = tmp;
    }

    console.log(chars.join(''));
}
```

## 题目66 最长的元音字符子串 返回长度
```java
package com.amoscloud.newcoder.easy;

import java.util.*;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/7/6
 * Time: 5:20
 * Description:
 */
public class Main73 {
    /*
    定义当一个字符串只有元音字母(a,e,i,o,u,A,E,I,O,U)组成,
    称为元音字符串，现给定一个字符串，请找出其中最长的元音字符串，
    并返回其长度，如果找不到请返回0，
    字符串中任意一个连续字符组成的子序列称为该字符串的子串

    输入描述：
      一个字符串其长度 0<length ,字符串仅由字符a-z或A-Z组成
    输出描述：
      一个整数，表示最长的元音字符子串的长度

    示例1：
      输入
        asdbuiodevauufgh
      输出
        3
      说明：
        最长的元音字符子串为uio和auu长度都为3，因此输出3
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String str = in.nextLine().toLowerCase();
        in.close();

        List<Character> vowel = Arrays.asList('a', 'e', 'i', 'o', 'u');

        int maxLen = 0, tmpLen = 0;
        for (char c : str.toCharArray()) {
            if (vowel.contains(c)) {
                tmpLen++;
            } else {
                maxLen = Math.max(maxLen, tmpLen);
                tmpLen = 0;
            }
        }
        maxLen = Math.max(maxLen, tmpLen);

        System.out.println(maxLen);
    }
}
```
### javascript
```javascript
// 最长元音字符子串的长度
// 输入：
// asdbuiodevauufgh
// 输出
// 3

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let chars = line.split('');    

    solution(chars);
});

function solution(chars) {
    let dict = ['a', 'e', 'i', 'o', 'u'];
    let maxLen = 0;
    let sum = 0;

    for (let char of chars) {
        if (dict.includes(char)) {
            sum++;
        } else {
            maxLen = Math.max(maxLen, sum);
            sum = 0;
        }
    }
    maxLen = Math.max(maxLen, sum);
    console.log(maxLen);
}
```

## 题目65 字符串中第K个最小ASCII码值的字母 
```java
package com.amoscloud.newcoder.easy;

import java.util.ArrayList;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/6/2
 * Time: 13:29
 * Description:
 */
public class Main67 {
    /*
    输入一个由N个大小写字母组成的字符串
    按照ASCII码值从小到大进行排序
    查找字符串中第K个最小ASCII码值的字母(k>=1)
    输出该字母所在字符串中的位置索引(字符串的第一个位置索引为0)
    k如果大于字符串长度则输出最大ASCII码值的字母所在字符串的位置索引
    如果有重复字母则输出字母的最小位置索引

    输入描述
      第一行输入一个由大小写字母组成的字符串
      第二行输入k k必须大于0 k可以大于输入字符串的长度

    输出描述
      输出字符串中第k个最小ASCII码值的字母所在字符串的位置索引
      k如果大于字符串长度则输出最大ASCII码值的字母所在字符串的位置索引
      如果第k个最小ASCII码值的字母存在重复  则输出该字母的最小位置索引

    示例一
     输入
        AbCdeFG
        3
     输出
        5
     说明
       根据ASCII码值排序，第三个ASCII码值的字母为F
       F在字符串中位置索引为5(0为字符串的第一个字母位置索引)

     示例二
       输入
        fAdDAkBbBq
        4
       输出
        6
       说明
        根据ASCII码值排序前4个字母为AABB由于B重复则只取B的第一个最小位置索引6
        而不是第二个B的位置索引8
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String line = in.nextLine();
        int k = in.nextInt();
        in.close();

        char[] chars = line.toCharArray();
        ArrayList<Character> list = new ArrayList<>();
        for (char aChar : chars) {
            list.add(aChar);
        }

        list.sort(Character::compareTo);
        char c = k >= list.size() ? list.get(list.size() - 1) : list.get(k - 1);
        System.out.println(line.indexOf(c));
    }
}
```
### javascript
```javascript
// 字符串中第k个最小ASCII码值的字母所在字符串的位置索引
//  输入
// AbCdeFG
// 3
// 输出
// 5
// 输入
// fAdDAkBbBq
// 4
// 输出
// 6

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let str = '';
let k = 0;
rl.on('line', line => {
    if (sign == -1) {
        str = line;

        sign = 1;
    } else if (sign == 1) {
        k = parseInt(line);

        solution(str, k);
        sign = -1;
        str = '';
        k = 0;
    }
});

function solution(str, k) {
    let chars = str.split('');
    let len = chars.length;
    chars.sort((a, b) => a.charCodeAt() - b.charCodeAt());
    let char = k >= len ? chars[len - 1] : chars[k - 1];
    let index = str.indexOf(char);
    console.log(index);
}
```
## 题目64  绘图笔  计算绘制的直线和横坐标轴以及x=E的直线组成的图形面积
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/7/3
 * Time: 16:04
 * Description:
 */
public class Main72 {
        /*
        绘图机器的绘图笔初始位置在原点(0,0)
        机器启动后按照以下规则来进行绘制直线
        1. 尝试沿着横线坐标正向绘制直线
         直到给定的终点E
        2. 期间可以通过指令在纵坐标轴方向进行偏移
        offsetY为正数表示正向偏移,为负数表示负向偏移

        给定的横坐标终点值E 以及若干条绘制指令
        请计算绘制的直线和横坐标轴以及x=E的直线组成的图形面积
        输入描述:
        首行为两个整数N 和 E
        表示有N条指令,机器运行的横坐标终点值E
        接下来N行 每行两个整数表示一条绘制指令x offsetY
        用例保证横坐标x以递增排序的方式出现
        且不会出现相同横坐标x
        取值范围:
          0<N<=10000
          0<=x<=E<=20000
          -10000<=offsetY<=10000

        输出描述:
          一个整数表示计算得到的面积 用例保证结果范围在0到4294967295之内
        示例1:
         输入:
           4 10
           1 1
           2 1
           3 1
           4 -2
         输出:
           12

         示例2:
          输入:
           2 4
           0 1
           2 -2
          输出:
           4

         */

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split(" ");
        int N = Integer.parseInt(split[0]);
        int E = Integer.parseInt(split[1]);

        int curX = 0, curY = 0, area = 0;

        for (int i = 0; i < N; i++) {
            String[] strs = in.nextLine().split(" ");
            int x = Integer.parseInt(strs[0]);
            int y = Integer.parseInt(strs[1]);

            area += (x - curX) * Math.abs(curY);

            curX = x;
            curY += y;
        }
        if (curX < E) {
            area += (E - curX) * Math.abs(curY);
        }

        System.out.println(area);
        in.close();
    }
}
```
### javascript
```javascript
// 绘图笔  计算绘制的直线和横坐标轴以及x=E的直线组成的图形面积
// 取值范围:
//           0<N<=10000
//           0<=x<=E<=20000
//           -10000<=offsetY<=10000
// 首行两个整数N 和 E 表示有N条指令,机器运行的横坐标终点值E
// 接下来N行 每行两个整数表示一条绘制指令x offsetY 输出面积
// 输入:
// 4 10
// 1 1
// 2 1
// 3 1
// 4 -2
// 输出:
// 12

// 输入:
// 2 4
// 0 1
// 2 -2
// 输出:
// 4

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let N = 0;
let E = 0;

let data = [];

rl.on('line', line => {
    if (sign == -1) {
        let arr1 = line.split(' ').map(i => parseInt(i));
        N = arr1[0];
        E = arr1[1];

        sign = 1;
    } else {
        data.push(line);
    }

    if (data.length == N) {
        solution(N, E, data);

        sign = -1;
        N = 0;
        E = 0;
        data = [];
    }
});

function solution(N, E, data) {
    let curX = 0, curY = 0, area = 0;

    for (let i = 0; i < N; i++) {
        let arr = data[i].split(' ');
        let x = parseInt(arr[0]);
        let offsetY = parseInt(arr[1]);

        area += (x - curX) * Math.abs(curY);
        curX = x;
        curY += offsetY;
    }
    if (curX < E) {
        area += (E - curX) * Math.abs(curY);
    }
    console.log(area);
}
```

## 题目63  给定两个字符集合 全量字符集 已占用字符集 输出剩余可用字符集
```java
package com.amoscloud.newcoder.easy;

import java.util.Comparator;
import java.util.HashMap;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/26
 * Time: 19:38
 * Description:
 */
public class Main60 {
    /*
    给定两个字符集合
    一个是全量字符集
    一个是已占用字符集
    已占用字符集中的字符不能再使用
    要求输出剩余可用字符集

    输入描述
     1. 输入一个字符串 一定包含@
     @前为全量字符集  @后的为已占用字符集
     2. 已占用字符集中的字符
     一定是全量字符集中的字符
     字符集中的字符跟字符之间使用英文逗号隔开
     3. 每个字符都表示为字符+数字的形式
      用英文冒号分隔
      比如a:1标识一个a字符
     4. 字符只考虑英文字母，区分大小写
      数字只考虑正整型 不超过100
     5. 如果一个字符都没被占用 @标识仍存在
     例如 a:3,b:5,c:2@

     输出描述：
       输出可用字符集
       不同的输出字符集之间用回车换行
       注意 输出的字符顺序要跟输入的一致
       不能输出b:3,a:2,c:2
       如果某个字符已全部占用 则不需要再输出

      示例一：
       输入
       a:3,b:5,c:2@a:1,b:2
       输出
       a:2,b:3,c:2
       说明：
       全量字符集为三个a，5个b，2个c
       已占用字符集为1个a，2个b
       由于已占用字符不能再使用
       因此剩余可用字符为2个a，3个b，2个c
       因此输出a:2,b:3,c:2
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split("@");
        in.close();

        HashMap<String, Info> map = new HashMap<>();

        String[] all = split[0].split(",");
        for (int i = 0; i < all.length; i++) {
            String[] char_count = all[i].split(":");
            String c = char_count[0];
            map.put(c, new Info(c, i, Integer.parseInt(char_count[1])));
        }

        if (split.length > 1)
            for (String s : split[1].split(",")) {
                String[] char_count = s.split(":");
                String c = char_count[0];
                Info value = map.get(c);
                value.count -= Integer.parseInt(char_count[1]);
                map.put(c, value);
            }

        StringBuilder sb = new StringBuilder();

        map.values().stream().filter(x -> x.count > 0)
                .sorted(new Comparator<Info>() {
                    @Override
                    public int compare(Info o1, Info o2) {
                        return o1.no - o2.no;
                    }
                }).forEach(x ->
                sb.append(x.c)
                        .append(":")
                        .append(x.count)
                        .append(","));

        System.out.println(sb.substring(0, sb.length() - 1));
    }

    public static class Info {

        public String c;
        public int no;
        public int count;

        public Info(String c, int no, int count) {
            this.c = c;
            this.no = no;
            this.count = count;
        }

    }
}
```
### javascript
```javascript
// 给定两个字符集合 全量字符集@已占用字符集 已占用字符集中的字符不能再使用 输出剩余可用字符集
// 不同的输出字符集之间用回车换行 输出的字符顺序要跟输入的一致
// 输入： a:3,b:5,c:2@a:1,b:2
// 输出： a:2,b:3,c:2

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    

    solution(line);
    rl.close();
});

function solution(line) {
    let arr = line.trim().split('@');
    let map = new Map();
    let all = arr[0].split(',');
    for (let i = 0; i < all.length; i++) {
        let chars = all[i].split(':');
        let key = chars[0];
        let obj = {key: key, no: i, count: parseInt(chars[1])};     
        map.set(key, obj);
    }

    if (line.length > 1) {
        let used = arr[1].split(',');

        for (let s of used) {
            if (s) {
                let uChars = s.split(':');
                let key = uChars[0];
                let item = map.get(key);
                item.count -= parseInt(uChars[1]);
                map.set(key, item);
            }
        }

        let tmpArr = Array.from(map.values()).filter(i => i.count > 0);
        let res = [];
        tmpArr.sort((a, b) => {
            return a.no - b.no;
        }).forEach(i => {
            let t = i.key + ':' + i.count;
            res.push(t);
        });
        console.log(res.join(','));
    }
}
```

## 题目61 从t中找到一个和p相同的连续子串 输出 子串第一个字符的下标
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/24
 * Time: 16:40
 * Description:
 */
public class Main57 {
    /*
    给你两个字符串t和p
    要求从t中找到一个和p相同的连续子串
    并输出该子串第一个字符的下标
    输入描述
        输入文件包括两行 分别表示字符串t和p
        保证t的长度不小于p
        且t的长度不超过1000000
        p的长度不超过10000
    输出描述
        如果能从t中找到一个和p相等的连续子串,
        则输出该子串第一个字符在t中的下标
        下标从左到右依次为1,2,3,...；
        如果不能则输出 "No"
        如果含有多个这样的子串
        则输出第一个字符下标最小的

     示例一：
        输入：
         AVERDXIVYERDIAN
         RDXI
        输出
         4
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String t = in.nextLine();
        String p = in.nextLine();
        in.close();

        int len = p.length();

        for (int i = 0; i <= t.length() - len; i++) {
            String substring = t.substring(i, i + len);
            if (substring.equals(p)) {
                System.out.println(i + 1);
                return;
            }
        }
        System.out.println("No");

    }
}
```
### javascript
```javascript
// 给你两个字符串t和p
// 从t中找到一个和p相同的连续子串
// 输出该子串第一个字符的下标, 不能找到 输出 No
// 输入两行 字符串 t 和 p
// 输入：
// AVERDXIVYERDIAN
// RDXI
// 输出
// 4

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let t = '';
let p = '';

rl.on('line', line => {
    if (sign == -1) {
        t = line.trim();

        sign = 1;
    } else if (sign == 1) {
        p = line.trim();

        solution(t, p);
        // rl.close();
        sign = -1;
        t = '';
        p = '';
    }
});

function solution(t, p) {
    let len = p.length;
    for (let i = 0; i <= t.length - len; i++) {
        let sub = t.substring(i, i + len);
        if (sub == p) {
            console.log(i + 1);
            return;
        }
    }
    console.log('No');
}
```

## 题目59 员工出勤信息 是否能获得出勤奖
```java
package com.amoscloud.newcoder.easy;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/24
 * Time: 13:50
 * Description:
 */
public class Main54 {
  /*
  公司用一个字符串来标识员工的出勤信息

  absent:    缺勤
  late:      迟到
  leaveearly:早退
  present:   正常上班

  现需根据员工出勤信息,判断本次是否能获得出勤奖,
  能获得出勤奖的条件如下：
      1.缺勤不超过1次
      2.没有连续的迟到/早退
      3.任意连续7次考勤 缺勤/迟到/早退 不超过3次

   输入描述：
    用户的考勤数据字符串记录条数  >=1
    输入字符串长度 <10000 ;
    不存在非法输入
    如：
     2
     present
     present absent present present leaveearly present absent

    输出描述：
    根据考勤数据字符串
    如果能得到考勤奖输出true否则输出false
    对于输出示例的结果应为
     true false

    示例一：
     输入：
      2
      present
      present present

     输出：
      true true

    示例二
     输入：
      2
      present
      present absent present present leaveearly present absent
     输出：
      true false

   */
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int n = Integer.parseInt(in.nextLine());
    ArrayList<List<String>> days = new ArrayList<>();
    for (int i = 0; i < n; i++) {
      String[] split = in.nextLine().split(" ");
      List<String> list = Arrays.stream(split)
          .collect(Collectors.toList());
      days.add(list);
    }
    in.close();

    StringBuilder sb = new StringBuilder();

    for (List<String> day : days) {

      //1.缺勤超过1次
      long absent = day.stream()
          .filter(x -> x.equals("absent"))
          .count();
      if (absent > 1) {
        sb.append("false").append(" ");
        continue;
      }

      //2.没有连续的迟到/早退
      boolean flag = true;
      for (int i = 0; i < day.size() - 1; i++) {
        String cur = day.get(i);
        String next = day.get(i + 1);
        if (("late".equals(cur) ||
            "leaveearly".equals(cur)) &&
            ("late".equals(next) ||
                "leaveearly".equals(next))) {
          flag = false;
          break;
        }
      }
      if (!flag) {
        sb.append(flag).append(" ");
        continue;
      }

      //3.任意连续7次考勤 缺勤/迟到/早退 不超过3次
      int[] ints = new int[day.size()];
      for (int i = 0; i < day.size(); i++) {
        ints[i] = "present".equals(day.get(i)) ? 0 : 1;
      }
      if (ints.length <= 7 && Arrays.stream(ints).sum() >= 3) {
        sb.append("false").append(" ");
      } else {
        flag = true;
        for (int i = 0; i < ints.length - 7; i++) {
          int[] subArr = Arrays.copyOfRange(ints, i, i + 7);
          if (Arrays.stream(subArr).sum() >= 3) {
            flag = false;
            break;
          }
        }
        sb.append(flag).append(" ");
      }
    }

    System.out.println(sb.substring(0, sb.length() - 1));

  }
}
```
### javascript
```javascript
// 根据员工出勤信息,判断本次是否能获得出勤奖
// 第一行 考勤记录条数
// 剩下n行数据
// 
// 输入:
// 2
// present
// present absent present present leaveearly present absent
// 输出: 
// true false

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let n = 0;
let data = [];

rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);

        sign = 1;
    } else {
        data.push(line.split(' '));
    }

    if (data.length == n) {
        solution(data);

        sign = -1;
        n = 0;
        data = [];
        // rl.close();
    }
});

function solution(days) {
    let res = [];
    for (let arr of days) {
        // 缺勤超过1次
        let absent = arr.filter(i => i == 'absent').length;
        if (absent > 1) {
            res.push('false');
            continue;
        }

        // 没有连续迟到/早退
        let delay = isDelay(arr);
        if(!delay) {
            res.push(delay);
            continue;
        }

        // 任意两次连续7次考勤 缺勤/迟到/早退 不超过3次
        res = day7(arr, res);
    }
    console.log(res.join(' '));
}

function isDelay(arr) {
    let flag = true;
    for (let i = 0; i < arr.length - 1; i++) {
        let curr = arr[i];
        let next = arr[i + 1];
        if (('late' == curr || 'leaveearly' == curr) 
            && ('late' == next || 'leaveearly' == next)) {
                flag = false;
                break;
            }
    }
    return flag;
}

function day7(arr, res) {
    let len = arr.length;
    let tmp = new Array(len);
    for (let i = 0; i < len; i++) {
        tmp[i] = 'present' == arr[i] ? 0 : 1;
    }

    let flag = true;
    if (tmp.length <= 7 &&  tmp.reduce((a,b) => a + b) >= 3) {
        res.push('false');
    } else {
        flag = true;
        for (let i = 0; i < tmp.length - 7; i++) {
            let sub = tmp.slice(i, i + 7);
            let sum = sub.reduce((a, b) => a + b);
            if (sum >= 3) {
                flag = false;
                break;
            }
        }
        res.push(flag);
    }
    return res;
}
```
## 题目58 工厂流水线  处理完所有作业的总时长
```java
package com.amoscloud.newcoder.easy;

import java.util.*;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/13
 * Time: 19:02
 * Description:
 */
public class Main51 {
       /*
        一个工厂有m条流水线
        来并行完成n个独立的作业
        该工厂设置了一个调度系统
        在安排作业时，总是优先执行处理时间最短的作业
        现给定流水线个数m
        需要完成的作业数n
        每个作业的处理时间分别为 t1,t2...tn
        请你编程计算处理完所有作业的耗时为多少
        当n>m时 首先处理时间短的m个作业进入流水线
        其他的等待
        当某个作业完成时，
        依次从剩余作业中取处理时间最短的
        进入处理

        输入描述：
        第一行为两个整数(采取空格分隔)
        分别表示流水线个数m和作业数n
        第二行输入n个整数(采取空格分隔)
        表示每个作业的处理时长 t1,t2...tn
        0<m,n<100
        0<t1,t2...tn<100

        输出描述
        输出处理完所有作业的总时长

        案例
        输入
3 5
8 4 3 2 10
        输出
        13
        说明
        先安排时间为2,3,4的三个作业
        第一条流水线先完成作业
        调度剩余时间最短的作业8
        第二条流水线完成作业
        调度剩余时间最短的作业10
        总共耗时 就是二条流水线完成作业时间13(3+10)

        3 9
        1 1 1 2 3 4 6 7 8

         */

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String[] strs = in.nextLine().split(" ");
        int m = Integer.parseInt(strs[0]);
        int n = Integer.parseInt(strs[1]);

        String[] split = in.nextLine().split(" ");
        int[] jobs = new int[split.length];
        for (int i = 0; i < split.length; i++) {
            jobs[i] = Integer.parseInt(split[i]);
        }
        Arrays.sort(jobs);

        if (n <= m) {
            System.out.println(jobs[jobs.length - 1]);
            return;
        }

        ArrayList<Integer> res = new ArrayList<>();

        for (int i = 0; i < m; i++) {
            res.add(jobs[i]);
        }

        for (int i = m; i < jobs.length; i++) {
            Integer min = new ArrayList<>(new TreeSet<>(res)).get(0);
            int index = res.indexOf(min);
            res.set(index, res.get(index) + jobs[i]);
        }

        ArrayList<Integer> r = new ArrayList<>(new TreeSet<>(res));

        System.out.println(r.get(r.size() - 1));

    }
}
```
### javascript
```javascript
// 工厂流水线 处理完所有作业的总时长
// 输入：流水线个数m 需完成的作业数n
// 3 5
// 8 4 3 2 10
// 输出:
// 13

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let m = 0;
let n = 0;
let times = [];

rl.on('line', line => {
    if (sign == -1) {
        let input = line.split(' ');
        m = parseInt(input[0]);
        n = parseInt(input[1]);

        sign = 1;
    } else {
        times = line.split(' ').map(i => parseInt(i));

        solution(m, n, times);
        sign = -1;
        m = 0;
        n = 0;
        times = [];
    }
});

function solution(m, n, times) {
    times.sort((a, b) => a - b);
    if (n <= m) {
        console.log(times[times.length - 1]);
        return;
    }

    let task = new Array(m);
    let taskCount = new Array(m);

    for (let i = 0; i < m; i++) {
        task[i] = times[i];
        taskCount[i] = times[i];
    }

    let pos = m;
    let flag = -1;
    while(flag == -1) {
        for (i = 0; i < m; i++) {
            task[i]--;
            if (task[i] == 0) {
                task[i] = times[pos];
                taskCount[i] += times[pos];
                pos++;
            }
            if (pos == n) {
                flag = 1;
                break;
            }
        }
    }

    taskCount.sort((a, b) => b - a);
    console.log(taskCount[0]);
}
```
## 题目62 简易压缩算法 判断 是否为合法压缩过的字符串
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/24
 * Time: 17:01
 * Description:   85
 */
public class Main58 {
    /*
    有一种简易压缩算法：针对全部为小写英文字母组成的字符串，
    将其中连续超过两个相同字母的部分压缩为连续个数加该字母
    其他部分保持原样不变.
    例如字符串aaabbccccd  经过压缩变成字符串 3abb4cd
    请您编写解压函数,根据输入的字符串,
    判断其是否为合法压缩过的字符串
    若输入合法则输出解压缩后的字符串
    否则输出字符串"!error"来报告错误

    输入描述
      输入一行，为一个ASCII字符串
      长度不超过100字符
      用例保证输出的字符串长度也不会超过100字符串

    输出描述
      若判断输入为合法的经过压缩后的字符串
      则输出压缩前的字符串
      若输入不合法 则输出字符串"!error"

     示例一：
      输入
       4dff
      输出
       ddddff
      说明
        4d扩展为4个d ，故解压后的字符串为ddddff

     示例二
       输入
         2dff
       输出
         !error
        说明
         2个d不需要压缩 故输入不合法

      示例三
       输入
        4d@A
       输出
        !error
        说明
         全部由小写英文字母做成的字符串，压缩后不会出现特殊字符@和大写字母A
         故输入不合法

     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String line = in.nextLine();
        in.close();

        if (line.replaceAll("[a-z]|[0-9]", "").length() > 0) {
            System.out.println("!error");
            return;
        }

        char[] chars = line.toCharArray();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < chars.length; i++) {
            char cur = chars[i];
            if (Character.isDigit(cur)) {
                char next = chars[i + 1];
                if (Character.isDigit(next)) {
                    int count = Integer.parseInt(line.substring(i, i + 2));
                    for (int j = 0; j < count; j++) {
                        sb.append(chars[i + 2]);
                    }
                    i += 2;
                } else {
                    int count = Character.digit(cur, 10);
                    if (count <= 2) {
                        System.out.println("!error");
                        return;
                    }
                    for (int j = 0; j < count; j++) {
                        sb.append(chars[i + 1]);
                    }
                    i++;
                }
            } else {
                sb.append(cur);
            }
        }

        System.out.println(sb.toString());
    }
}
```
### javascript
```javascript
// 简易压缩算法 针对全部为小写英文字母组成的字符串
// 将其中连续超过两个相同字母的部分压缩为连续个数加该字母 例如字符串aaabbccccd  经过压缩变成字符串 3abb4cd
// 判断其是否为合法压缩过的字符串
// 输出压缩前的字符串 或 !error
// 输入
// 4dff
// 输出
// ddddff
// 输入
//    2dff
// 输出
//    !error
// 说明
//    2个d不需要压缩 故输入不合法
// 输入
// 4d@A
// 输出
// !error

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});


rl.on('line', line => {
    let len =  line.replace(/[a-z]|[0-9]/g, '').length;
    if ( len > 0) {
        console.log('!error');
        return;
    }
    
    solution(line);
    // rl.close();
});

function solution(line) {
    let chars = line.split('');

    let res = [];
    for (let i = 0; i < chars.length; i++) {
        let curr = chars[i];
        // 是数字
        if (!isNaN(curr)) {
            let next = chars[i + 1];
            if (!isNaN(next)) {
                let count = parseInt(line.substring(i, i + 2));
                for (let j = 0; j < count; j++) {
                    res.push(chars[i + 2]);
                }
                i += 2;
            } else {
                let count = parseInt(curr);
                if (count <= 2) {
                    console.log('!error');
                    return;
                }
                for (let j = 0; j < count; j++) {
                    res.push(chars[i + 1]);
                }
                i++;
            }
        } else {
            res.push(curr);
        }
    }
    console.log(res.join(''));
}
```

## 题目56 GPU 执行完所有任务需要多少秒
```java
package com.amoscloud.newcoder.easy;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/8
 * Time: 19:04
 * Description:
 */
public class Main48 {
    public static void main(String[] args) {
        /*
         为了充分发挥Gpu算力，
         需要尽可能多的将任务交给GPU执行，
         现在有一个任务数组，
         数组元素表示在这1s内新增的任务个数，
         且每秒都有新增任务，
         假设GPU最多一次执行n个任务，
         一次执行耗时1s，
         在保证Gpu不空闲的情况下，最少需要多长时间执行完成。

         输入描述
           第一个参数为gpu最多执行的任务个数
           取值范围1~10000
           第二个参数为任务数组的长度
           取值范围1~10000
           第三个参数为任务数组
           数字范围1~10000

         输出描述
           执行完所有任务需要多少秒

         例子
           输入
            3
            5
            1 2 3 4 5
           输出
            6

            说明，一次最多执行3个任务  最少耗时6s

          例子2
            输入
             4
             5
             5 4 1 1 1
            输出
             5

           说明，一次最多执行4个任务  最少耗时5s
         */

        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine().trim());
        int len = Integer.parseInt(in.nextLine().trim());
        String[] split = in.nextLine().split(" ");
        int[] ints = new int[len];
        for (int i = 0; i < len; i++) {
            ints[i] = Integer.parseInt(split[i]);
        }

        int time = 0;
        int more = 0;
        for (int i : ints) {
            if (i + more > n) more = i + more - n;
            else more = 0;
            time++;
        }
        while (more > 0) {
            more -= n;
            time++;
        }

        System.out.println(time);
        in.close();
    }
}
```
### javascript
```javascript
// GPU 执行完所有任务需要多少秒
// 输入
// 3 最多执行任务数
// 5 任务数组长度
// 1 2 3 4 5  任务数组
// 输出
// 6
// 输入
// 4
// 5
// 5 4 1 1 1
// 输出
// 5

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let max = 0;
let n = 0;
let tasks = [];

rl.on('line', line => {
    if (sign == -1) {
        max = parseInt(line.trim());

        sign = 1;
    } else if (sign == 1) {
        n = parseInt(line.trim());

        sign = 2;
    } else {
        tasks = line.trim().split(' ').map(i => parseInt(i));

        solution(tasks, max);
        sign = -1;
        max = 0;
        n = 0;
        tasks = [];
        // rl.close();
    }
});

function solution(tasks, max) {
    let time = 0;
    let more = 0;
    for (let i of tasks) {
        if (i + more > max) {
            more = i + more - max;
        } else {
            more = 0;
        }
        time++;
    }
    while(more > 0) {
        more -= max;
        time++;
    }
    console.log(time);
}
```

## 题目77  双11  尽可能花费的最大资金数
```java
package com.amoscloud.newcoder.easy;

import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/11/2
 * Time: 16:48
 * Description:
 */
public class Main89 {
  /*
  双十一众多商品进行打折销售
  小明想购买自己心仪的一些物品
  但由于购买资金限制
  所以他决定从众多心仪商品中购买三件
  而且想尽可能得花完资金
  现在请你设计一个程序 计算小明尽可能花费的最大资金数

  输入描述：
    输入第一行为一维整型数组m
    数组长度小于100
    数组元素记录单个商品的价格
    单个商品价格小于1000

    输入第二行为购买资金的额度r
    r<100000

  输出描述：
     输出为满足上述条件的最大花费额度

   注意：如果不存在满足上述条件的商品请返回-1

  示例：
     输入
      23,26,36,27
      78
     输出
      76
     说明：
      金额23、26、27得到76而且最接近且小于输入金额78

   示例：
       输入
       23,30,40
       26
       输出
        -1
       说明
       因为输入的商品无法满足3件之和小于26
       故返回-1

   输入格式正确无需考虑输入错误情况

   */

  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    List<Integer> m = Arrays.stream(in.nextLine().split(","))
        .map(Integer::parseInt)
        .sorted()
        .collect(Collectors.toList());
    int r = Integer.parseInt(in.nextLine());
    in.close();

    int max = -1;
    for (int i = 0; i < m.size() - 2; i++) {
      for (int j = 0; j < m.size() - 1; j++) {
        for (int k = 0; k < m.size(); k++) {
          if (i != j && j != k && i != k) {
            int sum = m.get(i) + m.get(j) + m.get(k);
            if (sum <= r && sum > max) {
              max = sum;
            }
          }
        }
      }
    }

    System.out.println(max);
  }
}
```
### javascript
```javascript
// 双11 尽可能花费的最大资金数
// 输入：
// 23,26,36,27
// 78
// 输出:
// 76
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let prices = [];
let r = 0;

rl.on('line', line => {
    if (sign == -1) {
        prices = line.split(',').map(i => parseInt(i));
        
        sign = 1;
    } else if (sign == 1) {
        r = parseInt(line);

        solution(prices, r);
        sign = -1;
        prices = [];
        r = 0;
    }
});

function solution(prices, r) {
    let max = -1;
    let len = prices.length;
    for (let i = 0; i < len - 2; i++) {
        for (let j = 0; j < len - 1; j++) {
            for (let k = 0; k < len; k++) {
                if (i != j && j != k && i != k) {
                    let sum = prices[i] + prices[j] + prices[k];
                    if (sum <= r && sum > max) {
                        max = sum;
                    }
                }
            }
        }
    }
    console.log(max);
}
```

## 题目55  单词接龙
```java
package com.amoscloud.newcoder.easy;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/8
 * Time: 17:39
 * Description:
 */
public class Main47 {
    /*
        单词接龙的规则是
        可用于接龙的单词首字母必须要与前一个单词的尾字母相同
        当存在多个首字母相同的单词时
        取长度最长的单词
        如果长度也相等则取词典序最小的单词
        已经参与接龙的单词不能重复使用
        现给定一组全部由小写字母组成的单词数组
        并指定其中的一个单词为起始单词
        进行单词接龙
        请输出最长的单词串
        单词串是由单词拼接而成 中间没有空格

        输入描述：
            输入的第一行为一个非负整数
            表示起始单词在数组中的索引k  0<=k<=n
            第二行输入的是一个非负整数表示单词的个数n
            接下来的n行分别表示单词数组中的单词

        输出描述：
            输出一个字符串表示最终拼接的字符串

        示例1：
        输入
          0
          6
          word
          dd
          da
          dc
          dword
          d

        输出
          worddwordda

        说明：
        先确定起始单词word
        再确定d开头长度最长的单词dword
        剩余以d开头且长度最长的由 da dd dc
        则取字典序最小的da
        所以最后输出worddwordda

        示例二：
        输入：
            4
            6
            word
            dd
            da
            dc
            dword
            d
         输出：
         dwordda

     */

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int k = Integer.parseInt(in.nextLine());
        int n = Integer.parseInt(in.nextLine());
        ArrayList<String> words = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            words.add(in.nextLine());
        }
        in.close();

        StringBuilder builder = new StringBuilder();
        builder.append(words.get(k));
        words.remove(k);

        words.sort(new Comparator<String>() {
            @Override
            public int compare(String w1, String w2) {
                int len1 = w1.length();
                int len2 = w2.length();
                if (len1 != len2) {
                    return len2 - len1;
                } else {
                    return w1.compareTo(w2);
                }
            }
        });

        int len;
        do {
            len = builder.length();
            String last = builder.substring(builder.length() - 1);
            for (int i = 0; i < words.size(); i++) {
                String cur = words.get(i);
                if (cur.startsWith(last)) {
                    builder.append(cur);
                    words.remove(cur);
                    break;
                }
            }
        } while (builder.length() != len);

        System.out.println(builder.toString());

    }
}
```
### javascript
```javascript
// 单词接龙 输出最长的单词串
// 单词首字母必须要与前一个单词的尾字母相同
// 输入
// 0
// 6
// word
// dd
// da
// dc
// dword
// d
// 输出
// worddwordda
// 说明：
// 先确定起始单词word
// 再确定d开头长度最长的单词dword
// 剩余以d开头且长度最长的由 da dd dc
// 则取字典序最小的da
// 所以最后输出worddwordda

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let k = 0; // 第一个单词索引
let n = 0; // 单词个数
let words = [];

rl.on('line', line => {
    if (sign == -1) {
        k = parseInt(line);

        sign = 1;
    } else if (sign == 1) {
        n = parseInt(line);

        sign = 2;
    } else {
        words.push(line);
		// 注意结束
        if (words.length == n) {
            solution(words, k);

            sign = -1;
            words = [];
            k = 0;
            n = 0;
        }
    }
});

function solution(words, k) {
    let res = '';
    let first = words[k];
    res += first;
    words.splice(k, 1);

    words.sort((a, b) => {
        let lenA = a.length;
        let lenB = b.length;
        if (lenA != lenB) {
            return lenB - lenA;
        } else {
            return a.localeCompare(b);
        }
    });

    let lenTag;
    do {
        lenTag = res.length;
        let last = res.substring(res.length - 1);
        for (let i = 0; i < words.length; i++) {
            let curr = words[i];
            if (curr.startsWith(last)) {
                res += curr;
                words.splice(i, 1);
                break;
            }
        }
    } while (res.length != lenTag);
    console.log(res);
}
```
## 题目54  翻转指定区间的单词顺序
```java
package com.amoscloud.newcoder.easy;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/8
 * Time: 13:47
 * Description:
 */
public class Main42 {
    /*
    输入一个英文文章片段
    翻转指定区间的单词顺序，标点符号和普通字母一样处理
    例如输入字符串 "I am a developer."
     区间[0,3]则输出 "developer. a am I"

     输入描述：
     使用换行隔开三个参数
     第一个参数为英文文章内容即英文字符串
     第二个参数为反转起始单词下标，下标从0开始
     第三个参数为结束单词下标，

     输出描述
     反转后的英文文章片段，所有单词之间以一个半角空格分割进行输出

     示例一：
     输入
         I am a developer.
         1
         2
     输出
         I a am developer.

     示例二：
     输入
         Hello world!
         0
         1
     输出
         world! Hello

       说明：
       输入字符串可以在前面或者后面包含多余的空格，
       但是反转后的不能包含多余空格

     示例三：
     输入：
         I am a developer.
         0
         3
     输出
         developer. a am I

       说明：
       如果两个单词见有多余的空格
       将反转后单词间的空格减少到只含一个

      示例四：
      输入
       Hello!
       0
       3
      输出
       EMPTY

       说明：
       指定反转区间只有一个单词，或无有效单词则统一输出EMPTY
     */

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String line = in.nextLine();
        int start = Integer.parseInt(in.nextLine());
        int end = Integer.parseInt(in.nextLine());
        in.close();

        String[] words = line.trim().split("\\s+");

        start = Math.max(start, 0);
        end = Math.min(end, words.length - 1);

        if (end == start || start > words.length - 1 || end < 0) {
            System.out.println("EMPTY");
        } else {
            ArrayList<String> list = new ArrayList<>();
            list.addAll(Arrays.asList(words).subList(0, start));
            for (int i = end; i >= start; i--) {
                list.add(words[i]);
            }
            list.addAll(Arrays.asList(words).subList(end + 1, words.length));

            StringBuilder builder = new StringBuilder();
            for (String word : list) {
                builder.append(word).append(" ");
            }
            String res = builder.toString().trim();
            System.out.println(res);
        }

    }
}
```
### javascript
```javascript
// 英文文章 翻转指定区间的单词顺序 先分割单词 按单词的区间反转
// 第一行 英文字符串
// 第二行 起始下标
// 第三行 结束下标
// 输出 
// 输入
// I am a developer.
// 1
// 2
// 输出
// I a am developer.
// 输入
//     Hello world!
//     0
//     1
// 输出
//     world! Hello

const { listenerCount } = require('process');
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let wordsStr = '';
let start = 0;
let end = 0;

rl.on('line', line => {
    if (sign == -1) {
        wordsStr = line;

        sign = 1;
    } else if (sign == 1) {
        start = parseInt(line);

        sign = 2;
    } else {
        end = parseInt(line);

        solution(wordsStr, start, end);
        sign = -1;
        str = '';
        start = 0;
        end = 0;
        //rl.close();
    }
});

function solution(wordsStr, start, end) {
    let words = wordsStr.trim().split(/\s+/);
    start = Math.max(start, 0);
    end = Math.min(end, words.length - 1);

    if (end == start || start > words.length - 1 || end < 0) {
        console.log('EMPTY');
        return;
    } 

    let arr = [];
    arr.concat(words.slice(0, start));
    for (let i = end; i >= start; i--) {
        arr.push(words[i]);
    }
    arr.concat(words.slice(end + 1, words.length));

    console.log(arr.join(' '));
}
```

## 题目52  磁盘的容量
```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/7
 * Time: 18:29
 * Description:
 */
public class Main39 {
    /*
    磁盘的容量单位有M,G,T这三个等级
    他们之间的换算关系为
    1T=1024G
    1G=1024M
    现在给定N块磁盘的容量
      请对他们按从小到大的顺序进行稳定排序
      例如给定5块盘容量
      1T,20M,3G,10G6T,3M12G9M
    排序后的结果为20M,3G,3M12G9M,1T,10G6T

    注意单位可以重复出现
    上述3M12G9M为 3M+12G+9M和 12M12G相等

    输入描述:
    输入第一行包含一个整数N
    2<=N<=100 ,表示磁盘的个数
    接下来的N行每行一个字符串 长度 (2<l<30)
    表示磁盘的容量
    有一个或多个格式为 mv的子串组成
    其中m表示容量大小 v表示容量单位
    例如

    磁盘容量m的范围 1~1024正整数
    容量单位v的范围包含题目中提到的M,G,T

    输出描述:
     输出N行
     表示N块磁盘容量排序后的结果

     示例1:
     输入
         3
         1G
         2G
         1024M

     输出
        1G
        1024M
        2G

    说明 1G和1024M容量相等,稳定排序要求保留他们原来的相对位置
    故1G在1024M前

     示例二:
     输入
          3
          2G4M
          3M2G
          1T

      输出
        3M2G
        2G4M
        1T
        说明1T大于2G4M大于3M2G

     */

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int N = Integer.parseInt(in.nextLine());
        ArrayList<String> sizes = new ArrayList<>();
        for (int i = 0; i < N; i++) {
            sizes.add(in.nextLine());
        }
        in.close();
        sizes.sort(new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return Long.compare(parseLong(o1), parseLong(o2));
            }
        });

        sizes.forEach(System.out::println);

    }

    static Long parseLong(String size) {
        String[] units = size.split("[0-9]+");
        String[] nums = size.split("[A-Z]+");
        //[, M, G, M]
        //[3, 12, 9]

        long sum = 0;
        for (int i = 1; i < units.length; i++) {
            long num = Long.parseLong(nums[i - 1]);
            switch (units[i]) {
                case "M":
                    sum += num;
                    break;
                case "G":
                    sum += num * 1024;
                    break;
                case "T":
                    sum += num * 1024 * 1024;
                    break;
            }
        }

        return sum;
    }
}
```
### javascript
```javascript
// 磁盘容量
// 请对他们按从小到大的顺序进行稳定排序
// 输入：第一行 N 磁盘个数
// 接下来N 行 磁盘容量
// 输出 N行 排序后的结果
// 输入
// 3
// 1G
// 2G
// 1024M
// 输出
// 1G
// 1024M
// 2G

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let N = 0;
let memorys = [];

rl.on('line', line => {
    if (sign == -1) {
        N = parseInt(line);

        sign = 1;
    } else {
        memorys.push(line);

        if (memorys.length == N) {
            solution(memorys);

            sign = -1;
            N = 0;
            memorys = [];
            rl.close();
        }
    }
});

function solution(memorys) {
    memorys.sort((a, b) => getNum(a) - getNum(b));
    memorys.forEach(i => console.log(i));
}

function getNum(str) {
    let nums = str.split(/[A-Z]+/);
    let chars = str.split(/[0-9]+/);

    let sum = 0;
    for (let i = 1; i < chars.length; i++) {
        let num = parseInt(nums[i - 1]);
        let char = chars[i];
        if (char == 'M') {
            sum += num;
        } else if (char == 'G') {
            sum += num * 1024;
        } else if (char == 'T') {
            sum += num * 1024 * 1024;
        }
    }
    return sum;
}
```
## 题目51 最长的非严格递增连续数字序列长度
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/7
 * Time: 17:28
 * Description:
 */
public class Main38 {
    public static void main(String[] args) {
        /*
            输入一个字符串仅包含大小写字母和数字
            求字符串中包含的最长的非严格递增连续数字序列长度
            比如：
                12234属于非严格递增数字序列
            示例：
            输入
                abc2234019A334bc
            输出
                4
            说明：
                2234为最长的非严格递增连续数字序列，所以长度为4

                aaaaaa44ko543j123j7345677781
                aaaaa34567778a44ko543j123j71
                345678a44ko543j123j7134567778aa
         */

        Scanner in = new Scanner(System.in);
        String line = in.nextLine();
        in.close();

        char[] chars = line.toCharArray();

        int curLen = 0, maxLen = 0;

        char last = 'a';
        for (char cur : chars) {
            if (Character.isDigit(cur)) {
                if (curLen == 0) {
                    curLen++;
                } else if (cur >= last) {
                    curLen++;
                } else {
                    if (curLen > maxLen) {
                        maxLen = curLen;
                    }
                    curLen = 1;
                }
                last = cur;
            } else {
                if (curLen > maxLen) maxLen = curLen;
                curLen = 0;
                last = 'a';
            }
        }

        if (curLen > maxLen) {
            maxLen = curLen;
        }

        System.out.println(maxLen);
    }
}
```
### javascript
```javascript
// 一个字符串仅包含大小写字母和数字
// 求字符串中包含的最长的非严格递增连续数字序列长度
// 比如：
// 12234属于非严格递增数字序列
// 示例：
// 输入
//     abc2234019A334bc
// 输出
// 4
// 2234为最长的非严格递增连续数字序列，所以长度为4

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});


rl.on('line', line => {
    solution(line);

    rl.close();
});

function solution(str) {
    let chars = str.split('');
    let currLen = 0;
    let maxLen = 0;

    let lastChar = 'a';
    chars.forEach(char => {
        if (!isNaN(char)) {
            if (currLen == 0 || char >= lastChar) {
                currLen++;
            } else {
                if (currLen > maxLen) {
                    maxLen = currLen;
                }
                currLen = 1;
            }
            lastChar = char;
        } else {
            if (currLen > maxLen) {
                maxLen = currLen;
            }
            currLen = 0;
            lastChar = 'a';
        }
    });
    if (currLen > maxLen) {
        maxLen = currLen;
    }
    console.log(maxLen);
}
```
## 题目50  糖果盒 最少分至一颗糖果的次数
```java
package com.amoscloud.newcoder;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/29
 * Time: 8:23
 * Description:
 */
public class Main36 {
    /*
    小明从糖果盒中随意抓一把糖果
    每次小明会取出一半的糖果分给同学们
    当糖果不能平均分配时
    小明可以从糖果盒中(假设盒中糖果足够)取出一个或放回一个糖果
    小明至少需要多少次(取出放回和平均分配均记一次)能将手中糖果分至只剩一颗

    输入描述：
      抓取糖果数(小于1000000)：15
    输出描述：
      最少分至一颗糖果的次数：5

     示例1：
       输入
         15
       输出
         5
       备注
          解释：(1) 15+1=16;
               (2) 16/2=8;
               (3) 8/2=4;
               (4) 4/2=2;
               (5) 2/2=1;

     */

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        in.close();

        int count = 0;

        for (int i = n; i != 1; i /= 2, count++) {
            if (i == 3) {
                System.out.println(count += 2);
                return;
            }
            if (i % 2 != 0) {
                if ((i + 1) / 2 % 2 == 0) i++;
                else i--;
                count++;
            }
        }

        System.out.println(count);

    }
}
```
### javascript
```javascript
// 糖果盒 最少分至一颗糖果的次数
// 输入：抓取糖果数
// 输出: 最少分至一颗糖果的次数
// 输入
// 15
// 输出
// 5
// 备注
// 解释：(1) 15+1=16;
//     (2) 16/2=8;
//     (3) 8/2=4;
//     (4) 4/2=2;
//     (5) 2/2=1;

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let candy = parseInt(line);

    solution(candy);
    rl.close();
});

function solution(candy) {
    let count = 0;

    for (let i = candy; i != 1; i = parseInt(i / 2), count++) {
        if (i == 3) {
            count += 2;
            console.log(count);
            return;
        }
        if (i % 2 != 0) {
            if (parseInt((i + 1) / 2) % 2 == 0) {
                i++;
            } else {
                i--;
            }
            count++;
        }
    }
    console.log(count);
}

```
## 题目49 运送快递的货车 计算最多能装多少个快递
```java
package com.amoscloud.newcoder;

import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/29
 * Time: 7:54
 * Description:
 */
public class Main35 {
    public static void main(String[] args) {
        /*
        一辆运送快递的货车。运送的快递均放在大小不等的长方形快递盒中
        为了能够装载更多的快递 同时不能让货车超载
        需要计算最多能装多少个快递
        快递的体积不受限制
        快递数量最多1000个
        货车载重量50000

        输入描述：
        第一行输入 每个快递重量 用逗号分隔
        如5,10,2,11
        第二行 输入 货车的载重量
        如20
        不需要考虑异常输入

        输出描述：
        输出最多能装多少个快递

        货车的载重量为20 最多只能放3种快递 5,10,2因此输出3

        示例1：
         输入
         5,10,2,11
         20
         输出
         3

         */
        Scanner in = new Scanner(System.in);
        String[] line = in.nextLine().split(",");
        int p = Integer.parseInt(in.nextLine());
        in.close();

        List<Integer> ints = Arrays.stream(line)
                .map(Integer::parseInt)
                .sorted()
                .collect(Collectors.toList());

        int sum = 0, count = 0;
        for (Integer i : ints) {
            if (sum + i <= p) {
                sum += i;
                count++;
            } else break;
        }

        System.out.println(count);

    }
}
```
### javascript
```javascript
// 运送快递的货车 计算最多能装多少个快递
// 输入:
// 第一行 每个快递重量 逗号分割
// 第二行 货车的载重量
// 输出:
// 最多能装多少个快递
// 输入
// 5,10,2,11
// 20
// 输出
// 3

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let weights = [];
let W = 0;

rl.on('line', line => {
    if (sign == -1) {
        weights = line.split(',').map(i => parseInt(i));

        sign = 1;
    } else {
        W = parseInt(line);

        solution(weights, W);
        sign = -1;
        weights = [];
        W = 0;
        // rl.close();
    }

});

function solution(weights, W) {
    weights.sort((a, b) => a - b);

    let sum = 0; 
    let count = 0;
    for (let i of weights) {
        if (sum + i <= W) {
            sum += i;
            count++;
        } else {
            break;
        }
    }
    console.log(count);
}
```
## 题目48 两个没有相同字符的元素长度乘积的最大值
```java
package com.amoscloud.newcoder;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/23
 * Time: 17:14
 * Description:
 */
public class Main33 {
    public static void main(String[] args) {
        /*
        给定一个元素类型为小写字符串的数组
        请计算两个没有相同字符的元素长度乘积的最大值
        如果没有符合条件的两个元素返回0

        输入描述
          输入为一个半角逗号分割的小写字符串数组
          2<= 数组长度 <=100
          0< 字符串长度 <=50
        输出描述
          两个没有相同字符的元素长度乘积的最大值

        示例一
          输入
            iwdvpbn,hk,iuop,iikd,kadgpf
          输出
            14
          说明
           数组中有5个元组  第一个和第二个元素没有相同字符
           满足条件 输出7*2=14

         */

        Scanner in = new Scanner(System.in);
        String line = in.nextLine();
        in.close();

        String[] strings = line.split(",");

        int max = 0;
        for (int i = 0; i < strings.length; i++) {
            for (int j = i; j < strings.length; j++) {
                char[] chars = strings[j].toCharArray();
                int k = 0;
                while (k < chars.length) {
                    if (strings[i].indexOf(chars[k]) != -1) break;
                    k++;
                }
                int tmp = strings[i].length() * strings[j].length();
                if (k == chars.length && tmp > max) max = tmp;
            }
        }

        System.out.println(max);
    }
}
```
### javascript
```javascript
// 两个没有相同字符的元素长度乘积的最大值
// 输入： 逗号分割的小写字符串数组
// 输出： 最大值
// 输入
// iwdvpbn,hk,iuop,iikd,kadgpf
// 输出
// 14

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let strs = line.split(',');

    solution(strs);
    rl.close();
});

function solution(strs) {
    let max = 0;
    let len = strs.length;
    for (let i = 0; i < len; i++) {
        for (let j = i; j < len; j++) {
            let chars = strs[j].split('');
            let k = 0;
            while (k < chars.length) {
                if (strs[i].indexOf(chars[k]) != -1) {
                    break;
                }
                k++;
            }
            let t = strs[i].length * strs[j].length;
            if (k == chars.length && t > max) {
                max = t;
            }
        }
    }
    console.log(max);
}
```
## 题目47  含有相对开音节结构的子串个数
```java
package com.amoscloud.newcoder;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/23
 * Time: 16:12
 * Description:
 */
public class Main32 {
    public static void main(String[] args) {
        /*
         相对开音节构成的结构为辅音+元音(aeiou)+辅音(r除外)+e
         常见的单词有bike cake
         给定一个字符串，以空格为分隔符
         反转每个单词的字母
         若单词中包含如数字等其他非字母时不进行反转
         反转后计算其中含有相对开音节结构的子串个数
         (连续子串中部分字符可以重复)

         输入描述
            字符串 以空格分割的多个单词
            长度<10000 字母只考虑小写

         输出描述
             含有相对开音节结构的子串个数

         示例1：
            输入
              ekam a ekac
            输出
              2
            说明：
             反转后为  make a cake 其中make和cake为相对开音节子串
             返回2

          示例2：
             输入
                !ekam a ekekac
             输出
                 2
             说明
                 反转后为 !ekam a cakeke
                 因为!ekam含有非英文字母，所以未反转
                 其中 cake和keke 为相对开音节子串 返回2

         */

        Scanner in = new Scanner(System.in);
        String line = in.nextLine();
        in.close();

        String[] words = line.split(" ");

        int count = 0;

        for (String word : words) {
            char[] chars = word.toCharArray();
            if (word.replaceAll("[a-z]+", "").isEmpty()) {
                for (int i = 0, j = chars.length - 1; i < j; i++, j--) {
                    char tmp = chars[i];
                    chars[i] = chars[j];
                    chars[j] = tmp;
                }
            }
            if (chars.length < 4) continue;
            for (int i = 0; i < chars.length - 3; i++) {
                if (!isVowel(chars[i])
                        && isVowel(chars[i + 1])
                        && !isVowel(chars[i + 2]) && chars[i + 2] != 'r'
                        && chars[i + 3] == 'e') {
                    count++;
                }
            }
        }

        System.out.println(count);
    }

    private static boolean isVowel(char c) {
        return c == 'a' || c == 'e' || c == 'i'
                || c == 'o' || c == 'u';
    }
}
```
### javascript
```javascript
// 反转后计算其中含有相对开音节结构的子串个数
// 输入： 字符串 空格分割的多个单词
// 输出:  含有相对开音节结构的子串个数

// 输入
// ekam a ekac
// 输出
// 2
// 说明：
// 反转后为  make a cake 其中make和cake为相对开音节子串
// 返回2

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let words = line.split(' ');

    solution(words);
    rl.close();
});

function solution(words) {
    let count = 0;

    for (let word of words) {
        let chars = word.split('');
        if (word.replace(/[a-z]+/g, '').length == 0) {
            for (let i = 0, j = chars.length - 1; i < j; i++, j--) {
                let tmp = chars[i];
                chars[i] = chars[j];
                chars[j] = tmp;
            }
        }

        if (chars.length < 4) {
            continue;
        }

        for (let i = 0; i < chars.length - 3; i++) {
            if (!isValid(chars[i]) && isValid(chars[i + 1])
             && !isValid(chars[i + 2]) 
             && chars[i + 2] != 'r' && chars[i + 3] == 'e') {
                count++;
            }
        }
    }
    
    console.log(count);
}

function isValid(char) {
    return ['a', 'e', 'i', 'o', 'u'].includes(char);
}
```

## 题目46 火星人使用的运算符号
```java
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/20
 * Time: 16:47
 * Description:
 */
public class Main30 {
    public static void main(String[] args) {

        /*
           已知火星人使用的运算符号为#;$
           其与地球人的等价公式如下
           x#y=2*x+3*y+4
           x$y=3*x+y+2
           x y是无符号整数
           地球人公式按照c语言规则进行计算
           火星人公式中$符优先级高于#相同的运算符按从左到右的顺序运算

           输入描述：
           火星人字符串表达式结尾不带回车换行
           输入的字符串说明是 字符串为仅有无符号整数和操作符组成的计算表达式

           1.用例保证字符串中操作数与操作符之间没有任何分隔符
           2.用例保证操作数取值范围为32位无符号整数，
           3.保证输入以及计算结果不会出现整型溢出
           4.保证输入的字符串为合法的求值报文
           例如: 123#4$5#76$78
           5.保证不会出现非法的求值报文
           例如: #4$5 这种缺少操作数
                4$5#  这种缺少操作数
                4#$5  这种缺少操作数
                4 $5  有空格
                3+4-5*6/7 有其他操作符
                12345678987654321$54321 32位整数溢出

           输出描述：
             根据火星人字符串输出计算结果
             结尾不带回车换行

           案例1：
          输入：
           7#6$5#12
          输出：
           226

           说明 示例7#6$5#12=7#(3*6+5+2)#12
                           =7#25#12
                           =(2*7+3*25+4)#12
                           =93#12
                           =2*93+3*12+4
                           =226
         */

        Scanner in = new Scanner(System.in);
        String input = in.nextLine();
        in.close();

        List<String> operators = Arrays.stream(input.split("\\w+"))
                .filter(x -> !x.isEmpty())
                .collect(Collectors.toList());

        List<Integer> nums = Arrays.stream(input.split("\\W+"))
                .map(Integer::parseInt)
                .collect(Collectors.toList());

        int pos$ = operators.indexOf("$");
        while (pos$ != -1) {
            int tmp = dollar(nums.get(pos$), nums.get(pos$ + 1));
            nums.set(pos$, tmp);
            nums.remove(pos$ + 1);
            operators.remove(pos$);
            pos$ = operators.indexOf("$");
        }

        int res = nums.get(0);
        for (int i = 1; i < nums.size(); i++) {
            res = sharp(res, nums.get(i));
        }

        System.out.println(res);
    }

    public static int sharp(int x, int y) {
        return 2 * x + 3 * y + 4;
    }

    public static int dollar(int x, int y) {
        return 3 * x + y + 2;
    }
}
```
### javascript
```javascript
// 火星字符串
// 输入：
// 7#6$5#12
// 输出：
// 226
// 说明 示例7#6$5#12=7#(3*6+5+2)#12
//                            =7#25#12
//                            =(2*7+3*25+4)#12
//                            =93#12
//                            =2*93+3*12+4
//                            =226

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {

    solution(line);
    rl.close();
});

function solution(line) {
    let opts = line.split(/\w+/).filter(i => i);
    let nums = line.split(/\W+/).map(i => parseInt(i));

    let pos = opts.indexOf('$');
    while (pos != -1) {
        let tmp = getDollar(nums[pos], nums[pos + 1]);
        nums[pos] = tmp;
        nums.splice(pos + 1, 1);
        opts.splice(pos, 1);
        pos = opts.indexOf('$');
    }
    let res = nums[0];
    for (let i = 1; i < nums.length; i++) {
        res = getSharp(res, nums[i]);
    }
    console.log(res);
}

function getDollar(x, y) {
    return 3 * x + y + 2;
}

function getSharp(x, y) {
    return 2 * x + 3 * y + 4;
}
```
## 题目45 检查数组中是否存在满足规则的数组组合
```java
import java.util.*;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/19
 * Time: 16:06
 * Description:
 */
public class Main29 {
    /*
    给定一个正整数数组
    检查数组中是否存在满足规则的数组组合
    规则：
      A=B+2C
    输入描述
     第一行输出数组的元素个数
     接下来一行输出所有数组元素  用空格隔开
    输出描述
     如果存在满足要求的数
     在同一行里依次输出 规则里 A/B/C的取值 用空格隔开
     如果不存在输出0

     示例1：
       输入
       4
       2 7 3 0
       输出
       7 3 2
       说明：
        7=3+2*2
       示例2：
       输入
        3
        1 1 1
       输出
        0
        说明找不到满足条件的组合

        备注：
        数组长度在3~100之间
        数组成员为0~65535
        数组成员可以重复
        但每个成员只能在结果算式中使用一次
        如 数组成员为 [0,0,1,5]
        0出现两次允许，但结果0=0+2*0不允许  因为算式中使用了3个0

        用例保证每组数字里最多只有一组符合要求的解
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int num = Integer.parseInt(in.nextLine());
        String[] nums = in.nextLine().split(" ");
        in.close();

        TreeSet<Integer> intSet = new TreeSet<>();
        for (String s : nums) {
            intSet.add(Integer.parseInt(s));
        }

        ArrayList<Integer> list = new ArrayList<>(intSet);

        String res = "0";

        for (int a = list.size() - 1; a >= 0; a--) {
            for (int b = 0; b < a; b++) {
                for (int c = 0; c < a; c++) {
                    Integer A = list.get(a);
                    Integer B = list.get(b);
                    Integer C = list.get(c);
                    if (A == B + 2 * C) {
                        res = A + " " + B + " " + C;
                    }
                }
            }
        }

        System.out.println(res);
    }
}
```
## 题目44 返回次子序列的长度
```java
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/19
 * Time: 15:23
 * Description:
 */
public class Main28 {

    public static void main(String[] args) {
         /*
         有N个正整数组成的一个序列
         给定一个整数sum
         求长度最长的连续子序列使他们的和等于sum
         返回次子序列的长度
         如果没有满足要求的序列 返回-1
         案例1：
         输入
         1,2,3,4,2
         6
         输出
         3
         解析：1,2,3和4,2两个序列均能满足要求
         所以最长的连续序列为1,2,3 因此结果为3

         示例2：
         输入
         1,2,3,4,2
         20
         输出
         -1
         解释：没有满足要求的子序列，返回-1

         备注： 输入序列仅由数字和英文逗号构成
         数字之间采用英文逗号分割
         序列长度   1<=N<=200
         输入序列不考虑异常情况
         由题目保证输入序列满足要求
          */
        Scanner in = new Scanner(System.in);
        List<Integer> ints = Arrays.stream(in.nextLine().split(","))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
        int sum = Integer.parseInt(in.nextLine());
        in.close();

        int max_len = 0;

        for (int i = 0; i < ints.size(); i++) {
            int tmp_sum = 0;
            int sub_len = 0;
            for (int j = i; j < ints.size(); j++) {
                if (tmp_sum > sum) break;
                tmp_sum += ints.get(j);
                sub_len++;
                if (tmp_sum == sum && sub_len > max_len)
                    max_len = sub_len;
            }
        }

        max_len = max_len == 0 ? -1 : max_len;

        System.out.println(max_len);
    }
}
```
### javascript
```javascript
// 给定一个整数sum
// 求长度最长的的连续子序列使他们的和等于sum
// 返回次子序列的长度
// 输入
// 1,2,3,4,2
// 6
// 输出
// 3
// 输入
// 1,2,3,4,2
// 20
// 输出
// -1

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let data = [];
let sum = 0;

rl.on('line', line => {

    if (sign == -1) {
        data = line.split(',').map(i => parseInt(i));

        sign = 1;
    } else {
        sum = parseInt(line);

        solution(data, sum);
        sign = -1;
        data = [];
        sum = 0;
        rl.close();
    }
});

function solution(data, sum) {
    let max = 0;
    for (let i = 0; i < data.length; i++) {
        let tmpSum = 0;
        let subLen = 0;
        for (let j = i; j < data.length; j++) {
            if (tmpSum > sum) {
                break;
            }
            tmpSum += data[j];
            subLen++;
            if (tmpSum == sum && subLen > max) {
                max = subLen;
            }
        }
    }
    max = max == 0 ? -1 : max;
    console.log(max);
}
```

## 题目43 整数编码方法
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/1
 * Time: 17:00
 * Description:
 */
public class Main22 {
    public static void main(String[] args) {
        /*
        实现一个整数编码方法
        使得待编码的数字越小
        编码后所占用的字节数越小
        编码规则如下
        1.编码时7位一组，每个字节的低7位用于存储待编码数字的补码
        2.字节的最高位表示后续是否还有字节，置1表示后面还有更多的字节，
        置0表示当前字节为最后一个字节
        3.采用小端序编码，低位和低字节放在低地址上
        4.编码结果按16进制数的字符格式进行输出，小写字母需要转化为大写字母

        输入描述
        输入的为一个字符串表示的非负整数
        输出描述
        输出一个字符串表示整数编码的16进制码流

        示例一
        输入
        0
        输出
        00
        说明：输出的16进制字符不足两位的前面补零

        示例二
        输入
        100
        输出
        64
        说明:100的二进制表示为0110 0100只需一个字节进行编码
        字节的最高位0，剩余7位存储数字100的低7位(1100100)所以编码后的输出为64

        示例三
        输入
        1000
        输出
        E807
        说明
        1000的二进制表示为 0011 1110 1000 至少需要两个字节进行编码
        第一个字节最高位是1 剩余7位存储数字 1000的低7位(1101000)
        所以第一个字节的二进制位(1110 1000)即E8
        第二个字节最高位置0 剩余的7位存储数字 1000的第二个低7位(0000111)
        所以第一个字节的二进制为(0000 0111)即07
        采用小端序编码 所以低字节E8输出在前面
        高字节07输出在后面

        备注
            代编码数字取值范围为 [0,1<<64-1]
         */

        Scanner in = new Scanner(System.in);
        int num = Integer.parseInt(in.nextLine());
        in.close();

        String binary = Integer.toBinaryString(num);

        int len = binary.length();

        StringBuilder sb = new StringBuilder();
        for (int i = len; i > 0; i -= 7) {
            int start = Math.max(i - 7, 0);

            String bin = binary.substring(start, i);

            if (bin.length() < 7) {
                StringBuilder head = new StringBuilder();
                for (int j = 0; j < 7 - bin.length(); j++) {
                    head.append("0");
                }
                bin = head.append(bin).toString();
            }
            bin = i - 7 <= 0 ? "0" + bin : "1" + bin;
            String hex = Integer.toHexString(Integer.parseInt(bin, 2)).toUpperCase();
            if (hex.length() == 1) hex = "0" + hex;
            sb.append(hex);
        }

        System.out.println(sb);

    }
}
```
### javascript
```javacript
// 整数编码方法 使得待编码的数字越小  编码后所占用的字节数越小
// 输入：字符串表示的非负整数
// 输出：字符串表示整数编码的16进制码流
// 输入
// 0
// 输出
// 00
// 说明：输出的16进制字符不足两位的前面补零
// 输入
// 100
// 输出
// 64
// 说明:100的二进制表示为0110 0100只需一个字节进行编码

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {

    let num = parseInt(line);
    solution(num);
    // rl.close();
});

function solution(num) {
    let binary = Number(num).toString(2);
    let len = binary.length;

    let res = '';
    for (let i = len; i > 0; i -= 7) {
        let start = Math.max(i - 7, 0);
        let bin = binary.substring(start, i);

        if (bin.length < 7) {
            let head = '';
            for (let j = 0; j < 7 - bin.length; j++) {
                head += '0';
            }
            bin = head + bin;
        }
        bin = i - 7 <= 0 ? '0' + bin : '1' + bin;
        let hex = Number(parseInt(bin, 2)).toString(16).toUpperCase(); // 二进制字符串先转10进制,再转16进制
        if (hex.length == 1) {
            hex = '0' + hex;
        }
        res += hex;
    }
    console.log(res);
}
```

## 题目42 输出从vlan资源池中移除申请的vlan后的资源池
```java
import java.util.ArrayList;
import java.util.Scanner;
import java.util.TreeSet;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/4/1
 * Time: 16:07
 * Description:
 */
public class Main21 {
    /*
    Vlan是一种为局域网设备进行逻辑划分的技术
    为了标识不同的vlan 引入了vlan id 1~4094之间的整数
    定义一个vlan id 的资源池
    资源池中连续的vlan用开始vlan-结束vlan表示，
    不连续的用单个整数表示
    所有的vlan用英文逗号连接起来
    现有一个vlan资源池，业务需要从资源池中申请一个vlan
    需要你输出从vlan资源池中移除申请的vlan后的资源池

    输入描述
    第一行为字符串格式的vlan资源池
    第二行为业务要申请的vlan vlan的取值范围1~4094

    输出描述
    从输入vlan资源池中移除申请的vlan后
    字符串格式的vlan资源池
    输出要求满足题目中要求的格式，
    并且要求从小到大升序输出
    如果申请的vlan不在原资源池，输出升序排序的原资源池的字符串即可

    示例一
    输入
    1-5
    2
    输出
    1,3-5
    说明：原vlan资源池中有1 2 3 4 5 移除2后
    剩下的1 3 4 5按照升序排列的方式为 1 3-5

    示例二
    输入
    20-21,15,18,30,5-10
    15
    输出
    5-10,18,20-21,30
    说明：
    原vlan资源池中有5 6 7 8 9 10 15 18 20 21 30
    移除15后 剩下的为 5 6 7 8 9 10 18 20 21 30
    按照题目描述格式并升序后的结果为5-10,18,20-21,30

    示例三
    输入
    5,1-3
    10
    输出
    1-3,5
    资源池中有1 2 3 5
    申请的资源不在资源池中
    将原池升序输出为1-3,5

    输入池中vlan数量范围为2~2094的整数
    资源池中vlan不重复且合法1~2094的整数
    输入是乱序的

     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String input = in.nextLine();
        Integer request = Integer.parseInt(in.nextLine());
        in.close();
        TreeSet<Integer> set = new TreeSet<>();
        for (String str : input.split(",")) {
            if (str.contains("-")) {
                String[] split = str.split("-");
                int start = Integer.parseInt(split[0]);
                int end = Integer.parseInt(split[1]);
                for (int i = start; i <= end; i++) {
                    set.add(i);
                }
            } else {
                set.add(Integer.parseInt(str));
            }
        }

        set.remove(request);

        ArrayList<Integer> list = new ArrayList<>(set);
        StringBuilder sb = new StringBuilder();

        Integer start = list.get(0);
        Integer last = start;

        for (int i = 1; i < list.size(); i++) {
            Integer cur = list.get(i);
            if (cur == last + 1) {
                last = cur;
            } else {
                append(sb, start, last);
                start = last = cur;
            }
        }
        append(sb, start, last);

        System.out.println(sb.substring(0, sb.length() - 1));
    }

    private static void append(StringBuilder sb, Integer start, Integer last) {
        if (start.equals(last)) {
            sb.append(last).append(",");
        } else {
            sb.append(start).append("-")
                    .append(last).append(",");
        }
    }
}
```
### javascript
```javascript
// 输出从vlan资源池中移除申请的vlan后的资源池
// 输入：
// 第一行为字符串格式的vlan资源池
// 第二行为业务要申请的vlan
// 输出：
// 移除的vlan资源池
// 输入
// 1-5
// 2
// 输出
// 1,3-5
// 说明：原vlan资源池中有1 2 3 4 5 移除2后
// 剩下的1 3 4 5按照升序排列的方式为 1 3-5
// 输入
// 20-21,15,18,30,5-10
// 15
// 输出
// 5-10,18,20-21,30

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let vlans = [];
let delVlan = 0;

rl.on('line', line => {
    if (sign == -1) {
        vlans = line.split(',');

        sign = 1;
    } else {
        delVlan = parseInt(line);

        solution(vlans, delVlan);
        sign = -1;
        vlans = [];
        delVlan = 0;
        // rl.close();
    }
});

function solution(vlans, delVlan) {
    let set = new Set();
    for (let s of vlans) {
        if (s.indexOf('-') != -1) {
            let chars = s.split('-');
            let start = parseInt(chars[0]);
            let end = parseInt(chars[1]);
            for (let i = start; i <= end; i++) {
                set.add(i);
            }
        } else {
            set.add(parseInt(s));
        }
    }
    set.delete(delVlan);
    let arr = Array.from(set);
    arr.sort((a, b) => a - b);
    let res = '';
    let start = arr[0];
    let last = start;
    for (let i = 1; i < arr.length; i++) {
        let curr = arr[i];
        if (curr == last + 1) {
            last = curr;
        } else {
            res = addTo(res, start, last);
            start = last = curr;
        }
    }
    res = addTo(res, start, last);
    console.log(res);
}
function addTo(res, start, last) {
    if (start == last) {
        res += last + ',';
    } else {
        res += start + '-' + last + ',';
    }
    return res;
}
```
## 题目39 数列A[n]
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/26
 * Time: 14:26
 * Description:
 */
public class Main18 {
    public static void main(String[] args) {
        /*
            有一个数列A[n]
            从A[0]开始每一项都是一个数字
            数列中A[n+1]都是A[n]的描述
            其中A[0]=1
            规则如下
            A[0]:1
            A[1]:11 含义其中A[0]=1是1个1 即11
            表示A[0]从左到右连续出现了1次1
            A[2]:21 含义其中A[1]=11是2个1 即21
            表示A[1]从左到右连续出现了2次1
            A[3]:1211 含义其中A[2]从左到右是由一个2和一个1组成 即1211
            表示A[2]从左到右连续出现了一次2又连续出现了一次1
            A[4]:111221  含义A[3]=1211 从左到右是由一个1和一个2两个1 即111221
            表示A[3]从左到右连续出现了一次1又连续出现了一次2又连续出现了2次1

             输出第n项的结果
             0<= n <=59
             输入描述：
             数列第n项   0<= n <=59
             4
             输出描述
             数列内容
             111221
         */
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        in.close();

        StringBuilder next;
        String content = "1";

        if (n == 0) {
            System.out.println(content);
            return;
        }
        for (int i = 1; i <= n; i++) {
            next = new StringBuilder();

            char[] chars = content.toCharArray();
            char last = chars[0];
            int count = 1;
            for (int j = 1; j < chars.length; j++) {
                if (chars[j] == last) count++;
                else {
                    next.append(count).append(last);
                    count = 1;
                    last = chars[j];
                }
            }
            next.append(count).append(last);
            content = next.toString();
        }

        System.out.println(content);

    }
}
```
### javascript
```javascript
// 有一个数列A[n]
// 从A[0]开始每一项都是一个数字
// 数列中A[n+1]都是A[n]的描述
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let n = parseInt(line);
    if (n == 0) {
        console.log('1');
        return;
    }

    solution(n);
    
    rl.close();
 });

 function solution(n) {
     let content = '1';
     for (let i = 1; i <= n; i++) {
         let next = [];
         let chars = content.split('');
         let last = chars[0];
         let count = 1;
         for (let j = 1; j < chars.length; j++) {
             if (chars[j] == last) count++;
             else {
                 next.push(count);
                 next.push(last);
                 count = 1;
                 last = chars[j];
             }
         }
         next.push(count);
         next.push(last);
         content = next.join('');
     }
     console.log(content);
 }
```
## 题目38 url前缀和url后缀 拼接
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/23
 * Time: 15:11
 * Description:
 */
public class Main16 {
    public static void main(String[] args) {
        /*
        给定一个url前缀和url后缀
        通过,分割 需要将其连接为一个完整的url
        如果前缀结尾和后缀开头都没有/
        需要自动补上/连接符
        如果前缀结尾和后缀开头都为/
        需要自动去重
        约束：
         不用考虑前后缀URL不合法情况

         输入描述
         url前缀(一个长度小于100的字符串)
         url后缀(一个长度小于100的字符串)
         输出描述
         拼接后的url

         一、
         输入
         /acm,/bb
         输出
         /acm/bb

         二、
         输入
         /abc/,/bcd
         输出
         /abc/bcd

         三、
         输入
         /acd,bef
         输出
         /acd/bef

         四、
         输入
         ,
         输出
         /

         */

        Scanner in = new Scanner(System.in);
        String line = in.nextLine();
        in.close();

        String[] split = line.split(",");
        if (split.length == 0) {
            System.out.println("/");
            return;
        }

        String combine = split[0] + "/" + split[1];
        String url = combine.replaceAll("/+", "/");
        System.out.println(url);

    }
}
```
### javascript
```javascript
// 给定一个url前缀和url后缀
// 通过,分割 需要将其连接为一个完整的url
// 如果前缀结尾和后缀开头都没有/
// 需要自动补上/连接符
// 输入描述
// url前缀(一个长度小于100的字符串)
// url后缀(一个长度小于100的字符串)
// 输出描述
// 拼接后的url
// 输入
// /acm,/bb
// 输出
// /acm/bb

// 输入
// /abc/,/bcd
// 输出
// /abc/bcd
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let urls = line.split(',');
    if (!urls.length) {
        console.log('/');
        return;
    }

    solution(urls);
    
    rl.close();
 });

 function solution(urls) {
    let merge = urls[0] + '/' + urls[1];
    let url = merge.replace(/\/+/g, '/');
    console.log(url);
 }
```

## 题目36 比赛 最多可以派出的团队数量
```java
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/13
 * Time: 14:44
 * Description:
 */
public class Main14 {
    public static void main(String[] args) {
        /*
        用数组代表每个人的能力
        一个比赛活动要求 参赛团队的最低能力值为N
        每个团队可以由一人或者两人组成
        且一个人只能参加一个团队
        计算出最多可以派出多少只符合要求的队伍

        输入描述
        5
        3 1 5 7 9
        8
        第一行代表总人数，范围  1~500000
        第二行数组代表每个人的能力
           数组大小范围 1~500000
           元素取值范围 1~500000
        第三行数值为团队要求的最低能力值
         1~500000

         输出描述
         3
         最多可以派出的团队数量

         示例一
         输入
         5
         3 1 5 7 9
         8

         输出
         3

         说明 3、5组成一队   1、7一队  9自己一队  输出3

         7
         3 1 5 7 9 2 6
         8

        3
        1 1 9
        8

          */

        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        String[] nums = in.nextLine().split(" ");
        int base = Integer.parseInt(in.nextLine());
        in.close();

        Integer[] list = Arrays.stream(nums)
                .map(Integer::parseInt)
                .filter(x -> x < base)
                .sorted()
                .toArray(Integer[]::new);

        int count = nums.length - list.length;

        int i = 0, j = list.length - 1;
        while (i < j) {
            if (list[i] + list[j] >= base) {
                count++;
                i++;
                j--;
            } else i++;
        }
        System.out.println(count);
    }
}
```
### javascript
```javascript
// 参赛团队的最低能力值为N 最多可以派出的团队数量
// 每个团队可以由一人或者两人组成
// 且一个人只能参加一个团队
// 计算出最多可以派出多少只符合要求的队伍
// 输入描述
// 5
// 3 1 5 7 9
// 8
// 第一行代表总人数，范围  1~500000
// 第二行数组代表每个人的能力
// 数组大小范围 1~500000
// 元素取值范围 1~500000
// 第三行数值为团队要求的最低能力值
// 1~500000

// 输出描述
// 3
// 最多可以派出的团队数量
// 输入
// 5
// 3 1 5 7 9
// 8

// 输出
// 3

// 说明 3、5组成一队   1、7一队  9自己一队  输出3
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let n = 0; // 总人数
let nums = []; // 每个人能力
let base = 0; // 要求的最低能力

rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);

        sign = 1;
    } else if (sign == 1) {

        nums = line.split(' ').map(i => parseInt(i));
        sign = 2;
    } else {
        base = parseInt(line);

        solution(n, nums, base);
        sign = -1;
        n = 0;
        nums = [];
        base = 0;
        // rl.close();
    }
});

function solution(n, nums, base) {
    let arr = nums.filter(i => i < base).sort((a, b) => a - b);
    let count = n - arr.length;

    let i = 0, j = arr.length - 1;
    while(i < j) {
        if (arr[i] + arr[j] >= base) {
            count++;
            i++;
            j--;
        } else {
            i++;
        }
    }
    console.log(count);
}
```
## 题目35 数轴x有两个点的集合 (A(i),B(j))数对
```java
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/13
 * Time: 14:01
 * Description:
 */
public class Main13 {
    public static void main(String[] args) {
        /*
        同一个数轴x有两个点的集合A={A1,A2,...,Am}和B={B1,B2,...,Bm}
        A(i)和B(j)均为正整数
        A、B已经按照从小到大排好序，AB均不为空
        给定一个距离R 正整数，列出同时满足如下条件的
        (A(i),B(j))数对
        1. A(i)<=B(j)
        2. A(i),B(j)之间距离小于等于R
        3. 在满足1，2的情况下每个A(i)只需输出距离最近的B(j)
        4. 输出结果按A(i)从小到大排序

        输入描述
        第一行三个正整数m n R
        第二行m个正整数 表示集合A
        第三行n个正整数 表示集合B

        输入限制 1<=R<=100000
        1<=n,m<=100000
        1<= A(i),B(j) <= 1000000000

        输出描述
        每组数对输出一行 A(i)和B(j)
        以空格隔开

        示例一
        输入
        4 5 5
        1 5 5 10
        1 3 8 8 20

        输出
        1 1
        5 8
        5 8

         */
        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split(" ");
        int R = Integer.parseInt(split[2]);
        List<Integer> A = Arrays.stream(in.nextLine().split(" "))
                .map(Integer::parseInt)
                .collect(Collectors.toList());

        List<Integer> B = Arrays.stream(in.nextLine().split(" "))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
        in.close();

        for (int Ai : A) {
            for (int Bj : B) {
                if (Ai <= Bj && Bj - Ai <= R) {
                    System.out.println(Ai + " " + Bj);
                    break;
                }
            }
        }

    }
}
```
### javascript
```javascript
// 同一个数轴x有两个点的集合A B 给定一个距离R 输出
// (A(i),B(j))数对
// 1. A(i)<=B(j)
// 2. A(i),B(j)之间距离小于等于R
// 3. 在满足1，2的情况下每个A(i)只需输出距离最近的B(j)
// 4. 输出结果按A(i)从小到大排序
// 输入描述
// 第一行三个正整数m n R
// 第二行m个正整数 表示集合A
// 第三行n个正整数 表示集合B
// 输出描述
// 每组数对输出一行 A(i)和B(j)
// 以空格隔开
// 输入
// 4 5 5
// 1 5 5 10
// 1 3 8 8 20

// 输出
// 1 1
// 5 8
// 5 8
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let m = 0, n = 0, R = 0;
let A = [];
let B = [];

rl.on('line', line => {
    if (sign == -1) {
        let tmp = line.split(' ');
        m = tmp[0];
        n = tmp[1];
        R = tmp[2];

        sign = 1;
    } else if (sign == 1) {
        A = line.split(' ').map(i => parseInt(i));
        
        sign = 2;
    } else {
        B = line.split(' ').map(i => parseInt(i));

        solution(A, B, R);
        sign = -1;
        m = n = R = 0;
        A = [];
        B = [];
        // rl.close();
    }

});

function solution(A, B, R) {
    for (let Ai of A) {
        for (let Bj of B) {
            if (Ai <= Bj && Bj - Ai <= R) {
                console.log(Ai + ' ' + Bj);
                break;
            }
        }
    }
}
```
## 题目34 输出众数组成的新数组的中位数
```java
import java.util.*;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/12
 * Time: 17:25
 * Description:
 */
public class Main12 {
    public static void main(String[] args) {
        /*
        1.众数是指一组数据中出现次数多的数
        众数可以是多个
        2.中位数是指把一组数据从小到大排列，最中间的那个数，
        如果这组数据的个数是奇数，那最中间那个就是中位数
        如果这组数据的个数为偶数，那就把中间的两个数之和除以2就是中位数
        3.查找整型数组中元素的众数并组成一个新的数组
        求新数组的中位数

        输入描述
        输入一个一维整型数组，数组大小取值范围   0<n<1000
        数组中每个元素取值范围，  0<e<1000

        输出描述
        输出众数组成的新数组的中位数

        示例一
        输入：
        10 11 21 19 21 17 21 16 21 18 16
        输出
        21

        示例二
        输入
        2 1 5 4 3 3 9 2 7 4 6 2 15 4 2 4
        输出
        3

         示例三
        输入
        5 1 5 3 5 2 5 5 7 6 7 3 7 11 7 55 7 9 98 9 17 9 15 9 9 1 39
        输出
        7
         */
        Scanner in = new Scanner(System.in);
        HashMap<Integer, Integer> map = new HashMap<>();
        Arrays.stream(in.nextLine().split(" "))
                .forEach(x -> {
                    int i = Integer.parseInt(x);
                    map.put(i, map.getOrDefault(i, 0)+1);
                });
        in.close();

        Integer max = map.values().stream().max(Integer::compareTo).get();

        List<Integer> newArr = map.keySet().stream()
                .filter(k -> map.get(k).equals(max))
                .sorted(Integer::compareTo)
                .collect(Collectors.toList());

        Integer res = 0;
        int size = newArr.size();
        if (size % 2 == 0) {
            res = (newArr.get(size / 2) + newArr.get(size / 2 - 1)) / 2;
        } else {
            res = newArr.get(size / 2);
        }

        System.out.println(res);

    }
}
```
### javascript
```javascript
// 输出众数组成的新数组的中位数
// 1.众数是指一组数据中出现次数多的数
// 众数可以是多个
// 2.中位数是指把一组数据从小到大排列，最中间的那个数，
// 如果这组数据的个数是奇数，那最中间那个就是中位数
// 如果这组数据的个数为偶数，那就把中间的两个数之和除以2就是中位数
// 3.查找整型数组中元素的众数并组成一个新的数组
// 求新数组的中位数
// 输入：一维数组
// 输出：众数组成的新数组的中位数
// 输入：
// 10 11 21 19 21 17 21 16 21 18 16
// 输出
// 21
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});


rl.on('line', line => {
    let map = new Map();
    line.split(' ').forEach(i => {
        let key = parseInt(i);
        map.set(key, (map.get(key) || 0) + 1);
    });
    let arr = Array.from(map).sort((a, b) => b[1] - a[1]);
    let max = arr[0][1];
    let keys = arr.filter(i => i[1] === max).map(i => {return i[0]});
    let res = 0;
    let size = keys.length;
    if (size % 2 == 0) {
        res = (keys[parseInt(size / 2)] + keys[parseInt(size / 2) - 1]) / 2;
    } else {
        res = keys[parseInt(size / 2)];
    }
    console.log(res);

    rl.close();
});
```

## 题目33 简易内存池
```java
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.TreeMap;
import java.util.stream.Collectors;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/12
 * Time: 16:21
 * Description:
 */
public class Main11 {
    public static void main(String[] args) {
        /*
          有一个简易内存池，内存按照大小粒度分类
          每个粒度有若干个可用内存资源
          用户会进行一系列内存申请
          需要按需分配内存池中的资源
          返回申请结果成功失败列表
          分配规则如下
          1.分配的内存要大于等于内存的申请量
          存在满足需求的内存就必须分配
          优先分配粒度小的，但内存不能拆分使用
          2.需要按申请顺序分配
          先申请的先分配，有可用内存分配则申请结果为true
          没有可用则返回false
          注释：不考虑内存释放

          输入描述
          输入为两行字符串
          第一行为内存池资源列表
          包含内存粒度数据信息，粒度数据间用逗号分割
          一个粒度信息内用冒号分割
          冒号前为内存粒度大小，冒号后为数量
          资源列表不大于1024
          每个粒度的数量不大于4096

          第二行为申请列表
          申请的内存大小间用逗号分割，申请列表不大于100000

          如
          64:2,128:1,32:4,1:128
          50,36,64,128,127

          输出描述
          输出为内存池分配结果
          如true,true,true,false,false

          示例一：
          输入：
          64:2,128:1,32:4,1:128
          50,36,64,128,127
          输出：
          true,true,true,false,false

          说明:
          内存池资源包含：64k共2个、128k共1个、32k共4个、1k共128个的内存资源
          针对50,36,64,128,127的内存申请序列，
          分配的内存依次是，64,64,128,null,null
          第三次申请内存时已经将128分配出去，因此输出的结果是
          true,true,true,false,false
         */
        Scanner in = new Scanner(System.in);
        TreeMap<Integer, Integer> pool = new TreeMap<>();
        Arrays.stream(in.nextLine().split(","))
                .forEach(x -> {
                    String[] split = x.split(":");
                    pool.put(Integer.parseInt(split[0]),
                            Integer.parseInt(split[1]));
                });
        List<Integer> list = Arrays.stream(in.nextLine().split(","))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
        in.close();

        StringBuilder builder = new StringBuilder();

        for (Integer size : list) {
            boolean flag = false;
            for (Integer k : pool.keySet()) {
                Integer v = pool.get(k);
                if (k >= size && v != 0) {
                    builder.append("true,");
                    pool.put(k, v - 1);
                    flag = true;
                    break;
                }
            }
            if (!flag) builder.append("false,");
        }

        System.out.println(builder.substring(0, builder.length() - 1));
    }
}
```
### javascript
```javascript
// 简易内存池
// 输入描述
//         输入为两行字符串
//         第一行为内存池资源列表
//         包含内存粒度数据信息，粒度数据间用逗号分割
//         一个粒度信息内用冒号分割
//         冒号前为内存粒度大小，冒号后为数量
//         第二行为申请列表
//         如
//           64:2,128:1,32:4,1:128
//           50,36,64,128,127
// 输出描述
//         输出为内存池分配结果
//         如true,true,true,false,false
// 输入：
//     64:2,128:1,32:4,1:128
//     50,36,64,128,127
// 输出：
//     true,true,true,false,false
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;

let pool = new Map();
let list = [];

rl.on('line', line => {
    if (sign == -1) {
        let data = line.split(',');

        for (let i of data) {
            let t = i.split(':');
            let key = parseInt(t[0]);
            let val = parseInt(t[1]);
            pool.set(key, val);
        }

        sign = 1;
    } else {
        list = line.split(',').map(i => parseInt(i));

        solution(pool, list);
        sign = -1;
        pool = new Map();
        list = [];

        // rl.close();
    }
});

function solution(pool, list) {
    let res = [];
    for (let size of list) {
        let flag = false;

        for (let k of pool.keys()) {
            let val = pool.get(k);
            if (k >= size && val != 0) {
                res.push('true');
                pool.set(k, val - 1);
                flag = true;
                break;
            }
        }
        if (!flag) {
            res.push('false');
        }
    }
    console.log(res.join(','));
}
```
## 题目32  求窗口滑动产生的所有窗口和的最大值
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/11
 * Time: 17:36
 * Description:
 */
public class Main10 {
    public static void main(String[] args) {
        /*
        有一个N个整数的数组
        和一个长度为M的窗口
        窗口从数组内的第一个数开始滑动
        直到窗口不能滑动为止
        每次滑动产生一个窗口  和窗口内所有数的和
        求窗口滑动产生的所有窗口和的最大值

        输入描述
        第一行输入一个正整数N
        表示整数个数  0<N<100000
        第二行输入N个整数
        整数取值范围   [-100,100]
        第三行输入正整数M
        M代表窗口的大小
        M<=100000 并<=N

        输出描述
        窗口滑动产生所有窗口和的最大值

        示例一
        输入
        6
        12 10 20 30 15 23
        3

        输出
        68
         */
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        String[] strs = in.nextLine().split(" ");
        int m = Integer.parseInt(in.nextLine());
        in.close();

        int[] ints = new int[n];
        for (int i = 0; i < n; i++) {
            ints[i] = Integer.parseInt(strs[i]);
        }

        int res = Integer.MIN_VALUE;
        for (int i = 0; i < n - m + 1; i++) {
            int sum = 0;
            for (int j = i; j < i + m; j++) {
                sum += ints[j];
            }
            if (sum > res) res = sum;
        }
        System.out.println(res);
    }
}
```
### javascript
```javascript
// 滑动窗口内 所有数的和的最大值
// N个整数的数组 和长度为M的窗口
// 输入： 第一行 N
// 第二行 数组
// 第三行 M 窗口大小
// 输出：窗口内 和的最大值
// 输入
// 6
// 12 10 20 30 15 23
// 3
// 输出
// 68

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let N = 0;
let arr = [];
let M = 0;

rl.on('line', line => {
    if (sign == -1) {
        N = parseInt(line);
        
        sign = 1;
    } else if (sign == 1) {
        arr = line.split(' ').map(i => parseInt(i));

        sign = 2;
    } else {
        M = parseInt(line);

        solution(N, arr, M);
        sign = -1;
        N = 0;
        M = 0;
        arr = [];
        // rl.close();
    }
});

function solution(N, arr, M) {
    let res = -Infinity;
    for (let i = 0; i < N - M + 1; i++) {
        let sum = 0;
        for (let j = i; j < i + M; j++) {
            sum += arr[j];
        }
        if (sum > res) {
            res = sum;
        }
    }
    console.log(res);
}
```
## 题目31 整数有几种连续自然数之和的表达式
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/11
 * Time: 15:38
 * Description:
 */
public class Main8 {
    public static void main(String[] args) {
        /*
        一个整数可以由连续的自然数之和来表示
        给定一个整数
        计算该整数有几种连续自然数之和的表达式
        并打印出每一种表达式

        输入描述
        一个目标整数t  1<= t <=1000

        输出描述
        1.该整数的所有表达式和表达式的个数
        如果有多种表达式，自然数个数最少的表达式优先输出
        2.每个表达式中按自然数递增输出

        具体的格式参见样例
        在每个测试数据结束时，输出一行"Result:X"
        其中X是最终的表达式个数

        输入
        9

        输出
        9=9
        9=4+5
        9=2+3+4
        Result:3

        说明 整数9有三种表达方法：

        示例二
        输入
        10
        输出
        10=10
        10=1+2+3+4
        Result:2

         */

        Scanner in = new Scanner(System.in);
        int t = Integer.parseInt(in.nextLine());
        System.out.println(t + "=" + t);

        ArrayList<String> res = new ArrayList<>();

        for (int n = 1; n < t; n++) {
            int sum = 0;
            StringBuilder builder = new StringBuilder();
            for (int i = n; sum < t; i++) {
                sum += i;
                builder.append(i).append("+");
                if (sum == t) {
                    res.add(t + "=" + builder.substring(0, builder.length() - 1));
                    break;
                }
            }
        }
        res.sort(Comparator.comparingInt(String::length));
        res.forEach(System.out::println);

        System.out.println("Result:" + (res.size() + 1));
        in.close();
    }
}
```
### javascript
```javascript
// 给定一个整数
// 计算该整数有几种连续自然数之和的表达式
// 并打印出每一种表达式
// 输入: 一个目标整数t;
// 输出：所有表达式和表达式的个数
// 按自然数递增输出
// 输入
// 9

// 输出
// 9=9
// 9=4+5
// 9=2+3+4
// Result:3

// 说明 整数9有三种表达方法：
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    
    let T = parseInt(line);
    console.log(T + '=' + T);

    solution(T);
    // rl.close();
});

function solution(T) {
    let res = [];
    for (let n = 1; n < T; n++) {
        let sum = 0;
        let sub = [];
        for (let i = n; sum < T; i++) {
            sum += i;
            sub.push(i);
            if (sum == T) {
                res.push(`${T}=${sub.join('+')}`);
                break;
            }
        }
    }
    res.sort((a, b) => a.length - b.length);
    res.forEach(el => {
        console.log(el);
    });
    console.log(`Result:${res.length + 1}`);

}
```
## 题目30 计算二维矩阵的最大值
```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/11
 * Time: 14:59
 * Description:
 */
public class Main7 {
    public static void main(String[] args) {
        /*
        给定一个仅包含0和1的n*n二维矩阵
        请计算二维矩阵的最大值
        计算规则如下
        1、每行元素按下标顺序组成一个二进制数(下标越大约排在低位)，
        二进制数的值就是该行的值，矩阵各行之和为矩阵的值
        2、允许通过向左或向右整体循环移动每个元素来改变元素在行中的位置
        比如
        [1,0,1,1,1]   向右整体循环移动两位  [1,1,1,0,1]
        二进制数为11101 值为29
        [1,0,1,1,1]   向左整体循环移动两位  [1,1,1,1,0]
        二进制数为11110 值为30

        输入描述
        1.数据的第一行为正整数，记录了N的大小
        0<N<=20
        2.输入的第2到n+1行为二维矩阵信息
        行内元素边角逗号分割

        输出描述
        矩阵的最大值

        示例1

        输入
        5
        1,0,0,0,1
        0,0,0,1,1
        0,1,0,1,0
        1,0,0,1,1
        1,0,1,0,1

        输出
        122

        说明第一行向右整体循环移动一位，得到最大值  11000  24

        因此最大122
         */
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        int res = 0;
        for (int i = 0; i < n; i++) {
            LinkedList<Integer> ints = new LinkedList<>();
            Arrays.stream(in.nextLine().split(","))
                    .forEach(x -> ints.add(Integer.parseInt(x)));
            int max = Integer.MIN_VALUE;
            for (int j = 0; j < n; j++) {
                ints.addLast(ints.remove(0));

                String binInt = ints.toString()
                        .replaceAll("\\W+", "");

                int sum = Integer.parseInt(binInt, 2);
                if (sum > max) max = sum;
            }
            res += max;
        }
        System.out.println(res);
        in.close();
    }
}
```
### javascript
```javascript
// 给定一个仅包含0和1的n*n二维矩阵
//         请计算二维矩阵的最大值
// 二进制数的值就是该行的值，矩阵各行之和为矩阵的值
// 每行元素按下标顺序组成一个二进制数
// 输入：
// 数据的第一行为正整数，记录了N的大小
// 输入的第2到n+1行为二维矩阵信息
// 输出：矩阵的最大值
// 输入
//     5
//     1,0,0,0,1
//     0,0,0,1,1
//     0,1,0,1,0
//     1,0,0,1,1
//     1,0,1,0,1

// 输出
// 122
// 说明第一行向右整体循环移动一位，得到最大值

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let n = 0;
let matrix = [];

rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);

        sign = 1;
    } else {

        let rows = line.split(',').map(i => parseInt(i));
        matrix.push(rows);

        if (matrix.length == n) {
            solution(matrix, n);

            rl.close();
        }
    }
});

function solution(matrix, n) {
    let res = 0;
    for (let i = 0; i < n; i++) {
        let rows = matrix[i];
        let max = -Infinity;
        for (let j = 0; j < n; j++) {
            rows.push(rows.shift());
            let bin = rows.join('').replace(/\W+/g, '');
            let sum = parseInt(bin, 2);
            if (sum > max) {
                max = sum;
            }
        }
        res += max;
    }
    
    console.log(res);
}
```
## 题目29 单词联想
```java
package com.xahj.bd2006;

import java.util.Arrays;
import java.util.Scanner;
import java.util.TreeSet;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/8
 * Time: 17:53
 * Description:
 */
public class Main6 {
    public static void main(String[] args) {
        /*
         主管期望你来实现英文输入法单词联想功能
         需求如下
         依据用户输入的单词前缀
         从已输入的英文语句中联想出用户想输入的单词
         按字典序输出联想到的单词序列
         如果联想不到
         请输出用户输入的单词前缀
         注意
         英文单词联想时区分大小写
         缩略形式如
         "don't" 判定为两个单词 "don"和 "t"
         输出的单词序列不能有重复单词
         且只能是英文单词，不能有标点符号

         输入描述
         输入两行
         首行输入一段由英文单词word和标点构成的语句str

         接下来一行为一个英文单词前缀pre
         0 < word.length() <= 20
         0 < str.length <= 10000
         0 < pre <=20

         输出描述
         输出符合要求的单词序列或单词前缀
         存在多个时，单词之间以单个空格分割

         示例一
         输入
           I love you
           He

          输出
            He

          说明
            用户已输入单词语句"I love you",
            中提炼出"I","love","you"三个单词
            接下来用户输入"He" ，
            从已经输入信息中无法联想到符合要求的单词
            所以输出用户输入的单词前缀

            示例二
            输入
            The furthest distance in the world,Is not between life and death,But when I stand in front or you,Yet you don't know that I love you.
            f

            输出
            front furthest

         */

        Scanner in = new Scanner(System.in);
        String[] str = in.nextLine().split("\\W+");
        String pre = in.nextLine();
        in.close();

        TreeSet<String> words = new TreeSet<>(Arrays.asList(str));

        StringBuilder buffer = new StringBuilder();

        for (String word : words) {
            if (word.startsWith(pre)) {
                buffer.append(word).append(" ");
            }
        }
        if (buffer.length() == 0) buffer.append(pre);

        System.out.println(buffer.toString().trim());

    }
}
```
### javascript
```javascript 
// 单词联想
// 依据用户输入的单词前缀
// 从已输入的英文语句中联想出用户想输入的单词
// 按字典序输出联想到的单词序列
// 如果联想不到
// 请输出用户输入的单词前缀 
// 区分大小写
// 输入：
// 输入两行
//          首行输入一段由英文单词word和标点构成的语句str
//          接下来一行为一个英文单词前缀pre
// 输出
// 输出符合要求的单词序列或单词前缀
// 存在多个时，单词之间以单个空格分割
// 输入
//     I love you
//     He

//     输出
// He
// 输入
// The furthest distance in the world,Is not between life and death,But when I stand in front or you,Yet you don't know that I love you.
// f

// 输出
// front furthest

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let words;
let pre = '';

rl.on('line', line => {
    if (sign == -1) {
        words = new Set(line.split(/\W+/));

        sign = 1;
    } else {
        pre = line;

        solution(words, pre);
        sign = -1;
        words = null;
        pre = '';
        // rl.close();
    }
});

function solution(words, pre) {
    let res = [];
    for (let word of words) {
        if (word.startsWith(pre)) {
            res.push(word);
        }
    }
    if (res.length == 0) {
        res.push(pre);
    }
    console.log(res.join(" "));
}
```

## 题目27 按照数组元素十进制最低位从小到大进行排序
```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/3
 * Time: 15:56
 * Description:
 */
public class Main4 {
    public static void main(String[] args) {
        /*
         给定一个非空数组(列表)
         起元素数据类型为整型
         请按照数组元素十进制最低位从小到大进行排序
         十进制最低位相同的元素，相对位置保持不变
         当数组元素为负值时，十进制最低为等同于去除符号位后对应十进制值最低位

         输入描述
         给定一个非空数组(列表)
         其元素数据类型为32位有符号整数
        数组长度为[1,1000]
        输出排序后的数组

        输入
        1,2,5,-21,22,11,55,-101,42,8,7,32
        输出
        1,-21,11,-101,2,22,42,32,5,55,7,8

         */

        Scanner in = new Scanner(System.in);
        String[] nums = in.nextLine().split(",");
        in.close();

        ArrayList<Integer> list = new ArrayList<>();
        for (String num : nums) {
            list.add(Integer.parseInt(num));
        }
        list.sort(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return getKey(o1) - getKey(o2);
            }

            public Integer getKey(int i) {
                i = i > 0 ? i : -i;
                return i % 10;
            }
        });

        String listStr = list.toString();
        String res = listStr.substring(1, listStr.length() - 1)
                .replaceAll(" ", "");

        System.out.println(res);
    }
}
```

## 题目26 黑板报
```java
import java.util.*;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/3/3
 * Time: 14:38
 * Description:
 */
public class Main3 {
    public static void main(String[] args) {
        /*
        疫情过后希望小学终于又重新开学了
        3年2班开学第一天的任务是
        将后面的黑板报重新制作
        黑板上已经写上了N个正整数
        同学们需要给这每个数分别上一种颜色
        为了让黑板报既美观又有学习意义
        老师要求同种颜色的所有数都可以被这个颜色中最小的那个数整除
        现在帮小朋友们算算最少需要多少种颜色，给这N个数进行上色

        输入描述
            第一行有一个正整数N
            其中 1 <= n <=100
            第二行有N个int型数，保证输入数据在[1,100]范围中
            表示黑板上各个正整数的值

         输出描述
         输出只有一个整数，为最少需要的颜色种数

            输入
            3
            2 4 6
            输出
            1
            说明：
            所有数都能被2整除

            输入
            4
            2 3 4 9
            输出
            2
            说明：
            2与4涂一种颜色，4能被2整除
            3与9涂另一种颜色，9能被3整除
            不能涂同一种颜色

         */

        Scanner in = new Scanner(System.in);
        String nStr = in.nextLine();
        String[] nums = in.nextLine().split(" ");
        in.close();

        TreeSet<Integer> ints = new TreeSet<>();
        for (String num : nums) {
            ints.add(Integer.parseInt(num));
        }

        if (ints.contains(1)) {
            System.out.println(1);
            ints.remove(1);
            return;
        }

        ArrayList<Integer> intList = new ArrayList<>(ints);
        for (int i = 0; i < intList.size(); i++) {
            Integer cur = intList.get(i);
            for (int j = i + 1; j < intList.size(); ) {
                if (intList.get(j) % cur == 0) {
                    intList.remove(j);
                } else j++;
            }
        }
        System.out.println(intList.size());
    }
}
```
### javascript
```javascript
// 黑板报
// 要求同种颜色的所有数都可以被这个颜色中最小的那个数整除
// 现在帮小朋友们算算最少需要多少种颜色，给这N个数进行上色
// 输入 第一行有一个正整数N 第二行有N个int型数
// 输出描述
        //  输出只有一个整数，为最少需要的颜色种数
// 输入
//     3
//     2 4 6
//     输出
//     1
//     说明：
//     所有数都能被2整除

//     输入
//     4
//     2 3 4 9
//     输出
//     2
//     说明：
//     2与4涂一种颜色，4能被2整除
//     3与9涂另一种颜色，9能被3整除
//     不能涂同一种颜色
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let nums = [];

rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);

        sign = 1;
    } else if(sign == 1) {
        let input = line.split(' ').map(i => parseInt(i));
        input.sort((a, b) => a - b);
        let set = new Set(input);
        if (set.has(1)) {
            console.log(1);
            return;
        }

        let arr = Array.from(set);
        for (let i = 0; i < arr.length; i++) {
            let curr = arr[i];
            for (let j = i + 1; j < arr.length; ) {
                if (arr[j] % curr == 0) {
                    arr.splice(j, 1);
                } else {
                    j++;
                }
            }
        }
        console.log(arr.length);
    }
});
```
## 题目25
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/2/25
 * Time: 16:43
 * Description:
 */
public class Main2 {
    public static void main(String[] args) {
        /**
         * 输入
         * -1 -3 7 5 11 15
         * 输出
         * -3 5 2
         */

        Scanner sc = new Scanner(System.in);
        String[] numStr = sc.nextLine().split(" ");
        sc.close();
        int[] ints = new int[numStr.length];
        for (int i = 0; i < ints.length; i++) {
            ints[i] = Integer.parseInt(numStr[i]);
        }

        int a = 0, b = 0;
        int min = Integer.MAX_VALUE;

        for (int i = 0; i < ints.length; i++) {
            for (int j = 0; j < ints.length; j++) {
                int sum = ints[i] + ints[j];
                sum = sum > 0 ? sum : -sum;
                if (i != j && sum < min) {
                    a = ints[i];
                    b = ints[j];
                    min = sum;
                }
            }
        }
        System.out.println(a + " " + b + " " + min);
    }
}
```
### javascript
```javascript
let arr = line.split(' ').map(i => parseInt(i));
let a = 0, b = 0;
let min = Infinity;
for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
        let sum = arr[i] + arr[j];
        sum = sum > 0 ? sum : -sum;
        if (i != j && sum < min) {
            a = arr[i];
            b = arr[j];
            min = sum;
        }
    }
}
console.log(a + ' ' + b + ' '+ min)
```

## 题目22
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/12/16
 * Time: 1:30
 * Description:
 */
public class Demo22 {
    public static void main(String[] args) {
        /*
        现在有多组整数数组
        需要将他们合并成一个新的数组
        合并规则从每个数组里按顺序取出固定长度的内容
        合并到新的数组
        取完的内容会删除掉
        如果改行不足固定长度，或者已经为空
        则直接取出剩余部分的内容放到新的数组中继续下一行

        输入描述
          第一 行每次读取的固定长度
          长度0<len<10
          第二行是整数数组的数目
          数目 0<num<10000
          第3~n行是需要合并的数组
          不同的数组用换行分割
          元素之间用逗号分割
          最大不超过100个元素

         输出描述
          输出一个新的数组，用逗号分割

          示例1
          输入
              3
              2
              2,5,6,7,9,5,7
              1,7,4,3,4
          输出
              2,5,6,1,7,4,7,9,5,3,4,7

          说明  获得长度3和数组数目2
             先遍历第一行 获得2,5,6
             再遍历第二行 获得1,7,4
             再循环回到第一行获得7,9,5
             再遍历第二行获得3,4
             再回到第一行获得7

          示例2
          输入
             4
             3
             1,2,3,4,5,6
             1,2,3
             1,2,3,4
           输出
             1,2,3,4,1,2,3,1,2,3,4,5,6
         */

        Scanner scanner = new Scanner(System.in);
        int len = Integer.parseInt(scanner.nextLine());
        int num = Integer.parseInt(scanner.nextLine());
        ArrayList<ArrayList<String>> list = new ArrayList<>();
        ArrayList<String> res = new ArrayList<>();
        int sum = 0;
        for (int i = 0; i < num; i++) {
            String[] arr = scanner.nextLine().split(",");
            sum += arr.length;
            list.add(new ArrayList<String>(Arrays.asList(arr)));
        }
        while (res.size() != sum) {
            for (ArrayList<String> strList : list) {
                if (strList.size() == 0) continue;
                int times = Math.min(strList.size(), len);
                for (int i = 0; i < times; i++) {
                    res.add(strList.remove(0));
                }
            }
        }
        StringBuilder builder = new StringBuilder();
        for (String str : res) {
            builder.append(str).append(",");
        }
        String resStr = builder.toString();
        System.out.println(resStr.substring(0, resStr.length() - 1));

        scanner.close();
    }
}

```
## 题目21 通信系统 最优策略组合下的总的系统消耗资源数
```java
package com.amoscloud.newcoder.easy;

import java.util.ArrayList;
import java.util.Scanner;
import java.util.TreeMap;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2021/5/8
 * Time: 16:21
 * Description: 85%
 */
public class Main46 {
    /*
        在通信系统中有一个常见的问题是对用户进行不同策略的调度
        会得到不同系统消耗的性能
        假设由N个待串行用户，每个用户可以使用A/B/C三种不同的调度策略
        不同的策略会消耗不同的系统资源
        请你根据如下规则进行用户调度
        并返回总的消耗资源数
        规则是：相邻的用户不能使用相同的调度策略
        例如：
        第一个用户使用A策略 则第二个用户只能使用B和C策略
        对单的用户而言，不同的调度策略对系统资源的消耗可以规划后抽象为数值
        例如
        某用户分别使用ABC策略的系统消耗，分别为15 8 17
        每个用户依次选择当前所能选择的对系统资源消耗最少的策略,局部最优
        如果有多个满足要求的策略，选最后一个

        输入描述：
            第一行表示用户个数N
            接下来表示每一行表示一个用户分别使用三个策略的资源消耗
            resA resB resC

        输出描述：
            最优策略组合下的总的系统消耗资源数

         示例一：
          输入：
              3
              15 8 17
              12 20 9
              11 7 5
          输出：
              24
           说明:
            1号用户使用B策略
            2号用户使用C策略
            3号用户使用B策略
           系统资源消耗8+9+7

     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        ArrayList<TreeMap<Integer, Integer>> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            String[] split = in.nextLine().split(" ");
            TreeMap<Integer, Integer> map = new TreeMap<>();
            for (int j = 0; j < split.length; j++) {
                map.put(Integer.parseInt(split[j]), j);
            }
            res.add(map);
        }
        in.close();

        Integer res1 = new ArrayList<>(res.get(0).keySet()).get(0);
        int sum = res1;
        Integer type = res.get(0).get(res1);

        if (res.size() > 1) {
            for (int i = 1; i < res.size(); i++) {
                ArrayList<Integer> keyList = new ArrayList<>(res.get(i).keySet());
                Integer resN = keyList.get(0);
                Integer typeN = res.get(i).get(resN);
                if (!typeN.equals(type)) {
                    sum += resN;
                    type = typeN;
                } else {
                    sum += keyList.get(1);
                    type = res.get(i).get(keyList.get(1));
                }

            }
        }

        System.out.println(sum);
    }
}
```
### javascript
```javascript
// 通信系统策略
const { resolveNaptr } = require('dns');
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let sign = -1;
let matrix = [];
let n = 0;


rl.on('line', line => {
    if (sign == -1) {
        n = parseInt(line);

        sign = 1;
    } else {

        let map = new Map();
        let rows = line.split(' ').map(i => parseInt(i));
        map.set('resA', rows[0]);
        map.set('resB', rows[1]);
        map.set('resC', rows[2]);
        let arr = Array.from(map).sort((a, b) => a[1] - b[1]);
        matrix.push(arr);

        if (matrix.length == n) {
            solution(matrix);

            sign = -1;
            matrix = [];
            // rl.close();
        }
    }
});

function solution(matrix) {
    let first = matrix[0][0];
    let type = first[0];
    let sum = first[1];
    for (let i = 1; i < matrix.length; i++) {
        let rows = matrix[i];
        let curKey = rows[0][0]; // 已排序 // 优先取第1小的,
        let curVal = rows[0][1];
        if (type != curKey) {
            sum += curVal;
            type = curKey;
        } else {
            sum += rows[1][1]; // 位置已占 取第2小的
            type = rows[1][0]; 
        }
    }
    console.log(sum);
}
```
## 题目20 最小步骤数
```java
import java.util.Scanner;
import java.util.TreeSet;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/12/14
 * Time: 17:57
 * Description:
 */
public class Demo20 {

    private static int[] ints = null;
    private static int step = 0;

    public static void main(String[] args) {
        /*

        一个正整数数组 设为nums
        最大为100个成员
        求从第一个成员开始正好走到数组最后一个成员所使用的最小步骤数
                    3 5 9 4 2 6 8 3 5 4 3 9
        要求：
        1. 第一步 必须从第一元素起  且 1<=第一步步长<len/2  (len为数组长度)
        2. 从第二步开始只能以所在成员的数字走相应的步数，不能多不能少，
         如果目标不可达返回-1
         只输出最小的步骤数量
        3. 只能向数组的尾部走不能向回走

        输入描述：
        有正整数数组 空格分割
        数组长度<100

        输出描述 ：
         正整数  最小步数
         不存在输出-1

         例子：
         输入
             7 5 9 4 2 6 8 3 5 4 3 9
         输出
            2
         第一个可选步长选择2
         从第一个成员7开始走两步到9
         第二步：从9经过9个成员到最后

         例子：
         输入
          1 2 3 7 1 5 9 3 2 1
         输出
          -1
         */

        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split(" ");
        in.close();
        ints = new int[split.length];
        for (int i = 0; i < split.length; i++) {
            ints[i] = Integer.parseInt(split[i]);
        }
        int len = ints.length;

        TreeSet<Integer> set = new TreeSet<>();

        for (int i = 1; i < len / 2; i++) {
//            if (ints[i] >= len / 2) continue;
            step = 1;
            set.add(in(i, i));
        }
        System.out.println(set);

        if (set.size() != 1) set.pollFirst();

        System.out.println(set.first());
    }

    private static int in(int curPos, int lastPos) {
        int numStep = ints[curPos];
        if (lastPos == ints.length - 1) {
            return step;
        } else if (lastPos < ints.length - 1) {
            step++;
            return in(lastPos, lastPos + numStep);
        } else {
            return -1;
        }
    }
}

```
### javascript
```javascript
// 最小移动步骤
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let step = 1;
let arr = [];
rl.on('line', line => {
    arr = line.split(' ').map(i => parseInt(i));

    solution();
    arr = [];
    // rl.close();
});

function solution() {
    let set = new Set();
    for (let i = 1; i < arr.length / 2; i++) {
        step = 1;
        let next = getStep(i, i);
        set.add(next);
    }
    let res = Array.from(set).sort((a, b) => a - b);
    if (res.length != 1) res.shift();
    let first = res[0];
    console.log(first);
}

function getStep(curr, last) {
    let num = arr[curr];
    if (last == arr.length - 1) {
        return step;
    } else if (last < arr.length - 1) {
        step++;
        return getStep(last, last + num);
    } else {
        return -1;
    }
}
```

## 题目18 喊7
```java
import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/12/5
 * Time: 18:41
 * Description:
 */
public class Demo18 {
    public static void main(String[] args) {
        /*
        喊7 是一个传统的聚会游戏
        N个人围成一圈
        按顺时针从1-7编号
        编号为1的人从1开始喊数
        下一个人喊得数字是上一个人喊得数字+1
        但是当将要喊出数字7的倍数或者含有7的话
        不能喊出 而是要喊过

        假定N个人都没有失误。
        当喊道数字k时
        可以统计每个人喊 “过"的次数

        现给定一个长度n的数组
        存储打乱的每个人喊”过"的次数
        请把它还原成正确顺序

        即数组的第i个元素存储编号i的人喊“过“的次数

           输入为1行
           空格分割的喊过的次数
           注意k并不提供

           k不超过200
           数字个数为n
           输出描述

           输出为1行
           顺序正确的喊过的次数  空格分割

           例子
           输入
             0 1 0
           输出
             1 0 0

           只有一次过
           发生在7
           按顺序编号1的人遇到7  所以100
           结束时的k不一定是7 也可以是 8 9
             喊过都是100

             例子
           输入
             0 0 0 2 1
           输出
             0 2 0 1 0
           一共三次喊过
           发生在7 14 17
           编号为2 的遇到7 17
           编号为4 的遇到14
         */

        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split(" ");
        int sum = 0;
        for (String s : split) {
            sum += Integer.parseInt(s);
        }

        int[] res = new int[split.length];

        int j = 0;
        for (int i = 1; i < 300; i++, j++) {
            if (j == split.length) j = 0;
            if (i % 7 == 0 || (i + "").contains("7")) {
                res[j] += 1;
            }
            int sum1 = 0;
            for (int re : res) sum1 += re;
            if (sum == sum1) break;
        }

        StringBuilder builder = new StringBuilder();
        for (int re : res) builder.append(re).append(" ");

        String s = builder.toString();
        System.out.println(s.substring(0, s.length() - 1));

        in.close();

    }
}

```
### javascript
```javascript
// 喊7
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let nums = line.split(' ').map(i => parseInt(i));
    
    solution(nums);
});

function solution(nums) {
    let sum = nums.reduce((a, b) => a + b);
    let dp = new Array(nums.length).fill(0);
    let j = 0;
    for (let i = 1; i < 300; i++, j++) {
        if (j == nums.length) j = 0;
        if (i % 7 == 0 || (String(i).indexOf('7') != -1)) {
            dp[j] += 1;
        }
        let subSum = dp.reduce((a, b) => a + b);
        if (sum == subSum) break;
    }

    console.log(dp.join(' '));
}
```

## 题目15  航天器
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/30
 * Time: 11:44
 * Description:
 */
public class Demo15 {
    public static void main(String[] args) {
        /*
        给航天器一侧加装长方形和正方形的太阳能板(图中的斜线区域)
        需要先安装两个支柱(图中的黑色竖条)
        再在支柱的中间部分固定太阳能板
        但航天器不同位置的支柱长度不同
        太阳能板的安装面积受限于最短一侧的那支支柱的长度

        现提供一组整型数组的支柱高度数据
        假设每个支柱间的距离相等为一个单位长度
        计算如何选择两根支柱可以使太阳能板的面积最大

        输入描述
        10,9,8,7,6,5,4,3,2,1
        注释，支柱至少有两根，最多10000根，能支持的高度范围1~10^9的整数

        柱子的高度是无序的
        例子中的递减是巧合

        输出描述
        可以支持的最大太阳板面积:(10m高支柱和5m高支柱之间)
        25

        示例1
        输入
        10,9,8,7,6,5,4,3,2,1
        输出
        25
        备注 10米高支柱和5米高支柱之间宽度为5，高度取小的支柱高度也是5
        面积为25
        任取其他两根支柱所能获得的面积都小于25 所以最大面积为25

         */

        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split(",");

        long[] ints = new long[split.length];
        for (int i = 0; i < split.length; i++) {
            ints[i] = Long.parseLong(split[i]);
        }

        long res = 0;
        for (int i = 0; i < split.length; i++) {
            for (int j = i + 1; j < split.length; j++) {
                long area = Math.min(ints[i], ints[j]) * (j - i);
                if (area > res) res = area;
            }
        }

        System.out.println(res);
    }

}

```

## 题目13 从根节点到最小的叶子节点的路径
```java
import java.util.ArrayList;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/18
 * Time: 13:46
 * Description:
 */
public class Demo13 {

    public static void main(String[] args) {
        /*
        二叉树也可以用数组来存储
        给定一个数组
        树的根节点的值储存在下标1
        对于储存在下标n的节点，
        他的左子节点和右子节点分别储存在下标2*n和2*n+1
        并且我们用-1代表一个节点为空
        给定一个数组存储的二叉树
        试求从根节点到最小的叶子节点的路径
        路径由节点的值组成

        输入描述
        输入一行为数组的内容
        数组的每个元素都是正整数，元素间用空格分割
        注意第一个元素即为根节点的值
        即数组的第n元素对应下标n
        下标0在树的表示中没有使用
        所以我们省略了
        输入的树最多为7层

        输出描述
         输出从根节点到最小叶子节点的路径上各个节点的值
         由空格分割
         用例保证最小叶子节点只有一个

         例子
          输入
          3 5 7 -1 -1 2 4
          输出
           3 7 2

          例子
           输入
          5 9 8 -1 -1 7 -1 -1 -1 -1 -1 6
           输出
          5 8 7 6
         */

        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split(" ");
        ArrayList<Integer> tree = new ArrayList<>();
        tree.add(Integer.MAX_VALUE);
        for (String s : split) {
            tree.add(Integer.parseInt(s));
        }

        int min = Integer.MAX_VALUE;

        for (Integer i : tree) {
            if (i != 0 && i != -1 && i < min && !i.equals(tree.get(1))) min = i;
        }

        int index = tree.indexOf(min);
        ArrayList<String> res = new ArrayList<>();
        res.add(tree.get(index) + "");
        for (int i = index; i > 1; ) {
            if (i % 2 == 0) i = i / 2;
            else i = (i - 1) / 2;
            res.add(tree.get(i) + "");
        }
        StringBuilder builder = new StringBuilder();
        for (int i = res.size() - 1; i >= 0; i--) {
            builder.append(res.get(i)).append(" ");
        }

        System.out.println(builder.substring(0, builder.length() - 1));

        in.close();
    }

}
```
### javascript
```javascript
// 从根节点到最小叶子节点的路径上各个节点的值
// 例子
// 输入
// 3 5 7 -1 -1 2 4
// 输出
// 3 7 2

// 例子
// 输入
// 5 9 8 -1 -1 7 -1 -1 -1 -1 -1 6
// 输出
// 5 8 7 6
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let tree = line.split(' ').map(i => parseInt(i));
    tree.unshift(Infinity);

    solution(tree);
    // rl.close();
});

function solution(tree) {
    let min = Infinity;
    for (let i of tree) {
        if (i != 0 && i != -1 && i < min && i != tree[1]) {
            min = i;
        }
    }

    let index = tree.indexOf(min);
    let res = [];
    res.push(tree[index]);
    for (let i = index; i > 1;) {
        if (i % 2 == 0) {
            i = parseInt(i / 2);
        } else {
            i = parseInt((i - 1) / 2);
        }
        res.push(tree[i]);
    }
    let result = res.reverse().join(' ');
    console.log(result);
}
```
## 题目12 矩阵 f,m,m,f
```java
import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/13
 * Time: 17:15
 * Description:
 */
public class Demo12 {
    private static int max = 0;
    private static char[][] chars;

    public static void main(String[] args) {
        /*
        3,4
        f,m,m,f
        f,m,m,f
        f,f,f,m
        3
         */
        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split(",");
        int row = Integer.parseInt(split[0]);
        int col = Integer.parseInt(split[1]);
        chars = new char[row][col];
        for (int i = 0; i < row; i++) {
            char[] split1 = in.nextLine().replaceAll(",", "").toCharArray();
            for (int j = 0; j < col; j++) {
                chars[i][j] = split1[j];
            }
        }

        int i1 = find3(0, 1);
        if (i1 > max) max = i1;

        System.out.println(max);

        in.close();
    }

    private static int find1(int row, int col) {
        if (chars[row][col] != 'm') return 0;
        else return 1 + find1(row, col + 1);
    }

    private static int find2(int row, int col) {
        if (chars[row][col] != 'm') return 0;
        else return 1 + find2(row + 1, col);
    }

    private static int find3(int row, int col) {
        if (chars[row][col] != 'm') return 0;
        else return 1 + find3(row + 1, col + 1);
    }
}

```

## 题目11 有效类型分类
```java
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/13
 * Time: 16:18
 * Description:
 */
public class Demo11 {
    public static void main(String[] args) {
        /*
        对一个数据a进行分类
        分类方法是 此数据a(4个字节大小)的4个字节相加对一个给定值b取模
        如果得到的结果小于一个给定的值c则数据a为有效类型
        其类型为取模的值
        如果得到的结果大于或者等于c则数据a为无效类型

        比如一个数据a=0x01010101 b=3
        按照分类方法计算  (0x01+0x01+0x01+0x01)%3=1
        所以如果c等于2 则此a就是有效类型  其类型为1
        如果c等于1 则此a是无效类型

         又比如一个数据a=0x01010103 b=3
         按分类方法计算(0x01+0x01+0x01+0x03)%3=0
         所以如果c=2则此a就是有效类型  其类型为0
         如果c等于0 则此a是无效类型

         输入12个数据
         第一个数据为c  第二个数据为b
         剩余10个数据为需要分类的数据

         请找到有效类型中包含数据最多的类型
         并输出该类型含有多少个数据

         输入描述
         输入12个数据用空格分割
         第一个数据为c  第二个数据为b
         剩余10个数据为需要分类的数据

         输出描述
         请找到有效类型中包含数据最多的类型
         并输出该类型含有多少个数据

         实例：
           输入
             3 4 256 257 258 259 260 261 262 263 264 265
           输出
             3
           说明
           这10个数据4个字节相加后的结果分别是
            1 2 3 4 5 6 7 8 9 10
            故对4取模的结果为
            1 2 3 0 1 2 3 0 1 2
            c是3所以012都是有效类型
            类型为1和2的有3个数据
            类型为0和3的只有两个

         例子2
         输入
         1 4 256 257 258 259 260 261 262 263 264 265
         输出
         2
         */

        Scanner in = new Scanner(System.in);
        String[] split = in.nextLine().split("\\s+");
        int c = Integer.parseInt(split[0]);
        int b = Integer.parseInt(split[1]);
        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 2; i < split.length; i++) {
            int r = intByteSum(Integer.parseInt(split[i])) % b;
            if (r < c) map.put(r, map.containsKey(r) ? map.get(r) + 1 : 1);
        }

        int max = 0;
        for (Integer value : map.values()) {
            if (value > max) max = value;
        }
        System.out.println(max);

        in.close();

    }

    private static int intByteSum(int x) {
        int sum = 0;
        for (int i = 0; i < 4; i++) {
            sum += (byte) (x >> (i * 8));
        }
        return sum;
    }
}

```
### javascript
```javascript
// 对有效整型分类
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', line => {
    let arr = line.split(/\s+/).map(i => parseInt(i));

    solution(arr);
    
});

function solution(arr) {
    let c = arr[0];
    let b = arr[1];

    let map = new Map();
    for (let i = 2; i < arr.length; i++) {
        let r = convertByteSum(arr[i]) % b;
        if (r < c) {
            let val = map.has(r) ? map.get(r) +1 : 1;
            map.set(r, val);
        }
    }
    let max = 0;
    for (let value of map.values()) {
        if (value > max) max = value;
    }
    console.log(max);
}

function convertByteSum(x) {
    var bytes = new Array(); 
    for (i = 0; i < 4; i++) { 
        bytes[i] = (x >> (8*i)); 
    }
    let sum = bytes.reduce((a, b) => a + b);
    return sum;
}
```


## 题目5 顽猴想要从山脚爬到山顶
```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/2
 * Time: 16:41
 * Description:
 */
public class Demo5 {
    public static void main(String[] args) {
        /*
         一天一只顽猴想要从山脚爬到山顶
          途中经过一个有n个台阶的阶梯，但是这个猴子有个习惯，每一次只跳1步或3步
          试问？猴子通过这个阶梯有多少种不同的跳跃方式

          输入描述：
            输入只有一个这个数n    0<n<50
            此阶梯有多个台阶
          输出描述：
            有多少种跳跃方式

          实例:
           输入
             50
           输出
              122106097

           输入
              3
           输出
              2
         */

        Scanner in = new Scanner(System.in);
        int n = in.nextInt();

        int f1 = 1;
        int f2 = 1;
        int f3 = 2;
        int f4 = n == 1 || n == 2 ? 1 : 2;
        for (int i = 4; i <= n; i++) {
            f4 = f3 + f1;
            f1 = f2;
            f2 = f3;
            f3 = f4;
        }

        System.out.println(f4);

        in.close();

    }
}

```

## 题目4 TLV编码
```java
import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/2
 * Time: 16:05
 * Description:
 */
public class Demo4 {
    public static void main(String[] args) {

        /*
        TLV编码是按 Tag Length  Value格式进行编码的
        一段码流中的信元用tag标识，tag在码流中唯一不重复
        length表示信元value的长度  value表示信元的值
        码流以某信元的tag开头 ，tag固定占一个字节
        length固定占两个字节，字节序为小端序
        现给定tlv格式编码的码流以及需要解码的信元tag
        请输出该信元的value

        输入码流的16机制字符中，不包括小写字母
        且要求输出的16进制字符串中也不要包含字符字母
        码流字符串的最大长度不超过50000个字节

        输入描述
           第一行为第一个字符串 ，表示待解码信元的tag
           输入第二行为一个字符串， 表示待解码的16进制码流
          字节之间用空格分割
        输出描述
           输出一个字符串，表示待解码信元以16进制表示的value

           例子：
           输入：
            31
            32 01 00 AE 90 02 00 01 02 30 03 00 AB 32 31 31 02 00 32 33 33 01 00 CC

           输出
            32 33

           说明：
           需要解析的信源的tag是31
           从码流的起始处开始匹配，tag为32的信元长度为1(01 00,小端序表示为1)
           第二个信元的tag为90 其长度为2
           第三个信元的tag为30 其长度为3
           第四个信元的tag为31 其长度为2(02 00)
           所以返回长度后面的两个字节即可 为 32 33
         */

        Scanner in = new Scanner(System.in);
        String tag = in.nextLine();
        String data = in.nextLine();
        String[] split = data.split("\\s+");

        for (int i = 0; i < split.length; ) {
            int length = Integer.parseInt(split[i + 2] + split[i + 1], 16);
            if (tag.equals(split[i])) {
                StringBuilder builder = new StringBuilder();
                for (int j = i + 3; j < i + 3 + length; j++) {
                    builder.append(split[j]).append(" ");
                }
                System.out.println(builder.toString());
                break;
            } else {
                i += length + 3;
            }
        }

        in.close();

    }
}
```

## 题目3 求方阵里所有数的和

给出n阶方阵里所有数 求方阵里所有数的和

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/2
 * Time: 15:44
 * Description:
 */
public class Demo3 {
    public static void main(String[] args) {
        /*
        给出n阶方阵里所有数
        求方阵里所有数的和
        输入描述：
          输入有多个测试用例
          每个测试用例第一个第一个整数n   n<=1000 表示方阵阶数为n
          接下来是n行的数字，每行n个数字用空格隔开
        输出描述：
          输出一个整数表示n阶方阵的和
        例子：
          输入
              3
              1 2 3
              2 1 3
              3 2 1
          输出
              18
         */

        Scanner in = new Scanner(System.in);
        int sum = 0;
        int n = Integer.parseInt(in.nextLine());
        for (int i = 0; i < n; i++) {
            String[] split = in.nextLine().split("\\s+");
            for (int j = 0; j < n; j++) {
                sum += Integer.parseInt(split[j]);
            }
        }
        System.out.println(sum);

        in.close();
    }
}

```

## 题目2 计算和的最小值

取出k对元素，并对取出的所有元素求和，计算和的最小值

```java
import java.util.*;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/2
 * Time: 14:52
 * Description:
 */
public class Demo2 {
    public static void main(String[] args) {
//        给定两个整数数组
        //array1 array2  数组元素按升序排列
        // 假设从arr1 arr2中分别取出一个元素，可构成一对元素
        // 现在需要取出k对元素，并对取出的所有元素求和
        // 计算和的最小值
        // 注意：两对元素对应arr1 arr2的下标是相同的
        //       视为同一对元素

        //输入描述
        //    输入两行数组arr1 arr2
        //    每行首个数字为数组大小size   0<size<=100
        //    arr1，2中的每个元素   0< <1000
        //    接下来一行  正整数k   0<k<=arr1.size * arr2.size
        // 输出描述
        //   满足要求的最小值

        // 例子

        //输入
        //   3 1 1 2
        //   3 1 2 3
        //   2

        //输出
        //   4

        //说明：用例中需要取两个元素，
        // 取第一个数组第0个元素与第二个数组第0个元素组成一个元素
        // [1,1]
        //取第一个数组第1个元素与第二个数组第0个元素组成一个元素
        // [1,1]

        //求和为1+1+1+1=4 ,满足要求最小

        Scanner in = new Scanner(System.in);

        int[] arr1 = getArray(in.nextLine());
        int[] arr2 = getArray(in.nextLine());
        int k = in.nextInt();

        int sum = 0;

        ArrayList<Integer> list = new ArrayList<>();

        for (int i : arr1) {
            for (int j : arr2) {
                list.add(i + j);
            }
        }

        Integer[] res = new Integer[list.size()];
        list.toArray(res);
        Arrays.sort(res);

        for (int i = 0; i < k; i++) {
            sum += res[i];
        }
        System.out.println(sum);

        in.close();

    }

    private static int[] getArray(String line1) {
        String[] split1 = line1.split("\\s+");
        int[] arr1 = new int[Integer.parseInt(split1[0])];

        for (int i = 1; i < split1.length; i++) {
            arr1[i - 1] = Integer.parseInt(split1[i]);
        }

        return arr1;
    }
}


```

## 题目1 勾股数元组

如果三个正整数A B C ,A²+B²=C²则为勾股数
如果ABC之间两两互质，即A与B A与C B与C均互质没有公约数，
则称其为勾股数元组。
请求出给定n m 范围内所有的勾股数元组

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * Author: Amos
 * E-mail: amos@amoscloud.com
 * Date: 2020/11/2
 * Time: 13:55
 * Description:
 */
public class Demo1 {
    public static void main(String[] args) {
//        如果三个正整数A B C ,A²+B²=C²则为勾股数
        // 如果ABC之间两两互质，即A与B A与C B与C均互质没有公约数，
        // 则称其为勾股数元组。
//        请求出给定n m 范围内所有的勾股数元组
//        输入描述
//          起始范围 1<n<10000    n<m<10000
//        输出目描述
//           abc 保证a<b<c输出格式  a b c
//           多组勾股数元组 按照a升序b升序 c升序的排序方式输出。
//           给定范围内，找不到勾股数元组时，输出  Na

        // 案例
        //  输入
        //   1
        //   20
        //  输出
        //   3 4 5
        //   5 12 13
        //   8 15 17

        //  输入
        //    5
        //    10
        //  输出
        //    Na
        Scanner in = new Scanner(System.in);
        int count = 0;

        try {
            int n = in.nextInt();
            int m = in.nextInt();

            for (int i = n; i < m; i++) {
                for (int j = n + 1; j < m; j++) {
                    for (int k = n + 2; k < m; k++) {
                        if (i < j && j < k
                                && k * k == i * i + j * j
                                && huzhi(i, j) == 1
                                && huzhi(j, k) == 1
                                && huzhi(i, k) == 1) {
                            System.out.println(i + " " + j + " " + k);
                            count++;
                        }
                    }
                }
            }

        } catch (Exception e) {

        } finally {
            if (count == 0)
                System.out.println("Na");
            in.close();
        }

    }

    private static int huzhi(int a, int b) {
        if (a == 0 || b == 0)
            return 1;
        if (a % b == 0)
            return b;
        else
            return huzhi(b, a % b);
    }
}
```

