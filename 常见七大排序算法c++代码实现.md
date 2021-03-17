@[TOC](十大排序算法C++代码实现)

# 排序的分类

> 稳定排序：如果a原本在b前面，而a=b，排序之后a仍然在b的前面；其中稳定得排序有**冒泡排序、插入排序、归并排序、计数排序、桶排序、基数排序。**
> 不稳定排序：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；不稳定的排序有**选择排序、希尔排序、快速排序和堆排序**。
> 内排序 ：所有排序操作都在内存中完成； 
> 外排序 ：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；

各算法的对比如下表所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201213213817414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1paY3BwYw==,size_16,color_FFFFFF,t_70)

> n: 数据规模；k: “桶”的个数；In-place: 占用常数内存，不占用额外内存；Out-place: 占用额外内存
> 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201213214059297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1paY3BwYw==,size_16,color_FFFFFF,t_70)
**比较排序和非比较排序的区别：**

> 常见的**快速排序、归并排序、堆排序、冒泡排序** 等属于**比较排序** 。在排序的最终结果里，元素之间的次序依赖于它们之间的比较。每个数都必须和其他数进行比较，才能确定自己的位置 。

在冒泡排序之类的排序中，问题规模为n，又因为需要比较n次，所以平均时间复杂度为O(n²)。在归并排序、快速排序之类的排序中，问题规模通过分治法消减为logN次，所以时间复杂度平均O(nlogn)。

>  **计数排序、基数排序、桶排序**则属于**非比较排序**；非比较排序是通过确定每个元素之前，应该有多少个元素来排序。针对数组arr，计算arr[i]之前有多少个元素，则唯一确定了arr[i]在排序后数组中的位置。

非比较排序只要确定每个元素之前的已有的元素个数即可，所有一次遍历即可解决。算法时间复杂度O(n)。
## 1.冒泡排序
### 1.1 算法描述
从小到大排序：
> 步骤1: 比较相邻的元素。如果第一个比第二个大，就交换它们两个；
> 步骤2:对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数； 
> 步骤3:针对所有的元素重复以上的步骤，除了最后一个； 
> 步骤4: 重复步骤1~3，直到排序完成。

### 1.2 动画演示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201213214854130.gif#pic_center)
### 1.3 代码实现

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

void bubble_sort(int *arr,int len){
    for(int i=0;i<len;i++){
        for(int j=0;j<len-i-1;j++){
            if(arr[j]>arr[j+1]){
                swap(arr[j],arr[j+1]);
            } 
        }
    }
}

int main(){
    int arr[] = { 61, 17, 29, 22, 34, 60, 72, 21, 50, 1, 62  };
    int len = (int) sizeof(arr) / sizeof(*arr);
    bubble_sort(arr, len);
    for (int i = 0; i < len; i++)
        cout << arr[i] << ' ';
    cout << endl;
    return 0;
}
```
### 1.4算法复杂度分析

> 最佳情况：T(n) = O(n)
>  最差情况：T(n) = O(n2) 
>  平均情况：T(n) = O(n2)
## 2.选择排序

> 选择排序简单直观，英文称为Selection Sort，先在数据中找出最大或最小的元素，放到序列的起始；然后再从余下的数据中继续寻找最大或最小的元素，依次放到排序序列中，直到所有数据样本排序完成。 **选择排序 是表现最稳定的排序算法之一 ，因为无论什么数据进去都是O(n2)的时间复杂度** 

 ### 2.1算法描述
 n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。具体算法描述如下：

> **1**.初始状态：无序区为R[1..n]，有序区为空；
> **2**.第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1..i-1]和R(i..n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
> **3**.n-1趟结束，数组有序化了。

### 2.2动画演示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201213220720387.gif#pic_center)
### 2.3 代码实现
```cpp
#include<iostream>
#include<algorithm>
using namespace std;

void selection_sort(float *arr,int len){
    for(int i=0;i<len-1;i++){
        int minIndex=i;
        for(int j=i+1;j<len;j++){
            if(arr[j]<arr[minIndex]){
                minIndex=j;
            }
        }
        swap(arr[i],arr[minIndex]);
    }
}

int main(){
    float arrf[] = { 17.5, 19.1, 0.6, 1.9, 10.5, 12.4, 3.8, 19.7, 1.5, 25.4, 28.6, 4.4, 23.8, 5.4  };
    int len=(int)sizeof(arrf)/sizeof(*arrf);
    selection_sort(arrf,len);
    for (int i = 0; i < len; i++)
        cout << arrf[i] << ' ';
    return 0;
}

```
### 2.3 算法复杂度分析

> 最佳情况：T(n) = O(n2)
最差情况：T(n) = O(n2)
平均情况：T(n) = O(n2)

## 3.插入排序

> 插入排序（Insertion-Sort）的算法描述是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。
### 3.1 算法描述
一般来说，插入排序都采用in-place在数组上实现。具体算法描述如下：
> 步骤1: 从第一个元素开始，该元素可以认为已经被排序； 
> 步骤2: 取出下一个元素，在已经排序的元素序列中从后向前扫描
> 步骤3:如果该元素（已排序）大于新元素，将该元素移到下一位置；
>  步骤4: 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置； 
> 步骤5:将新元素插入到该位置后； 步骤6: 重复步骤2~5。
### 3.2 动画演示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201213222043621.gif#pic_center)
### 3.3 代码实现
```cpp
#include<iostream>
#include<algorithm>
using namespace std;

/*void insert_sort(int *arr, int len) {
	for (int i = 1; i < len; i++) {
		int tmp = arr[i];
		int j=i-1;
		while (j >= 0 && arr[j] > tmp) {
			arr[j + 1] = arr[j];
			j--;
		}
		arr[j + 1] = tmp;
	}
}*/

void insert_sort(int *arr,int len){
    for(int i=1;i<len;i++){
        int key=arr[i];
        int j;
        //从后向前面扫描，如果小于
        for(j=i-1;j>=0&&key<arr[j];j--){
            arr[j+1]=arr[j];
        }
        arr[j+1]=key;
    }
}

int main(){
    int arr[]={20,12,13,15,7,0,2,46,45};
    int len=(int)sizeof(arr)/sizeof(*arr);
    insert_sort(arr,len);
    for(int i=0;i<len;i++){
        cout<<arr[i]<<' ';
    }
    cout<<endl;
    return 0;
}
```
### 3.4 算法复杂度分析
> 最佳情况：T(n) = O(n) 
> 最坏情况：T(n) = O(n2) 
> 平均情况：T(n) = O(n2)

## 4.希尔排序
   希尔排序是希尔（Donald Shell） 于1959年提出的一种排序算法。希尔排序也是一种插入排序，它是简单插入排序经过改进之后的一个更高效的版本，也称为缩小增量排序，同时该算法是冲破O(n2）的第一批算法之一。它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序又叫缩小增量排序。

希尔排序在插入排序的基础上进行了改进，它的基本思路是先将整个数据序列分割成若干子序列分别进行直接插入排序，待整个序列中的记录基本有序时，再对全部数据进行依次直接插入排序。

### 4.1算法描述
先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，具体算法描述：

> 步骤1：选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1； 
> 步骤2：按增量序列个数k，对序列进行k 趟排序；
> 步骤3：每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为1时，整个序列作为一个表来处理，表长度即为整个序列的长度。
### 4.2 过程演示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201213231451143.gif#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201213231227700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1paY3BwYw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201213231528125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1paY3BwYw==,size_16,color_FFFFFF,t_70)

 
假如有这样一组数据，[ 13 14 94 33 82 25 59 94 65 23 45 27 73 25 39 10 ]，如果以步长5进行分割，每一列为一组，那么这组数据应该首先分成这样

> 13 14 94 33 82
25 59 94 65 23
45 27 73 25 39
10

之后对每列进行插入排序：

> 10 14 73 25 23
13 27 94 33 39
25 59 94 65 82
45

将上述四行数据依序拼接在一起，得到[ 10 14 73 25 23 13 27 94 33 39 25 59 94 65 82 45 ]，此时10已经移到正确的顺序了，之后以步长3进行插入排序：

> 10 14 73
25 23 13
27 94 33
39 25 59
94 65 82
45

排序之后变为：

> 10 14 13
25 23 33
27 25 59
39 65 73
45 94 82
94

最后以步长1进行排序。
步长的选择是希尔排序的关键，只要最终步长为1，任何步长序列都可以。建议最初步长选择为数据长度的一半，直到最终的步长为1。

### 4.3 代码实现

```cpp
void shell_sort(int *arr,int len) {
	for (int gap = len / 2; gap > 0; gap /= 2) {
		for (int i = gap; i < len; i++) {
			int j = i;
			int current = arr[i];
			while (j - gap >= 0 && current < arr[j - gap]) {
				arr[j] = arr[j - gap];
				j = j - gap;
			}
			arr[j] = current;
		}
	}
}
/*void shell_sort(int *arr,int len) {
	for (int gap = len / 2; gap > 0; gap /= 2) {
		for (int i = gap; i < len; i++) {
			int tmp = arr[i];
			int preIndex = i - gap;
			while (preIndex >= 0 && arr[preIndex] > tmp) {
				arr[preIndex + gap] = arr[preIndex];
				preIndex -= gap;
			}
			arr[preIndex + gap] = tmp;
		}
	}
*/
```
### 4.4 算法复杂度分析

> 最佳情况：T(n) = O(nlog2 n) 
> 最坏情况：T(n) = O(nlog2 n) 
> 平均情况：T(n) =O(nlog2n)

## 5.归并排序
归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。 

归并排序严格遵循从左到右或从右到左的顺序合并子数据序列, 它不会改变相同数据之间的相对顺序, **因此归并排序是一种稳定的排序算法.**
### 5.1 算法描述

> 1、把长度为n的输入序列分成两个长度为n/2的子序列；
> 2、 对这两个子序列分别采用归并排序； 
>  3、将两个排序好的子序列合并成一个最终的排序序列

### 5.2 过程演示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201214111701421.gif#pic_center)

### 5.3 代码实现

```cpp
#include<iostream>
using namespcae std;

void merge_(int *arr, int left, int mid, int right,int *tmp) {
	int i = left, j = mid+1,k=0;
	while (i <= mid && j <= right) {
		if (arr[i] >= arr[j]) {
			tmp[k++] = arr[j++];
		}
		else {
			tmp[k++] = arr[i++];
		}
	}
	while (i <=mid) {
		tmp[k++] = arr[i++];
	}
	while (j <= right) {
		tmp[k++] = arr[j++];
	}
	for ( k = 0,i=left; i<=right;i++, k++) {
		arr[i] = tmp[k];
	}
}

void merge_sort(int *arr, int left, int right, int *tmp) {
	if (left >= right) {
		return;
	}
	int mid = (left + right)/2;
	merge_sort(arr, left, mid, tmp);
	merge_sort(arr, mid + 1, right, tmp);
	merge_(arr, left, mid, right, tmp);
}

int main() {
	int arr[] = {61,17,29,22,34,60,72,21,50,1,62 };
	int len = (int) sizeof(arr) / sizeof(*arr);
	int *tmp = new int[len];
	
	merge_sort(arr, 0, len-1, tmp);
	for (int i = 0; i < len; i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
	system("pause");
	return 0;
}
```

### 5.4 算法复杂度分析

> 最佳情况：T(n) = O(n)
最差情况：T(n) = O(nlogn)
平均情况：T(n) = O(nlogn)

## 6.快速排序
快速排序,英文称为Quicksort，又称划分交换排序 partition-exchange sort 简称快排。快速排序使用分治法（Divide and conquer）策略来把一个序列（list）分为两个子序列（sub-lists）。

### 6.1 算法描述

> 1.首先从数列中挑出一个元素，并将这个元素称为「基准」，英文pivot。
>2. 重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（相同的数可以到任何一边）。在这个分区结束之后，该基准就处于数列的中间位置。这个称为分区（partition）操作。
> 3.递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。
### 6.2 动画演示
![这里是引用](https://img-blog.csdnimg.cn/20201214142425176.gif#pic_center)
### 6.3 代码实现

```cpp
/*获得枢轴的位置，此时在它之前都不大于它，之后不小于它*/
int Partition(int *arr, int low, int high) {
	int pivotkey = arr[low];/*用子表的第一个记录作枢轴记录*/
	while (low < high) {/*从表的两端交替向中间扫描*/
		while (low < high&&arr[high] >=pivotkey) 
			high--;
		swap(arr[low], arr[high]);/*将比枢轴效地记录交换到低端*/
		while (low < high&&arr[low] <= pivotkey)
			low++;
		swap(arr[low], arr[high]);/*将比枢轴记录大的交换到高端*/
	}
	return low;/*返回枢轴的位置*/
}

void Quick_Sort(int *arr, int low, int high) {
	int pivot=0;
	if (low < high) {
		pivot = Partition(arr, low, high);

		Quick_Sort(arr, low, pivot - 1);
		Quick_Sort(arr, pivot + 1, high);
	}
}


int main() {
	int arr[] = {35,17,29,22,34,60,72,21,50,1,62 };
	int len = (int) sizeof(arr) / sizeof(*arr);
	int *tmp = new int[len];

	Quick_Sort(arr, 0,len-1);
	for (int i = 0; i < len; i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
	system("pause");
	return 0;
}
```

### 6.4 算法复杂度分析

> 最佳情况：T(n) = O(nlogn)
最差情况：T(n) = O(n2)
平均情况：T(n) = O(nlogn)

### 6.5快速排序的优化
#### 6.5.1优化选取枢轴
如果一直固定选取第一个管子见作为首个枢轴对于基本有序的系列可能极为不合理；因此改进的办法有人提出随机获得一个low和high之间的数rnd；这被称为随机选取枢轴法。在改进有了**三数取中**，即取三个关键字先进行排序，将中间数作为枢轴，一般先取左端、右端和中间三个数，也可以随机选取。更进一步还有九数取中。

优化选取枢轴的代码如下：
```cpp
/*获得枢轴的位置，此时在它之前都不大于它，之后不小于它*/
int Partition(int *arr, int low, int high) {
	int pivotkey;
	int m= low+(high-low)/2;
	if (arr[low] > arr[high])
		swap(arr[low], arr[high]);/*交换左右两端数据，保证左边最小*/
	if (arr[m] > arr[high])
		swap(arr[m], arr[high]);
	if (arr[m] > arr[low])
		swap(arr[m], arr[low]);
	/*此时arr[low]已经为整个序列左中右三个关键字的中间值*/
	pivotkey = arr[low];
	while (low < high) {/*从表的两端交替向中间扫描*/
		while (low < high&&arr[high] >=pivotkey) 
			high--;
		swap(arr[low], arr[high]);/*将比枢轴效地记录交换到低端*/
		while (low < high&&arr[low] <= pivotkey)
			low++;
		swap(arr[low], arr[high]);/*将比枢轴记录大的交换到高端*/
	}
	return low;/*返回枢轴的位置*/
}
```
#### 6.5.2优化不必要的交换
将枢轴的位置用临时变量tmp存储，中间少了多次交换数据的操作，变为替换操作。
```cpp
/*获得枢轴的位置，此时在它之前都不大于它，之后不小于它*/
int Partition(int *arr, int low, int high) {
	int pivotkey;
	int m= low+(high-low)/2;
	if (arr[low] > arr[high])
		swap(arr[low], arr[high]);/*交换左右两端数据，保证左边最小*/
	if (arr[m] > arr[high])
		swap(arr[m], arr[high]);
	if (arr[m] > arr[low])
		swap(arr[m], arr[low]);
	/*此时arr[low]已经为整个序列左中右三个关键字的中间值*/
	pivotkey = arr[low];
	int tmp = pivotkey;//将枢轴关键子备份到临时变量中
	while (low < high) {/*从表的两端交替向中间扫描*/
		while (low < high&&arr[high] >=pivotkey) 
			high--;
		//swap(arr[low], arr[high]);/*将比枢轴效地记录交换到低端*/
		arr[low] = arr[high];//优化为替换
		while (low < high&&arr[low] <= pivotkey)
			low++;
		//swap(arr[low], arr[high]);/*将比枢轴记录大的交换到高端*/
		arr[high] = arr[low];//优化为替换
	}
	arr[low] = tmp;//将枢轴位置替换为arr[low];
	return low;/*返回枢轴的位置*/
}
```
#### 6.5.3 优化递归操作
源代码中其尾部有两次递归操作，可以考虑减少递归的次数，将会大大地提高性能。于是对于Quick_Sort实行尾递归优化，

```cpp
void Quick_Sort(int *arr, int low, int high) {
	int pivot = 0;
	//此处改为while
	while(low < high) {
		pivot = Partition(arr, low, high);

		Quick_Sort(arr, low, pivot - 1);
		low = pivot + 1;//此处减少递归，将low赋值为pivot+1,再次循环时，效果等同于(pivot+1,high);
	}
}
```
## 7.堆排序
堆排序（Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点为大顶堆（小顶堆）。
### 7.1 算法描述
基本思想：

> 将待排序的序列构造成一个大顶堆，此时，整个序列的最大值就是对顶的根节点，将他移走（将其与堆数组的元素交换，此时末尾的元素就是最大值），然后将剩余的N-1个序列重新构造成一个堆，这样就会得到n个元素中次大的值。如此反复执行，便能得到一个有序的序列。
> 
算法描述：
> 步骤1：将初始待排序关键字序列(R1,R2….Rn)构建成大顶堆，此堆为初始的无序区；
步骤2：将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,……Rn-1)和新的有序区(Rn),且满足R[1,2…n-1]<=R[n]；
步骤3：由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,……Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2….Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。
### 7.2 动画演示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201214195858509.gif#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201214195916852.gif#pic_center)
### 7.3 代码实现

```cpp
//使得arr[s]到arr[m]成为一个大顶堆
void HeapAdjust(int *arr, int s, int m) {
	int temp, j;
	temp = arr[s];
	//循环遍历其结点的孩子，完全二叉树，当前节点为s,其左子树的序号一定是2*s;右孩子一定是2*s+1，孩子以2的位数序号增加;
	for (j = 2 * s; j <= m; j *= 2) {
		if (j < m&&arr[j] < arr[j + 1])
			++j; /* j为关键字中较大的记录的下标*/
		if (temp >= arr[j])
			break;   /*rc应插入在位置s上*/
		arr[s] = arr[j];
		s = j;
	}
	arr[s] = temp; //插入
}

void HeapSort(int *arr, int len) {
	int i;
	/*构建大顶堆，遍历的都是有孩子的结点*/
	//大顶堆是从下往上、从右到左都是递减的
	for (int i = len / 2; i > 0; i--) {
		HeapAdjust(arr, i, len);
	}
	for (int i = len; i > 0; i--) {
//将堆顶记录和当前未经排序子序列的最后一个记录交换
		swap(arr[0], arr[i]);
//将arr[0]到arr[i-1]重新调整为大顶堆;
		HeapAdjust(arr, 0, i - 1);
	}
}
```
### 7.4 算法复杂度分析

> 最佳情况：T(n) = O(nlogn)
最差情况：T(n) = O(nlogn)
平均情况：T(n) = O(nlogn)

## 8.总的程序
```cpp
#include<iostream>
#include<algorithm>
using namespace std;

void bubble_sort(int *arr, int len) {
	for (int i = 0; i < len; i++) {
		for (int j = 0; j < len - i - 1; j++) {
			if (arr[j + 1] < arr[j]) {
				swap(arr[j + 1], arr[j]);
			}
		}
	}
}

void select_sort(int *arr, int len) {
	for (int i = 0; i < len; i++) {
		int minIndex = i;
		for (int j = i; j < len; j++) {
			if (arr[j] < arr[minIndex]) {
				minIndex = j;
			}
		}
		swap(arr[i], arr[minIndex]);
	}
}

void insert_sort(int *arr, int len) {
	for (int i = 1; i < len; i++) {
		int tmp = arr[i];
		int j = i - 1;
		while (j >= 0 && arr[j] > tmp) {
			arr[j + 1] = arr[j];
			j--;
		}
		arr[j + 1] = tmp;

	}
}

void shell_sort(int *arr, int len) {
	for (int gap = len / 2; gap > 0; gap /= 2) {
		for (int i = gap; i < len; i++) {
			int tmp = arr[i];
			int j = i;
			while (j - gap >= 0 && arr[j - gap] > tmp) {
				arr[j] = arr[j - gap];
				j -= gap;
			}
			arr[j] = tmp;
		}
	}
}

void merge_(int *arr, int left, int mid, int right,int *tmp) {
	int i = left, j = mid+1,k=0;
	while (i <= mid && j <= right) {
		if (arr[i] >= arr[j]) {
			tmp[k++] = arr[j++];
		}
		else {
			tmp[k++] = arr[i++];
		}
	}
	while (i <=mid) {
		tmp[k++] = arr[i++];
	}
	while (j <= right) {
		tmp[k++] = arr[j++];
	}
	for ( k = 0,i=left; i<=right;i++, k++) {
		arr[i] = tmp[k];
	}
}

void merge_sort(int *arr, int left, int right, int *tmp) {
	if (left >= right) {
		return;
	}
	int mid = (left + right)/2;
	merge_sort(arr, left, mid, tmp);
	merge_sort(arr, mid + 1, right, tmp);
	merge_(arr, left, mid, right, tmp);
}

/*获得枢轴的位置，此时在它之前都不大于它，之后不小于它*/
int Partition(int *arr, int low, int high) {
	int pivotkey;
	int m= low+(high-low)/2;
	if (arr[low] > arr[high])
		swap(arr[low], arr[high]);/*交换左右两端数据，保证左边最小*/
	if (arr[m] > arr[high])
		swap(arr[m], arr[high]);
	if (arr[m] > arr[low])
		swap(arr[m], arr[low]);
	/*此时arr[low]已经为整个序列左中右三个关键字的中间值*/
	pivotkey = arr[low];
	int tmp = pivotkey;//将枢轴关键子备份到临时变量中
	while (low < high) {/*从表的两端交替向中间扫描*/
		while (low < high&&arr[high] >=pivotkey) 
			high--;
		//swap(arr[low], arr[high]);/*将比枢轴效地记录交换到低端*/
		arr[low] = arr[high];//优化为替换
		while (low < high&&arr[low] <= pivotkey)
			low++;
		//swap(arr[low], arr[high]);/*将比枢轴记录大的交换到高端*/
		arr[high] = arr[low];//优化为替换
	}
	arr[low] = tmp;//将枢轴位置替换为arr[low];
	return low;/*返回枢轴的位置*/
}

/*void Quick_Sort(int *arr, int low, int high) {
	int pivot=0;
	if (low < high) {
		pivot = Partition(arr, low, high);

		Quick_Sort(arr, low, pivot - 1);
		Quick_Sort(arr, pivot + 1, high);
	}
}*/

void Quick_Sort(int *arr, int low, int high) {
	int pivot = 0;
	//此处改为while
	while(low < high) {
		pivot = Partition(arr, low, high);

		Quick_Sort(arr, low, pivot - 1);
		low = pivot + 1;//此处减少递归，将low赋值为pivot+1,再次循环时，效果等同于(pivot+1,high);
	}
}

void HeapAdjust(int *arr, int s, int m) {
	int temp, j;
	temp = arr[s];
	for (int j = 2 * s; j <= m; j *= 2) {
		if (j < m&&arr[j] < arr[j + 1])
			++j;
		if (temp >= arr[j])
			break;
		arr[s] = arr[j];
		s = j;
	}
	arr[s] = temp;
}

void HeapSort(int *arr, int len) {
	int i;
	for (int i = len / 2; i > 0; i--) {
		HeapAdjust(arr, i, len);
	}
	for (int i = len; i > 0; i--) {
		swap(arr[0], arr[i]);
		HeapAdjust(arr, 0, i - 1);
	}
}

int main() {
	int arr[] = {35,17,29,22,34,60,72,21,50,1,62 };
	int len = (int) sizeof(arr) / sizeof(*arr);
	int *tmp = new int[len];
	//bubble_sort(arr, len);
	//select_sort(arr, len);
	//insert_sort(arr, len);
	//shell_sort(arr, len);
	//merge_sort(arr, 0, len-1, tmp);
	//Quick_Sort(arr, 0,len-1);
	HeapSort(arr, len-1);
	for (int i = 0; i < len; i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
	system("pause");
	return 0;
}
```


