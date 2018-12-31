# 1회차 스터디

2018.12.29(토)

오아리 진행



## 문제

https://www.hackerrank.com/challenges/is-binary-search-tree/problem?h_r=internal-search

https://leetcode.com/problems/validate-binary-search-tree/



## 원숭이들의 생각

### 경원님

```javascript
function solution(node) {
    const result = left(node)
    return result !== -1
}

function left(node) {
    if (!node) {
        return true
    }

    const leftResult = node.left ? left(node.left) : node.val
    const rightResult = node.right ? right(node.right) : node.val

    if (leftResult === -1 || rightResult === -1) {
        return -1
    } else if (leftResult <= node.val && node.val <= rightResult) {
        return rightResult
    } else {
        return -1
    }
}

function right(node) {
    if (!node) {
        return true
    }

    const leftResult = node.left ? left(node.left) : node.val
    const rightResult = node.right ? right(node.right) : node.val

    if (leftResult === -1 || rightResult === -1) {
        return -1
    } else if (leftResult <= node.val && node.val <= rightResult) {
        return leftResult
    } else {
        return -1
    }
}
```

위 솔루션은 불필요하게 장황한 형태를 하고 있다.

거기다 이번 문제를 완전히 해결하지 못한다.

트리의 데이터가 더 많아져 깊이가 깊어지면 의도했던 왼쪽에서 가장 큰 값, 오른쪽에서 가장 작은 값을 재귀적으로 반환하지 못할 것이다.

가장 먼저 발표를 하면서 처음 시작할때 제약조건을 하나 빼먹으면서 첫 단추를 잘못끼웠는데, 앞으로는 좀 더 문제의 제약조건에 주의할 필요가 있어보인다. 



### 지훈님

```kotlin
/**
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int = 0) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */
class Solution {
    val bstResults = mutableListOf<Boolean>()
    
    fun isValidBST(root: TreeNode?): Boolean {
        treversal(root)
        
        return bstResult.none{it == false}
    }
    
    fun treversal(node: TreeNode) {
        bstResults.add(isBST(node))
        if (node.left != null) {
            treversal(node.left)
        }
        if (node.right != null) {
            treversal(node.right)
        }
    }
    
    fun isBST(node: TreeNode) {
        return node.value < node.right.value && node.value > node.left.value
    }
}
```

\* 리프노드일 때, 대비가 안되있음

\* 루트노드가 null 일때 대비가 안되있음

\* 각 노드가 BST를 만족하더라도, 전체 node가 BST 라는 것을 보장할 수 없음

\* 스터디 중에 느낀점

​    \* 설명을 할 때, 중심되는 함수부터 설명을 하자.

​    \* main 함수 내에서 변수와 함수 정의를 먼저 하고 설명하자

​    \* 화이트 보드는 제한적이므로, 공간 활용을 잘 하자

​    \* testSet 을 잘 정의하자

​    \* 테스트를 할때 여분의 공간에 콜스텍을 적으면서 하자



### 경훈님

```java
public class BST_Validate {

	public class TreeNode {
		public int val;
		public TreeNode left;
		public TreeNode right;
	}

	public boolean solve(TreeNode n) {
		if(n == null) {
			return false;
		}

		if(n.left != null) {
			boolean t = trav(n.left, n.val, -1);
			if(t == false) {
				return t;
			}
		}

		if(n.right != null) {
			return trav(n.right, 10000, n.val);
		}

		return true;
	}

	boolean trav(TreeNode n, int max, int min) {
		if(n.val < max && n.val > min) {
			if(n.left != null) {
				boolean t = trav(n.left, n.val, min);
				if(t == false) {
					return t;
				}
			}
			if(n.right != null) {
				return trav(n.right, max, n.val);
			}
		}

		return false;
	}
}		
```

\* 초기 10분내의 위의 솔루션을 생각해 내지 못함

  \- 문제는 항상 핵심을 생각하고, 쉽게 생각하자 !

\* 문제를 풀기전에 조건(conditions)을 명확히 파악할것

\* complexity 계산방법을 까먹었다. 다시 공부하자.

\* 리팩토링 할 중복로직이 안나오도록 연습하자.

  \- but 모든코드를 극단적으로 줄일려고 하지는 말자



### 아리님

```java
boolean isBST(Node head) {
  if (head  == null) {
    return true;
  }
  return checkBST(head, Long.MIN_VALUE, Long.MAX_VALUE);
}

boolean checkBST(Node node, long min, long max) {
  if (node == null) {
    return true;
  }
  int data = node.data;
  return (min < data && data < max) && 
        checkBST(node.left, min, Math.min(max, data)) && checkBST(node.right, Math.max(min, data), max);
}
```

- head에 대해 checkBST를 처음 호출할 때, `Integer.MIN_VALUE`, `Integer.MAX_VALUE`를 호출했었다. 하지만 노드의 val이 integer 범위이므로, 노드의 val이 (각각의 엣지 케이스인) 정수의 최솟값, 최댓값일 때의 경우를 커버하지 못한다. 따라서 checkBST의 min/max 인자의 타입은 int가 아니라 long이어야 한다.
- space complexity를 계산 할 때, call stack도 사용하는 공간으로 계산을 했었다. space complexity는 O(N)이라고 했는데, 생각해보니 평균적으로 트리의 height만큼 스택에 쌓일 것이고 O(logN)이 되겠다. Worst일 때에는 O(N)일테고.



## 우리가 생각하는 최적의 솔루션

java 버전으로 하면 아래와 같다.

```java
boolean isBST(Node head) {
  if (head  == null) {
    return true;
  }
  return checkBST(head, Long.MIN_VALUE, Long.MAX_VALUE);
}

boolean checkBST(Node node, long min, long max) {
  if (node == null) {
    return true;
  }
  int data = node.data;
  return (min < data && data < max) && 
        checkBST(node.left, min, Math.min(max, data)) && checkBST(node.right, Math.max(min, data), max);
}
```

트리 내 노드 갯수가 N 개라 할 때,



시간 복잡도: O(N)



공간 복잡도: O(log N)
