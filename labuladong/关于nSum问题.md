#   关于nSum问题

> 首先考虑到nSum问题，毫无疑问用双指针，这点毋庸置疑。不然面试官都会觉得你的算法复杂性差。



##  2Sum问题

```javascript
如果假设输入一个数组 nums 和一个目标和 target，请你返回 nums 中能够凑出 target 的两个元素的值，比如输入 nums = [5,3,1,6], target = 9，那么算法返回两个元素 [3,6]。可以假设只有且仅有一对儿元素可以凑出 target。
```

对于以上问题，双指针显然是一种很好的思路，当`sum>target`时，肯定右边的`hi(hight缩写)`，需要减减。     

同理当`sum<target`时，肯定左边的`lo(low)`下标要加加，使之接近target。     

最后当相等时就可以入录了`res.push(left,right)`当然因为不允许重复，这样在等于后面要加一层`while`来判断`left`是否和当前`nums[lo]`相等，`right`是否和当前`nums[hi]`相等。    

总结代码如下：

```javascript
    var lo = start, hi = nums.length-1;
    while(lo<hi) {
      var left = nums[lo],right = nums[hi];
      var sum = left+right;
      if(sum > target) {
        hi--;
      } else if(sum<target) {
        lo++;
      } else {
        res.push([left,right]);
        while(lo<hi && left === nums[lo]) lo++; 
        while(lo<hi && right === nums[hi]) hi--; 
      }
    }
```

这里的`start`是确认初始位置因为当`多sum`时是从上一层`i+1`开始的，因为是增序数列所以只要往右找就好了，左边的话是使用过的。



##   3sum问题

因为已经知道`2sum`的结果，那我们遍历一遍`nums`再从i+1的位置开始传入`target-nums[i]`找`2sum`返回的结果再拼接，返回结果.



##  NSum的问题

知道上面两问可知，要找`nsum`问题可以从`(n-1)sum`开始找,而`n>2`时的方程都差不多。大致为从start位置开始遍历数组，生成`(n-1)sum`类型的返回`res`，遍历`res`，再将自己`nums[i]`推入，最后通过`while`去除相同的当前数字.      

大体如下所示：

```javascript
for(var i = start;i<nums.length;i++) {
    var midRes = numsum(nums, n-1, i+1, target-nums[i]); // n-1是当前(n-1)sum结果， i+1开始找和为target-nums[i]的res集合
    for(var arr of midRes) {
        arr.push(nums[i]);
        res.push(arr);
    }
    while(i<nums.length-1 && nums[i]===nums[i+1]) i++;
}
```



完整代码如下:

```javascript
// 传入排序数组，几sum，0，target
var numsum = function(nums, n, start, target) {
  var res = [];
  if(n===2) {
    var lo = start, hi = nums.length-1;
    while(lo<hi) {
      var left = nums[lo],right = nums[hi];
      var sum = left+right;
      if(sum > target) {
        hi--;
      } else if(sum<target) {
        lo++;
      } else {
        res.push([left,right]);
        while(lo<hi && left === nums[lo]) lo++; 
        while(lo<hi && right === nums[hi]) hi--; 
      }
    }
  } else {
    for(var i = start;i<nums.length;i++) {
      var midRes = numsum(nums, n-1, i+1, target-nums[i]);
      for(var arr of midRes) {
        arr.push(nums[i]);
        res.push(arr);
      }
      while(i<nums.length-1 && nums[i]===nums[i+1]) i++;
    }
  }
  return res;
}
```



