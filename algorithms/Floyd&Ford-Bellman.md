## Флойд
> Если есть отрицательные циклы то работает некорректно, а $ii$ отрицательна если она достижима из них  
> Если в матрице смежности нет ребра то ставить не сильно большую бесконечность

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
Через Форда-Беллмана можно находить циклы отрицательного веса, берем все вершины которые улучшились на последней итерации, они и все их наследники не будут иметь кратчайшего расстояния.
