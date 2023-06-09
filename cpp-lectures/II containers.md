#### Массив
```cpp
int array[3] = {1, 0, 0};
int array[3];                //UB
int array[3] = {1};          //оставшиеся по дефолту - нули
int numbers[2][3] = {{0, 1, 2}, {4, 5, 6}}; 

sizeof(array);               //размер в битах
array[i]  <=>  *(array + i);
func (<type> array[])  <=>  func (*array)
 ```

Массив всегда передается в функцию как указатель на его начало.
Элементы массива хранятся в памяти подряд.

#### С строка
```cpp
char string[4] = "abc"          //в конце символ конца строки "\0"
const char* string = "abc"      //то же самое в С-стиле
```
Массив это указатель на начало строки. Обычное сравнение строк не работает.
```cpp
#include <cstring>
std::strcmp(str1, str2)                           //положительное если str1 больше
std::strncmp(str1, str2, размер наименьшей)       //безопаснее
```

#### Нормальная строка
По сути хранит указатель и `sizeof()`
```cpp
string.c_str()        //возвращает С-строку
std::to_string()
std::stoi()           //string to int
```
```cpp
std::string_view                  //только на чтение. Хранит индекс и длину подстроки
std::string_view(адрес, длина)
std::string::npos                 //возвращается если find не нашел подстроку
```

#### Вектор
Выделяется буфер в памяти подряд.
```cpp
std::vector<type> vct = {...};
.capacity()       
.size()
.front()    .back()        //прочитать
.reserve()      
.resize()
.shrink_to_fit()
.swap(vct_2)     
```
Итератор не проверяет чтобы вектор не релоцировался, дает ссылку
```cpp
for (auto iter = vct.begin(); iter != vct.end(); ++iter) {
	std::cout << *iter;
{

std::vector<type> vct_2 = {vct.begin(), vct.end()};       //можно копировать
```

#### Array
```cpp
std::array<ty pe, size> arr{...};       //вектор фиксированного размера
```

#### Лист
```cpp
.remove(value)            //удаляет все вхождения
.sort()
.unique()                 //удаляет неуникальные подряд
.merge(lst)               //сохраняет сортированность
iterator = .erase()       //возвращает индекс следующего
```
В листе нет произвольного доступа, он хранит адреса предыдущего и следуещего.  
Если удалим элемент, то итератор становится невалидным.
```cpp
std::forward_list         //хранит только адрес следующего
.insert_after()
```

#### Deque
Разбивает на несколько буферов, связанных ссылками. Немного хуже по времени и памяти
