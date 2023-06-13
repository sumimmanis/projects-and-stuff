## Treap
Все операции за $O(\log{n})$
```cpp
struct Node {
    int x_, y_;
    Node *left = nullptr, *right = nullptr;

    Node (int _key) : x_(_key), y_(rand()) {}

    typedef std::pair<Node*, Node*> Pair;

    Pair split (Node *node, int x) {
        if (node == nullptr) return {nullptr, nullptr};
        if (node->x_ < x) {
            auto [l, r] = split(node->right, x);
            node->right = l;
            return {node, r};
        }
        else {
            auto [l, r] = split(node->right, x);
            node->right = r;
            return {l, node};
        }
    }

    Node* merge (Node *l, Node *r) {
        if (l == nullptr) return r;
        if (r == nullptr) return l;
        if (l->y_ > r->y_) {
            l->right = merge(l->right, r);
            return l;
        }
        else {
            r->left = merge(l, r->left);
            return r;
        }
    }

    void insert (Node *node, int x) {
        auto [l, r] = split(node, x);
        Node *t = new Node(x);
        node = merge(l, merge(t, r));
    }

    void del (Node *node, int x) {
        auto [l, r_x] = split(node, x);
        auto [_x, r] = split(r_x, x + 1);
        node = merge(l, r);
    }
};
```
