# 8회차 스터디
2019.03.31
전경훈 진행

## 문제
EASY 난이도 3문제를 모두 빠르게 풀어보기

## 원숭이들의 회고
### 경훈님
내가 현회사에 입사할때 경험처럼, 3문제 정도를 모두 알려준 후, 얼마나 빠르게 & 많이 풀수 있는지 확인해보는 연습을 해보자 !

## 지훈님
### Problem
* 문제가 3개였는데
    * [invert binary tree](https://leetcode.com/problems/invert-binary-tree/)
    * [toeplitz matrix](https://leetcode.com/problems/toeplitz-matrix/)
    * [merge two binary tree](https://leetcode.com/problems/merge-two-binary-trees/)
* 이 문제들을 제한시간 안에 생각하고, 가장 자신 있는 문제를 앞에서 풀기로 했다.
* 내가 선택한 문제는 `merge two binary tree`

```
Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.
```

### Solve
```
<사진 & 직접수정 예정>
```

### 회고
* 시간안에 3개의 문제를 푸는게 꽤 힘들었다.
    * idea를 빠르게 생각하고 검증하고, 문제가 있는지 찾은 후 다음문제로 넘어갔다.
    * 다행히 생각이 빠르게 정리되어 문제를 쉽게 풀 수 있었음
* 한참 쓰다가, condition 이 너무 햇갈린다.
    * 침착하게 문제 상황을 주석으로 달면서 정리하자.
    * 우왕좌왕 하면 시간만 흐른다.

## 아리님
### Problem
3 문제를 제한 시간 안에 생각하고, [Toeplitz matrix](https://leetcode.com/problems/toeplitz-matrix/)문제를 발표했다.
```
A matrix is Toeplitz if every diagonal from top-left to bottom-right has the same element.

Now given an M x N matrix, return True if and only if the matrix is Toeplitz.
```

### Solution
```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        final int COL = matrix[0].length;
        for(int r = 0; r < matrix.length - 1; ++r){
            for(int c = 0; c < COL - 1; ++c){
                if(matrix[r][c] != matrix[r + 1][c + 1]) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

### 회고
- brute force하게 포인터를 이동해가며 찾는 방법으로 로직을 전개
- 면접관이 준 힌트를 얻어 위의 솔루션을 도출(sliding window)
- 힌트를 빠르게 캐치한 것이 잘한 것


## 경원님

### 
- 첫 단추를 잘못끼웠다. 재귀적으로 문제를 해결할 때 결과를 반환받아서 조합하는 방식이 아닌 결과 객체를 미리 생성하고 데이터를 누적하면서 만들어 나가는 방식으로 했더니 갑자기 문제가 복잡해졌다. 중간 중간에 다른분들이 힌트를 주신 것 같은데 내가 고집을 부린 것은 아닐까 싶다...
- 오늘 풀었던 방식 말고 재귀적으로 호출된 결과를 조합하는 방식으로 다시 풀어보자. 코드가 훨씬 간단해 질 것이다.
- 어떤 방법을 바꾸려고 할 때 불필요하게 많은 부분을 제거하여 나중에 코드를 다시 작성할 때 불필요한 시간을 소비했다. 침착하게 정말 수정이 필요한 부분을 파악하여 조금씩 수정해 나가자.
- nullable한 객체가 많아지니 은근히 복잡해진다....

#### 푼 문제
- Invert Binary Tree
- is Toeplitz Matrix?
- **Merge Two Binary Trees**
	- 내가 선택한 문제

```java
public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {  
	if (t1 == null && t2 == null) {  
		return null;  
	}  
	TreeNode result = new TreeNode(0);  
	mergeTrees(t1, t2, result);  
	return result;  
}  

public void mergeTrees(TreeNode t1, TreeNode t2, TreeNode result) {  
	if (t1 != null && t2 != null) {  
		result.val = t1.val + t2.val;  
		if (t1.left != null || t2.left != null) {  
			result.left = new TreeNode(0);  
			mergeTrees(t1.left, t2.left, result.left);  
		}  
		if (t1.right != null || t2.right != null) {  
			result.right = new TreeNode(0);  
			mergeTrees(t1.right, t2.right, result.right);  
		}  
	} else if (t1 != null) {  
		result.val = t1.val;  
		if (t1.left != null) {  
			result.left = new TreeNode(0);  
			mergeTrees(t1.left, null, result.left);  
		}  
		if (t1.right != null) {  
			result.right = new TreeNode(0);  
			mergeTrees(null, t1.right, result.right);  
		}  
	} else if (t2 != null) {  
		result.val = t2.val;  
		if (t2.left != null) {  
			result.left = new TreeNode(0);  
			mergeTrees(t2.left, null, result.left);  
		}  
		if (t2.right != null) {  
			result.right = new TreeNode(0);  
			mergeTrees(t2.right, null, result.right);  
		}  
	}
}
```
