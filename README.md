# Data representation module

## Undefined behavior

#### Test that a signed addition will not overflow
```c
int add (int x, int y) {
    return x + y;
}
```
Here are a few options:
1. ```c
   assert(x >= 0 ? (unsigned int) x + (unsigned int) y >= (unsigned int) y :(unsigned int) x + (unsigned int) y < (unsigned int) y);
   ```
2. ```c
   assert ((x > 0 && y > 0 && INT_MAX - x > y) || (x < 0 && y < 0 && INT_MIN - x < y));
   ```
3. ```c
   assert(~((((unsigned int)a ^ ~(unsigned int)b) & ((unsigned int)a ^ (unsigned int)(a+b))) & 0x80000000));
   ```

#### memcpy
```c
int main (int argc, char* argv[]){
    if (argc <= 3) {
        exit(1);
    }

    int src_size = strtol(argv[1], 0, 0);
    int dst_size = strtol(argv[2], 0, 0);
    int copy_size = strtol(argv[3], 0, 0);

    int * src = malloc(sizeof(int) * src_size); 
    int * dst = malloc(sizeof(int) * dst_size);
    for (int i = 0; i < src_size; i ++)
        src[i] = i;
    memcpy(dst, src, copy_size);
}
```
1. Test that memcpy does not overflow
    ```c
    assert(src_size >= size);
    assert(dst_size >= size);
    ```
2. Test if memcpy area overlaps
   ```c
   assert(dst+size < src || src + size < dst)
   ```

<!-- 
assert(dst!=NULL);
    assert(src!=NULL); 
-->



## Data representation


## Alignments

Write down the size and the alignment of the following `struct`s. Hint: draw out how an instance of each `struct` would look in memory.

```c
struct struct1 {
    int i;
};
```
* Alignment:
* Size:

```c
struct struct2 {
    short a;
    short b;
    short c;
};
```
* Alignment:
* Size:

```c
struct struct3 {
    char a;
    int b;
};
```
* Alignment:
* Size:

```c
struct struct4 {
    int b;
    char a;
};
```
* Alignment:
* Size:

```c
struct struct5 {
    char a;
    char b;
    int c;
};
```
* Alignment:
* Size:

```c
struct struct6 {
    char a;
    struct {
        char b;
        int c;
    } s;
};
```
* Alignment:
* Size:

```c
union union1 {
    char a;
    int b;
};
```
* Alignment:
* Size:

```c
struct struct7 {
    char a;
    union {
        char b;
        int c;
    } u;
};
```
* Alignment:
* Size:

## Pointers

```c
struct Node { 
    int data; 
    struct Node* next; // Pointer to next node in DLL 
    struct Node* prev; // Pointer to previous node in DLL 
};
```

The following function pushes a `new_data` at the front of the double linked list. Is it correct? 
```c
void push(struct Node** head_ref, int new_data) 
{ 
    /* allocate node */
    struct Node* new_node = (struct Node*)malloc(sizeof(struct Node)); 
  
    /* put in the data  */
    new_node->data = new_data; 
  
    /* set prev for head node */
    (*head_ref)->prev = new_node;

    /* reset head node */
    (*head_ref) = new_node;

    /* set prev/next for new node */
    new_node->next = (*head_ref); 
    new_node->prev = NULL;
}
```

## Bitwise arithmetic & operators




## Arena allocator (summary)




<!-- 
bugs: Check NULL of head_ref
orders matter. 
Write single linked list first?
Compare performance of single list and double linked in sorting?

    /* 3. Make next of new node as head and previous as NULL */
    new_node->next = (*head_ref); 
    new_node->prev = NULL; 
  
    /* 4. change prev of head node to new node */
    if ((*head_ref) != NULL) 
        (*head_ref)->prev = new_node; 
  
    /* 5. move the head to point to the new node */
    (*head_ref) = new_node;  
    
-->