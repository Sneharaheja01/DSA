#include <stdio.h>
#include <ctype.h>
#include <string.h>

#define MAX 100

char stack[MAX];
int top = -1;

void push(char c) {
    if (top == MAX - 1)
        return;
    stack[++top] = c;
}

char pop() {
    if (top == -1)
        return '\0';
    return stack[top--];
}

char peek() {
    if (top == -1)
        return '\0';
    return stack[top];
}

int precedence(char op) {
    switch (op) {
        case '^': return 3;
        case '*':
        case '/': return 2;
        case '+':
        case '-': return 1;
        default: return 0;
    }
}

int isoperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '^');
}

void infixToPostfix(char infix[]) {
    char postfix[MAX];
    int j = 0;

    for (int i = 0; i < strlen(infix); i++) {
        char c = infix[i];
        if (isalnum(c)) {
            postfix[j++] = c;
        } else if (c == '(') {
            push(c);
        } else if (c == ')') {
            while (peek() != '(')
                postfix[j++] = pop();
            pop(); // remove '('
        } else if (isoperator(c)) {
            while (precedence(peek()) >= precedence(c))
                postfix[j++] = pop();
            push(c);
        }
    }
    while (top != -1)
        postfix[j++] = pop();
    postfix[j] = '\0';
    printf("Postfix Expression: %s\n", postfix);
}

int main() {
    char infix[MAX];
    printf("Enter infix expression: ");
    scanf("%s", infix);
    infixToPostfix(infix);
    return 0;
}
