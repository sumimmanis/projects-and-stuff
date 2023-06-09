## Непересекающиеся отрезки

Отрезок целочисленной прямой длины N разбит на единичные отрезки, которые пронумерованы от 1 до N.
Их объединяют в группы по следующим правилам: 

1. Несколько подряд идущих отрезков, ни один из которых не принадлежит ни одной из групп, могут быть объединены в группу. 
2. Любая ранее созданная группа может быть уничтожена, при этом входившие в нее отрезки больше не относятся ни к какой группе и могут впоследствии быть отнесены к другим группам. 


```cpp
std::vector<std::pair<int, int>> vct;

struct SegTree {

    struct Node {
        Node *left = nullptr, *right = nullptr;
        int l = 0, r = 0;
        bool any = false;
        int assign = 0;

        Node(int begin, int end) : l(begin), r(end) {}

        void extend() {
            if (left == nullptr && l < r) {
                int mid = (l + r) / 2;
                left = new Node(l, mid);
                right = new Node(mid + 1, r);
            }
        }

        void unite(int begin, int end, int i) {
            if (!(end < l || r < begin)) {
                if (begin <= l && r <= end) {
                    assign = i;
                    vct[i].first = std::min(l, vct[i].first);
                    vct[i].second = std::max(r, vct[i].second);
                    any = true;
                } else {
                    extend();
                    if (left != nullptr) {
                        left->unite(begin, end, i);
                        right->unite(begin, end, i);
                        any = left->any || right->any;
                    }
                }
            }
        }

        void check(int begin, int end, bool &e) {
            if (e) {
                return;
            }
            if (!(end < l || r < begin)) {
                if (assign) {
                    e = true;
                } else if (begin <= l && r <= end) {
                    if (any) e = true;
                } else {
                    extend();
                    if (left != nullptr) {
                        // если есть дети и отрезок запроса хоть как-то пересекается с нашим
                        left->check(begin, end, e);
                        right->check(begin, end, e);
                    }
                }
            }
        }

        void find(int v, int &mem) const {
            if (assign) {
                mem = assign;
                return;
            }
            if (left != nullptr) {
                if (v <= left->r) {
                    left->find(v, mem);
                } else {
                    right->find(v, mem);
                }
            }
        }

        void del(int begin, int end) {
            if (!(end < l || r < begin)) {
                if (begin <= l && r <= end) {
                    assign = 0;
                    any = false;
                } else {
                    extend();
                    if (left != nullptr) {
                        left->del(begin, end);
                        right->del(begin, end);
                        any = left->any || right->any;
                    }
                }
            }
        }


        ~Node() {
            delete left;
            delete right;
        }
    } *node;

    explicit SegTree(int end) {
        node = new Node(0, end);
    }

    void del(int v) {
        int mem = 0;
        node->find(v, mem);
        if (mem == 0) {
            std::cout << 0 << ' ' << 0 << '\n';
        } else {
            std::cout << vct[mem].first + 1 << ' ' << vct[mem].second + 1 << '\n';
            node->del(vct[mem].first, vct[mem].second);
            vct[mem] = {INT32_MAX - 1, -1};
        }
    }

    void unite_if_not_united(int u, int v, int i) const {
        bool e = false;
        node->check(u, v, e);
        if (!e) {
            node->unite(u, v, i);
            std::cout << '1' << '\n';
        } else {
            std::cout << '0' << '\n';
        }
    }

    ~SegTree() {
        delete node;
    }
};


int main() {
    int n, k;
    std::cin >> n >> k;
    SegTree segTree(n - 1);
    vct.resize(k + 1, {INT32_MAX, -1});
    for (int i = 0; i < k; ++i) {
        int a;
        std::cin >> a;
        if (a == 1) {
            int b, c;
            std::cin >> b >> c;
            segTree.unite_if_not_united(b - 1, c - 1, i + 1);
        } else {
            int b;
            std::cin >> b;
            segTree.del(b - 1);
        }
    }
    return 0;
}
```
