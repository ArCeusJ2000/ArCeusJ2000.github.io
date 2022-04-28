+++
title = "Basic template"
date = 2021-04-10 17:21:06
slug = "202104101721"

[taxonomies]
tags = ["Cram" ]
categories = ["Cram"]

+++

<!-- more -->

# 数据结构

## 二叉树

```c++
// Definition for a binary tree node.
struct TreeNode {
      int val;
      TreeNode *left;
      TreeNode *right;
      TreeNode() : val(0), left(nullptr), right(nullptr) {}
      TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
      TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};
```

### 遍历

```c++
class Solution {
public:
    //前序
    void dfs(TreeNode *cur, vector<int> &vec) {
        if (cur == NULL) return;
        vec.push_back(cur->val);  // 中
        dfs(cur->left, vec);      // 左
        dfs(cur->right, vec);     // 右
    }
    //中序
    void dfs(TreeNode *cur, vector<int> &vec) {
        if (cur == NULL) return;
        dfs(cur->left, vec);
        vec.push_back(cur->val);
        dfs(cur->right, vec);
    }
    //后序
     void dfs(TreeNode *cur, vector<int> &vec) {
        if (cur == NULL) return;
        dfs(cur->left, vec);
        dfs(cur->right, vec);
        vec.push_back(cur->val);
    }
    vector<int> Traversal(TreeNode *root) {
        vector<int> res;
        dfs(root, res);
        return res;
    }
};
```

### 层序遍历

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode *root) {
        vector<vector<int>> res;
        if (!root) return res;

        queue<TreeNode *> q;
        q.push(root);
        while (!q.empty()) {
            int layerSz = q.size();
            vector<int> layer;
            for (int i = 0; i < layerSz; i++) {
                auto node = q.front();
                q.pop();
                layer.push_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            res.push_back(layer);
        }
        return res;
    }
};
```

### 前序中序序列构造二叉树

```c++
class Solution {
private:
    unordered_map<int, int> index;

public:
    TreeNode* myBuildTree(const vector<int>& preorder, const vector<int>& inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right) {
        if (preorder_left > preorder_right) {
            return nullptr;
        }
        
        // 前序遍历中的第一个节点就是根节点
        int preorder_root = preorder_left;
        // 在中序遍历中定位根节点
        int inorder_root = index[preorder[preorder_root]];
        
        // 先把根节点建立出来
        TreeNode* root = new TreeNode(preorder[preorder_root]);
        // 得到左子树中的节点数目
        int size_left_subtree = inorder_root - inorder_left;
        // 递归地构造左子树，并连接到根节点
        // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
        root->left = myBuildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
        // 递归地构造右子树，并连接到根节点
        // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
        root->right = myBuildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        // 构造哈希映射，帮助我们快速定位根节点
        for (int i = 0; i < n; ++i) {
            index[inorder[i]] = i;
        }
        return myBuildTree(preorder, inorder, 0, n - 1, 0, n - 1);
    }
};
```

**复杂度分析**

时间复杂度：O(n)，其中 n 是树中的节点个数。

空间复杂度：O(n))，除去返回的答案需要的 O(n)空间之外，我们还需要使用 O(n) 的空间存储哈希映射，以及 O(h)其中 h 是树的高度）的空间表示递归时栈空间。这里 h < n，所以总空间复杂度为 O(n)。



### 中序后序序列构造二叉树

```c++
class Solution {
    int post_idx;
    unordered_map<int, int> idx_map;
public:
    TreeNode* helper(int in_left, int in_right, vector<int>& inorder, vector<int>& postorder){
        // 如果这里没有节点构造二叉树了，就结束
        if (in_left > in_right) {
            return nullptr;
        }

        // 选择 post_idx 位置的元素作为当前子树根节点
        int root_val = postorder[post_idx];
        TreeNode* root = new TreeNode(root_val);

        // 根据 root 所在位置分成左右两棵子树
        int index = idx_map[root_val];

        // 下标减一
        post_idx--;
        // 构造右子树
        root->right = helper(index + 1, in_right, inorder, postorder);
        // 构造左子树
        root->left = helper(in_left, index - 1, inorder, postorder);
        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        // 从后序遍历的最后一个元素开始
        post_idx = (int)postorder.size() - 1;

        // 建立（元素，下标）键值对的哈希表
        int idx = 0;
        for (auto& val : inorder) {
            idx_map[val] = idx++;
        }
        return helper(0, (int)inorder.size() - 1, inorder, postorder);
    }
};
```

**复杂度分析**

时间复杂度：O(n)，其中 n 是树中的节点个数。

空间复杂度：O(n)。我们需要使用 O(n)的空间存储哈希表，以及 O(h)（其中 h 是树的高度）的空间表示递归时栈空间。这里 h < n，所以总空间复杂度为 O(n)。

## 二叉搜索树

二叉搜索树能只够通过前序序列或后序序列构造

### 序列化和反序列化二叉搜索树

普通二叉树同样适用

#### BFS

```c++
public class Codec {

    //把树转化为字符串（使用BFS遍历）
    public String serialize(TreeNode root) {
        //边界判断，如果为空就返回一个字符串"#"
        if (root == null)
            return "#";
        //创建一个队列
        Queue<TreeNode> queue = new LinkedList<>();
        StringBuilder res = new StringBuilder();
        //把根节点加入到队列中
        queue.add(root);
        while (!queue.isEmpty()) {
            //节点出队
            TreeNode node = queue.poll();
            //如果节点为空，添加一个字符"#"作为空的节点
            if (node == null) {
                res.append("#,");
                continue;
            }
            //如果节点不为空，把当前节点的值加入到字符串中，
            //注意节点之间都是以逗号","分隔的，在下面把字符
            //串还原二叉树的时候也是以逗号","把字符串进行拆分
            res.append(node.val + ",");
            //左子节点加入到队列中（左子节点有可能为空）
            queue.add(node.left);
            //右子节点加入到队列中（右子节点有可能为空）
            queue.add(node.right);
        }
        return res.toString();
    }

    //把字符串还原为二叉树
    public TreeNode deserialize(String data) {
        //如果是"#"，就表示一个空的节点
        if (data == "#")
            return null;
        Queue<TreeNode> queue = new LinkedList<>();
        //因为上面每个节点之间是以逗号","分隔的，所以这里
        //也要以逗号","来进行拆分
        String[] values = data.split(",");
        //上面使用的是BFS，所以第一个值就是根节点的值，这里创建根节点
        TreeNode root = new TreeNode(Integer.parseInt(values[0]));
        queue.add(root);
        for (int i = 1; i < values.length; i++) {
            //队列中节点出栈
            TreeNode parent = queue.poll();
            //因为在BFS中左右子节点是成对出现的，所以这里挨着的两个值一个是
            //左子节点的值一个是右子节点的值，当前值如果是"#"就表示这个子节点
            //是空的，如果不是"#"就表示不是空的
            if (!"#".equals(values[i])) {
                TreeNode left = new TreeNode(Integer.parseInt(values[i]));
                parent.left = left;
                queue.add(left);
            }
            //上面如果不为空就是左子节点的值，这里是右子节点的值，注意这里有个i++，
            if (!"#".equals(values[++i])) {
                TreeNode right = new TreeNode(Integer.parseInt(values[i]));
                parent.right = right;
                queue.add(right);
            }
        }
        return root;
    }
}
```



#### DFS

```c++
class Codec {

    //把树转化为字符串（使用DFS遍历，也是前序遍历，顺序是：根节点→左子树→右子树）
    public String serialize(TreeNode root) {
        //边界判断，如果为空就返回一个字符串"#"
        if (root == null)
            return "#";
        return root.val + "," + serialize(root.left) + "," + serialize(root.right);
    }

    //把字符串还原为二叉树
    public TreeNode deserialize(String data) {
        //把字符串data以逗号","拆分，拆分之后存储到队列中
        Queue<String> queue = new LinkedList<>(Arrays.asList(data.split(",")));
        return helper(queue);
    }

    private TreeNode helper(Queue<String> queue) {
        //出队
        String sVal = queue.poll();
        //如果是"#"表示空节点
        if ("#".equals(sVal))
            return null;
        //否则创建当前节点
        TreeNode root = new TreeNode(Integer.valueOf(sVal));
        //分别创建左子树和右子树
        root.left = helper(queue);
        root.right = helper(queue);
        return root;
    }
}

```



#### stringstream

```c++
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if(root == nullptr)
            return "#";

        return to_string(root->val) + ' ' + serialize(root->left) + ' ' + serialize(root->right);
    }

    // Decodes encoded data to tree.
    TreeNode* rebuildTree(stringstream &ss){
        string tmp;
        ss >> tmp;
        
        if(tmp == "#")
            return nullptr;

        TreeNode* node = new TreeNode(stoi(tmp));
        node->left = rebuildTree(ss);
        node->right = rebuildTree(ss);

        return node;
    }
    
    TreeNode* deserialize(string data) {
        stringstream ss(data);
        return rebuildTree(ss);
    }
};

```



## 并查集

### 路径压缩（常用）

```c++
class UnionFind{
public:
    //查找根节点，如果节点的父节点不为空，那就不断迭代。
    int find(int x){
        int root = x;
        
        while(father[root] != -1){
            root = father[root];
        }
        //路径压缩
        while(x != root){
            int original_father = father[x];
            father[x] = root;
            x = original_father;
        }
        
        return root;
    }
    
    //判断两节点是否相连，判断它们的祖先是否相同
    bool is_connected(int x,int y){
        return find(x) == find(y);
    }
    
    //合并两个节点，y的根结点作为x根节点的父亲
    void merge(int x,int y){
        int root_x = find(x);
        int root_y = find(y);
        
        if(root_x != root_y){
            //root_x 接在 root_y 后面
            father[root_x] = root_y;
            num_of_sets--;
        }
    }
    
    //添加新节点，它的父节点应该为空
    void add(int x){
        if(!father.count(x)){
            father[x] = -1;
            num_of_sets++;
        }
    }
    //  返回集合数
    int get_num_of_sets(){
        return num_of_sets;
    }
    
private:
    // 记录父节点
    unordered_map<int,int> father;
    // 记录集合数量
    int num_of_sets = 0;
};

```

### 启发式合并（按秩排序）

在平均情况下

- 不使用启发式合并、只使用路径压缩，时间复杂度依然是 O(ma(m, n))
- 如果只使用启发式合并，而不使用路径压缩，时间复杂度为 O(mlogn)

```c++
class UnionFind {
public:
    vector<int> parent;  // 下标idx为节点，parent[idx]为该节点的父亲
    vector<int> size;    // 若idx为父亲根节点，size[idx]为该连通分量的大小
    int n;               // 节点数量
    int setCount;        // 连通分量的数量

public:
    UnionFind(int _n) : n(_n), setCount(_n), parent(_n), size(_n, 1) {
        iota(parent.begin(), parent.end(), 0);
    }

    //递归查找根节点，如果节点i的父节点为本身就找到了根，结束递归
    int find(int x) {
        return parent[x] == x ? x : parent[x] = find(parent[x]);
    }

    //合并两个节点
    bool unite(int x, int y) {
        x = find(x);
        y = find(y);
        if (x == y) return false;

        if (size[x] < size[y]) {
            swap(x, y);
        }
        parent[y] = x;       // x 作为 y 的父亲
        size[x] += size[y];  // 父亲节点x的联通分量大小加上y节点的
        --setCount;
        return true;
    }

    //判断两个节点是否连通
    bool is_connected(int x, int y) {
        x = find(x);
        y = find(y);
        return x == y;
    }

    //断开节点与他父节点的连接
    void disconnected(int x) {
        if (x != parent[x]) {
            parent[x] = x;
            size[x] = 1;
            ++setCount;
        }
    }
};
```



# 算法

## 二分

```c++
int l = 0, r = MAXN - 1;
while (l < r) {
	int mid = l + r >> 1;
	if (check(mid))
		r = mid;
	else
        l = mid + 1;
}
return left;
```

## 回溯

```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

### 回溯算法几种问题的复杂度分析

#### 子集问题分析：

**时间复杂度**： ![[公式]](https://www.zhihu.com/equation?tex=O%28n%5Ctimes2%5En%29) 。因为每一个元素的状态无外乎取与不取，一共![[公式]](https://www.zhihu.com/equation?tex=2%5En) 种状态，每种状态都需要 ![[公式]](https://www.zhihu.com/equation?tex=O%28n%29) 的构造时间，最终时间复杂度为 ![[公式]](https://www.zhihu.com/equation?tex=O%28n%5Ctimes2%5En%29) 。

**空间复杂度**： ![[公式]](https://www.zhihu.com/equation?tex=O%28n%29) ，递归深度为n，所以系统栈所用空间为 ![[公式]](https://www.zhihu.com/equation?tex=O%28n%29) 。

#### 排列问题分析：

**时间复杂度：**： ![[公式]](https://www.zhihu.com/equation?tex=O%28n%5Ctimes+n%21%29) 。因为一共![[公式]](https://www.zhihu.com/equation?tex=n%21) 种排列，每种排列都需要 ![[公式]](https://www.zhihu.com/equation?tex=O%28n%29) 的构造时间，最终时间复杂度为 ![[公式]](https://www.zhihu.com/equation?tex=O%28n%5Ctimes+n%21%29) 。

**空间复杂度**： ![[公式]](https://www.zhihu.com/equation?tex=O%28n%29) ，递归深度为n，所以系统栈所用空间为 ![[公式]](https://www.zhihu.com/equation?tex=O%28n%29) 。

#### 组合问题分析：

**时间复杂度：** ![[公式]](https://www.zhihu.com/equation?tex=O%28C_n%5Ek+%5Ctimes+k%29) ，总共有 ![[公式]](https://www.zhihu.com/equation?tex=C_n%5Ek) 种组合，每种组合需要 ![[公式]](https://www.zhihu.com/equation?tex=O%28k%29) 的时间复杂度。另一方面，组合问题其实就是一种子集的问题，所以组合问题最坏的情况，也不会超过子集问题的时间复杂度 ![[公式]](https://www.zhihu.com/equation?tex=O%28n%5Ctimes+2%5En%29) 。

**空间复杂度**： ![[公式]](https://www.zhihu.com/equation?tex=O%28n%29) ，递归深度为n，所以系统栈所用空间为 ![[公式]](https://www.zhihu.com/equation?tex=O%28n%29) 。

#### N皇后问题分析

**时间复杂度：** ![[公式]](https://www.zhihu.com/equation?tex=O%28N%21%29) ，其中 N 是皇后数量，由于每个皇后必须位于不同列，因此已经放置的皇后所在的列不能放置别的皇后。第一个皇后有 N 列可以选择，第二个皇后最多有 N-1列可以选择...。

**空间复杂度**： ![[公式]](https://www.zhihu.com/equation?tex=O%28N%29) ，递归深度为n，所以系统栈所用空间为 ![[公式]](https://www.zhihu.com/equation?tex=O%28N%29) 。

#### 解数独问题分析

**时间复杂度：** ![[公式]](https://www.zhihu.com/equation?tex=O%289%5Em%29) ，m是'.'的数目。

**空间复杂度：** ![[公式]](https://www.zhihu.com/equation?tex=O%28n%5E2%29) ，n是数独盘子的大小，递归的深度是 ![[公式]](https://www.zhihu.com/equation?tex=n%5E2) 。

### 子集

输入一个不包含重复数字的数组，算法输出这些数字的所有子集。

```c++
vector<vector<int>> subsets(vector<int>& nums) {
    // base case，返回一个空集
    if (nums.empty()) return {{}};
    // 把最后一个元素拿出来
    int n = nums.back();
    nums.pop_back();
    // 先递归算出前面元素的所有子集
    vector<vector<int>> res = subsets(nums);

    int size = res.size();
    for (int i = 0; i < size; i++) {
        // 然后在之前的结果之上追加
        res.push_back(res[i]);
        res.back().push_back(n);
    }
    return res;
}
```

时间复杂度：总的迭代次数应该是 2^N，因为 `res[i]` 也是一个数组，`push_back` 把 `res[i]` copy 一份然后添加到数组的最后，所以一次操作的时间是 O(N)。总的时间复杂度就是 O(N*2^N)。

空间复杂度，如果不计算储存返回结果所用的空间的，只需要 O(N) 的递归堆栈空间。如果计算 `res` 所需的空间，应该是 O(N*2^N)。

**第二种通用方法是回溯算法**

```c++
vector<vector<int>> res;

vector<vector<int>> subsets(vector<int>& nums) {
    // 记录走过的路径
    vector<int> track;
    backtrack(nums, 0, track);
    return res;
}

void backtrack(vector<int>& nums, int start, vector<int>& track) {
    res.push_back(track);
    // 注意 i 从 start 开始递增
    for (int i = start; i < nums.size(); i++) {
        // 做选择
        track.push_back(nums[i]);
        // 回溯
        backtrack(nums, i + 1, track);
        // 撤销选择
        track.pop_back();
    }
}
```

对 `res` 的更新是一个**前序遍历**，`res` 就是树上的所有节点

### 组合

输入两个数字 `n, k`，算法输出 `[1..n]` 中 k 个数字的所有组合。

直接套回溯算法模板，`k` 限制了树的高度，`n` 限制了树的宽度

```c++
vector<vector<int>>res;

vector<vector<int>> combine(int n, int k) {
    if (k <= 0 || n <= 0) return res;
    vector<int> track;
    backtrack(n, k, 1, track);
    return res;
}

void backtrack(int n, int k, int start, vector<int>& track) {
    // 到达树的底部
    if (k == track.size()) {
        res.push_back(track);
        return;
    }
    // 注意 i 从 start 开始递增
    for (int i = start; i <= n; i++) {
        // 做选择
        track.push_back(i);
        backtrack(n, k, i + 1, track);
        // 撤销选择
        track.pop_back();
    }
}
```

`backtrack` 函数和计算子集的差不多，区别在于，更新 `res` 的地方是树的底端。

### 排列

输入一个不包含重复数字的数组 `nums`，返回这些数字的全部排列。

```c++
vector<vector<int>>res;

vector<vector<int>> permute(vector<int>& nums) {
    if (k <= 0 || n <= 0) return res;
    vector<int> track;
    backtrack(nums, track);
    return res;
}

void backtrack(vector<int>& nums, vector<int>& track) {
    // 到达树的底部
    if (track.size() == nums.size()) {
        res.push_back(track);
        return;
    }
    for (int i = 0; i < nums.size(); i++) {
        // 排除不合法的选择
        if(count(track.begin(), track.end(), nums[i])){
            continue;
        }
        // 做选择
        track.push_back(nums[i]);
        // 进入下一层决策树
        backtrack(nums, track);
        // 撤销选择
        track.pop_back();
    }
}
```

排列问题的树比较对称，而组合问题的树越靠右节点越少。

在代码中的体现就是，排列问题每次通过排除在 `track` 中已经选择过的数字；而组合问题通过传入一个 `start` 参数，来排除 `start` 索引之前的数字。

## 前缀和

前缀和对于一个给定的数组`nums`，额外开辟一个前缀和数组进行预处理

例题：算出一共有几个和为 k 的子数组

```c++
int subarraySum(vector<int>& nums, int k) {
    int n = nums.size();
    //前缀和：该前缀和出现的次数
    unordered_map<int, int> preSum;
    // base case
    preSum[0] = 1;

    int ans = 0, sum0_i = 0;
    // 穷举所有子数组
    for (int i = 0; i < n; i++){
        sum0_i += nums[i];
        // 目标前缀和 nums[0...j]
        int sum0_j = sum0_i - k;
        // 如果前面出现过这个前缀和，直接更新答案
        if(preSum.count(sum0_j)){
            ans += preSum[sum0_j];
        }
        preSum[sum0_i]++;
    }
        

    return ans;
}
```

例题：统计班上同学考试成绩在不同分数段的百分比

```java
int[] scores; // 存储着所有同学的分数
// 试卷满分 150 分
int[] count = new int[150 + 1]
// 记录每个分数有几个同学
for (int score : scores)
    count[score]++
// 构造前缀和
for (int i = 1; i < count.length; i++)
    count[i] = count[i] + count[i-1];
```

