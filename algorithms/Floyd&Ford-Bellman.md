## Флойд
> нет отрицательных циклов
> в матрице смежности если нет ребра то не сильно большая бесконечность
```cpp
std::vector<std::vector<int>> floyd() {
        std::vector<std::vector<int>> second_in_path(dim_, std::vector<int>(dim_));    //восстановление пути
        for (int from = 0; from < dim_; ++from) {
            for (int to = 0; to < dim_; ++to) {
                second_in_path[from][to] = to;
            }
        }

        for (int k = 0; k < dim_; ++k) {
            for (int from = 0; from < dim_; ++from) {
                for (int to = 0; to < dim_; ++to) {
                    if (matrix_[from][to] > matrix_[from][k] + matrix_[k][to]) {   //не сильно большая бесконечность
                        matrix_[from][to] = matrix_[from][k] + matrix_[k][to];
                        second_in_path[from][to] = second_in_path[from][k];
                    }
                }
            }
        }
        return second_in_path;
    }
```
Через Форда-Беллмана можно находить циклы отрицательного веса, берем все вершины которые улучшились на последней итерации, они и все их наследники не будут иметь кратчайшего расстояния.
