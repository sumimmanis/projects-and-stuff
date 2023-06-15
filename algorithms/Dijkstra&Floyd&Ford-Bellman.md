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

#### Вершины и ребра на кратчайших путях

Вершина $x$ лежит на кратчайшем пути из $u$ в $v$ если $d(xu) + d(xv) = d(uv)$. Запускаем две дейкстры и перебираем все вершины.  
Для ребер: $d(x_au) + d(x_bv) + dx = d(uv)$.

#### Самолеты
Запускаем две дейкстры и перебираем все вершины $d(x_au) + d(x_bv) = d(uv)$.

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
Через Форда-Беллмана можно находить циклы отрицательного веса, берем все вершины которые улучшились на $V - 1$ итерации, они и все их наследники не будут иметь кратчайшего расстояния.



## [Levit's algorithm](https://e-maxx.ru/algo/levit_algorithm)

## Yen's algorithm

## Johnson's algorithm

#

```cpp
bool test_negative_cycles() {
    for (int v = 1; v <= n; ++v) {
        for (auto u : g[v]) {
            if (distance_to_vertex[v] != inf && distance_to_vertex[u] > distance_to_vertex[v] + weight[{v, u}]) {
                cout << "Negative cycle: " << u << " ";
                while (v != u) {
                    cout << v << " ";
                    v = previous[v];
                }
                cout << endl;
                return true;
            }
        }
    }
    cout << "No negative cycles." << endl;
    return false;
}

int main() {
    read_data();
    init_data();
    Bellman_Ford(start_vertex);
    if (!test_negative_cycles()) {
        print_distance();
    }
}
```

## Прима
```cpp
// входные данные
int n;
vector < vector<int> > g;
const int INF = 1000000000; // значение "бесконечность"
 
// алгоритм
vector<bool> used (n);
vector<int> min_e (n, INF), sel_e (n, -1);
min_e[0] = 0;
for (int i=0; i<n; ++i) {
	int v = -1;
	for (int j=0; j<n; ++j)
		if (!used[j] && (v == -1 || min_e[j] < min_e[v]))
			v = j;
	if (min_e[v] == INF) {
		cout << "No MST!";
		exit(0);
	}
 
	used[v] = true;
	if (sel_e[v] != -1)
		cout << v << " " << sel_e[v] << endl;
 
	for (int to=0; to<n; ++to)
		if (g[v][to] < min_e[to]) {
			min_e[to] = g[v][to];
			sel_e[to] = v;
		}
}
```
