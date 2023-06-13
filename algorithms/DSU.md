## Disjoint-set-union
```cpp
std::vector<int> parent_;
std::vector<int> rank_;

void make_set(int v) {
    parent_[v] = v;
	rank_[v] = 1;
}

void init(int n) {
    parent_.resize(n);
    rank_.resize(n);
    for (int i = 0; i < n; ++i) {
        make_set(i);
    }
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

#### Алгоритм Краскала
Нужно построить связный граф с наименьшей суммой весов ребер:  
Сортируем ребра и если вершины из разных компонент свяности, то объединяем.
