#  寻找第K大的数（排序集合）



##  快排思想集合O(n)

> 先想到有快排的思路，我们随便找个数（如最末尾的数）让它定位到正确位置，左边都比它大，右边都比它小。之后再用二分的思想，看当前下标i和k-1的比较，小了去左边找，大了去右边找。



记住要等于

快排的`parition`思路要记得

```js
// 因为真实遍历次数是n + n/2 + n/4 +...2;等比求和为2n即时间复杂度O（n）；
function findMaxK(arr, k, left, right) {
  if(left <= right) {
    let mid = partition(arr,left,right);
    console.log(arr);
    console.log(mid);
    if(mid === k-1) {
      return arr[mid];
    } else {
      return mid > k - 1 ? findMaxK(arr, k, left,mid-1): findMaxK(arr,k,mid+1,right);
    }
  }
}

function partition(arr,left,right) {
  let pivot = left;
  let pivotValue = arr[right];
  for(let i=left;i<right;i++) {
    if(arr[i] > pivotValue) {
      [arr[i],arr[pivot]] = [arr[pivot], arr[i]];
      pivot++;
    }
  }
  [arr[pivot], arr[right]] = [arr[right],arr[pivot]];
  return pivot;
}
// var arr = [4,2,1,7,8];
// console.log(findMaxK(arr,2,0,arr.length-1));

```

记得从大往小排。那遇到小于`pivotValue`就要换位置



##   最大堆找k值O(nlogk)

> 也可能是O(klogn)但是k可能为n/2所以平均O(nlogn)

其实就是建一个最大堆，然后再获取k次最大值，

主要是思路是建立最大堆

就是通过一个`adjustHeap`函数，它可以将当前位置的数向下迭代做调整，维护最大堆。



通过每一头尾换位置，记得要设置len长度。

```
function adjustHeap(arr, i, len) {
  let right = i*2+1;
  let left = i*2;
  let mid = i;
  if(left < len && arr[mid] < arr[left]) {
    mid = left;
  } 
  if(right < len && arr[mid] < arr[right]) {
    mid = right;
  }
  [arr[mid],arr[i]] = [arr[i],arr[mid]];
  if(mid !== i && mid < len) {
    adjustHeap(arr, mid, len);
  }
}


function  findKHeap(arr, k) {
  let n = Math.floor(arr.length/2);
  for(let i=n;i>=0;i--) {
    adjustHeap(arr, i, arr.length);
  }
  console.log(arr);
  for(let j=0;j<k;j++) {
    [arr[0], arr[arr.length-1-j]] = [arr[arr.length-1-j], arr[0]];
    adjustHeap(arr, 0, arr.length-1-j);
  }
  console.log(arr[arr.length-k]);
  return arr[arr.length-k];
} 


// var arr = [4,2,1,7,8];
// console.log(findKHeap(arr,2));
```







#   各种排序算法

##  快排

```js

// 快速排序
function quickSort(arr, left, right) {
  if(left < right) {
    let mid = partition(arr, left, right);
    quickSort(arr, left,mid-1);
    quickSort(arr, mid+1, right);
  }
}

function partition(arr, left, right) {
  let pivot = left;
  let pivotVal = arr[right];
  for(let i=left;i<right;i++) {
    if(arr[i] < pivotVal) {
      [arr[i], arr[pivot]] = [arr[pivot], arr[i]];
      pivot++;
    }
  }
  [arr[pivot], arr[right]] = [arr[right],arr[pivot]];
  return pivot;
}
```



##   分治法

```js

// 归并排序，分为分割和合并
function splitSort(arr, temp, left, right) {
  if (left < right) {
    let mid = Math.floor((left + right)/2);
    splitSort(arr, temp, left, mid);
    splitSort(arr, temp, mid+1, right);
    merge(arr, temp, left, mid, right);
  }
}

function merge(arr, temp, left, mid, right) {
  let i = left, j = mid+1, k = 0;
  while(i<=mid && j<=right) {
    temp[k++] = arr[i] > arr[j] ? arr[j++] : arr[i++];
  }
  while(i<=mid) {
    temp[k++] = arr[i++];
  }
  while(j<=right) {
    temp[k++] = arr[j++];
  }

  for(let z = 0;z<k;z++) {
    arr[left+z] = temp[z];
  }
}

```



##   堆排序

```js
//  堆排序不稳定排序，主要写好堆调整，从当前位置一直向下调整
function heapify(arr, i, len) {
  let left = i*2;
  let right = i*2+1;
  let largest = i;
  if(left < len && arr[largest] < arr[left]) {
    largest = left;
  }
  if(right < len && arr[largest] < arr[right]) {
    largest = right;
  }
  if(largest !== i) {
    [arr[i], arr[largest]] = [arr[largest], arr[i]];
    heapify(arr, largest, len);
  }
}

function buildMaxHeap(arr) {
  let len = arr.length;
  for(let i=Math.floor(len/2);i >= 0; i--) {
    heapify(arr, i, len);
  }
}

function heapSort(arr) {
  buildMaxHeap(arr);
  let len = arr.length;
  for(let i= arr.length-1;i>0;i--) {
    [arr[0],arr[i]] = [arr[i], arr[0]];
    len--;
    heapify(arr, 0, len);
  }
}

// let arr = [3,44,1,55,8,5,9,5];
// heapSort(arr);
// console.log(arr);
```



