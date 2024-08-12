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

时间复杂度：初始化为O(1)，其余操作为O(n)，n为插入或查询的字符串长度
空间复杂度：O(n*∑)，其中n为所有插入字符串的长度之和，∑为字符集的大小，这里为26

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
"""
思路：
1.构造字典树：问题是插入单词，搜索多种可能性单词，即涉及前缀，
需要构造字典树。建立Node类，一个要素是定长孩子列表，一个要素是
判断是否是结尾，对于字典树，直接把根节点用Node()赋值
2.插入字符串：使用ord(ch)-ord('a')计算index，判断child[index]
是否存在，若不存在则创建，直到末尾的字符需要把isEnd设置为True
3.匹配多种可能性字符串：需要利用深度优先搜索DFS，
def dfs(index: int, node: Node) -> bool:
首先考虑边界情况，若index和字符串长度相等，则返回其isEnd值。
将ch赋值为word[index]，先判断ch != '.'时，判断node的孩子
节点是否为None，同时递归dfs。当ch == '.'时，遍历所有孩子节点
是否为None，同时递归dfs

时间复杂度：初始化O(1)，新增字符串O(n)，n为字符串长度，
搜索单词为O(∑^n)，其中∑为字符集长度即26，n为字符串长度，
表示最坏可能每个字符都是'.'的情况
空间复杂度：O(n*∑)，n为所有添加字符串的长度之和
"""
class Node:
    def __init__(self):
        self.child = [None] * 26
        self.isEnd = False

class WordDictionary:

    def __init__(self):
        self.root = Node()

    def addWord(self, word: str) -> None:
        node = self.root
        for ch in word:
            index = ord(ch) - ord('a')
            if node.child[index] == None:
                node.child[index] = Node()
            node = node.child[index]
        node.isEnd = True

    def search(self, word: str) -> bool:
        def dfs(index: int, node: Node) -> bool:
            if index == len(word):
                return node.isEnd
            ch = word[index]
            if ch != '.':
                temp = ord(ch) - ord('a')
                if node.child[temp] != None and dfs(index+1,node.child[temp]):
                    return True
            else:
                for child in node.child:
                    if child != None and dfs(index+1, child):
                        return True
            return False

        return dfs(0, self.root)


# Your WordDictionary object will be instantiated and called as such:
# obj = WordDictionary()
# obj.addWord(word)
# param_2 = obj.search(word)
```

3. 单词搜索II

```python
"""
思路：
1.构造字典树。目的是将words中的字符串构造成字典树，首先定义节点属性，
包括孩子字典child，记录str，当为尾节点时用于记录完整字符串
2.插入字符串。正常插入字符串，最后的尾节点的str属性赋值成字符串
3.深度优先遍历DFS。定义函数格式如下，
dfs(board: List[List[str]], r: int, c: int, node: Node, length: int)。
首先获取当前行列字符ch=board[r][c]，如果ch不在node的孩子中，直接return，
然后将node更新为node.child[ch]，判断当前节点的str是否存在，若存在则将其
append进res中，然后将length++，将board[r][c] = '*'。接着往四个方向递归dfs，
最后将board[r][c] = ch进行还原，其中length存在的意义是用于后续的剪枝，可以通过
length的长度是否大于10在一开始来判断是否进行下去。
4.运行顺序。初始化字典树根，然后将words列表的每个word进行插入，初始化res=[]，
对board遍历，调用dfs，最后返回res
5.剪枝。I.在dfs中，首先根据传入的length判断是否继续，因为题目说明了搜索字符串
长度不大于10。II.在dfs中，若一个节点没有孩子字典了，说明这个节点是某个单词的最后
一个字符，并且不是其他任何单词的前缀，而且前面已经进行了匹配，append进了res，因此
通过last记录先前的node，然后将其last,child.pop(ch)删除，return即可


时间复杂度：O(m*n*3^(l-1))。其中m，n为二维列表的行列数，l为最长单词的
长度，需要遍历m*n个单元格，每个单元格最多需要遍历4*3^(l-1)条路径
空间复杂度：O(k*l)，其中k是列表words的长度，空间用于存储字典树

完整题解：https://leetcode.cn/problems/word-search-ii/solutions/2663477/javapython3chui-su-fa-zi-dian-shu-jian-z-0o8a
"""
class Node:
    def __init__(self):
        self.child = {}
        self.str = ""

class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        def insert(word: str):
            node = self.root
            for ch in word:
                if ch not in node.child:
                    node.child[ch] = Node()
                node = node.child[ch]
            node.str = word
        
        def dfs(board: List[List[str]], r: int, c: int, node: Node, length: int):
            if length > 10:
                return
            ch = board[r][c]
            if ch not in node.child:
                return
            last = node
            node = node.child[ch]
            if node.str != "":
                res.append(node.str)
                node.str = ""
            if not node.child:
                last.child.pop(ch)
                return
            length += 1
            board[r][c] = '*'
            for x, y in [[-1, 0], [1, 0], [0, 1], [0, -1]]:
                temp_r = r + x
                temp_c = c + y
                if temp_r >= 0 and temp_r < len(board) and temp_c >= 0 and temp_c < len(board[0]):
                    dfs(board, temp_r, temp_c, node, length)
            board[r][c] = ch

        r = len(board)
        c = len(board[0])
        self.root = Node()
        for word in words:
            insert(word)

        res = []
        for i, row in enumerate(board):
            for j, _ in enumerate(row):
                dfs(board, i, j, self.root, 0)

        return res
```

### 回溯

1. 电话号码的字母组合

```python
"""
思路：创建两个列表combination1和combination2，其中combination1
用于组合各个数字代表的字母，combination2用于保存组合好的字符串。
回溯算法backtrack(index: int)，首先定义递归出口，当index==len(digits)时，
combination2进行对combination1的内容进行append操作，当!=时，
combination1进行组合，递归，pop操作。

时间复杂度：O(3^m*4^n)。m为对应3个字母的数字个数，n为对应4个字母的数字
个数，组合起来有3^m*4^n种
空间复杂度：O(m+n)。哈希表大小为常数，递归调用的最大层数为m+n
"""

class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return list()

        phone_map = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz",
        }

        def backtrack(index: int):
            if index == len(digits):
                combination2.append("".join(combination1))
            else:
                digit = digits[index]
                for ch in phone_map[digit]:
                    combination1.append(ch)
                    backtrack(index+1)
                    combination1.pop()

        combination1 = list()
        combination2 = list()
        backtrack(0)
        return combination2
```

2. 组合

```python

```

3. 全排列

```python

```

4. 组合总和

```python

```

5. N皇后II

```python

```

6. 括号生成

```python

```

7. 单词搜索

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
