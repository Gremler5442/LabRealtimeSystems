# LabRealtimeSystems
Код вычисления целочисленного квадратного корня на языке C
```C
#include <stdio.h>
#include <stdlib.h>

//глобальные переменные
int small;
int large;
int n = 1234;

//функция вычисления целочисленного корня
int sqrtFunc(int value) {
    //если значение отрицательное
    if (value < 0) {
        //сообщение об ошибке
        printf("error\n");
        //остановка выполнения программы
        exit(EXIT_FAILURE);
    }
    //если значение не отрицательное и меньше двух
    else if (value < 2) {
        //возвращаем значение
        return value;
    }
    else {
        //делаем побитовый сдвиг, записываем как наименьшее
        small = sqrtFunc(value >> 2) << 1;
        //вычисляем наибольшее
        large = small + 1;
        //если исходное число меньше квадрата наибольшего
        if (large * large > value) {
            //возвращаем наименьшего кандидата
            return small;
        }
        else {
            //возвращаем наибольшего кандидата
            return large;
        }
    }
}

int main() {
    n = sqrtFunc(n);
    printf("%d\n", n);
    return 0;
}
```
Код вычисления целочисленного корня на Asm
```asm
//объявляем переменные
small:
        .zero   4
large:
        .zero   4
n:
        .long   1234
.LC0:
        .string "error"
sqrtFunc:
        //помещаем rbp в стек
        push    rbp
        //копируем содержимое rbp в rsp
        mov     rbp, rsp
        //вычитаем 16 из регистра rsp
        sub     rsp, 16
        //копируем значение регистра rbp, вычитаем 4 и помещаем в edi
        mov     DWORD PTR [rbp-4], edi
        //сравниваем значение регистра rbp за вычетом 4 с нулем
        cmp     DWORD PTR [rbp-4], 0
        //если значение положительное jmp в точку .L2
        jns     .L2
        mov     edi, OFFSET FLAT:.LC0
        call    puts
        mov     edi, 1
        call    exit
.L2:
        cmp     DWORD PTR [rbp-4], 1
        //если значение больше 1 jmp в точку .L3
        jg      .L3
        mov     eax, DWORD PTR [rbp-4]
        //иначе jmp в точку .L4
        jmp     .L4
.L3:
        mov     eax, DWORD PTR [rbp-4]
        //сдвигаем вправо на 2 бита
        sar     eax, 2
        mov     edi, eax
        call    sqrtFunc
        add     eax, eax
        mov     DWORD PTR small[rip], eax
        mov     eax, DWORD PTR small[rip]
        add     eax, 1
        mov     DWORD PTR large[rip], eax
        mov     edx, DWORD PTR large[rip]
        mov     eax, DWORD PTR large[rip]
        //умножаем eax с edx
        imul    eax, edx
        cmp     DWORD PTR [rbp-4], eax
        //если значение больше или равно, jmp в точку .L5
        jge     .L5
        mov     eax, DWORD PTR small[rip]
        jmp     .L4
.L5:
        mov     eax, DWORD PTR large[rip]
.L4:
        leave
        ret
.LC1:
        .string "%d\n"
main:
        push    rbp
        mov     rbp, rsp
        mov     eax, DWORD PTR n[rip]
        mov     edi, eax
        call    sqrtFunc
        mov     DWORD PTR n[rip], eax
        mov     eax, DWORD PTR n[rip]
        mov     esi, eax
        mov     edi, OFFSET FLAT:.LC1
        mov     eax, 0
        call    printf
        mov     eax, 0
        pop     rbp
        ret
```
