```cpp
sizeof()    //размер точно не задан, свой под архитектуру
static_cast<>()

Integral tipes:
1 bite <= short <= int <= long <= long long

     >= 8     >= 16    >= 32    >= 64    in bits 
```
```cpp
char  = 1 bite >= 8 bit      //литерал, может быть знаковым
wchar_t                     //хранит все символы юникода
bool  =  1 bite            //default = false
```
```cpp
Overflow:
-128…127                //старший бит отвечает за знак, он всегда -2**31
1111 1111 → -1


Сразу пытается привести к signed потом к unsigned, в зависимости от максимально возможного значения. 
Например int64_t приведется к uint64_t. Может привести к ошибке из-за переполнения при операциях с разными типами. 
Переполнение знаковых дает UB, для беззнаковых модуль.
```
```cpp

Not integral tipes:
float       double      long double    nan(inf, 0/0, etc)


Префиксы:
0x - шестнадцатиричная
0 - восьмиричная
0b - бинарная 
```
```cpp
switch(n){     (если не писать 'break' то будут выполняться все строки ниже)
	case 0:
		...
		break;
	case 1:
		...
		break;
	default:
		...
}
```
```cpp
n++  (выводит n)        ++n  (выводит n + 1)

&& and       || or       ^ xor

<condition> ? <true> : <false>;

void function(int a);   (forward declaration)
```
