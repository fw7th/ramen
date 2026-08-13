## Shape
Computes the shape of a given tensor, has two args start and end which denote the rank you are starting and ending from.
Returns a 1D tensor specifying the shape of the input tensor.
Examples;
```
Input tensor with shape: [2, 3, 4]
No attributes specified.
Output: [2, 3, 4]
```

```
Input tensor with shape: [2, 3, 4]
start: 1
end: 2
Output: [3]
```

```
Input tensor with shape: [2, 3, 4]
end: -1
Output: [2, 3]
```
