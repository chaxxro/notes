# 柔性数组

## 数组形式

1. 定长数组：浪费空间

```cpp
struct buf {
    int len;
    char data[1024];
}
```

2. 指针数组：需要手动申请和释放内存

```cpp
struct buf {
    int len;
    char* data;
}
```

3. 柔性数组

## 定义

```cpp
struct buf {
    int len;
    char buf[];
}
```

- 柔性数组成员必须定义在结构体里面且为最后元素

- 结构体中不能单独只有柔性数组成员

- 柔性数组不占内存

在一个结构体的最后，申明一个长度为空的数组，就可以使得这个结构体是可变长的

对于编译器来说，此时长度为 0 的数组并不占用空间，因为数组名本身不占空间，它只是一个偏移量，数组名这个符号本身代表了一个不可修改的地址常量

结构体使用指针地址不连续（两次 `malloc`），柔性数组地址连续，只需要一次 `malloc`，同样释放前者需要两次，后者可以一起释放

`sizeof` 返回不包括柔性数组的内存

```cpp
typedef struct pen {
    int len;
    char data[];//柔性数组
} pen;

int main(int argc, char **argv) {
    char str[] = "this is a new string";//需要填入的字符串
    struct pen *p = (struct pen*)malloc(sizeof(pen) + strlen(str) + 1);
    p->data= NULL;
    p->len = strlen(str);
    strcpy((char *)(p+1), str);
    printf("pen->data: %s\n", p->data);
}
```