## Treap
$O(\log{n})$
```cpp
struct Node {
    int x_, y_;
    Node *left = nullptr, *right = nullptr;
    explicit Node(int x) : x_(x), y_(std::rand()) {}
} *head;


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

std::pair<Node*, Node*> split(Node *node, int x) {
    if (node == nullptr) return {nullptr, nullptr};
    if (node->x_ < x) {
        auto [l, r] = split(node->right, x);
        node->right = l;
        return {node, r};
    } else {
        auto [l, r] = split(node->left, x);
        node->left = r;
        return {l, node};
    }
}

std::pair<Node*, Node*> split_left(Node *node, int x) {
    if (node == nullptr) return {nullptr, nullptr};
    if (node->x_ <= x) {
        auto [l, r] = split_left(node->right, x);
        node->right = l;

        return {node, r};
    } else {
        auto [l, r] = split_left(node->left, x);

        node->left = r;
        return {l, node};
    }
}

Node* insert(Node *node, int x) {
    auto [l, r] = split(node, x);
    Node *t = new Node(x);
    return merge(l, merge(t, r));
}

void print(Node *node) {
    if (node == nullptr) return;
    print(node->left);
    std::cout << node->x_ << ' ';
    print(node->right);
}

Node* del(int x) {
    auto [l, r_x] = split(head, x);
    auto [_x, r] = split(r_x, x + 1);
    return merge(l, r);
}
```


#### Implicit treap
Вместо ключей размер поддеревьев. 
```cpp
struct Node {
    int x_, y_, size;
    Node *left = nullptr, *right = nullptr;
    explicit Node(int x) : x_(x), y_(std::rand()), size(0) {}
} *head;

int get_size(Node *node) { return node ? node->size : 0; }

void update(Node *node) { if (node) node->size = 1 + get_size(node->left) + get_size(node->right); }

Node* merge(Node *l, Node *r) {
    if (l == nullptr) return r;
    if (r == nullptr) return l;
    if (l->y_ > r->y_) {
        l->right = merge(l->right, r);
        update(l);
        return l;
    }
    else {
        r->left = merge(l, r->left);
        update(r);
        return r;
    }
}

std::pair<Node*, Node*> split(Node *node, int k) {
    if (node == nullptr) return {nullptr, nullptr};
    if (get_size(node->left) < k) {
        auto [l, r] = split(node->right, k - 1 - get_size(node->left));
        node->right = l;
        update(node);
        return {node, r};
    } else {
        auto [l, r] = split(node->left, k);
        node->left = r;
        update(node);
        return {l, node};
    }
}
```
