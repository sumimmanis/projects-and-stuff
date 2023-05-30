## Range minimum query
_find & change_ &nbsp; $O(\log{n})$  
```cpp
template<typename T>
struct SegmentTree {
    std::vector<T> tree;
    int right_;

    explicit SegmentTree(std::vector<T> &arr) {
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

```cpp
#include <iostream>
#include <list>
#include <queue>
#include <vector>

template<typename T>
struct SegTree {

    struct Node {
        Node *left = nullptr, *right = nullptr;
        int l = 0, r = 0;
        T sum = 0;

        Node(int begin, int end) : l(begin), r(end) {
            if (begin < end) {
                int mid = (begin + end) / 2;
                left = new Node(begin, mid);
                right = new Node(mid + 1, end);
            }
        }

        void add(int v, T value) {
            this->sum += value;
            if (left != nullptr) {
                if (v <= left->r) left->add(v, value);
                if (v >= right->l) right->add(v, value);
            }
        }

        T get_sum(int begin, int end) {
            if (begin <= l && r <= end) return this->sum;
            if (end <= l || r <= begin) return 0;
            return left->get_sum(begin, end) + right->get_sum(begin, end);
        }

        ~Node() {
            delete left;
            delete right;
        }
    } *node;

//    void add(int index, T value) {
//        root->add(index, value);
//    }

//    SegTree(int begin, int end) {
//        node = new Node(begin, end);
//    }

    explicit SegTree(const std::vector<T> &arr) {
        node = new Node(0, arr.size());
        for (int i = 0; i < arr.size(); ++i) {
            node->add(i, arr[i]);
        }
    }

    T get_sum(int begin, int end) {
        return node->get_sum(begin, end);
    }

    ~SegTree() {
        delete node;
    }
};

int main() {
    std::vector<int> v = {1, 2, 3, 4, 5, 6, 7};
    SegTree<int> segTree(v);
    std::cout << segTree.get_sum(0, 1);
    return 0;
}
```
