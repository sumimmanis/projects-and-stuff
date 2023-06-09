## Dijkstra
$O(E\log{V})$
> Все ребра неотрицательные

Можно использовать для всех задач в которых характеристика пути не может стать лучше.

```cpp
int dim_, x_;
std::vector<std::vector<std::pair<int, int>>> adjacent_vert_list_;    //[i] = j, k

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
Самая эффективная реализация на фибоначчиевой куче $O(V\log{V} + E)$

#### Вершины на кратчайших путях
Вершина $x$ лежит на кратчайшем пути из $u$ в $v$ если $d(xu) + d(xv) = d(uv)$. Запускаем две дейкстры и перебираем все вершины.  

#### Ребра на кратчайших путях. Самолеты
Запускаем две дейкстры и перебираем все вершины $d(x_au) + d(x_bv) + dx = d(uv)$.

#### Bottleneck edge problem
При переходе по ребру берем минимум веса ребра и значения в начальний вершине, а конечной вершише присваиваем максимум.

## Флойда
$O(V^3)$
> Если вершина достижима из отрицательного цикла, то работает некорректно, а ребро $ii$ отрицательно

```cpp
std::vector<std::vector<int>> floyd() {
        std::vector<std::vector<int>> next(dim_, std::vector<int>(dim_));    //восстановление пути
        
        for (int from = 0; from < dim_; ++from) {
            for (int to = 0; to < dim_; ++to) {
                next[from][to] = to;
            }
        }

        for (int k = 0; k < dim_; ++k) {    //важно что это именно внешний цикл
            for (int from = 0; from < dim_; ++from) {
                for (int to = 0; to < dim_; ++to) {
                    if (matrix_[from][k] != INT32_MAX && matrix_[k][to] != INT32_MAX &&
                        (matrix_[from][to] > matrix_[from][k] + matrix_[k][to])) {
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
int dim_, vert_cnt_;
std::vector<std::tuple<int, int, int>> edges_;
std::vector<int> dist_(dim_, INT32_MAX);

void fordbellman() {
    dist_[0] = 0;
    for (int i = 0; i < dim_ - 1; ++i) {
        bool finish = true;
        for (int j = 0; j < vert_cnt_; ++j) {
            auto [a, b, d] = edges_[j];

            if ((dist_[a] != INT32_MAX) && (dist_[a] + d < dist_[b])) {
                dist_[b] = dist_[a] + d;
                finish = false;
            }
        }
        if (finish) {
            return;
        }
    }
}
```
Через Форда-Беллмана можно находить циклы отрицательного веса, берем все вершины которые улучшились на $V - 1$ итерации, они и все их наследники не будут иметь кратчайшего расстояния. В теории код снизу тоже должен работать.
```cpp
bool test_negative_cycles() {
    for (int i = 0; i < dim_; ++i) {
        for (int j = 0; j < vert_cnt_; ++j) {
            auto [a, b, d] = edges_[j];
		
            if ((dist_[a] != INT32_MAX) && (dist_[a] + d < dist_[b])) {
                std::cout << "Negative cycle: " << b << " ";
                while (a != b) {
                    std::cout << a << " ";
                    a = prev[a];    
                }
                return true;
            }
        }
    }
    return false;
}
```



## [Levit's algorithm](https://e-maxx.ru/algo/levit_algorithm)

## Yen's algorithm

## Johnson's algorithm


```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

const int INF = 2e9; // бесконечность

int main() {
    int n, s, m;
    cin >> n >> s >> m;

    vector<vector<pair<int, int>>> adj(n + 1); // список смежности

    for (int i = 0; i < m; i++) {
        int a, b, w;
        cin >> a >> b >> w;
        adj[a].push_back({b, w}); // добавляем ребро в список смежности
    }

    vector<int> dist(n + 1, INF); // массив для хранения расстояний
    dist[s] = 0; // расстояние от столицы до самой себя равно 0

    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq; // очередь с приоритетом
    pq.push({0, s}); // добавляем столицу в очередь

    while (!pq.empty()) { // пока в очереди есть элементы
        int u = pq.top().second;
        int d = pq.top().first;
        pq.pop();

        if (d > dist[u]) continue; // если расстояние до нашей вершины уже меньше, то пропускаем ее

        for (auto v : adj[u]) { // проходимся по всем ее соседям
            if (dist[u] + v.second < dist[v.first]) { // если путь через нашу вершину короче
                dist[v.first] = dist[u] + v.second; // обновляем расстояние до вершины
                pq.push({dist[v.first], v.first}); // добавляем ее в очередь
            }
        }
    }

    for (int i = 1; i <= n; i++) {
        if (dist[i] == INF) cout << "? "; // если расстояние до вершины равно бесконечности, то выводим "?"
        else cout << dist[i] << " ";
    }

    return 0;
}
```
