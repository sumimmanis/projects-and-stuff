## Range minimum query
_find & change_ &nbsp; $O(\log{n})$  

```cpp
template<typename T>
struct SegTree {

    struct Node {
        Node *left = nullptr, *right = nullptr;
        int l = 0, r = 0;
        T sum = 0;

        Node(int begin, int end) : l(begin), r(end) {}

        void extend() {
            if (left == nullptr && l < r) {
                int mid = (l + r) / 2;
                left = new Node(l, mid);
                right = new Node(mid + 1, r);
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

        T get_sum(int begin, int end) {
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

    explicit SegTree(const std::vector<T> &arr) {
        node = new Node(0, arr.size());
        for (int i = 0; i < arr.size(); ++i) {
            node->add(i, arr[i]);
        }
    }

    void add(int v, T value) {
        node->add(v, value);
    }

//    void add(int u, int v, T value) {
//        node->add(u, v, value);
//    }

    T get_sum(int begin, int end) {
        return node->get_sum(begin, end);
    }

    ~SegTree() {
        delete node;
    }
};
```
