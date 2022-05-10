# 9Hash

## 哈希函数

哈希函数通过hashCode把参数转化为数值，一般hashcode是通过特定编码方式，可以将其他数据格式转化为不同的数值，这样就把参数映射为哈希表上的索引数字了。

## 哈希碰撞

两个参数同时影射到哈希表的索引下标1的位置

### 拉链法

发生冲突的元素都被存储在链表中。 这样我们就可以通过索引找到小李和小王了

（数据规模是dataSize， 哈希表的大小为tableSize）

### 线性探测法

```
使用线性探测法，一定要保证tableSize大于dataSize。 我们需要依靠哈希表中的空位来解决碰撞问题。
例如冲突的位置，放了小李，那么就向下找一个空位放置小王的信息。所以要求tableSize一定要大于dataSize ，要不然哈希表上就没有空置的位置来存放 冲突的数据了。
```

## 三种常见的哈希结构

### 1、数组

如果限制数组长度，就可以用数组来替代哈希表；若哈希值数量少、分散，则数组会造成很大的空间浪费，因此可以使用set。

### 2、hash map

```java
Map<> map = new HashMap<>();

Map.getOrDefault(Object key, V defaultValue)方法的作用是：
  当Map集合中有这个key时，就使用这个key值；
  如果没有就使用默认值defaultValue。
Map.get() 方法返回指定键所映射的值。如果此映射不包含该键的映射关系，则返回 null。
```

### 3、hashset

```java
Set<String> set = new HashSet<String>();
set.contains()
substring(a,b)
substring() 方法返回字符串的子字符串。
```

## 1、有效的字母异位词

```java
class Solution {
    public boolean isAnagram(String s, String t) {

        int[] record = new int[26];
        for (char c : s.toCharArray()) {
            record[c - 'a'] += 1;
        }
        for (char c : t.toCharArray()) {
            record[c - 'a'] -= 1;
        }
        for (int i : record) {
            if (i != 0) {
                return false;
            }
        }
        return true;
    }
}
//
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        char[] str1 = s.toCharArray();
        char[] str2 = t.toCharArray();
        Arrays.sort(str1);
        Arrays.sort(str2);
        return Arrays.equals(str1, str2);
    }
}

```

### 2赎金信

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        char[] str1= ransomNote.toCharArray();
        char[] str2= magazine.toCharArray();
        int[] record = new int[26];
        for(int i : str2){
            record[i-'a']+=1;
        }
        for(int i :str1){
            record[i-'a']-=1;
        }
        for(int i :record){
            if(i < 0){
                return false;
            }
        }
        return true;
    }
}
```



## 2、两个数组的交集

```java
//hashset 去重复
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<Integer>();
        Set<Integer> ret = new HashSet<Integer>();
        for(int i : nums1){
            set.add(i);
        }
        for(int i :nums2){
            if(set.contains(i)){
                ret.add(i);
            }
        }
        int[] ret1 = new int[ret.size()];
        int index=0;
        for(int i:ret){
            ret1[index++] = i;
        }
        return ret1;
    }
}
```

### 3、两个数组的交集2

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
      
        ArrayList<Integer> res1 = new ArrayList<Integer>();
        ArrayList<Integer> res2 = new ArrayList<Integer>();
        
        res1=res(nums1,nums2);
        res2=res(nums2,nums1);
        int[] res3= new int[res1.size()];
        int[] res4= new int[res2.size()];
        res3=res12(res1);
        res4=res12(res2);
        if(res3.length>res4.length){
            return res3;
        }else{
            return res4;
        }

    }
    public ArrayList res(int[] nums1, int[] nums2){
        Set<Integer> set = new HashSet<Integer>();
        ArrayList<Integer> ret = new ArrayList<Integer>();
        for(int i : nums1){
            set.add(i);
        }
        for(int i :nums2){
            if(set.contains(i)){
                ret.add(i);
            }
        }
        return ret;
    }
    public int[] res12(ArrayList<Integer> res3) {
        int[] res4 = new int[res3.size()]; 
        int index=0;
        for(int i:res3){
            res4[index++] = i;
        }
        return res4;
    }
}
```

## 3、快乐数

```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();
        while(n != 1 && !set.contains(n)){  
            set.add(n);
            n= getnew(n);
        }
        if (n==1){return true;}
            else{  return false;}
    }
    public int getnew(int n){
        int totalnum=0;
        while(n > 0){
            int s = n % 10;
            n= n/10;
            totalnum += s*s;
        } 
        return totalnum;
    }
}
```

## 4、两数之和

```java
public int[] twoSum(int[] nums, int target) {
    int[] res = new int[2];
    if(nums == null || nums.length == 0){
        return res;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for(int i = 0; i < nums.length; i++){
        int temp = target - nums[i];
        if(map.containsKey(temp)){
            res[1] = i;
            res[0] = map.get(temp);
        }
        map.put(nums[i], i);
    }
    return res;
}
```

## 5、四数相加

```java
首先定义 一个unordered_map，key放a和b两数之和，value 放a和b两数之和出现的次数。
遍历大A和大B数组，统计两个数组元素之和，和出现的次数，放到map中。
定义int变量count，用来统计 a+b+c+d = 0 出现的次数。
在遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就用count把map中key对应的value也就是出现次数统计出来。
最后返回统计值 count 就可以了
//
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        int temp;
        int res = 0;
        //统计两个数组中的元素之和，同时统计出现的次数，放入map
        for (int i : nums1) {
            for (int j : nums2) {
                temp = i + j;
                if (map.containsKey(temp)) {
                    map.put(temp, map.get(temp) + 1);
                } else {
                    map.put(temp, 1);
                }
            }
        }
        //统计剩余的两个元素的和，在map中找是否存在相加为0的情况，同时记录次数
        for (int i : nums3) {
            for (int j : nums4) {
                temp = i + j;
                if (map.containsKey(0 - temp)) {
                    res += map.get(0 - temp);
                }
            }
        }
        return res;
    }
}
```

## 6、三数之和

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
 
        ArrayList<List<Integer>> res= new ArrayList<List<Integer>>();
        for(int i =0;i<nums.length-2;i++){
            if(nums[i]>0)break;
            if(i>0 && nums[i]==nums[i-1])continue;
            int l2=i+1;
            int l3=nums.length-1;
        while(l2<l3){
            int sum=nums[i]+nums[l2]+nums[l3];
            if (sum<0){
                 while(l2 < l3 && nums[l2] == nums[++l2]);

            }else if (sum>0){
                while(l2 < l3 && nums[l3] == nums[--l3]);
            }else if(sum==0){
                res.add(new ArrayList<Integer>(Arrays.asList(nums[i],nums[l2],nums[l3])));
                while(l2 < l3 && nums[l2] == nums[++l2]);
                while(l2 < l3 && nums[l3] == nums[--l3]);
            }
        }
        }
        return res;
    }
}
```

## 7、四数之和

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
       
        for (int i = 0; i < nums.length; i++) {

            if (i > 0 && nums[i - 1] == nums[i]) {
                continue;
            }
            
            for (int j = i + 1; j < nums.length; j++) {

                if (j > i + 1 && nums[j - 1] == nums[j]) {
                    continue;
                }

                int left = j + 1;
                int right = nums.length - 1;
                while (right > left) {
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum > target) {
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        result.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;

                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
}
```

