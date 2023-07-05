# Brainfuck in Leaf

## Initialize 30,000 cells

```leaf
+*>+*>+*>+*> 29,999 times +*>+*>+*>+*>
(^)
```

## `>` Move right

```leaf
>
```

## `<` Move left

```leaf
^
```

## `+` Increment cell (non-wrapping)

```leaf
{<      Set the root and enter the cell
(>)*    Move to the tail of the cell and add a leaf
(^)}    Return to the tape and pop the root
```

```leaf
{<(>)*(^)}
```

## `-` Decrement cell (non-wrapping)

```leaf
<{      Enter the cell and set the root
(
  (>)   Move to the tail of the cell
  ?     Break, if the value is 0
  -(^)  Otherwise, remove the tail leaf and return to the root
^)      Break
}^      Pop the root and return to the tape
```

```leaf
<{((>)?-(^)^)}^
```

## `+` Increment cell (wrapping)

```leaf
<{      Enter the cell and set the root
(>)*    Move to the tail of the cell and add a leaf
(
        Move to the root and break, if the new value is less than 256
  ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^^^
  ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^^^
  ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^^^
  ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^^^
  ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^^^
  ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^^^
  ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^^^
  ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^ ?
  -^    Otherwise, delete the value to set it to 0
)
}^      Pop the root and return to the tape
```

```leaf
<{(>)*(^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^?-^)}^
```

## `-` Decrement cell (wrapping)

```leaf
<{      Enter the cell and set the root
(>)+    Move to the tail of the cell and add a "flag" left branch
(?      If the value is non-zero,
  -(^)  remove the leaf and flag and return to the root
)
<(?     If the flag is present,
  -     remove the flag,
        set the value to 255,
  *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*>
  *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*>
  *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*>
  *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*>
  *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*>
  *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*>
  *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*>
  *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>*> *>*>*>*>*>*>*>
  (^)   and return to the root
)
}^      Pop the root and return to the tape
```

```leaf
<{(>)+(?-(^))<(?-*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>*>(^))}^
```

## `[ … ]` Loop

```leaf
(       Loop
<{      Enter the cell and set the root
>?      Break, if the value is 0
}^^     Otherwise, pop the root and return to the tape
…       Execute the loop body
)       Repeat
}^      Pop the root and return to the tape
```

```leaf
(<{>?}^^ … )}^
```

## Header comment

```leaf
(? … )
```