# LabRealtimeSystems
Код вычисления целочисленного квадратного корня на языке C
```C
#include <stdio.h>
#include <stdlib.h>

int small;
int large;
int n = 1234;

int sqrtFunc(int value) {
    if (value < 0) {
        printf("error\n");
        exit(EXIT_FAILURE);
    }
    else if (value < 2) {
        return value;
    }
    else {
        small = sqrtFunc(value >> 2) << 1;
        large = small + 1;
        if (large * large > value) {
            return small;
        }
        else {
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
Код вычисления целочисленного кода на Asm
```asm
small:
        .zero   4
large:
        .zero   4
n:
        .long   -1234
.LC0:
        .string "error"
sqrtFunc:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     DWORD PTR [rbp-4], edi
        cmp     DWORD PTR [rbp-4], 0
        jns     .L2
        mov     edi, OFFSET FLAT:.LC0
        call    puts
        mov     edi, 1
        call    exit
.L2:
        cmp     DWORD PTR [rbp-4], 1
        jg      .L3
        mov     eax, DWORD PTR [rbp-4]
        jmp     .L4
.L3:
        mov     eax, DWORD PTR [rbp-4]
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
        imul    eax, edx
        cmp     DWORD PTR [rbp-4], eax
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
