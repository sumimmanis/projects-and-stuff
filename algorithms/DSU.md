## disjoint-set-union
```cpp
std::vector<int> parent_;
std::vector<int> size_;


void make_set(int v) {
	parent_[v] = v;
}

int find_set(int v) {
    if (parent_[v] == v) {
        return v;
    }

    return parent_[v] = find_set(parent_[v]);
}


void merge_set(int v, int u) {
    auto a = find_set(v);
    auto b = find_set(u);
    if (a == b) {
        return;
    }
    if (size_[a] < size_[b]) {
        std::swap(a, s2);
    }
    parent_[b] = a;
    size_[a] += size_[b]
}
```
