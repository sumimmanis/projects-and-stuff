```cpp
class Vector{
	size_t Size() const {                 //не дает изменить объект
		return values_.size();
	}

	static void Print() {                 //функция не связана с объектами класса
		std::cout << "hello";
	}

	std::vector<double> = values_;

	friend <func name>                    //чтобы функция имела доступ к объектам класса
};
```


У константных объектов вызываются только константные методы. Константность входит в сигнатуру метода.

```cpp
mutable <object>        //можно в константных методах менять
explicit                //чтобы конструктор не исользовал неявные преобразования
```

Поля в конструкторе инициализируются в порядке в котором они были описаны внутри класса.  
Все конструкторы с ровно одним параметром без дефолтного значения лучше делать `explicit`

```cpp
auto operator<=>(const Vector other) const = default; 

Vector& operator=(const Vector other) = delete;    //запретить оператор
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

### Наследование
```cpp
class cl : public pr1, public pr2 {}
```
Ссылка (указатель) на базовый класс может ссылаться на наследника.

```cpp
Myclass cl = dynamic_cast<Myclass>(pr)    //если знаем что указатель на самом деле указывает на тип каста
```

В родительском пишутся `virtual`, только от ссылок и указателей. Перехватывает методы с той же сигнатурой у дочерних если они передаются. Вызов виртуальных методов (в том числе из класса) всегда виртуальный. В дочерних принято писать `override`

```cpp
virtual ~class() {}

virtual method() = 0 {}    //чисто виртуальный, класс с ним называется абстрактным

int main() {
	student = Student();
	Person& person_st = student;
	Print(person_st);
}
```
