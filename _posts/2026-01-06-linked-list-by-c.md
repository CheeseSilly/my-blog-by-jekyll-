---
title: 2026/1/6 链表模板(C/C++)
author: Sillycheese
date: 2026/1/6 11:20:51 +0800
categories: 八股&板子
---

```c
typedef struct MyLinkedList {

    int val;

    struct MyLinkedList* next;

} MyLinkedList;

  

MyLinkedList* myLinkedListCreate() {

    MyLinkedList* head = malloc(sizeof(MyLinkedList)); // dummy head

    head->next = NULL;

    return head;

}

  

int myLinkedListGet(MyLinkedList* obj, int index) {

    MyLinkedList* cur = obj;

  

    while (cur && index > 0) {

        cur = cur->next;

        index--;

    }

  

    if (!cur || !cur->next) return -1;

    return cur->next->val;

}

  

void myLinkedListAddAtHead(MyLinkedList* obj, int val) {

    MyLinkedList* node = malloc(sizeof(MyLinkedList));

    node->val = val;

    node->next = obj->next;

    obj->next = node;

}

  

void myLinkedListAddAtTail(MyLinkedList* obj, int val) {

    MyLinkedList* cur = obj;

    while (cur->next) {

        cur = cur->next;

    }

    MyLinkedList* node = malloc(sizeof(MyLinkedList));

    node->val = val;

    node->next = NULL;

    cur->next = node;

}

  

void myLinkedListAddAtIndex(MyLinkedList* obj, int index, int val) {

    if (index < 0) return;

  

    MyLinkedList* cur = obj;

    while (cur && index > 0) {

        cur = cur->next;

        index--;

    }

  

    if (!cur) return;

  

    MyLinkedList* node = malloc(sizeof(MyLinkedList));

    node->val = val;

    node->next = cur->next;

    cur->next = node;

}

  

void myLinkedListDeleteAtIndex(MyLinkedList* obj, int index) {

    MyLinkedList* cur = obj;

  

    while (cur && index > 0) {

        cur = cur->next;

        index--;

    }

  

    if (!cur || !cur->next) return;

  

    MyLinkedList* to_delete = cur->next;

    cur->next = to_delete->next;

    free(to_delete);

}

  

void myLinkedListFree(MyLinkedList* obj) {

    MyLinkedList* cur = obj;

    while (cur) {

        MyLinkedList* next = cur->next;

        free(cur);

        cur = next;

    }

}
```
