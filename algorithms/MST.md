## Minimum spanning tree

#### Алгоритм Борувки
Сначала каждая вершина компонент связности. Рекуррентно от каждой компоненты выбираем самое маленькое инцидентное ребро и объединяем компоненты. 
Важно, что при неуникальности весов ребер нужно либо аккуратно перебирать, либо ставить условие на несвязность. 


#### Алгоритм прима

Реккурентно берем минимальную непомеченную вершину по соседям улучшаем расстояния и помечаем. Можно писать как Дейкстру.
 
#### Алгоритм Краскала
Сортируем ребра и если вершины из разных компонент свяности, то объединяем c помощью DSU.
