# LeetCode150-implemented-by-Python
LeetCode150题（由Python语言实现，解法均是最低复杂度）

[📑题单:Leetcode150题](https://leetcode.cn/studyplan/top-interview-150/)


- [字典树](#字典树)
- [回溯](#回溯)
- [分治](#分治)
- [Kadane算法](#Kadane算法)
- [二分查找](#二分查找)
- [堆](#堆)
- [位运算](#位运算)
- [数学](#数学)
- [一维动态规划](#一维动态规划)
- [多维动态规划](#多维动态规划)

### 字典树

1. 实现Trie(前缀树)

```python
"""
思路：
1.字典树构造：为每个节点的子节点创建一个定长的列表，每个节点添加属性
isEnd，用来后续判断由根到该节点到底只是前缀还是一个完整字符串。可以
新建一个Node类，Trie类的构造函数直接self.root=Node()即可
2.字典树插入：将根节点赋值给临时指针node，对字符串的逐个字符遍历，先用
ord(ch)-ord('a')得到index，然后判断是否存在子节点，若不存在则创建，
然后临时指针移动到子节点，完成遍历后，将该节点的isEnd设为True
3.查找字典树是否包含前缀：对字符串进行遍历，若有节点不存在，返回None，
直到最后返回最后一个节点，返回True
4.查找字典树是否包含字符串：利用是否包含前缀，查询到最后一个节点时，
验证isEnd是否为True


完整题解：https://leetcode.cn/problems/implement-trie-prefix-tree/solutions/2614334/javapython3cdfsbiao-zhi-wei-gou-zao-zi-d-zxrc/?envType=study-plan-v2&envId=top-interview-150
"""
class Node:
    def __init__(self):
        # 创建一个长度为26且值均为None的列表
        self.children = [None] * 26
        self.isEnd = False

class Trie:

    def __init__(self):
        self.root = Node()

    def insert(self, word: str) -> None:
        node = self.root
        for ch in word:
            # ord函数作用是返回一个字符的ASCLL码
            index = ord(ch) - ord('a')
            if not node.children[index]:
                node.children[index] = Node()
            node = node.children[index]
        # True表示一个完整的字符串
        node.isEnd = True

    def search(self, word: str) -> bool:
        node = self.search_prefix(word)
        if node != None and node.isEnd == True:
            return True
        else:
            return False

    def startsWith(self, prefix: str) -> bool:
        return self.search_prefix(prefix) != None

    # 查找字典树是否包含word前缀
    def search_prefix(self, word: str) -> Node:
        node = self.root
        for ch in word:
            index = ord(ch) - ord('a')
            if not node.children[index]:
                return None
            node = node.children[index]
        return node


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

2. 添加与搜索单词-数据结构设计

```python

```

3. 单词搜索II

```python

```

### 回溯

1. 

```python

```



### 分治

1. 

```python

```



### Kadane算法

1. 

```python

```



### 二分查找

1. 

```python

```



### 堆

1. 

```python

```



### 位运算

1. 

```python

```



### 数学

1. 

```python

```



### 一维动态规划

1. 

```python

```



### 多维动态规划

1. 

```python

```



### 许可证

该项目根据GPL许可证的条款进行许可。详情请参见[LICENSE](LICENSE)文件。

### 联系

项目链接：https://github.com/wxtt-github/LeetCode150-implemented-by-Python

非常欢迎向我提起issue。
