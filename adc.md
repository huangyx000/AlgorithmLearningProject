### 题目

给定两个数组，编写一个函数两计算他们的交集。

示例：
给定数组：num1 = [1, 2, 3]; num2 = [2, 3, 4, 5, 6]；
输入：[2, 3]
如果给定的数组已经排好序呢？你将如何优化你的算法？

### 解法-1 (无序数组) 
#### 思路

1：为两个数组分别建立 map，用来存储 num -> count 的键值对，统计每个数字出现的数量；

2：对其中一个 map 进行遍历，查看这个数字在两个数组中分别出现的数量，取出现的最小的那个数量 push 到结果数组中即可。

###### 存储 num -> count 的键值对映射
```
function calculateNumCount(arr) {
  let map = new Map();
  for (let index = 0; index < arr.length; index++) {
    const curNum = arr[index];
    let count = map.get(curNum);
    // map中已存在则计数 +1
    if (count) map.set(curNum, ++count);
    // 否则设置新key
    else map.set(curNum, 1);
  }
  return map;
}
```
###### 计算交集
```
function getIntersection_3(arr1, arr2) {
  const map1 = calculateNumCount(arr1);
  const map2 = calculateNumCount(arr2);
  let result = [];
  for (const num of map1.keys()) {
    const count1 = map1.get(num);
    const count2 = map2.get(num);
    if (count1 && count2) {
      const pushCount = Math.min(count1, count2);
      for (let i = 0; i < pushCount; i++) {
        result.push(num)
      }
    }
  }
  return result;
}

```

### 解法-2(有序数组) 
#### 思路

对于排序好的数组，双指针更优
```
function getIntersection_2(arr1, arr2) {
  let arrPos1 = 0, arrPos2 = 0;
  let result = [];
  // 数组指针未达边界时持续推进
  while (arrPos1 < arr1.length && arrPos2 < arr2.length) {
    if (arr1[arrPos1] < arr2[arrPos2]) arrPos1++;
    if (arr1[arrPos1] > arr2[arrPos2]) arrPos2++;
    else {
      result.push(arr1[arrPos1]);
      // 推进指针
      arrPos1++;
      arrPos2++;
    }
  }
  return result;
}
```

PS：两个数组数量差距很大。优先遍历容量小的数组，提前结束循环。
