FoldConstantsPass has a dependency `onnx_ir`
It's a class that should fold constant expressions in a model.
`What is a constant expression?:`
A constant expression is basically a pre-defined function that is independent of input data and unchanged during inference. Things like model params, hyperparams (clipping thresholds etc).

So folding is the process where the compiler pre-calculated the results during compile time to reduce computational load and shrink the comp graph.

The class is big so right now I don't know it's full internals, but it's class docs are:
```markdown
    A pass that folds constant expressions in the model.

    Attributes:
        shape_inference: Whether to perform shape inference.
        input_size_limit: Maximum size of input tensors to fold.
        output_size_limit: Maximum size of output tensors to fold.
        should_fold: An optional function that takes a node and returns True if
            the node should be considered for folding.
            The function should return True/False value to indicate if this particular
            node should be folded, or None to use the default folding rules.
```

--- 

ONNX IR is an in-memory Intermediate Representation that supports the full ONNX spec for graph construction, analysis, and transformation, for representing machine learning models as a framework-independent computational graph. Allowing models trained in various frameworks to be run on targeted hardware without rewriting code.

---

Ok after a bit of probling, I noticed an issue, if the input dim in [?, 512, 4, 1] is unspecified or is -1, the return value will be [-1, -1] meaning that onnxscript will have to infer all dimensions in the output tensor. 

I don't fully know if this is what the issue is refering to, but I'll check out:

1. What issue creator is actually tagging and talking about, line by line
2. Commits linked by copilot as fixes
3. Their proposed solution, if it has already been fixed and if the issue should be closed.

---
[13/08/2026]
The user says:


#### description
For the below pattern, it will return a invalid model after `FoldConstantsPass(shape_inference=True)`. And the `should_fold` attribute cannot control the `Shape`'s evaluator. The issue only worked on `-1` shape rather than `SymbolicDim`.

input module

<img width="367" height="529" alt="Image" src="https://github.com/user-attachments/assets/76610af0-447c-4700-9371-a2b3d67e9911" />

output module

<img width="452" height="310" alt="Image" src="https://github.com/user-attachments/assets/71a8f9a8-241c-4f73-8969-e53b235cd565" />

Let's check the objects this concerns. first is the `SymbolicDim`, it's part of the `onnx_ir library`, onnxscript just imports it as a dependency and uses it like it's own object with some wrappers for convenience.

SymbolicDim -> It's a immutable way to represent a non-integer dimension in a tensor shape. It can be compared or hashed.

- Let's look at the first sentence in the description: the pattern below is the model, now `FoldConstantsPass(shape_inference=True)`.

FoldConstatntsPass -> 
```
A pass that folds constant expressions in the model.

    Attributes:
        shape_inference: Whether to perform shape inference.
        should_fold: An optional function that takes a node and returns True if
            the node should be considered for folding.
            The function should return True/False value to indicate if this particular
            node should be folded, or None to use the default folding rules.
        ...
```

- OK I think I understand the problem, when the model is -1 in it's ? dim, it fails -> we tested this, we know why (double shape inference), however what if `SymbolicDim.value` is also -1? Model should be invalid as well, however the user is saying a check is never done for this, lemme see whether a fix was applied. 

---
I'm trying to understand the fix, but I'm indecisive, should I go into a whole debug style approach and see what code we're touching? I think I do here. Lemme work back.

The fix was this:
```
-    if all(isinstance(d, int) for d in shape_slice):
+    if all(isinstance(d, int) and d >= 0 for d in shape_slice):

in shape function which has a decorator `register` that also has an argument for a callable, we'll check that later, let me see what shape function does rq, I might just document most things in my notes.
```

Ok that was really quick, I understand the copilot fix, why wasn't it merged? Just maintainer hasn't gotten to it yet, fair enough.

So this is # Closed IMHO
