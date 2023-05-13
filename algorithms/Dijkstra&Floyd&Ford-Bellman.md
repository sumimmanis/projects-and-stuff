## Dijkstra
Обычно $O(E\log{V})$
> Все ребра неотрицательные

Можно использовать для всех задач в которых характеристика пути не может стать лучше.

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

## Флойда
> Если в матрице смежности нет ребра то ставить не сильно большую бесконечность  
> С отрицательными циклами работает некорректно, а ребро $ii$ отрицательно если вершина $i$ достижима из них  


```cpp
std::vector<std::vector<int>> floyd() {
        std::vector<std::vector<int>> next(dim_, std::vector<int>(dim_));    //восстановление пути
        for (int from = 0; from < dim_; ++from) {
            for (int to = 0; to < dim_; ++to) {
                next[from][to] = to;
            }
        }

        for (int k = 0; k < dim_; ++k) {
            for (int from = 0; from < dim_; ++from) {
                for (int to = 0; to < dim_; ++to) {
                    if (matrix_[from][to] > matrix_[from][k] + matrix_[k][to]) {   //не делать inf > INT32_MAX - 1
                        matrix_[from][to] = matrix_[from][k] + matrix_[k][to];
                        next[from][to] = next[from][k];
                    }
                }
            }
        }
        return next;
    }
```

## Форда-Беллмана
$O(VE)$
> С отрицательными циклами работает некорректно  

```cpp
void fordbellman() {
    bool finish = true;
    dist_[0] = 0;
    for (int i = 0; i < dim_ - 1; ++i) {
        for (int j = 0; j < vert_cnt_; ++j) {
            auto [a, b, d] = edges_[j];

            if ((dist_[a] != INT32_MAX) && (dist_[a] + d < dist_[b])) {
                dist_[b] = dist_[a] + d;
                finish = false;
            }
        }
    }
    if (finish) {
        return;
    }
}
```
Через Форда-Беллмана можно находить циклы отрицательного веса, берем все вершины которые улучшились на $V - 1$ итерации, они и все их наследники не будут иметь кратчайшего расстояния.


## Левита
> Не работает с отрицательными циклами
 
```cpp
std::vector<int> levits() {
    std::deque<int> q;
    std::vector<int> prev(dim_, -1), dist(dim_, INT32_MAX);
    q.push_front(x_);
    dist[x_] = 0;
    while(!q.empty()) {
        int curr = q.front();
        q.pop_front();
        for (auto [next, len] : adjacent_vert_list_[curr]) {
            if (prev[next] == curr) {
                continue;
            }
            if (dist[next] > dist[curr] + len) {
                if (dist[next] != INT32_MAX) {
                    q.push_front(next);
                } else {
                    q.push_back(next);
                }
                dist[next] = dist[curr] + len;
                prev[next] = curr;
            }
        }
    }
    return prev;
} 
```
