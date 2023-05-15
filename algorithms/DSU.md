## disjoint-set-union
```cpp
std::vector<int> parent_;
std::vector<int> rank_;


void make_set(int v) {
    parent_[v] = v;
}


int find_set(int v) {
    if (parent_[v] == v)
        return v;

    return parent_[v] = find_set(parent_[v]);
}


void union_sets(int a, int b) {
    a = find_set(a);
    b = find_set(b);
    if (a != b) {
        if (rank_[a] < rank_[b])
            std::swap(a, b);
        if (rank_[a] == rank_[b])
            ++rank_[a];

        parent_[b] = a;
    }
}

```
