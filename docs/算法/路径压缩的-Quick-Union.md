# 路径压缩的 Quick Union

路径压缩的并查集 Quick Union 算法

```java
class UnionFind {
    private int[] id;
    private int[] weight;
    public UnionFind(int n) {
        id = new int[n];
        weight = new int[n];
        for (int i = 0; i < n; ++i) {
            id[i] = i;
            weight[i] = 1;
        }
    }
    
    public int find(int p) {
        int root = p;
        while (root != id[root]) {
            root = id[root];
        }
        while (p != root) {
            int parent = id[p];
            id[p] = root;
            p = parent;
        }
        return p;
    }
    
    public void union(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        
        if (pRoot == qRoot) {
            return ;
        }
        
        if (weight[pRoot] > weight[qRoot]) {
            id[qRoot] = pRoot;
            weight[pRoot] += weight[qRoot];
        } else {
            id[pRoot] = qRoot;
            weight[qRoot]  += weight[pRoot];
        }
        
    }
    
    public boolean connected(int p, int q) {
        return find(p) == find(q);
    }
}
```