`mutable` если хотим менять поле даже в константном методе   
`static`  
`friend` для внешних функций
Все конструкторы с ровно одним параметром без дефолтного значения лучше делать `explicit`
Константность входит в сигнатуру метода.

```cpp
auto operator<=>(const Vector other) const = default; 

Vector& operator=(const Vector other) = delete;    (запретить оператор)
```
Если переопределять `<=>` то `==` и `!=` не работают.

##### Для вывода
```cpp
std::ostream& operator<<(std::ostream stream; const Vector& vector) {
	for (auto value : vector.values_) {
			stream << value << ",";
		}
	return stream;
}
```

##### Приведение к bool
```cpp
explicit operator bool() const {
	return size_t != 0;
}
```



