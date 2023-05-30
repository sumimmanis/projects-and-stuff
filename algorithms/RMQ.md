## Range minimum query

```cpp
template<typename T>
struct SegmentTree {
    std::vector<T> tree;
    int right_;

    explicit SegmentTree(std::vector<T> arr) {
        right_ = arr.size() - 1;
        tree.resize(4 * arr.size(), 0);
        build(arr, 0, 0, arr.size() - 1);
        for (int i: tree) {
            std::cout << i << ' ';
        }
    }


    void build(std::vector<T> &arr, int v, int l, int r) {
        if (l == r)
            tree[v] = arr[l];
        else {
            int mid = (l + r) / 2;
            build(arr, v * 2 + 1, l, mid);
            build(arr, v * 2 + 2, mid + 1, r);
            tree[v] = tree[v * 2 + 1] + tree[v * 2 + 2];
        }
    }

    T sum(int v, int l, int r, int from, int to) {
        if (from > to) return 0;
        if (from == l && to == r) return tree[v];
        int mid = (l + r) / 2;
        return sum(v * 2 + 1, l, mid, from, std::min(to, mid))
               + sum(v * 2 + 2, mid + 1, r, std::max(from, mid + 1), to);
    }

    T sum(int u, int v) {
        return sum(0, 0, right_, u, v);
    }

    void update(int v, int l, int r, int pos, T val) {
        if (l == r) {
            tree[v] = val;
        } else {
            int mid = (l + r) / 2;
            if (pos <= mid)
                update(v * 2 + 1, l, mid, pos, val);
            else
                update(v * 2 + 2, mid + 1, r, pos, val);
            tree[v] = tree[v * 2 + 1] + tree[v * 2 + 2];
        }
    }

    void update(int pos, T val) {
        update(0, 0, right_, pos, val);
    }
};
```
