## Dijkstra
$O(V\log{V} + E\log{V})$ обычно $O(E\log{V})$
> Все ребра неотрицательные
```cpp
std::vector<int> dijkstra() {
    std::set<std::pair<int, int>> set;
    std::vector<int> prev(dim_, -1), dist(dim_, INT32_MAX);
    set.insert({0, x_});
    dist[x_] = 0;
    while(!set.empty()) {
        auto [r, curr] = *set.begin();
        set.erase(set.begin());
        for (auto [next, len] : adjacent_vert_list_[curr]) {
            if (dist[next] > dist[curr] + len) {
                if (dist[next] != INT32_MAX) {
                    set.erase(set.find({dist[next], next}));
                }
                dist[next] = dist[curr] + len;
                prev[next] = curr;
                set.insert({dist[next], next});
            }
        }
    }
    return prev;
}
```

#### Вершины и ребра на кратчайших путях

Вершина $x$ лежит на кратчайшем пути из $u$ в $v$ если $d(xu) + d(xv) = d(uv)$. Запускаем две дейкстры и перебираем все вершины.  
Для ребер: $d(x_au) + d(x_bv) + dx = d(uv)$.

Например если нужно найти самый дешевый перелет с купоном на одну бесплатную поездку, то запускаем две дейкстры и перебираем все вершины $d(x_au) + d(x_bv) = d(uv)$.
