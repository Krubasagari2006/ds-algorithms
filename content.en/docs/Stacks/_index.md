---
title: 'Stacks'
weight: 5
references:
    books:
        - b1:
            title: Data Structures and Algorithm Analysis in C by Mark Allen Weiss 
            url: https://mrajacse.files.wordpress.com/2012/08/data-structures-and-algorithm-analysis-in-c-mark-allen-weiss.pdf
        - b2:
            title: Data_Structures_and_Algorithm_Analysis_2e By mark-allen-weiss
            url: https://www.google.co.in/books/edition/Data_Structures_and_Algorithm_Analysis_i/83RWbPynhkgC?hl=en&gbpv=1

---
# Chapter 5: Stacks
## Of Wads Of Notes

### Why This Chapter Matters?

Be it items in a store, books in a library, or notes in a bank, the moment they become more than handful we start stacking them neatly. Similarly, while maintaining data in an orderly fashion it is placed in a stack. Stack data structure is used widely for storing variables, managing function calls, evaluating arithmetic expressions, etc. Hence it is important to understand this data structure.

---

## Stack Fundamentals

Stack is a data structure in which addition of new element or deletion of an existing element always takes place at the same end. This end is known as **top of stack**. This situation can be compared to a stack of plates in a cafeteria where every new plate added to the stack is added at the top. Similarly, every new plate taken off the stack is also from the top of the stack.

When an item is added to a stack, the operation is called **push**, and when an item is removed from the stack the operation is called **pop**.

Because of the nature of push and pop operations, Stack is also called **last-in-first-out (LIFO)** list.

![Figure 5-1: Insertion and deletion of elements in a Stack](image-placeholder)

A stack data structure can be maintained as an **array** or as a **linked list**.

---

## Stack as an Array

Stack contains an ordered collection of elements. An array is used to store ordered list of elements. Hence, a stack can be implemented using an array. However, we are required to declare the size of the array before using it. So, when we use it to store elements of a stack the stack can grow or shrink within the memory reserved for the array.

### Program 5-1: Stack as an array

```c
#include <stdio.h>
#define MAX 10

struct stack {
    int arr[MAX];
    int top;
};

void initstack(struct stack *);
void push(struct stack *, int item);
int pop(struct stack *);

int main() {
    struct stack s;
    int n;
    
    initstack(&s);
    push(&s, 2);
    push(&s, -4);
    push(&s, 8);
    push(&s, 11);
    
    n = pop(&s);
    if (n != NULL)
        printf("Item popped: %d\n", n);
    n = pop(&s);
    if (n != NULL)
        printf("Item popped: %d\n", n);
    n = pop(&s);
    if (n != NULL)
        printf("Item popped: %d\n", n);
    n = pop(&s);
    if (n != NULL)
        printf("Item popped: %d\n", n);
    n = pop(&s);
    if (n != NULL)
        printf("Item popped: %d\n", n);
    
    return 0;
}

/* initializes the stack */
void initstack(struct stack *s) {
    s->top = -1;
}

/* adds an element to the stack */
void push(struct stack *s, int item) {
    if (s->top == MAX - 1) {
        printf("Stack is full\n");
        return;
    }
    s->top++;
    s->arr[s->top] = item;
}

/* removes an element from the stack */
int pop(struct stack *s) {
    int data;
    if (s->top == -1) {
        printf("Stack is empty\n");
        return NULL;
    }
    data = s->arr[s->top];
    s->top--;
    return data;
}
```

### Output:
```
Item popped: 11
Item popped: 8
Item popped: -4
Item popped: 2
Stack is empty
```

**Explanation:** In this program we have defined a structure called `stack`. The `push()` and `pop()` functions are respectively used to add and delete items from the top of the stack. The actual storage of stack elements is done in an array `arr`. The variable `top` is an index into this array. It contains a value where the addition or deletion is going to take place in the array, and thereby in the stack. To indicate that the stack is empty to begin with, the variable `top` is set with a value -1 by calling the function `initstack()`.

Every time an element is added to stack, it is verified whether such an addition is possible at all. If it is not, then the message 'Stack is full' is displayed. Since we have declared the array to hold 10 elements, the stack would be considered full if the value of top becomes equal to 9.

---

## Stack as a Linked List

In the earlier section we had used arrays to store the elements that get added to the stack. However, when implemented as an array it suffers from the basic limitation of an array—that its size cannot be increased or decreased once it is declared. As a result, one ends up reserving either too much memory or too less memory for an array and in turn for a stack. This problem can be overcome if we implement a stack using a linked list.

Each node in the linked list contains the data and a pointer that gives location of the next node in the list. The pointer to the beginning of the list serves the purpose of the top of the stack.

![Figure 5-2: Representation of stack as a linked list](image-placeholder)

### Program 5-2: Stack as a linked list

```c
#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node *link;
};

void push(struct node **, int);
int pop(struct node **);
void delstack(struct node **);

int main() {
    struct node *s = NULL;
    int n;
    
    push(&s, 14);
    push(&s, -3);
    push(&s, 18);
    push(&s, 29);
    push(&s, 31);
    push(&s, 16);
    
    n = pop(&s);
    if (n != NULL)
        printf("Item popped: %d\n", n);
    n = pop(&s);
    if (n != NULL)
        printf("Item popped: %d\n", n);
    n = pop(&s);
    if (n != NULL)
        printf("Item popped: %d\n", n);
    
    delstack(&s);
    return 0;
}

/* adds a new node at beginning of linked list */
void push(struct node **top, int item) {
    struct node *temp;
    temp = (struct node *)malloc(sizeof(struct node));
    if (temp == NULL)
        printf("Stack is full\n");
    temp->data = item;
    temp->link = *top;
    *top = temp;
}

/* deletes a node from beginning of linked list */
int pop(struct node **top) {
    struct node *temp;
    int item;
    
    if (*top == NULL) {
        printf("Stack is empty\n");
        return NULL;
    }
    temp = *top;
    item = temp->data;
    *top = (*top)->link;
    free(temp);
    return item;
}

/* deallocates memory */
void delstack(struct node **top) {
    struct node *temp;
    if (*top == NULL)
        return;
    while (*top != NULL) {
        temp = *top;
        *top = (*top)->link;
        free(temp);
    }
}
```

### Output:
```
Item popped: 16
Item popped: 31
Item popped: 29
```

**Explanation:** Here we have declared a structure called `node`. The variable `s` is a pointer to the structure `node`. Initially `s` is set to `NULL` to indicate that the stack is empty. In every call to the function `push()` we are creating a new node dynamically. As long as there is enough memory for dynamic allocation, `temp` would never become `NULL`. If value of `temp` happens to be `NULL` then that would be the stage when stack would become full.

After creating a new node, the pointer `s` should point to the newly created item of the list. Hence, we have assigned the address of this new node to `s` using the pointer `top`.

In the `pop()` function, first we are checking whether or not a stack is empty. If so, then a message 'Stack is empty' gets displayed. If the stack is not empty then the topmost item gets removed from the list.

---

## Applications of Stacks

Stacks are often used in evaluation of arithmetic expression. An arithmetic expression consists of operands and operators. The operands can be constant or variables. The operators used in an arithmetic expression can be `+`, `-`, `*`, `/`, and `$` (exponentiation).

### Notation Types

| Notation | Description | Example |
|----------|-------------|---------|
| **Infix** | Operator is placed between two operands | `A + B` |
| **Prefix** | Operator comes before the operands | `+ A B` |
| **Postfix** | Operator follows the two operands | `A B +` |

**Features of prefix and postfix expressions:**
- The operands maintain the same order as in the equivalent infix expression
- Parentheses are not needed to designate the expression unambiguously
- While evaluating the expression the priority of the operators is irrelevant

**Operator Precedence (Highest to Lowest):**
1. Highest priority: Exponentiation (`$`)
2. Next highest priority: Multiplication (`*`) and Division (`/`)
3. Lowest priority: Addition (`+`) and Subtraction (`-`)

---

## Infix to Postfix Conversion

### Program 5-3: Infix to Postfix conversion

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define MAX 50

struct infix {
    char target[MAX];
    char stack[MAX];
    char *s, *t;
    int top;
};

void initinfix(struct infix *);
void setexpr(struct infix *, char *);
void push(struct infix *, char);
char pop(struct infix *);
void convert(struct infix *);
int priority(char);
void show(struct infix);

int main() {
    struct infix p;
    char expr[MAX];
    
    initinfix(&p);
    printf("Enter an expression in infix form:\n");
    gets(expr);
    setexpr(&p, expr);
    convert(&p);
    printf("The postfix expression is:\n");
    show(p);
    return 0;
}

/* initializes structure elements */
void initinfix(struct infix *p) {
    p->top = -1;
    strcpy(p->target, "");
    strcpy(p->stack, "");
    p->t = p->target;
    p->s = "";
}

/* sets s to point to given expression */
void setexpr(struct infix *p, char *str) {
    p->s = str;
}

/* adds an operator to the stack */
void push(struct infix *p, char c) {
    if (p->top == MAX)
        printf("Stack is full\n");
    else {
        p->top++;
        p->stack[p->top] = c;
    }
}

/* pops an operator from the stack */
char pop(struct infix *p) {
    if (p->top == -1) {
        printf("Stack is empty\n");
        return -1;
    }
    else {
        char item = p->stack[p->top];
        p->top--;
        return item;
    }
}

/* converts the given expression from infix to postfix form */
void convert(struct infix *p) {
    char opr;
    
    while (*(p->s)) {
        if (*(p->s) == ' ' || *(p->s) == '\t') {
            p->s++;
            continue;
        }
        if (isdigit(*(p->s)) || isalpha(*(p->s))) {
            while (isdigit(*(p->s)) || isalpha(*(p->s))) {
                *(p->t) = *(p->s);
                p->s++;
                p->t++;
            }
        }
        if (*(p->s) == '(') {
            push(p, *(p->s));
            p->s++;
        }
        if (*(p->s) == '*' || *(p->s) == '+' || *(p->s) == '/' || 
            *(p->s) == '%' || *(p->s) == '-' || *(p->s) == '$') {
            if (p->top != -1) {
                opr = pop(p);
                while (priority(opr) >= priority(*(p->s))) {
                    *(p->t) = opr;
                    p->t++;
                    opr = pop(p);
                }
                push(p, opr);
                push(p, *(p->s));
            }
            else
                push(p, *(p->s));
            p->s++;
        }
        if (*(p->s) == ')') {
            opr = pop(p);
            while ((opr) != '(') {
                *(p->t) = opr;
                p->t++;
                opr = pop(p);
            }
            p->s++;
        }
    }
    while (p->top != -1) {
        char opr = pop(p);
        *(p->t) = opr;
        p->t++;
    }
    *(p->t) = '\0';
}

/* returns the priority of an operator */
int priority(char c) {
    if (c == '$')
        return 3;
    if (c == '*' || c == '/' || c == '%')
        return 2;
    else {
        if (c == '+' || c == '-')
            return 1;
        else
            return 0;
    }
}

/* displays the postfix form of given expr. */
void show(struct infix p) {
    printf("%s", p.target);
}
```

### Output:
```
Enter an expression in infix form:
4$2*3-3+8/4/(1+1)
The postfix expression is:
42$3*3-84/11+/+
```

### Conversion Example Table

**Infix Expression:** `4 $ 2 * 3 - 3 + 8 / 4 / ( 1 + 1 )`

| Char Scanned | Stack Contents | Postfix Expression |
|-------------|----------------|-------------------|
| 4 | Empty | 4 |
| $ | $ | 4 |
| 2 | $ | 4 2 |
| * | * | 4 2 $ |
| 3 | * | 4 2 $ 3 |
| - | - | 4 2 $ 3 * |
| 3 | - | 4 2 $ 3 * 3 |
| + | + | 4 2 $ 3 * 3 - |
| 8 | + | 4 2 $ 3 * 3 - 8 |
| / | + / | 4 2 $ 3 * 3 - 8 |
| 4 | + / | 4 2 $ 3 * 3 – 8 4 |
| / | + / | 4 2 $ 3 * 3 – 8 4 / |
| ( | + / ( | 4 2 $ 3 * 3 – 8 4 / |
| 1 | + / ( | 4 2 $ 3 * 3 – 8 4 / 1 |
| + | + / ( + | 4 2 $ 3 * 3 – 8 4 / 1 |
| 1 | + / ( + | 4 2 $ 3 * 3 – 8 4 / 1 1 |
| ) | + / | 4 2 $ 3 * 3 – 8 4 / 1 1 + |
| | Empty | 4 2 $ 3 * 3 – 8 4 / 1 1 + / + |

---

## Postfix to Prefix Conversion

### Program 5-4: Postfix to Prefix conversion

```c
#include <stdio.h>
#include <string.h>
#define MAX 50

struct postfix {
    char stack[MAX][MAX], target[MAX];
    char temp1[2], temp2[2];
    char str1[MAX], str2[MAX], str3[MAX];
    int i, top;
};

void initpostfix(struct postfix *);
void setexpr(struct postfix *, char *);
void push(struct postfix *, char *);
void pop(struct postfix *, char *);
void convert(struct postfix *);
void show(struct postfix);

int main() {
    struct postfix q;
    char expr[MAX];
    
    initpostfix(&q);
    printf("Enter an expression in postfix form:\n");
    gets(expr);
    setexpr(&q, expr);
    convert(&q);
    printf("The Prefix expression is:\n");
    show(q);
    return 0;
}

/* initializes the elements of the structure */
void initpostfix(struct postfix *p) {
    p->i = 0;
    p->top = -1;
    strcpy(p->target, "");
}

/* copies given expr. to target string */
void setexpr(struct postfix *p, char *c) {
    strcpy(p->target, c);
}

/* adds an operator to the stack */
void push(struct postfix *p, char *str) {
    if (p->top == MAX - 1)
        printf("Stack is full\n");
    else {
        p->top++;
        strcpy(p->stack[p->top], str);
    }
}

/* pops an element from the stack */
void pop(struct postfix *p, char *a) {
    if (p->top == -1)
        printf("Stack is empty\n");
    else {
        strcpy(a, p->stack[p->top]);
        p->top--;
    }
}

/* converts given expr. to prefix form */
void convert(struct postfix *p) {
    while (p->target[p->i] != '\0') {
        /* skip whitespace, if any */
        if (p->target[p->i] == ' ')
            p->i++;
        if (p->target[p->i] == '%' || p->target[p->i] == '-' ||
            p->target[p->i] == '/' || p->target[p->i] == '*' ||
            p->target[p->i] == '+' || p->target[p->i] == '$') {
            pop(p, p->str2);
            pop(p, p->str3);
            p->temp1[0] = p->target[p->i];
            p->temp1[1] = '\0';
            strcpy(p->str1, p->temp1);
            strcat(p->str1, p->str3);
            strcat(p->str1, p->str2);
            push(p, p->str1);
        }
        else {
            p->temp1[0] = p->target[p->i];
            p->temp1[1] = '\0';
            strcpy(p->temp2, p->temp1);
            push(p, p->temp2);
        }
        p->i++;
    }
}

/* displays the prefix form of expr. */
void show(struct postfix p) {
    char *temp = p.stack[0];
    while (*temp) {
        printf("%c ", *temp);
        temp++;
    }
}
```

### Output:
```
Enter an expression in postfix form:
4 2 $ 3 * 3 - 8 4 / 1 1 + / +
The Prefix expression is:
+ - * $ 4 2 3 3 / / 8 4 + 1 1
```

---

## Other Inter-Conversions

![Figure 5-3: Summary of inter-conversion of expressions](image-placeholder)

| Conversion | Scan from | Token | Operation |
|-----------|-----------|-------|-----------|
| In to Post | Left to Right | Operand | Add to expression |
| | | Operator | Priority |
| In to Pre | Right to Left | ( | Add to stack / delete |
| | | ) | Add to stack / delete |
| Post to Pre | Left to Right | Operand | Push to stack |
| Post to In | Left to Right | | Pop stack into s1 |
| Pre to Post | Right to Left | | Pop stack into s2 |
| Pre to In | Right to Left | Operator | Construct expression |
| | | | Push expression on stack |

---

## Evaluation of Postfix Expression

The virtue of postfix notation is that it enables easy evaluation of expressions. To begin with, the need for parentheses is eliminated. Secondly, the priority of the operators is no longer relevant. The expression can be evaluated by making a left to right scan, stacking operands, and evaluating operators using operands popped from the stack and finally placing the result onto the stack.

### Program 5-5: Evaluation of Postfix expression

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <ctype.h>
#define MAX 50

struct postfix {
    int stack[MAX];
    int top, nn;
    char *s;
};

void initpostfix(struct postfix *);
void setexpr(struct postfix *, char *);
void push(struct postfix *, int);
int pop(struct postfix *);
void calculate(struct postfix *);
void show(struct postfix);

int main() {
    struct postfix q;
    char expr[MAX];
    
    initpostfix(&q);
    printf("Enter postfix expression to be evaluated:\n");
    gets(expr);
    setexpr(&q, expr);
    calculate(&q);
    show(q);
    return 0;
}

/* initializes structure elements */
void initpostfix(struct postfix *p) {
    p->top = -1;
}

/* sets s to point to the given expr. */
void setexpr(struct postfix *p, char *str) {
    p->s = str;
}

/* adds digit to the stack */
void push(struct postfix *p, int item) {
    if (p->top == MAX - 1)
        printf("Stack is full\n");
    else {
        p->top++;
        p->stack[p->top] = item;
    }
}

/* pops digit from the stack */
int pop(struct postfix *p) {
    int data;
    if (p->top == -1) {
        printf("Stack is empty\n");
        return NULL;
    }
    data = p->stack[p->top];
    p->top--;
    return data;
}

/* evaluates the postfix expression */
void calculate(struct postfix *p) {
    int n1, n2, n3;
    
    while (*(p->s)) {
        /* skip whitespace, if any */
        if (*(p->s) == ' ' || *(p->s) == '\t') {
            p->s++;
            continue;
        }
        /* if digit is encountered */
        if (isdigit(*(p->s))) {
            p->nn = *(p->s) - '0';
            push(p, p->nn);
        }
        else {
            /* if operator is encountered */
            n1 = pop(p);
            n2 = pop(p);
            switch (*(p->s)) {
                case '+':
                    n3 = n2 + n1;
                    break;
                case '-':
                    n3 = n2 - n1;
                    break;
                case '/':
                    n3 = n2 / n1;
                    break;
                case '*':
                    n3 = n2 * n1;
                    break;
                case '%':
                    n3 = n2 % n1;
                    break;
                case '$':
                    n3 = (int)pow((double)n2, (double)n1);
                    break;
                default:
                    printf("Unknown operator\n");
                    exit(1);
            }
            push(p, n3);
        }
        p->s++;
    }
}

/* displays the result */
void show(struct postfix p) {
    p.nn = pop(&p);
    printf("Result is: %d\n", p.nn);
}
```

### Output:
```
Enter postfix expression to be evaluated:
4 2 $ 3 * 3 - 8 4 / 1 1 + / +
Result is: 46
```

### Evaluation Example Table

**Postfix Expression:** `4 2 $ 3 * 3 - 8 4 / 1 1 + / +`

| Char. Scanned | Stack Contents |
|--------------|----------------|
| 4 | 4 |
| 2 | 4, 2 |
| $ | 16 |
| 3 | 16, 3 |
| * | 48 |
| 3 | 48, 3 |
| - | 45 |
| 8 | 45, 8 |
| 4 | 45, 8, 4 |
| / | 45, 2 |
| 1 | 45, 2, 1 |
| 1 | 45, 2, 1, 1 |
| + | 45, 2, 2 |
| / | 45, 1 |
| + | **46 (Result)** |

---

## Chapter Summary

- (a) Stack data structure is a **LIFO list** in which addition of new elements and deletion of existing elements takes place at the same end.
- (b) Addition of a new element to a stack is called **push** operation.
- (c) Deletion of an existing element from a stack is called **pop** operation.
- (d) Stack data structure can be implemented using an **array** or a **linked list**.
- (e) If stack is implemented as a linked list, push operation is like adding a new node at the beginning of the linked list.
- (f) If stack is implemented as a linked list, pop operation is like deleting an existing node from the beginning of the linked list.
- (g) Stack data structure has many applications like keeping track of function calls, storing local variable, evaluation of arithmetic expression, etc.

---

## Exercises

### Level I: Fill in the blanks

(a) A stack is a data structure in which addition of new element or deletion of an existing element always takes place at an end called ______.

(b) The data structure stack is also called _____________ list.

(c) In __________ notation the operator precedes the two operands.

(d) In __________ notation the operator follows the two operands.

### Level I: Multiple Choice

(a) Adding an element to the stack means
   1. Placing an element at the front end
   2. Placing an element at the top
   3. Placing an element at the rear end
   4. None of the above

(b) Pushing an element to a stack means
   1. Removing an element from the stack
   2. Searching a given element in the stack
   3. Adding a new element to the stack
   4. Sorting the elements in the stack

(c) Popping an element from the stack means
   1. Removing an element from the stack
   2. Searching a given element in the stack
   3. Adding a new element to the stack
   4. Sorting the elements in the stack

(d) The expression `A B *`
   1. is an infix expression
   2. is a postfix expression
   3. is a prefix expression
   4. is a stack expression

### Level II: Expression Conversion

**[C]** Transform the following infix expressions into their equivalent postfix expressions:
- `(A - B) * (D / E)`
- `(A + B ^ D) / (E - F) + G`
- `A * (B + D) / E - F * (G + H / K)`
- `(A + B) * (C - D) $ E * F`
- `(A + B) * (C $ (D - E) + F) / G) $ (H - J)`

**[D]** Transform the following infix expressions into their equivalent prefix expressions:
- `(A - B) * (D / E)`
- `(A + B ^ D) / (E - F) + G`
- `A * (B + D) / E - F * (G + H / K)`

**[E]** Transform each of the following prefix expressions to postfix and infix:
- `+ A - B C`
- `+ + A - * $ B C D / + E F * G H I`
- `+ - $ A B C * D * * E F G`

**[F]** Transform each of the following postfix expressions to prefix and infix:
- `A B C + -`
- `A B - C + D E F - + $`
- `A B C D E - + $ * E F * -`

### Level III: Coding Interview Questions

**[G]** Write programs for the following:

(a) Copying contents of one stack to another

(b) To check whether in a string containing an arithmetic expression, the opening and closing parenthesis are well-formed or not

### Case Scenario Exercise

**Prefix to postfix and infix forms**

Write a program to convert an arithmetic expression in prefix form to equivalent infix and postfix forms. Refer Figure 5-4 for the steps to be carried out in each of these conversions.
```