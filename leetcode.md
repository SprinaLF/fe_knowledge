
## leetcode 07 整数反转
```js
// 在java中整除用/  取余用% 但js /  所的结果为浮点数
// 应该调用以下方法
//     var n1 = Math.round(exp1); //四舍五入
//     var n2 = Math.round(exp2); //四舍五入
   
//     var rslt = n1 / n2; //除
//         rslt = Math.floor(rslt); //返回值为小于等于其数值参数的最大整数值。
//         rslt = Math.ceil(rslt); //返回值为大于等于其数字参数的最小整数。
var reverse = function(x) {
    var p,a=[] ;
    let flag=true;
    // 取绝对值
    let v=Math.abs(x);
    while(v!=0){
        p=v%10;
        // 若开头是0，去掉
        while(flag&&p==0){
            v=Math.floor(v/10);
            p=v%10;
        }
        v=Math.floor(v/10);
        flag=false;
        a.push(p);
        
    }
    let s=a.join('');
    // 是否是负数的情况
   let result=x>0?s:-s;
   // 判断溢出
   return result>Math.pow(2,31)-1||result<-Math.pow(2,31)?0:result;
};
console.log(reverse(10200));
```
## 面试题 16.16. 部分排序
```js
var subSort = function(array) {
  let f1=false,f2=false
  let max,min;
  let temp1=array[0]
  let temp2=array[array.length-1]
  // 1.找不合理的第一个数和最后一个数字   前后同时开始寻找
  // [1,2,4,7,10,11,15,7,12,6,7,16,18,19],先找出7和12
  for(let i in array){   //第一次遍历找数
    if(array[i]<temp1&&f1==false){//找到不符合的第一个数
        min=array[i];
        f1=true;  //从此时的位置往后开始找最小数
    }else temp1=array[i]  //temp更新  

    if(f1&&array[i]<min){  //找最小数
      min=array[i]
    }
    if(array[array.length-1-i]>temp2&&f2==false){
      max=array[array.length-1-i]
      f2=true;   //从此时的位置往后开始找最大数
    }else temp2=array[array.length-1-i]
    if(f2&&array[array.length-1-i]>max){
      max=array[array.length-1-i]
    }
  }
  //找位置
  let rmin=-1,rmax=-1;
  if(f1){    //第二次遍历找位置，两个for可以写在一起，但执行时间会变多
    for(let i=array.length-1;i>0;i--){
    if(max>array[i]){
      rmax=i;
      break;
    }
  }
  for(let i in array){
    if(min<array[i]){
      rmin=i;
      break;
    }
  }
  }
  return [rmin*1,rmax];
};
/////////人家的方法更简单
var subSort = function(array) {
    let r=-1,l=-1;
    //正向遍历记录最右区间值
    let max = Number.MIN_SAFE_INTEGER;
    for(let i=0;i<array.length;i++){
        if(array[i]>=max){
            max = array[i];
        }else{
            r = i;
        }
    }
    //反向遍历记录最左区间值
    let min = Number.MAX_SAFE_INTEGER;
    for(let i=array.length-1;i>=0;i--){
        if(array[i]<=min){
            min = array[i]
        }else{
            l = i;
        }
    }
    return [l,r]
};
console.log(subSort( [1,2,4,7,10,11,7,12,6,7,16,18,19]));
```
## 58 字符串左旋    字符串截取/拼接

<!-- 常用的字符串截取函数要信手拈来，三个：slice(start, end)，substring(start, end)，substr(start,length)，其中slice跟substring效果一样，包含start，但是不包含end-->

```js
  var reverseLeftWords = function(s, n) {
      //起始位置，截取长度
    //   第0个位置开始的两个字符
      let subStr1=s.substr(0,n);
      console.log(subStr1);
    // 截取索引n及之后的字符
       let subStr2=s.substr(n);
       console.log(subStr2)
       let newStr=subStr2+subStr1;
       return newStr;
   };
   console.log( reverseLeftWords('abcdefg',2));

```
## 数组

### 数组遍历性能

![img](file:///C:/Users/sprina/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)



![image-20200614211050085](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20200614211050085.png)



### 数组中只出现一次的数字，空间最小

  方法一  时间长···

```js
    //考虑用数组对象的index方法
    /////!!!!!!注意 ，for…in 最好不要用来遍历数组 https://blog.csdn.net/VagueCoder/article/details/47294481?%3E
    // forEach() 方法对数组的每个元素执行一次提供的函数。总是返回undefined；
    var singleNumber = function(nums) {
        let f=false;
    // 没有办法中止或者跳出 forEach() 循环，除了抛出一个异常。如果你需要这样，使用 forEach() 方法是错误的。break和return不行   http://www.fly63.com/article/detial/2421
        nums.forEach(e=> {
          if(nums.indexOf(e)===nums.lastIndexOf(e)){
            f=e;   
          }         
        });
       return f;
    };
    console.log(singleNumber([1,3,3,1,2]));  
```

方法二 哈希表

```js
var singleNumber = function(nums) {
  let numsObj = {};
  for (let i = 0; i < nums.length; i++) {
    if (numsObj[nums[i]]) delete numsObj[nums[i]];
    else numsObj[nums[i]] = 1;
  }
  return Object.keys(numsObj);
};
```

### 数组中数字出现的次数

indexOf效率低

```js
var singleNumbers = function(nums) {
  let map = new Map() 
  for(let i=0;i<nums.length;i++){
      map.has(nums[i])?map.delete(nums[i]):map.set(nums[i],0)
  }
  return [...map.keys()]
};

console.log(singleNumbers([1,1,3,6,6,4,7]) );
```

### 88. 合并两个有序数组

```js
//splice 向特定位置添加元素
var merge = function(nums1, m, nums2, n) {
    nums1.splice(m,nums1.length-m)   //截取nums1有用部分
    if(m==0){
        // 或者nums1.splice(0,nums1.length) 清空数组  arr = []不行，数组所指会变，不是原地
        nums1.push(...nums2)
        return
    } 
    let index=0;
    let i=0   //nums1的数组指针
    nums1.splice(m,nums1.length-n)
    while(index<n){  //nums2所有元素都已经插入nums1
        if(nums2[index]<=nums1[i]){
            nums1.splice(i,0,nums2[index])
            index++
        }else{
            i++
        }
        if(i>=nums1.length){   //slice不会改变数组而是返回新的，splice会改变数组
            // console.log(nums2.slice(index))
            // nums1.splice(f,nums1.length-m)
            //concat不会改变nums1
            nums1.push(...nums2.slice(index))   
            break
        }
    }  
    
};
merge(
[1,2,3,0,0,0],
3,
[2,5,6],
3)
```
### 面试题45. 把数组排成最小的数 //sort

打印能拼接出的所有数字中最小的一个

```
输入: [3,30,34,5,9]
输出: "3033459"
```

```js
//1.回溯 暴力法
//2. sort函数
var minNumber = function(nums) {
    let compare=function(x,y){
        let a=''+x+y   //''要放前面，先转字符串
        let b=''+y+x
        if(a-b>=0) return 1   //大于0，交换x,y
        else return -1  

    }
    return nums.sort(compare).join('')
};
```

### 面试题61. 扑克牌中的顺子

1.检测一组数是否连续（先排序再检测）

2.王可作为任意数字（记录王数，用王减去不连续数的个数，若王数小于0，false）

3.有相同牌的情况（tmp=nums[i+1]-nums[i]-1）会小于0，false

## 求数组的最大元素**

```js
//第一种：遍历数组每一项与当前最大值比较
Array.prototype.max = function(){
    let max = this[0];
    this.forEach((item,index) => {
    max = max > item ? max : item;
    })
    return max;
}
console.log(arr.max())

//第二种：数组的内置函数sort()排序，a-b从小到大，b-a从大到小
let arr1 = arr.sort((a,b)=>{
	return b-a;
})[0];
console.log(arr1)

//第三种：数组的内置函数reduce(),回调函数中两个必要的参数，累计器和当前值
let arr2 = arr.reduce((a,b)=>{
	return a > b ? a : b
})
console.log(arr2)

//第四种：内置函数Math.max(),支持传递多个参数，但不支持直接传递一个数组，借助apply()或数组解构
let arr3 = Math.max.apply(null,arr)
let arr3 = Math.max(...arr)
```

其中第一种方法标准循环运行速度最快，其次是第四种内置函数max方法；最慢的是ES6扩展

## 链表

### 合并两个有序链表

```js
var mergeTwoLists = function(l1, l2) {
    let h=new ListNode(0);
    let curr=h,p=l1,q=l2;
    while(p&&q){
        if(p.val<q.val){
            curr.next=p;
            p=p.next;
        }else{
            curr.next=q;
            q=q.next;
        } 
        curr=curr.next;
    }
    if(p) curr.next=p;
    if(q) curr.next=q;
    return h.next;
};
```

### 2 两数相加 (链表) ***

```js
var addTwoNumbers = function(l1, l2) {
    let node = new ListNode('head')
    let temp = node , sum , n = 0
    while( l1 || l2 ){
        const n1 = l1 ? l1.val : 0
        const n2 = l2 ? l2.val : 0
        sum = n1 + n2 + n
        temp.next = new ListNode( sum % 10 )
        temp = temp.next
        n = parseInt(sum / 10 )
        if(l1) l1 = l1.next
        if(l2) l2 = l2.next
    }
    // 不要忘了最后一位
    if( n > 0 ) temp.next = new ListNode(1)
    return node.next
};
```
### 61. 旋转链表
```js
//相当于每次把最后一个数移动到最前面，做这样的操作k次
//这种方法效率低，考虑另一种方法
var rotateRight = function(head, k) {
  if(!head) return null
  let p=head,size=0
  while(p){
    p=p.next
    size++
  }
  let num=k%size
  while(num){
     p=head  //用于寻找最后一个位置
    while(p.next){
      pre=p
      p=p.next
    } //找到最后一个节点和它的前驱
    pre.next=null //删除最后一个节点
    p.next=head
    head=p   //更新第一个节点
    num--
  }
  return head
};

//////2.直接寻找位置
var rotateRight = function(head, k) {
  if(!head||k==0) return head
  let p=head,s=head,size=1
  while(s.next){
    s=s.next
    size++
  } //s指向最后的节点 
  let num=k%size
  if(num==0)  return head  
  ////第size-num个元素后面的元素需要移动
  for(let count=1;count<size-num;count++){
    p=p.next
  }
  s.next=head  //连接到头部
  head=p.next  //该处为新的头部
  p.next=null
  return head
};
```
### 1600反转链表
```js
///////1.头插法
var reverseList = function(head) {
    if(!head||!head.next) return head
    let h=new ListNode('head')  //设置哨兵节点
    let p=head.next     //p用于遍历
    h.next=head
    let curr
    let c=h.next  //c始终指向第二个节点  新节点插入c和h之间
    c.next=null
    while(p){
      curr=p
      p=p.next
      curr.next=c
      h.next=curr
      c=curr
    }
    return h.next
};

/////////2.原地反转指针
var reverseList = function(head) {
    if(!head){
        return head;
    }
    let p1=head,
        p2=head.next
        temp=null;
    while(p2){
        temp=p2.next;  //后续托付
        p2.next=p1;  //反转
        p1=p2;      //后移动
        p2=temp
    }
    head.next=null;
    return p1;
};
```

### **面试题  判断链表是否有环

```js 
// 两种思路，1.判断该节点是否访问过，用Map/Set  2.快慢指针，空间复杂度为1
// 1.Map   m.has  m.set   m.delete  m.get
var hasCycle = function(head) {
    let m = new Map()
    let p = head
    while(p) {
        if(m.has(p)) return true
        m.set(p,true)
        p=p.next
    }
    return false
};
// 2，Set   set.add  set.delete   set.has   可存储任何类型
var hasCycle = function(head) {
    let s = new Set()
    let p = head
    while(p) {
        if(s.has(p)) return true
        s.add(p)
        p=p.next
    }
    return false
};
// 3. 快慢指针
var hasCycle = function(head) {
    if(!head) return false
    let fast = head.next, slow = head
    while(fast) {
        if(fast==slow||!fast.next) break
        fast = fast.next.next
        slow = slow.next
    }
    // 跳出循环有两种情况，一是指针相遇，二是快指针走到头
    if(fast==slow) return true
    return false
};
```

### II链表环的起点

```js
////1. map/set
/// 2. 快慢指针 数学推导 相遇时再定义一个指针,再相遇时就是交点
var detectCycle = function(head) {
    if(!head) return null
    let fast = head, slow = head
     while(fast&&fast.next) {
         fast = fast.next.next
         slow = slow.next
         if(fast==slow) {
             let n = head
             while(n!=slow) {      
                 n = n.next
                 slow = slow.next
             }
             return n
         }
     }
     return null
};
```

### 面试题 两个链表是否相交

```js
// 1. set/map记录节点，去另一个链表中找

// 2. 都遍历一遍，记录长度，长度先走的跟短的一样长的位置，再同时走

// 3. ***双指针法，走完当前链表去走另外的链表
// 双指针时一定相交与D点(速度相同) AD+DC+BD = BD+DC+AD   不相交时最后会同时指向null
var getIntersectionNode = function(headA, headB) {
    if(!headA||!headB) return null
    let p = headA, q = headB
    while(p!=q) {   // 一种是相交点相遇，另一种是都为null
        if(!p) p=headB
        else p = p.next      //////////这里else容易忘记！！
        if(!q) q=headA
        else q = q.next
    }
    return p
};
```

![ListNode.png](https://pic.leetcode-cn.com/9361ba6ca0b2203a0304156cb3d5e10337836d52f4a41464b66854ea0562418f-ListNode.png)

### 面试题35. 复杂链表的复制 ***

思路一：先复制相同节点放在后面，再复制random指针，最后拆分链表

```js
var copyRandomList = function(head) {
    if (!head) {
        return head
    }
    let curr=head
    let n=head.next    //后继
    while(n){
        let p=new Node()
        p.val=curr.val
        curr.next=p
        p.next=n
        curr=n
        n=n.next
    }
    let p=new Node(curr.val)  //复制最后一个节点
    curr.next=p
    //拷贝random
    curr=head
    let copy=head.next
    while(curr){
        if(curr.random){
            copy.random=curr.random.next
        }
        curr=copy.next
        if(!curr){   //到尾部，结束
           break
        }
         copy=copy.next.next  //考虑越界问题
    }
       //分离,恢复原链表
        let newHead = head.next;
        let temp
        for(let node = head; node.next;) {  
            temp = node.next;
            node.next = temp.next;
            node = temp;
        }
        return newHead;
};
```

精简版：

```js
var copyRandomList = function(head) {
   if (head == null) {
            return head;
        }
        //将拷贝节点放到原节点后面，例如1->2->3这样的链表就变成了这样1->1'->2'->3->3'
        for (let node = head, copy = null; node != null; node = node.next.next) {
            copy = new Node(node.val);
            copy.next = node.next;
            node.next = copy;
        }
        //把拷贝节点的random指针安排上
        for (let node = head; node != null; node = node.next.next) {
            if (node.random != null) {
                node.next.random = node.random.next;
            }
        }
        //分离拷贝节点和原节点，变成1->2->3和1'->2'->3'两个链表，后者就是答案
        let newHead = head.next;
        for (let node = head, temp = null; node != null && node.next != null;) {
            temp = node.next;
            node.next = temp.next;
            node = temp;
        }
        return newHead;
};
```

思路二：Map() 保存映射关系



### 147. 对链表进行插入排序 ****
```js
//链表的原地插入排序
var insertionSortList = function(head) {
    // 边界条件
    if(head === null) return head;
    // 哨兵节点
    let h = new ListNode(0);
    // 将要移动重新插入链表中的元素
    let curr = head;
    // 插入链表中的前驱位置 ~~~~~插入必须记录前驱
    let pre = h;
    let next;
    // 遍历
    while(curr){
        // 记录下一个将要插入的元素
        next = curr.next;
        // 记录插入的前驱位置
        while(pre.next && pre.next.val < curr.val){
            pre = pre.next;
        }
        // 插入
        curr.next = pre.next;
        pre.next = curr;
        // 从头开始遍历,pre归位
        pre = h;
        // 下一个
        curr = next;
    }
    return h.next;
};
```

## 189. 旋转数组 空间最小

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。
```js
// 输入: [1,2,3,4,5,6,7] 和 k = 3
// 输出: [5,6,7,1,2,3,4]
// 解释:
// 向右旋转 1 步: [7,1,2,3,4,5,6]
// 向右旋转 2 步: [6,7,1,2,3,4,5]
// 向右旋转 3 步: [5,6,7,1,2,3,4]

// 思路一  整体反转，然后前k个数饭庄，后面的数反转   原地算法？？？
var rotate = function(nums, k) {
    //////!!!需考虑到这种情况
    if(k>nums.length){
        k=k%nums.length;
    }
    nums.reverse();   //整体反转
    for(let i=0;i< Math.floor(k/2);i++){  
         //反转前k个数，采用交换的方法
         //ES6 这种交换的方法非常慢。临时变量的方法快
      [nums[i],nums[k-i-1]]=[nums[k-i-1],nums[i]]    //解构实现交换
    }
    for(let i=0;i<Math.floor((nums.length-k)/2);i++){
      [nums[i+k],nums[nums.length-1-i]]=[nums[nums.length-1-i],nums[i+k]];
    }
    return nums;
  };
```
思路二：
每次截取最后一项放在最前面

```js
var rotate = function(nums, k) {
  for(let i=0;i<k;i++){
    nums.unshift(nums.pop());
  }
};
```
思路三：
```js
splice(2,6),删除并返回从索引2开始的六个元素
数组的splice方法
var rotate = function(nums, k) {
  //为什么合起来写不行？？？
  // nums.unshift(...nums.splice(nums.lenght-k));
  let a=nums.splice(nums.length-k);
  nums.unshift(...a);
  return nums;

};

var rotate = function(nums, k) {
  nums.splice(0, 0, ...nums.splice(nums.length - k));
};

```

### 用队列实现栈

```js
var MyStack = function() {
    this.queue = [];
};
MyStack.prototype.push = function(x) {
    this.queue.push(x);
};
/**
 * Removes the element on top of the stack and returns that element.
 * @return {number}
 */
MyStack.prototype.pop = function() {
    let res = [];  //临时队列
    for(let i=0;i<this.queue.length-1;i++){
        res.push(this.queue[i]);
    }
    let r = this.queue[this.queue.length-1];
    this.queue = res;
    return r;
};

/**
 * Get the top element.
 * @return {number}
 */
MyStack.prototype.top = function() {
    if(this.queue.length === 0) {
        return null;
    }
    return this.queue[this.queue.length-1];
};

/**
 * Returns whether the stack is empty.
 * @return {boolean}
 */
MyStack.prototype.empty = function() {
    return this.queue.length === 0;
};

/**
 * Your MyStack object will be instantiated and called as such:
 * var obj = new MyStack()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.empty()
 */

```



## 263 丑数

不断整除2，3，5，直到不能整除为止，判断整除后的数是否为1
```js
    var isUgly = function(num) {
       if(num==0)
          return false;
        while(num%2==0){
          num=num/2;
        }
        while(num %3==0){
          num=num/3
        }
        while(num %5==0){
          num=num/5;
        }
        let res=num==1? true:false;
        return res;
      };
```
## 49 丑数进阶 *****动态规划
```js

```
## 二分查找

```js
var search = function(nums, target) {
    let low=0,high=nums.length-1
    while(low<=high){
        let mid=Math.floor((low+high)/2)
        if(target<nums[mid]){
            high=mid-1
        }else if(target>nums[mid]){
            low=mid+1
        }else{
            return mid
        }
    }
    return -1
};
```

### 374.猜数字大小   基本

```js
var guessNumber = function(n) {
    let i=1,j=n   //首尾指针
    while(i<=j){
        let mid=Math.floor((i+j)/2)
        let res=guess(mid)
        if(res==0) return mid
        if(res==1) i=mid+1
        else j=mid-1
    }
};
```



### 278. 第一个错误的版本

```js
var solution = function(isBadVersion) {
    /**
     * @param {integer} n Total versions
     * @return {integer} The first bad version
     */
    return function(n) {
      let mid
      let low=1,high=n
      while(low<=high){
         mid= Math.floor((low+high)/2)  //新的中位数
        //坏，从坏的之前开始找，若坏的刚好就是第一个坏的，有low=mid+1
        if(isBadVersion(mid)){
            high=mid-1
        } else{  //没坏，从中间到后面开始查找
            low=mid+1
        }
      }
      return low
    };
};
```
### 面试题 0～n-1中缺失的数字

```js
 var missingNumber = function(nums) {
   let mid
   let left=0,right=nums.length-1
   if(nums[right]==right){  //不缺元素
       return nums.length
   }
   while(left<=right)
   {
       mid=Math.floor((left+right)/2)
       if(nums[mid]==mid){
           left=mid+1
       }else{
           right=mid-1
       }
   }   
   return left
};
console.log(missingNumber([0,2]))
```



#### 斐波那契数列

```js
//非递归求斐波那契，递归会超时
var fib = function(n) {
  if(n<2){
    return n;
  }
  let p0 = 0,p1 = 1;
  let res = 0;
  for(let i = 2;i<=n;i++){
    res = (p0 + p1) %(1e9+7);
    p0 = p1;
    p1 = res;
  }
  return res;
};
```

### 130被围绕的区域

```js
//类似孤岛问题，用DFS算法
var solve = function(board) {
    if(!board.length) return board
    let map={}  //记录不用修改的O
    let row=board.length-1
    let col=board[0].length-1

    let dfs=function(x,y){
    ////////!map[x+'-'+y]这个条件必须加，否则栈会溢出
    ///会有很多重复的递归
      if(board[x][y]=='O'&&!map[x+'-'+y]){
        map[x+'-'+y]=true  ////////用于记录二维数组的索引
        if(x<row) dfs(x+1,y)
        if(x>0) dfs(x-1,y)
        if(y<col) dfs(x,y+1)
        if(y>0) dfs(x,y-1)  
      } 
    }
    for(let i=0;i<=row;i++){
      dfs(i,0)
      dfs(i,col)
    }
    for(let j=0;j<=col;j++){
       dfs(0,j)
       dfs(row,j)
    }

    ////边界的O不用管
    for(let i=1;i<row;i++){
      for(let j=1;j<col;j++){
        if(board[i][j]=='O'&&!map[i+'-'+j]){ //需要变
          board[i][j]='X'
        }  
      }
    }
    return board
};
```



## 289. 生命游戏

```js
 var gameOfLife = function(board) {
    // 八个方向的偏移量
    const idx = [0, 1, 0, -1, -1, -1, 1, 1];
    const jdx = [1, 0, -1, 0, 1, -1, 1, -1];

    // const CopyBoard = board.map(ary => {
    //     // 值是基础类型（Number），不存在引用问题，直接解构比较方便
    //     return [...ary];
    // })

    // Array.from只能实现数组浅拷贝，含有引用对象（含有数组/对象）的不行

    let rows=board.length;
    let cols = board[0].length;
   // 二维数组的深拷贝
    let copy = new Array(rows);
    for (let row = 0; row < rows; row++) {
        copy[row] = new Array(cols);
    }

    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
            copy[row][col] = board[row][col];
        }
    }

    // 遍历每个细胞
    for(let i = 0; i < copy.length; i++) {
        for(let j = 0; j < copy[i].length; j++) {
            // 记录周边活细胞数
            let liveAmount = 0;
            // 需走八个方向
            for(let index = 0; index < 8; index++) {
                let x = i + idx[index];
                let y = j + jdx[index];
                // 判断边界
                if(x < 0 || y < 0 || x >= copy.length || y >= copy[i].length) continue;
                // 活细胞则数加1
                liveAmount += copy[x][y] ? 1 : 0;
            }

            // 判断该位置细胞死活
            if(liveAmount < 2 || liveAmount > 3) {
                board[i][j] = 0;
            } else if (liveAmount <= 3 && copy[i][j]) {
                board[i][j] = 1;
            } else if (liveAmount === 3 && !copy[i][j]) {
                board[i][j] = 1;
            }
        }
    }
};
```

## 905. 按奇偶排序数组
给定一个非负整数数组 A，返回一个数组，在该数组中， A 的所有偶数元素之后跟着所有奇数元素。
不要求顺序
法1：参照快速排序，用双指针   原地排序~~
```js
var sortArrayByParity = function(A) {
    let i=0,j=A.length-1
    let temp
    while(i<=j){
        if(A[i]%2>A[j]%2) //i奇数，j偶数
        {
            temp=A[i]
            A[i]=A[j]
            A[j]=temp
        }
        if(A[i]%2==0) i++
        if(A[j]%2>0) j--
    }
    return A
};
```
法2：新建数组，前后同时扫描，偶数奇数分别插入数组的开头，末尾
法3：令数组的每个元素%2，按照%2的结果调用sort排序
## 1108 替换字符串中的.
```js
var defangIPaddr = function(address) {
  //核心：将字符串中的.替换为[.]
  str=address.replace(/\./g,'[.]');   //转义，替换多个
  return str;
};

s.replace(/\s/g,'%20')  //替换空格
s.replace(/ /g,'%20')  //替换空格
```
### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

```
var numIslands = function(grid) {
    if(!grid.length) return 0  //[]
    let res=0
    let row=grid.length-1,col=grid[0].length-1
    function dfs(x,y){
        if(grid[x][y]==1){
            grid[x][y]=0
            //四个方向
            if(x>0) dfs(x-1,y)
            if(x<row) dfs(x+1,y)
            if(y>0) dfs(x,y-1)
            if(y<col) dfs(x,y+1)
        } 
    }
    for(let i=0;i<=row;i++){
        for(let j=0;j<=col;j++){
            if(grid[i][j]==1){
                res++
                dfs(i,j)
            }
            
        }
    }
    return res
};
```

### 1254. 封闭岛屿的数目（边界不算岛屿的情况，更复杂）

```js
var closedIsland = function (grid) {
   let res=0;
   let row=grid.length-1;  //行数
   let col=grid[0].length-1  //列数
   function dfs(x,y){
      if(grid[x][y]==0){  //与孤岛连通的孤岛
        grid[x][y]=1  //看作海域(两种情况))
        /////四个方向
        if(x>0) dfs(x-1,y) //下方
        if(y>0) dfs(x,y-1)  //左方
        if(x<row) dfs(x+1,y)
        if(y<col)  dfs(x,y+1)
      } 
  }
  ////遍历边界
  for(let x=0;x<=row;x++){
      dfs(x,col)
      dfs(x,0) 
  }
  for(let y=0;y<=col;y++){
      dfs(0,y)
      dfs(row,y)
  }
  //////dfs算法
  /////因为边界已经遍历过，肯定都是1
  ///所以这步可优化为只遍历内部的
   for (let x = 1; x<row; x++) {       //行数
    for (let y = 1; y<col; y++) {      //列数   
      if (grid[x][y]==0) {     //孤岛
        ++res    ///统计完之后翻脸不认人，当作1
        dfs(x,y);
      }
    }
  }
    return res
};
```
### 岛屿周长

https://leetcode-cn.com/problems/island-perimeter/

```
var islandPerimeter = function(grid) {
    let row = grid.length,col=grid[0].length
    let count = 0
    function dfs(x,y) {
        if(x<0||x>row-1||y<0||y>col-1) {  //到了边界
            return 1
        } 
        if(grid[x][y]==0) {   //旁边是海水
            return 1
        }
      if(grid[x][y]==2) {  //已访问过
           return 0
        }
        grid[x][y]=2
        return dfs(x-1,y)+ dfs(x,y-          1)+dfs(x+1,y)+dfs(x,y+1)
    }
    for(let i = 0;i<row;i++) {
        for(let j=0;j<col;j++) {
            if(grid[i][j]==1) {
               return dfs(i,j)
            }
        }
    }
    return 0
};
```

### 岛屿的最大面积

```js
var maxAreaOfIsland = function(grid) {
    let res = 0, num = 0
    let row = grid.length-1, col = grid[0].length-1
    const dfs = (x,y) => {
        if(grid[x][y]==1) {
            num++
            grid[x][y] = 0
            if(x>0) dfs(x-1,y)
            if(y>0) dfs(x,y-1)
            if(x<row) dfs(x+1,y)
            if(y<col) dfs(x,y+1)
        }
    }
    for(let i = 0;i<=row;i++) {
        for(let j = 0; j<=col;j++) {
            if(grid[i][j]==1) {
                 dfs(i,j)
                 res = Math.max(res,num)
                 num = 0  // 重新计数
            }
        }
    }
    return res
};
```

## 1337 方阵中战斗力最弱的 K 行

```js
var kWeakestRows = function(mat, k) {
    let arr=[];
    for(let i=0;i<mat.length;i++){    //mat.length表示矩阵的行
       let count=0;
      for(let j=0;j<mat[i].length;j++){    //mat[i].length表示矩阵的列
        if(mat[i][j]==1)
          count++;
      }
      arr.push([i,count]);    //只保存1的个数不行，还需保存i的值    
    }
    /////////在V8 V7.0和Chrome 70中，我们的Array.prototype.sort现在的执行是稳定的。
// 以前，V8对数组使用了不稳定的快速排序十多个元素..(array.sort少于10个用插排，大于10个用快排。)现在，V8使用稳定的TimSort算法。   火狐中是“归并排序”
// 唯一的主要引擎JavaScript引擎仍然有一个不稳定的Array#sort实现是Chakra，在MicrosoftEdge中使用。。 因此值相同时还要根据索引进行排序
    let arr2= arr.sort((a, b) => a[1] === b[1] ? a[0] - b[0] : a[1] - b[1]);  //用后面的元素排序
    const res = arr2.map(item=>item[0]);   //保留前面的元素得到结果
    return res.splice(0,k);    //截取前k个元素 并返回
};
console.log(kWeakestRows(a,k)); 
```
## 字符串

### 字符串压缩

```
 输入："aabcccccaaa"
 输出："a2b1c5a3"
```

```js
var compressString = function(S) {
    if(S.length<3) return S;
    let count=1;
    let res=S[0];  //可以不用tmp,用S[i-1]与S[i]
    for(let i=1;i<S.length;i++){
        if(S[i-1]==S[i]){
            count++;
            continue;
        }
        res+=count;
        res+=S[i];
        count=1;
    }
    res+=count; //最后一个没有记录上
    return res.length<S.length? res:S
};
```



### 查找子串

```js
//或者直接用indexOf方法求位置
var strStr = function(haystack, needle) {
    if(!needle.length) return 0
    let f=false
    for(let i=0;i<haystack.length-needle.length+1;i++){
        if(haystack[i]===needle[0]){
            if(needle.length===1) return i
            for(let j=1;j<needle.length;j++){
                if(haystack[i+j]!==needle[j]){
                    f=false
                    break
                }
                f=true
            }
            if(f)  return i   //此处需要注意        
        }
    }
    return -1
};
```



### 面试题58 翻转单词顺序

例如输入字符串"I am a student. "，则输出"student. a am I"。

```js
var reverseWords = function(s) {
    let res=s.split(/\s+/).reverse()
    return res.join(' ').trim()
};
```

### 557. 反转字符串中的单词 III

```
输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc" 
```

```js
var reverseWords = function(s) {
    let a=s.split(' ')
    let res=''
    for(let i=0;i<a.length;i++){
        res+=a[i].split('').reverse().join('')
        if(i==a.length-1){ //首尾无空格
            break
        }
        res+=' '
    }
    return res
};
```

### 1370. 上升下降字符串

```
输入：s = "aaaabbbbcccc"
输出："abccbaabccba"
```

```js
var sortString = function(s) {
    let stack1=s.split('').sort((a,b)=>{if (a < b) return 1;  
                if (a > b) return -1;  
                return 0;})     ////***字符降序排序
    
    let stack2=[]
    let res=''
    while(stack1.length||stack2.length){
        if(stack1.length){
            res=res+stack1.pop()  //栈顶一定满足条件，先进去一个
        }
        while(stack1.length){
            let p=stack1.pop()
            if(p==res[res.length-1]){
                stack2.push(p)
            }else{
                res=res+p
            }
        }
        if(stack2.length){
            res=res+stack2.pop()  //栈顶一定满足条件，先进去一个
        }
        while(stack2.length){
            let p=stack2.pop()
            if(p==res[res.length-1]){
                stack1.push(p)
            }else{
                res=res+p
            }
        }
    }
    return res
};
```



## 栈

### 155 设计一个支持 push ，pop ，top 检索最小元素的栈。

```js
var MinStack = function() {
    this.items=[];
};
MinStack.prototype.push = function(x) {
    this.items.push(x);
};
MinStack.prototype.pop = function() {
    this.items.pop();
};
MinStack.prototype.top = function() {
    return this.items[this.items.length-1]
};

MinStack.prototype.getMin = function() {
    let min=this.items[0];
    for(let i=1;i<this.items.length;i++){
        if(this.items[i]<min)
            min=this.items[i];
    }
    return min;
};
/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```

### 394. 字符串解码

```js
/////其他情况入栈，遇到右括号出栈
//////"3[a]2[bc]", 返回 "aaabcbc".
//字符串连接， +号
var decodeString = function(s) {
    let num=0     //存储重复次数
    let res=''
    let stack=[]
    for(let i=0;i<s.length;i++){
        if(s[i]!=']'){
            stack.push(s[i])
        }else{
            //1.字符串出栈
            let tmp=''
            let s0=stack.pop()
            while(s0!='['){   
               tmp=s0+tmp
                s0=stack.pop()
            }
            //2.数字出栈
            let num=''
            s0=stack.pop()
            while(!isNaN(s0)){
                num = s0 + num 
                s0 = stack.pop() // 再拉出一个
            }
            stack.push(s0)   //多出来一次，回去
            stack.push(tmp.repeat(num))  //字符串重复某次 ES6新增方法
        }
    }
    return stack.join('')
};
console.log(decodeString("3[a]2[bc]"));
```

### 面试题09. 用两个栈实现队列

```js
var CQueue = function() {
    this.stack1=[]
    this.stack2=[]
};

/** 
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function(value) {
    this.stack1.push(value)
};

/**
 * @return {number}
 */
CQueue.prototype.deleteHead = function() {
    let a
    if(this.stack2.length){
        return this.stack2.pop()
    }else{
        if(!this.stack1.length) return -1
        while(this.stack1.length){  //栈1需为空
        a=this.stack1.pop()
        this.stack2.push(a)
        }  
        return this.stack2.pop()
    }
};
```

### 括号匹配

```js
var isValid = function(s) {
    if(!s) return true
    let stack = []
    let map = new Map([['(',')'],['[',']']])
    map.set('{','}')
    for(let i = 0;i<s.length;i++) {
        if(map.has(s[i])) {  //左括号入栈
            stack.push(s[i])
        } else {   // 右括号，匹配
             if(s[i]==map.get(stack[stack.length-1])) {
                stack.pop()
            } else {
                return false
            }
        }
    }
    return stack.length==0
};
```

```js
//栈&&字符串指针
///题目只包括括号，故可用ASCALL码的差值判断
////（）差1  []{}差2
var isValid = function(s) {
    let stack=[s[0]]
    for(let i=1;i<s.length;i++){
        if(stack.length){
            let a=s[i].charCodeAt()-stack[stack.length-1].charCodeAt()
            if(a==1||a==2){   //匹配成左括号就出栈
                 stack.pop()
                 continue
            }
        }
        stack.push(s[i])    //不匹配就入栈
    }
    return stack.length==0?true:false
};
console.log(isValid('{[]}'))
```

### 面试题31. 栈的压入、弹出序列

```js
var validateStackSequences = function(pushed, popped) {
    const stack = [];   //辅助栈
    for(let i = 0;i<=pushed.length-1;i++){
        stack.push(pushed[i])
        while(stack.length!=0 && stack[stack.length-1] ==popped[0]){    //注意越界判断   ****一直弹出直到不同
            stack.pop()
            popped.shift()    //注意，这里用指针效率更高，不需要挪动数组元素
        }
    }
    return !stack.length
};
```

## 树

### **二叉树构造字符串

面试构造HTML

```
var tree2str = function(node) {
        if(!node) return '' 
        if(!node.left&&!node.right) return node.val+''
        if(!node.right) return node.val+'(' + tree2str(node.left) + ')'
        return node.val +'('+ tree2str(node.left)+')' +'(' + tree2str(node.right) + ')'
        // 或者不区分，最后用replace去除空括号()
};
```



### 二叉树前中后序遍历 迭代

```
  ///1. 前序
  var preorderTraversal = (root)=>{
        let res = []
        if(!root) return res
        let stack = [root]
        while(stack.length) {
            let top = stack.pop()
            res.push(top.val)
            if(top.right) stack.push(top.right) ///先放右子树
            if(top.left) stack.push(top.left)
        }
        return res
    }
   ////2. 中序遍历
  	var inorderTraversal = function(root) {
        const res = [];
        const stack = [];
        while (root||stack.length) {
            while (root) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            res.push(root.val);
            root = root.right;
        }
        return res;
	};
    /////后序
    var postorderTraversal = function(root) {
      let res = [],stack = []
        while(root||stack.length) {
            res.unshift(root.val)
            if(root.left) stack.push(root.left)
            if(root.right) stack.push(root.right)
            root = stack.pop()

        }
        return res
};
```

### 700. 二叉搜索树中的搜索

```js
var searchBST = function(root, val) {
    if(root==null) return null;
    if(root.val==val) return root;
    if(root.val<val){
     //外层函数需接收返回值
        return searchBST(root.right,val)
    }else {
       return searchBST(root.left,val)
    }      
};
```
### 1564 二叉搜索树转链表

思路  二叉搜索树的中序遍历时做指针移动操作
```js
var convertBiNode = function(root){
    if(!root)  return null
    let p=new TreeNode('head')  //哨兵节点
    let curr=p  //始终指向最新的节点
    var LDR=function(node){
        if(!node) return null
        LDR(node.left)
        //指针移动
        node.left=null  //当前节点的左节点置空，不指向其他节点
        curr.right=node  //连接
        curr=curr.right
        LDR(node.right)  
    }     
    LDR(root)
    return p.right
}
```
### 二叉搜索树与双向链表

[剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

```js
//没有头节点的循环链表
var treeToDoublyList = function(root) {
   if(!root) return
    let head=null
    p=head      //p用于遍历
    LDR(root)
    p.right=head  //最后首尾的处理
    head.left=p
    return head

    function LDR(node){    //放到外部传参的话不能改变p
        if(!node) return 
        LDR(node.left)
        {
            if(p){
                p.right=node
            }else{  //最左边节点
                head=node
            }
            node.left=p
            p=node
        }
        LDR(node.right)
    }
};
```

### 平衡二叉树    ***

```js
var isBalanced = function(root) {
    if(judge(root)==-1) return false
    return true
};
function judge(root){
    if(!root){
        return 0
    }
    let left=judge(root.left)
    if(left==-1)  //该节点左孩子的左右子树不平衡，直接将该节点也返回不平衡
        return -1
    let right=judge(root.right)
    if(right==-1) return -1
    if (Math.abs(left-right)>1) return -1
    return Math.max(left,right)+1
}

/// 简化上面的代码
var isBalanced = function(root) {
    let judge = function(root) {
        if(!root) return 0     // 高度为0
        let left = judge(root.left)
        if(left==-1) return -1
        let right = judge(root.right)
        if(right==-1) return -1
        return Math.abs(left-right)>1?-1: Math.max(left,right)+1
    }
    return judge(root)==-1?false:true
};
```

### 102 二叉树层序遍历

出队时左右孩子入队
```js
      var levelOrder=function(root){
        let result=[];
        if(root==null)
          return result;
        let queue=[root];  //根节点入队
        while(queue.length>0){    
          let arr=[];  
          //////根据题目要求，每层的放在一个数组。因此还需一个循环控制 层
          let len=queue.length;
          while(len){      //遍历当前层
            let node=queue.shift();  //删除并返回首元素
            arr.push(node.val);
            if(node.left)  queue.push(node.left)
            if(node.right)  queue.push(node.right)
            len--;   ///**易漏
          }
          result.push(arr);
        }
        return result;    //每层放在一个数组中
      }

```
补充： 自低向下的层序遍历只需将每层的元素插入到最前面（push改为unshift）

### 515. 在每个树行中找最大值

```js
var largestValues = function(root) {
    if(!root) return []
    let res=[]
    let queue=[root]
    while(queue.length){
        let len=queue.length
        let tmp=[]
        while(len){
            let t=queue.shift()
            if(t.left) queue.push(t.left)
            if(t.right) queue.push(t.right)
            tmp.push(t.val)
            len--
        }
        res.push(Math.max(...tmp))
    }
    return res
};
```

### 二叉树的镜像/翻转二叉树

```js
///////////递归
var mirrorTree = function(root) {
    let change=function(node){
        if(!node){
            return
        }
        //这里加入对子节点的判断能提高效率
        //避免对子节点的两个null交换
        if(node.left||node.right){
            let temp=node.left       //用[ ]解构交换更快
            node.left=node.right
            node.right=temp
            change(node.left)
            change(node.right) 
        }   
    }
    change(root)
    return root
};
```
### 对称二叉树

给定一个二叉树，检查它是否是镜像对称的。

```js
//1. 递归 dfs
var isSymmetric = function(root) {
    if(!root) return true
    let isEqual=function(left,right){
        if(!left&&!right) return true  //到底了,肯定为ture,否则中途会返回false
        if(!left||!right) return false  //一空一不空
        if(left.val!=right.val) return false
        //只剩值相等的情况，继续判断
        return isEqual(left.left,right.right)&&isEqual(left.right,right.left)
    }
    return isEqual(root.left,root.right)
};


//2. 迭代，层序遍历bfs
var isSymmetric = (root) => {
  if (!root) return true
  let queue = [root.left, root.right]
  while (queue.length) {          // 队列为空代表没有可入列的节点，遍历结束
    let len = queue.length        // 获取当前层的节点数
    for (let i = 0; i < len; i += 2) { // 一次循环出列两个
      let left = queue.shift()    // 左右子树分别出列
      let right = queue.shift()
      if ((left && !right) || (!left && right)) return false // 不满足对称
      if (left && right) { // 左右子树都存在
        if (left.val !== right.val) return false // 左右子树的根节点值不同
        queue.push(left.left, right.right) // 让左子树的left和右子树的right入列
        queue.push(left.right, right.left) // 让左子树的right和右子树的left入列
      }
    }
  }
  return true // 循环结束也没有遇到返回false
}
```

### 104. 二叉树的最大深度

dfs，bfs

```js
var maxDepth = function(root) {
    if(!root) return 0
    let right=maxDepth(root.right)
    let left=maxDepth(root.left)
    return Math.max(left,right)+1
};

  var maxDepth = function(root) {
    if(!root) return 0
    let res = 0
    let queue = [root]
    while(queue.length) {
      let len = queue.length
      while(len) {
        let node = queue.shift()
        if(node.left) queue.push(node.left)
        if(node.right)  queue.push(node.right)
        len --
      }
      res++
    }
    return res
  }
```
### N叉树的最大深度

**深度优先和广度优点**

1. 深度优先
递归求出每个子树的最大深度，再加上根节点

```
var maxDepth = function(root) {
    if(!root) return 0
    let max=0  //当前节点的最大高度
    for(let i in root.children){
        max=Math.max(max,maxDepth(root.children[i]))
    }
    return max+1
};
```

时间复杂度：每个节点遍历一次，所以时间复杂度是 O(N)，N为节点数。

空间复杂度：最坏情况下, 树完全非平衡，
例如 每个节点有且仅有一个孩子节点，递归调用会发生 NN 次（等于树的深度），所以存储调用栈需要 O(N)。
但是在最好情况下（树完全平衡），树的高度为log(N)。
所以在此情况下空间复杂度为 O(log(N))。

2.广度优先
算出总共有几层即可

```js
//广度优先遍历 BFS
var maxDepth = function(root) {
    if(!root) return 0
    let queue=[root],level=0
    while(queue.length){
        let len=queue.length
        while(len){
            let node=queue.shift()
            for(let i=0;i<node.children.length;i++){
                queue.push(node.children[i])
            }
            len--
        }
        level++
    }
    return level
};
```

- 时间复杂度：O(N)
- 空间复杂度：O(N)

### 111. 二叉树的最小深度

1.DFS   递归

```js
var minDepth = function(root) {
    if(!root) return 0;
    let left=minDepth(root.left);
    let right=minDepth(root.right);
    if(!root.left||!root.right) return left+right+1;
    return Math.min(left,right)+1;  //左右均不为空
};
```

2. BFS

BFS 更直观，因为是一层层遍历的，一旦发现当前层的某个节点没有子节点，就意味着当前处在最小深度了。

![image.png](https://pic.leetcode-cn.com/1597965123-xvjTsn-image.png)

BFS 代码

```js
const minDepth = (root) => {
  if (root == null) return 0;

  const queue = [root];
  let depth = 1;

  while (queue.length) {
    const levelSize = queue.length;
    for (let i = 0; i < levelSize; i++) {
      const cur = queue.shift();
      if (cur.left == null && cur.right == null) {
        return depth;
      }
      if (cur.left) queue.push(cur.left);
      if (cur.right) queue.push(cur.right);
    }
    depth++; // 肯定有下一层，如果没有早就return了
  }
};
```

### 路径总和

```js
var hasPathSum = function(root, sum) {
    const dfs = (root,path)=> {   // path存放当前路径和
        if(!root) return false
        if(!root.left&&!root.right) {
            return (path+root.val)===sum
        }
        return dfs(root.left,path+root.val)||dfs(root.right,path+root.val)
    }
    return dfs(root,0)
};
```

### **面试题34. 二叉树中和为某一值的路径**

```js
///// 自己写的
var pathSum = function(root, sum) {
    const res = []
    const dfs = (root,sum,path)=> {
        if(!root) return
        if(!root.left&&!root.right) {
            if(sum-root.val==0) {
                path.push(root.val)
                res.push(path) 
                return
            }
        }
        path.push(root.val)
        dfs(root.left,sum-root.val,path.slice())
        dfs(root.right,sum-root.val,path.slice())
        path.pop()  /////关键
    }
    dfs(root,sum,[])
    return res
};



var pathSum = function(root, sum) {
    let res = []
    let dfs=function(root, path, sum){
        if(!root) return
        sum -= root.val  
        if(sum == 0 && !root.left && !root.right){
            res.push([...path, root.val])
            return
        }
    path.push(root.val)
    dfs(root.left, path, sum)
    dfs(root.right, path, sum)
    path.pop()
}
    dfs(root, [], sum)
    return res
};
```

### 两颗二叉树合并

都有节点时求和

```js
var mergeTrees = function(t1, t2) {
    if(!t2) return t1  //t2空。t1是否空不重要
    if(!t1) return t2  //t1空,t2不空
    if(t1&&t2){
        t1.val+=t2.val
    }
   t1.left=mergeTrees(t1.left,t2.left)
   t1.right=mergeTrees(t1.right,t2.right)
    return t1   //新树用t1返回
};
```

### 数组转二叉排序树（平衡二叉树）

```js
var sortedArrayToBST = function(nums){
    if(nums.length==0){
        return null
    }
    let m=Math.floor((nums.length-1)/2)
    let p=new TreeNode(nums[m])
    p.left=sortedArrayToBST(nums.slice(0,m))
    p.right=sortedArrayToBST(nums.slice(m+1))
    return p
};
```

### 二叉树公共祖先

（注意是二叉搜索树，容易确定在哪个子树上）

1. 从根节点开始遍历树
2. 如果节点 ![[公式]](https://www.zhihu.com/equation?tex=+p) 和节点 ![[公式]](https://www.zhihu.com/equation?tex=q) 都在右子树上，那么以右孩子为根节点继续 1 的操作
3. 如果节点 ![[公式]](https://www.zhihu.com/equation?tex=p) 和节点 ![[公式]](https://www.zhihu.com/equation?tex=q) 都在左子树上，那么以左孩子为根节点继续 1 的操作
4. 如果条件 2 和条件 3 都不成立，已经找到

- 临界条件：
  - 根节点是空节点
  - 根节点是q节点
  - 根节点是p节点
- 从左右子树分别进行递归，即查找左右子树上是否有p节点或者q节点
  - 左右子树均无p节点或q节点
  - 左子树找到，右子树没有找到，返回左子树的查找结果
  - 右子树找到，左子树没有找到，返回右子树的查找结果
  - 左、右子树均能找到,说明此时的p节点和q节点在当前root节点两侧，返回root节点

```js
var lowestCommonAncestor = function(root, p, q) {
    if(!root||root===p||root===q)
        return root;
    let left = lowestCommonAncestor(root.left,p,q);
    let right = lowestCommonAncestor(root.right,p,q);
    if(left&& right)  //分别在root的左右两边
        return root;
    return left!= null ? left : right;  //都在左子树或都在右子树上
};
```

### 剑指 Offer 33. 二叉搜索树的后序遍历序列

```js
//分治  每次判断左子树所有节点均小于当前根节点，右子树均大于当前根节点
//
var verifyPostorder = function (postorder){
    let len=postorder.length
    if(len<3) return true   //长度0，1，2时，一定能构造出来满足条件的树
    root=postorder[len-1]

    //划分,找第一个比根节点大的
    let i=0;
    for(;i<len-1;i++){
        if(postorder[i]>root) break
    }
    //右子树是否满足
    let flag=true
    for(let j=i+1;j<len;j++){
        if(postorder[j]<root) flag = false
    }
    ////注意这里的区间
    if(flag) return verifyPostorder(postorder.slice(0,i))&&verifyPostorder(postorder.slice(i,len-1))
    (else) return false
}
```

### 剑指 Offer 07. 重建二叉树

```js
var buildTree = function(preorder, inorder) {
    if(!preorder.length) return null  //上个节点为叶节点，左右为null
    let node=new TreeNode(preorder[0]),i=0
    for(;i<inorder.length;i++){
        if(inorder[i]===node.val) break
    }
    node.left=buildTree(preorder.slice(1,1+i),inorder.slice(0,i))
    node.right=buildTree(preorder.slice(1+i),inorder.slice(i+1))
    return node
};
```

### 剑指 Offer 26. 树的子结构

​	判断A是不是B的子树

```js
var isSubStructure = function(A, B) {
    if(!A||!B) return false
    if(A.val===B.val&& find(A.left,B.left)&&find(A.right,B.right)) return true  //根节点相同并且左右子树都相同（find递归）
    else{
        return isSubStructure(A.left,B)||isSubStructure(A.right,B)
    }
};
let find=function(A,B){
     if(!B) return true
    if(!A) return false
    if(A.val!==B.val){
        return false 
    }
    return find(A.left,B.left)&&find(A.right,B.right)
}
```

### [100. 相同的树](https://leetcode-cn.com/problems/same-tree/)

比较两颗二叉树是否相同

```
var isSameTree = function(p, q) {
    if(!q&&!p) {
        return true
    }else if(!p||!q) {
        return false
    }
    if(p.val!==q.val) {
        return false
    }
    return isSameTree(p.left,q.left)&&isSameTree(p.right,q.right)
};
```

### 另一个树的子树

```js
/////转为比较s的左/右子树是否与t相同(100.相同的树)
let isSame = function(s,t) {
    if(!s&&!t) return true
    if(!s||!t) return false
    return s.val==t.val&&isSame(s.left,t.left)&&isSame(s.right,t.right)
}
var isSubtree = function(s, t) {
  if(!s) return false
  if(isSame(s,t)) return true
  return isSubtree(s.left,t)||isSubtree(s.right,t)
};
```

### [654. 最大二叉树](https://leetcode-cn.com/problems/maximum-binary-tree/)

数组->二叉树    由数组构造二叉树

```js
var constructMaximumBinaryTree = function(nums) {
    if(!nums.length) return null
    let max = Math.max(...nums)
    const pos = nums.indexOf(max)
    let left = nums.slice(0,pos)
    let right = nums.slice(pos+1)
    let node = new TreeNode(max)
    node.left = constructMaximumBinaryTree(left)
    node.right = constructMaximumBinaryTree(right)
    return node
};
```

## 滑动窗口

### 最长不含重复字符的子字符串

```js
var lengthOfLongestSubstring = function(s) {
    if(s.length==1) return 1
    let left=0,right=0,len=0
    let map=new Map()
    for(;right<s.length;right++){
        if(!map.has(s[right])){    //不重复
            map.set(s[right])
        }else{     //移动左指针,判断是否为最大长度
            len=Math.max(len,right-left) //每次重复都产生一段新的长度
            while(s[left]!==s[right]){
                map.delete(s[left])
                left++
            }
            left++
        }
    }
    len=Math.max(len,right-left)  ///考虑到else未执行到的情况
    return len
};
```

### 面试题57 - II. 和为s的连续正数序列

```js
var findContinuousSequence = function(target) {
    let left=1,right=1
    let res=[]
    let sum=0
    while(left<=Math.floor(target/2)){
        if(sum<target){    //只可能向右增长
            sum+=right
            right++  //注意此时没有包括右边的！！
        }
        if(sum==target){
            let tmp=[]
            for(let i=left;i<right;i++){
                tmp.push(i)
            }
            res.push(tmp)
            sum=sum-left   //****左减少一些，向右扩张
            left++
        }
        if(sum>target){  //只可能向左减少，因为过程是逐渐向右增长的
            sum-=left
            left++
        }
    }  
    return res
};

```

## 动态规划

### **最长公共子序列

```
/////  动态规划
///   状态转移方程：i,j位置不相同时， dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j])
var longestCommonSubsequence = function(text1, text2) {
    const m = text1.length, n = text2.length
    let dp = Array.from(Array(m+1),()=>Array(n+1).fill(0))
    for(var i = 1;i<=m;i++) {
        for(var j=1;j<=n;j++) {
            if(text1[i-1]==text2[j-1]){   // 注意dp和对应数组的下标！！
                dp[i][j]=dp[i-1][j-1]+1
            } 
            else {
                dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j]) 
            }
        }
    }
    return dp[i-1][j-1]
};
```



### 打家劫舍

```js
var rob = function(nums) {
    if(!nums.length) return 0
    if(nums.length===1) return nums[0]
    let dp = [], max = 0
    dp[0] = nums[0]
    dp[1] = Math.max(nums[0],nums[1])
    for(let i = 2; i<nums.length; i++) {
        dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i])  // 核心！！！！！
    }
    let last = nums.length-1
    return dp[last]>dp[last-1]?dp[last]:dp[last-1]
};
```

### 70. 爬楼梯

```js
//动态规划 dp[n]=dp[n-1]+dp[n-2]
var climbStairs = function(n) {
    let dp=[]
    dp[0]=1,dp[1]=1
    for(let i=2;i<=n;i++){
        dp[i]=dp[i-1]+dp[i-2]
    }
    return dp[n]
};
```

### 面试题46. 把数字翻译成字符串

```js
//动态规划，类似于跳台阶问题  加个判断条件
var translateNum = function(num) {
	num=num+''    //转字符串
	let dp=[1]
	for(let i=1;i<num.length;i++){
		if(num[i-1]+num[i]<26 && num[i-1]!== '0'){
			if(i==1){
				dp[i]=2
				continue
			}
            dp[i]=dp[i - 1]+dp[i - 2] 
        }else{
            dp[i]=dp[i - 1];
        }
	}
	return dp[num.length-1]
};
```

### 面试题47. 礼物的最大价值

```js
////dp  dp[i][j] = grid[i][j] + max(dp[i - 1][j], dp[i][j - 1]) 
var maxValue = function(grid) {
    let dp=[];
    let rows=grid.length;
    let cols=grid[0].length;
    for(let i=0;i<rows;i++){     //初始化
        dp[i]=[];   //试试new array??
        for(let j=0;j<cols;j++){
            dp[i][j]=0;
        }
    }
    dp[0][0]=grid[0][0]
    //边界需要 上界，左界，原点三个判断做不同处理
    //不如先遍历边界，确定边界值，再遍历内部
    for(let i=1;i<rows;i++){
        dp[i][0]=dp[i-1][0]+grid[i][0];
    }
    for(let j=1;j<cols;j++){
        dp[0][j]=dp[0][j-1]+grid[0][j];
    }
    for(let i=1;i<rows;i++){
        for(let j=1;j<cols;j++){
            dp[i][j]=grid[i][j]+Math.max(dp[i - 1][j], dp[i][j - 1])
        }
    }
    return dp[rows-1][cols-1]
}
```

### 面试题63. 股票的最大利润

```js
//1. 记录当前最小的min   2.当天价格减去min，与最大利润比较，若是大，则更新最大利润
var maxProfit = function(prices) {
    let dp=[];
    let max=Number.MIN_SAFE_INTEGER;
    let min=prices[0];
    for(let i=1;i<prices.length;i++){
        dp[i]=prices[i]-min;
        if(prices[i]<min){
            min=prices[i];
        }
        max=Math.max(dp[i],max);
    }
    if(max<0)  return 0;
    else return max;
};
```

### 面试题 16.17. 连续数列

```js
var maxSubArray = function(nums) {
    let dp=[],res;
    dp[0]=res=nums[0];
    for(let i=1;i<nums.length;i++){   
        dp[i]=nums[i];
        if(dp[i-1]>0){
            dp[i]+=dp[i-1];
        }
        res=Math.max(res,dp[i]);
    }
    return res;
}
```

### 丑数

```js
var nthUglyNumber = function(n) {
	let dp=[];
	dp[1]=1;    //第一个丑数
	let i=2;
	let p1=1,p2=1,p3=1;  //指针   每个丑数的2，3，5倍
	while(i<=n){
		dp[i]=Math.min(dp[p1]*2,dp[p2]*3,dp[p3]*5);
		if(dp[i]==dp[p1]*2) p1++;
		if(dp[i]==dp[p2]*3) p2++;
		if(dp[i]==dp[p3]*5) p3++;
		i++;
	} 
	 return dp[n];
};
```

二、动态规划
确定dp[i][j]是否是回文数，只需要dp[i+1][j-1]是回文数并且s[i] == s[j]即可。
但是长度为0或1的回文传需要特殊处理，即j-i < 2;
因为知道dp[i]需要先知道dp[i+1],所以i需要从大到小开始遍历
因为知道dp[j]需要先知道dp[j-1],所以j需要从小到大开始遍历
即: dp[i][j] = s[i] == s[j] && ( dp[i+1][j-1] || j - i < 2)

```js
var longestPalindrome = function(s) {
    let res=''
    let len=s.length
    let dp=new Array(len)
    for(let i=0;i<len;i++){
        dp[i]=new Array(len).fill(0)  //列
    }
    //或let dp = Array.from(new Array(len), () => new Array().fill(0));
    for(let i=len-1;i>=0;i--){
        for(let j=i;j<len;j++){  //dp存储布尔值，表示是否是回文串
            dp[i][j] = (j-i<2||dp[i+1][j-1])&&(s[i]===s[j]) 
            if(dp[i][j]&&j-i+1>res.length) res=s.substring(i,j+1)
        }
    }
    return res
};
```

时间复杂度: O(N^2)
空间复杂度: 0(N^2)

## 贪心

#### 1403. 非递增顺序的最小子序列

输入：nums = [4,3,10,9,8]     输出：[10,9] 
解释：子序列 [10,9] 和 [10,8] 是最小的、满足元素之和大于其他各元素之和的子序列。但是 [10,9] 的元素之和最大。 

```js
var minSubsequence = function(nums) {
    let res=[]
    nums.sort((a,b)=>a-b)
    let sum=nums.reduce((a,b)=>a+b)
    let tmp=0
    for(let i=nums.length-1;i>=0;i--){
        tmp+=nums[i]
        res.push(nums[i])
        if(tmp>sum-tmp) return res   //当前和大于剩余元素和  
    }
};
```

## 回溯

### 全排列1

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

```js
var permute = function(nums) {
    let res=[]
    let backtrack=function(path){
        if(path.length==nums.length){
            res.push(path)
            return
        } 
        for(let i=0;i<nums.length;i++){
            if(!path.includes(nums[i])){
                path.push(nums[i])
                backtrack(path.slice()) //该元素的后续排序组合全部完成，弹出该元素
                path.pop() 
            }
        }
    }
    backtrack([])
    return res
};
```

### 全排列2

无重复字符串全排列

```js
var permutation = function(S) {
    let res = []
    let backTrack = function(path) {
        if(path.length==S.length) {
            res.push(path)
            return
        }
        for(let i in S) {
            if(path.indexOf(S[i])==-1) {
                path.concat(S[i])
                backTrack(path)
                path.slice(0,path.length-1)
            }
        }
    }
    backTrack("")
    return res
};
```

### 字符串全排列

用了set

```js
var permutation = function(s){
    let res=new Set()
    //这块写外面这里运行会出错，本地没事
    let backtrack=function(curr,s){   //curr后面拼接s
        if(!s.length)
            res.add(curr)
        else{
            for(let i=0;i<s.length;i++){
                //循环中curr不能改...
                let t=curr+s[i]
                backtrack(t,s.slice(0,i)+s.slice(i+1)) //去除当前字符，其他字符继续向后拼接
            }
        }
    }
    backtrack('',s)
    return [...res]
}
```

### 幂集

```
 输入： nums = [1,2,3]
 输出：
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
var subsets = function(nums) {
    let res = [[]]
    for(let i in nums) {
        for(let j in res) { // 遍历res每一项，把当前nums[i]拼上
            res.push(res[j].concat(nums[i]))
        }
    }
    return res
};

```

### [面试题 08.09. 括号](https://leetcode-cn.com/problems/bracket-lcci/)

给定数量，生成有效括号组合

```js
// 回溯+剪枝
var generateParenthesis = function(n) {
    const res = []
    const dfs = (l,r,str) => {   // 左右剩余括号 当前字符串
        if(str.length==n*2) {
            res.push(str)
            return
        }
        if(l) {
            dfs(l-1,r,str+'(')
        }
        if(r>l) {    // 剩的右括号多时才可以选
            dfs(l,r-1,str+')')
        }  
    }
    dfs(n,n,'')
    return res
};
```

### 17. 电话号码的字母组合

```js
var letterCombinations = function(digits){
  let res=[];
    let a={
        2:'abc',
        3:'def',
        4:'ghi',
        5:'jkl',
        6:'mno',
        7:'pqrs',
        8:'tuv',
        9:'wxyz'
    }
      for(let i=0;i<digits.length;i++){
        let temp=[];
        let word=a[digits[i]];
        if(res.length>0){  //有数字，开始result中与当前word组合，没有就先放入
          for(let char of word){  //对每个字符分别与每个res组合
            for(let t of res){  //result之前的数与新单词的组合，再放入result
              temp.push(t+char)
            }
          }
          res=temp;
        }else{
          res.push(...word);  //['abc']与['a','b','c']的差别
          // let s='abcd';
          // let a=[]
          //  a.push(...s)
          //  console.log(a)
        }
    } return res;
};
////////求笛卡尔积方法   reducer
// var descartFn = function(nums) {   //a:累计
// 	let res= nums.reduce((a, curr)=> {
// 		let m = a.map(item=> curr.map(i=> [i].concat(item)))
// 		return  m.reduce((c, d)=> c.concat(d))
// 	})
//   return res;
// }
// console.log( descartFn([[1,5],[2,3],[3,4]]));
```

### 面试题 16.20. T9键盘

```js
var getValidT9Words = function(num, words){
    let f,res=[]
    let map={
        2:['a','b','c'],
        3:['d','e','f'],
        4:['g','h','i'],
        5:['j','k','l'],
        6:['m','n','o'],
        7:['p','q','r','s'],
        8:['t','u','v'],
        9:['w','x','y','z']
    }
    for(let word of words){   //检查每个单词
        f=true
        for(let i=0;i<num.length;i++){ //检查当前单词中的每个字符
            if(!map[num[i]].includes(word[i])){
                f=false
                break
            }
        }
        if(f) res.push(word)
    }
    return res
};
```

顺时针打印矩形***

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

```js
var spiralOrder = function (matrix) {
    if (!matrix.length) return []
    if (!matrix[0].length) return []

    let height = matrix.length;
    let width = matrix[0].length;
    let result = [];
    //左上角坐标(startX,startY)，右下角坐标(endX,endY)
    let startX = 0, startY = 0, endX = width - 1, endY = height - 1;
    while (endX >= startX && endY >= startY) {
        for (let i = startX; i <= endX; i++) {
            result.push(matrix[startY][i]);
        }
        for (let j = startY + 1; j <= endY; j++) {
            result.push(matrix[j][endX]);
        }
        for (let i = endX - 1; i >= startX; i--) {
            if (endY === startY) return result
            result.push(matrix[endY][i]);
        }
        for (let j = endY - 1; j >= startY + 1; j--) {
            if (startX === endX) return result
            result.push(matrix[j][startX]);
        }
        startX++;
        startY++;
        endX--;
        endY--;
    }
    return result
};
```

## 面试题29. 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

```js
var spiralOrder = function (matrix) {
    
    if (!matrix.length) return []
    if (!matrix[0].length) return []

    let height = matrix.length;
    let width = matrix[0].length;
    let result = [];
    //左上角坐标(startX,startY)，右下角坐标(endX,endY)
    let startX = 0, startY = 0, endX = width - 1, endY = height - 1;
    while (endX >= startX && endY >= startY) {
        for (let i = startX; i <= endX; i++) {
            result.push(matrix[startY][i]);
        }
        for (let j = startY + 1; j <= endY; j++) {
            result.push(matrix[j][endX]);
        }
        for (let i = endX - 1; i >= startX; i--) {
            if (endY === startY) return result //***只剩一列，没有行了*
            result.push(matrix[endY][i]);
        }
        for (let j = endY - 1; j >= startY + 1; j--) {
            if (startX === endX) return result //只剩一行，没有列了
            result.push(matrix[j][startX]);
        }
        startX++;
        startY++;
        endX--;
        endY--;
    }
    return result
};
```

## 数学

### 202.快乐数

```js
var isHappy = function(n) {
   let s
   let map=new Map()  //检查是否产生了循环
    while(n!=1){
        let sum=0
        s=n+''
        for(let i of s){
            sum=sum+Math.pow(parseInt(i),2)
        }
        if(!map.has(sum)) map.set(sum)
        else{
            return false
        }
        n=sum
    }
    return true
};
```

## 脑筋急转弯

### [面试题 02.03. 删除中间节点](https://leetcode-cn.com/problems/delete-middle-node-lcci/)

相当于它变成了下一个节点，再删除了它的下一个节点

```js
var deleteNode = function(node) {
    node.val = node.next.val
    node.next = node.next.next
};
```

## 应用

删除 最久未访问过的项目

### 面试题 16.25. LRU缓存

```js
var LRUCache = function(capacity) {
    this.capacity=capacity
    this.map=new Map()
};

LRUCache.prototype.get = function(key) {
    if(this.map.has(key)){
        //使用了，将它放到最后面
        let t=this.map.get(key)
        this.map.delete(key)
        this.map.set(key,t)
        return t
    } 
    return -1
};

/////没有考虑到put时访问已存在的key的情况，此时也需换位(放最后)
LRUCache.prototype.put = function(key, value) {
    if(this.map.has(key)){
        this.map.delete(key)
        this.map.set(key,value)
    }
    else if(this.map.size==this.capacity){
        //第一个m.keys().next().value   或map.keys()转数组
        this.map.delete(this.map.keys().next().value) ////第一个键
        this.map.set(key,value)
    }else{
        this.map.set(key,value)
    } 
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```

### 汉诺塔

```js
var hanota = function(A, B, C) {
   let move=function(length,A,B,C){
       if(length==1){
           C.push(A.pop())
           return
       }
       //把前length-1个移动到B
       move(length-1,A,C,B)
       //移动完之后剩余的一个挪过来
       C.push(A.pop())
       //把前n-1个由B挪到C
       move(length-1,B,A,C)
   }
   move(A.length,A,B,C)
};
```

### **实现千分位的分隔（有小数，负数）

1. 最简单的方式，API一行解决
   `num.toLocaleString()`
2. 正则表达式(只有整数部分)
   `num.toString().replace(/(\d)(?=(?:\d{3})+$)/g,'$1,')`
   将该num转化为字符串后，全局（/g）正向匹配，看是否符合断言（?=(?:\d{3})+$）部分，直到匹配结束。即遇到 数字 + 该数字后面紧跟连续的三位数字（并且不管这连续的三位数字出现多少次），符合则在该数字（’$1’）后加入逗号，替换的时候忽略（？：）这连续的三位数.
3. 从右往左，每隔三位，加个逗号

```javascript
function format(num){
  num=num+'';//数字转字符串
  const arr=num.split('.')  //处理数字中的小数点
  num=arr[0]   //求小数点前面的整数位
  let str="";//字符串累加
  for(let i=num.length- 1,j=1;i>=0;i--,j++){   //从后往前数三位
      if(j%3==0 && i!=0){//每隔三位加逗号，过滤正好在第一个数字的情况
          str+=num[i]+",";//加千分位逗号
          continue;
      }
      str+=num[i];//倒着累加数字
  }
  str=str.split('').reverse().join("");//字符串=>数组=>反转=>字符串
  arr[1]&&(str+='.'+arr[1]) //还原小数位
  return str
}
```

### 杨辉三角

处理成直角三角形

思路：第一列和最后一列赋值1，其余列为row[j]= res\[i-1][j-1]+res\[i-1][j]

```js
/**
 * @param {number} numRows
 * @return {number[][]}
 */
var generate = function(numRows) {
    if(!numRows) return []
    let res = [[1]]
    for(let i = 1; i<numRows; i++) {
        let row = []
        row[0] = 1
        row[i] = 1
        for(let j=1; j<i; j++) {
        row[j] = res[i-1][j-1]+res[i-1][j]
        }
        res.push(row)
    }
    return res
};
```



## 其它

### 不使用临时变量交换值

https://blog.csdn.net/weixin_44827421/article/details/93717370

解法：

加减法 a=b-a;b=b-a;a=b+a （要考虑溢出问题）
数组反转 numbers.reverse(); （取巧！！！）
es6新特性 [a,b]=[b,a]
y=[x, x = y] [0]
异或 a=a^b;b=a^b;a=a^b;

### 数组去重方法(十几种)

https://blog.csdn.net/qq_29483485/article/details/83115224?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control

```js
 //判断英文，数字，中文
var result1 = !isNaN(input);
var result2 = ((input >= 'A' && input <= 'Z') || (input >= 'a' && input <= 'z'));
var result3 = (input >= '\u4e00' && input <= '\u9fa5');

num.toString(2)  //转字符串同时转2进制

let s='abcd';
let a=[]
a.push(...s)
//   a=s.split('')
console.log(a,s)

arr.toString();
作用：将数组转换为字符串，并返回转换后的结果。   //得到的字符串包括逗号 而join更灵活

ram=Math.floor(Math.random()*(max-min+1))+min //包含m,n   不加1，不包含n

let dp = Array.from(new Array(5), () => new Array());  //创建2维数组，每一行是一个空[]
```

**字符串翻转：用split变成数组后调用数组的reverse方法**

str.split('').reverse().join('')

Number.MAX_SAFE_INTEGER / Number.MIN_SAFE_INTEGER

**第一个只出现一次的字符**  

效率超低

```js
//map
var firstUniqChar = function(s){
    if(s.length){
        let map=new Map()
        for(let i of s){ //用in效率超低 5%？ for>for of>for in
            if(map.has(i)){
                map.set(i,false)
            }else{
                map.set(i,true)
            }              
        }
        for(let key of map.keys()){
            if(map.get(key)) return key
        }
    }
    return ' '
};
```

除了map, 考虑用数组，26个位置存储次数

```js
var firstUniqChar = function(s) {
    let arr = new Array(26).fill(0);
for (let c of s) {
    arr[c.charCodeAt() - 97] += 1;
}
for (let c of s) {
    if (arr[c.charCodeAt() - 97] == 1) {
        return c;
    }
}
return ' ';
};
```

### [面试题 01.01. 判定字符是否唯一](https://leetcode-cn.com/problems/is-unique-lcci/)

```
1. set/map 空间复杂度
2. indexOf, lastIndexOf
3. 排序，比较相邻的
4. 双指针(双重循环)
5. 位运算
```



### Map对象的方法

https://blog.csdn.net/wuyujin1997/article/details/88739311

### 循环、迭代与递归

http://www.nowamagic.net/librarys/veda/detail/2324

### 不用数组，实现栈结构 

用对象 count记录栈顶

```js
class Stack {
  constructor() {
    this.item={}
    this.count=0
  }
  //栈顶添加
  push(item){
    this.item[this.count]=item
    this.count++
  }
  //删除
  pop(){?
    if(this.isEmpty()){
      return undefined
    }
    this.count--
    const result = this.item[this.count]
    delete this.item[this.count]
    return result
  }
  //查看栈顶元素
  peek() {
    if(this.isEmpty()){
      return undefined
    }
    return this.item[this.count]
  }
  //清空栈
  clear() {
    this.item = {}
    this.count=0
  }
  //栈是否为空
  isEmpty() {
    return this.count===0
  }
  //查看栈长度
  size(){
    return this.count
  }
  //创建toString方法
  toString() {
    if (this.isEmpty()) {
      return ``
    }
    let str = `${this.item[0]}`
    for (let i =1;i < this.count; i++){
      str+=`,${this.item[i]}`
    }
    return str
  }
}
```

## 排序

### 桶排序；

原理：将数组分到有限数量的桶里。每个桶再个别排序（<u>有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序</u>）

1. 什么时候最快

当输入的数据可以均匀的分配到每一个桶中。

2. 什么时候最慢

当输入的数据被分配到了同一个桶中。

3. 示意图

元素分布在桶中：

![img](https://www.runoob.com/wp-content/uploads/2019/03/Bucket_sort_1.svg_.png)

然后，元素在每个桶中排序：

![img](https://www.runoob.com/wp-content/uploads/2019/03/Bucket_sort_2.svg_.png)

**代码：**

```js
function bucketSort(arr, bucketSize) {
    if (arr.length === 0) {
      return arr;
    }

    let minValue = arr[0],maxValue = arr[0];
    for (let i = 1; i < arr.length; i++) {
      if (arr[i] < minValue) {
          minValue = arr[i];                // 输入数据的最小值
      } else if (arr[i] > maxValue) {
          maxValue = arr[i];                // 输入数据的最大值
      }
    }

    //桶的初始化
    var DEFAULT_BUCKET_SIZE = 5;            // 桶的默认数量为5
    bucketSize = bucketSize || DEFAULT_BUCKET_SIZE;
    var bucketCount = Math.floor((maxValue - minValue) / bucketSize) + 1;  
    var buckets = new Array(bucketCount);
    for (let i = 0; i < buckets.length; i++) {
        buckets[i] = [];
    }

    //利用映射函数将数据分配到各个桶中
    for (let i = 0; i < arr.length; i++) {
        buckets[Math.floor((arr[i] - minValue) / bucketSize)].push(arr[i]);
    }
    arr.length = 0;    //清空，重新放入排序好的数
    for (let i = 0; i < buckets.length; i++) {
        insertionSort(buckets[i]);            // 对每个桶进行排序，这里使用了插入排序
        for (let j = 0; j < buckets[i].length; j++) {
            arr.push(buckets[i][j]);                      
        }
    }
    return arr;
}

//补充：插入排序
function insertionSort(arr){
  let length = arr.length;
  for(let i = 1; i < length; i++) {
    let temp = arr[i];
    let j = i;
    for(; j > 0; j--) {
      if(temp >= arr[j-1]) {
        break;      // 当前考察的数大于前一个数，证明有序，退出循环
      }
      arr[j] = arr[j-1]; // 将前一个数复制到后一个数上
    }
    arr[j] = temp;  // 找到考察的数应处于的位置
  }
  return arr;
}
```

###  