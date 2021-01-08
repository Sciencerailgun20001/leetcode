# leedcode刷题心得

##算法：搜索二维矩阵

题目描述

编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。


示例 1：


输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
示例 2：


输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false


提示：

m == matrix.length
n == matrix[i].length
1 <= n, m <= 300
-109 <= matix[i][j] <= 109
每行的所有元素从左到右升序排列
每列的所有元素从上到下升序排列
-109 <= target <= 109

解法一：

暴力破解



```python
class Solution：

	def searchMatrix(self,matex,target):
		for row in matrix:					#for in 语句是遍历整个循环体
			if target in row:
				return True
		return False

```

时间复杂度为O(mn).

空间复杂度：O(1)

解法二：

二分搜索

```python
class Soultion:
	def binary_search(self,matrix,target,vertical):
        lo = start
        hi = len(matrix[0])-1 if vertical else len(matrix)-1
        #vertical ? matrix[0].length-1 : matrix.length-1;
        
        
        while hi >=lo:
            mid = (lo+hi)//2			#python里的除法分成三类“/”除，“//”取整，“%”取余
            if vertical:
                if matrix[start][mid]<target:
                    lo = mid+1
                elif matrix[start][mid]>target:
                    hi = mid-1
                else:
                    return true
            else:
                 if matrix[mid][start]<target:
                    lo = mid+1
                elif matrix[mid][start]>target:
                    hi = mid-1
                else:
                    return true
          return False
   def searchMatrix(self, matrix,target):
    if not matrix:
        return False
    for i in range(min(len(martix),len(martix[0])))：
    	vertical_found = self.binary_search(martix,target,i,Ture)
        horizontal_found = self.binary_search(martix,target,i,Ture)
        if vertical_found or horizontal_found:
            return True
    return False
```

复杂度分析

时间复杂度：O(lg(n!))O(lg(n!))。
这个算法产生的时间复杂度并不是特别明显的是 O(lg(n!))O(lg(n!)) ，所以让我们一步一步地分析它。在主循环中执行的工作量逐渐最大，它运行 min(m,n)min(m,n) 次迭代，其中 mm 表示行数，nn 表示列数。在每次迭代中，我们对长度为 m-im−i 和 n-in−i 的数组片执行两次二分搜索。因此，循环的每一次迭代都以 O(lg(m-i)+lg(n-i))O(lg(m−i)+lg(n−i)) 时间运行，其中 ii 表示当前迭代。我们可以将其简化为 O(2 \cdot lg(n-i))= O(lg(n-i))O(2⋅lg(n−i))=O(lg(n−i)) 在最坏的情况是 n\approx mn≈m 。当 n \ll mn≪m 时会发生什么（不损失一般性）；nn 将在渐近分析中占主导地位。通过汇总所有迭代的运行时间，我们得到以下表达式：
\quad {O}(lg(n) + lg(n-1) + lg(n-2) + \ldots + lg(1))
O(lg(n)+lg(n−1)+lg(n−2)+…+lg(1))

然后，我们可以利用对数乘法规则（lg(a)+lg(b)=lg(ab)lg(a)+lg(b)=lg(ab)）将复杂度改写为：

{O}(lg(n) + lg(n-1) + lg(n-2) + \ldots + lg(1))O(lg(n)+lg(n−1)+lg(n−2)+…+lg(1))
={O}(lg(n \cdot (n-1) \cdot (n-2) \cdot \ldots \cdot 1))=O(lg(n⋅(n−1)⋅(n−2)⋅…⋅1))
= {O}(lg(1 \cdot \ldots \cdot (n-2) \cdot (n-1) \cdot n))=O(lg(1⋅…⋅(n−2)⋅(n−1)⋅n))
= {O}(lg(n!))=O(lg(n!))

空间复杂度：O(1)O(1)，因为我们的二分搜索实现并没有真正地切掉矩阵中的行和列的副本，我们可以避免分配大于常量内存。

#### 方法三：

因为矩阵的行和列是排序的（分别从左到右和从上到下），所以在查看任何特定值时，我们可以修剪O(m)O(m)或O(n)O(n)元素。

如下代码，从左下角以此剪除行、列，查找target

```python
class Solution:
    def searchMatrix(self,matrix,target):
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return False
        hight = len(matrix)
        width = len(matrix[0])

        row = hight-1
        col = 0

        while col<width and row >=0:
            if matrix[row][col]>target:
                row-=1
            elif matrix[row][col]<target:
                col+=1
            else:
                return True
        return False
```

复杂度分析

时间复杂度：O(n+m)O(n+m)。
时间复杂度分析的关键是注意到在每次迭代（我们不返回 true）时，行或列都会精确地递减/递增一次。由于行只能减少 mm 次，而列只能增加 nn 次，因此在导致 while 循环终止之前，循环不能运行超过 n+mn+m 次。因为所有其他的工作都是常数，所以总的时间复杂度在矩阵维数之和中是线性的。
空间复杂度：O(1)O(1)，因为这种方法只处理几个指针，所以它的内存占用是恒定的。
