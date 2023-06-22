## Range minimum query
$O(\log{n})$ на изменение и запись, можно динамически строить, эффективно присваивать и многое другое.

```cpp
template<typename T>
struct SegTree {

    struct Node {
        Node *left = nullptr, *right = nullptr;
        int l = 0, r = 0;
        T sum = 0, assign = 0;

        Node(int begin, int end) : l(begin), r(end) {}

        void extend() {
            if (left == nullptr && l < r) {
                int mid = (l + r) / 2;
                left = new Node(l, mid);
                right = new Node(mid + 1, r);
            }
        }

        void push() {
            if (assign != 0) {
                if (left != nullptr) {
                    left->assign += assign; left->sum += assign;
                    right->assign += assign; right->sum += assign;
                }
                assign = 0;
            }
        }

        void add(int v, T value) {
            extend();
            this->sum += value;
            if (left != nullptr) {
                if (v <= left->r) {
                    left->add(v, value);
                } else {
                    right->add(v, value);
                }
            }
        }

        void add(int begin, int end, int x) {
            if (begin <= l && r <= end) {
                assign += x; sum += x;
            } else if (!(end < l || r < begin)) {
                extend();
                if (left != nullptr) {
                    push();
                    left->add(begin, end, x);
                    right->add(begin, end, x);
                    sum = left->sum + right->sum;
                }
            }
        }

        T get_sum(int begin, int end) {
            push();
            if (begin <= l && r <= end) return this->sum;
            if (end < l || r < begin) return 0;
            extend();
            return left->get_sum(begin, end) + right->get_sum(begin, end);
        }

        ~Node() {
            delete left;
            delete right;
        }
    } *node;

    explicit SegTree(int end) {
        node = new Node(0, end);
    }

    void add(int v, T value) {
        node->add(v, value);
    }

    void add(int u, int v, int value) {
        node->add(u, v, value);
    }

    T get_sum(int begin, int end) {
        return node->get_sum(begin, end);
    }

    ~SegTree() {
        delete node;
    }
};
```

#### Реализация на массиве
```cpp
template<typename T>
struct SegmentTree {
    std::vector<T> tree;
    int right_;

    explicit SegmentTree(std::vector<T> &arr) {
        right_ = arr.size() - 1;
        tree.resize(4 * arr.size(), 0);
        build(arr, 0, 0, arr.size() - 1);
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

#

[непересекающиеся отрезки](contests/UnintersectingSets.md)
