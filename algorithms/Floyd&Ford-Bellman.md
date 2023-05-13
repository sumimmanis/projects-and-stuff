## Флойд
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

## Форд-Беллман
$O(VE)$
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
