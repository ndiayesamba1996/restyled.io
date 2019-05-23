## `parse error on input \8216@\8217`

If the HLint restyler fails, and a message like the following is visible:

```
refactor: (RealSrcSpan SrcSpanOneLine "./src/Something.hs" 12 33 34,"parse error on input \8216@\8217")
CallStack (from HasCallStack):
  error, called at src/Refact/Run.hs:190:22 in main:Refact.Run
```

This is because the module contains [`TypeApplications`](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeApplications), which `apply-refact` can't handle.

Unfortunately, the only workaround is to [exclude the file from Restyler](https://github.com/restyled-io/restyled.io/wiki/Configuration-Reference#pattern).