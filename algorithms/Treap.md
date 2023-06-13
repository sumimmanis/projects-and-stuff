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


#### Implicit treap
Вместо ключей размер поддеревьев. Теперь в merge порядок не важен.
```cpp
struct Node {
    int x_, y_, size_ = 1;
    Node *left = nullptr, *right = nullptr;

    Node(int _key) : x_(_key), y_(rand()) {}

    typedef std::pair<Node*, Node*> Pair;

    int size(Node *node) { return node != nullptr ? node->size_ : 0; }

    void upd(Node *node) { node->size_ = 1 + size(node->left) + size(node->right); }


    Pair split(Node *node, int k) {
        if (node == nullptr) return {nullptr, nullptr};
        if (size(node->left) <= k) {
            auto [l, r] = split(node->right, k - size(node->left));
            node->right = l;
            upd(node);
            return {node, r};
        } else {
            auto [l, r] = split(node->right, k);
            node->right = r;
            upd(node);
            return {l, node};
        }
    }

    Node* merge(Node *l, Node *r) {
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

    void insert(Node *node, int x) {
        auto [l, r] = split(node, x);
        Node *t = new Node(x);
        node = merge(l, merge(t, r));
    }

    Node* ctrl_x(Node *node, int begin, int end) {
        auto [L, r] = split(node, end);
        auto [l, R] = split(L, begin - 1);
        node = merge(l, r);
        return R;
    }

    void ctrl_v(Node *node, Node *v, int k) {
        auto [l, r] = split(node, k);
        node = merge(l, merge(v, r));
    }
};
```
