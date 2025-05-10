`x >> n` means shifting all the bits of `x` to the right by `n` places using sign extension (no padding).

This is the equivalent of x / 2<sup>n</sup>.

`x >>> n` means to right-shift `x` by `n` places and padding the left-added bits with `0`s.