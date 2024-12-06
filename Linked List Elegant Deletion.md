```cpp
void delete(node **head, node *entry) {
  printf("head **: %p \thead*: %p\n", head, *head);
  node **indirect = head;

  while ((*indirect) != entry){
    indirect = &(*indirect)->next;
  } 

  node *temp = *indirect;
  *indirect = (*indirect)->next;
  free(temp);
}
```

Linus Torvalds showed this as an example of good taste in coding. This is a method for deleting a given node from a linked list.

This solution is not intiutive at the first glance.

```
1 (Node adress: 0x5575516632a0  Next Node Adress: 0x5575516632c0        Adress of the next node adress: 0x5575516632a8)--> 
2 (Node adress: 0x5575516632c0  Next Node Adress: 0x5575516632e0        Adress of the next node adress: 0x5575516632c8)--> 
3 (Node adress: 0x5575516632e0  Next Node Adress: (nil) Adress of the next node adress: 0x5575516632e8)--> 

1 (Node adress: 0x5575516632a0  Next Node Adress: 0x5575516632e0        Adress of the next node adress: 0x5575516632a8)--> 
3 (Node adress: 0x5575516632e0  Next Node Adress: (nil) Adress of the next node adress: 0x5575516632e8)--> 

3 (Node adress: 0x5575516632e0  Next Node Adress: (nil) Adress of the next node adress: 0x5575516632e8)--> 
```

- Indirect is pointing to the head of the list.
- For each iteration, it checks whether 