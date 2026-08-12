When converting .pt files to another library's format, you should take these steps:
- Deserialize: Get the raw tensors out, get the state_dict
- Transplant: Map into your tensor format; Once you have the raw data and shape, you construct equivalent tensors in your library.
- Wire: Reconstruct the computation graph; The `.pt` file contains no graph. It is just a bag of weights. You must know the architecture separately. For each named tensor, you attach it to the corresponding operation in your library:

| PyTorch name      | What it is                 | Your library equivalent         |
| ----------------- | -------------------------- | ------------------------------- |
| `fc.weight`       | 2D matrix `[out, in]`      | Feedforward layer weight matrix |
| `fc.bias`         | 1D vector `[out]`          | Feedforward layer bias vector   |
| `conv.weight`     | 4D `[out_ch, in_ch, H, W]` | Convolution kernel              |

- Operator alignment: Make sure the math matches; ensure your lib's operators compute the same thing as pytorch's. E.g. pytorch does `y = x @ W.T + b`, but some libs do `y = x @ W + b`, so you need to check operations match.

- Verification: Bit-level parity; after conversion, check whether everything matches exactly error should be like `1e-5` or `1e-6`, if it's bigger then there's a mismatch with what you're doing.
