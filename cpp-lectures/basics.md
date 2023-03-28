sizeof()    (размер точно не задан, свой под архитектуру)
static_cast<long>()

Integral tipes:
1 bite <= short <= int <= long <= long long

     >= 8     >= 16    >= 32    >= 64    in bits 

int64_t     (64 бита ровно)
int_fast64_t    (может быть и больше 64, если быстрее)

unsigned

const 

char   (литерал, может быть знаковым) = 1 bite >= 8 bit
wchar_t    (хранит символы юникода)

bool  =  1 bite  (default = false)

overflow:
-128…127   старший бит отвечает за знак (он всегда -2**31)
Переполнение signed типа undefined, для unsigned - mod

Not integral tipes:
float       double      long double    nan(inf, 0/0, etc)


Префиксы:
0x - шестнадцатиричная
0 - восьмиричная
0b - бинарная 

Сразу пытается привести к signed потом к unsigned, в зависимости от вместимости.
Может привести к ошибке из-за переполнения при операциях с разными типами.
Например int64t приведется к uint64t

overflow:
-128…127   старший бит отвечает за знак (он всегда -2**7)

1111 1111 → -1

std::endl    (принудительная печать и \n)

<condition> ? <true> : <false>;
