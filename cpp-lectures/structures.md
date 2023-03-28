```cpp
std::tuple<...> = tpl(...);      (можно от разных типов)
std::get<index>(tpl);
auto tuple = std::make_tuple(1, 2, "Hello");
```
Для структур `pair` и `tuple` определены операторы сравнения
```cpp
auto tie = std::tie(&z, &y);      (tuple из ссылок, нельзя передавать временный объект)
```

#### Structured binding
```cpp
std::pair<type1, type2> = pr(x, y);
auto [a, b] = pr;      (как бы распаковываем)
auto& [a, b] = pr;
```

### Ассоциативные контейнеры
`std::map` `std::set`

На ключах должен обязательно быть задан оператор `<`. Поиск по ключу и вставка осуществляется за логарифм.

`std::unordered_map` `std::unordered_set`

Нужег `hash` и `==`

```cpp
template<>
struct std::hash<Point> {
	size_t operator()(const Point& p) {
		return p.x * 17 + p.y;          //пример хэша
	}
}
```
`std::multimap` позволяем иметь несколько ключей
