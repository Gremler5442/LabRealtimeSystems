# LabRealtimeSystems

#include <stdio.h>

int small;
int large;
int n = 1234;

int sqrtFunc(int value) {
    if (value < 0) {
        return 0;
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
