## Unknown syntactical constructs

```
ERROR: encountered unknown syntactical constructs:
HsSpliceE{}
```

Brittany (as of v0.11.0.0) doesn't handle Template Haskell (among other things), and generates the above error when it sees it. The solution is to ignore the offending files.

```yaml
---
- brittany:
    include:
      - "**/*.hs"
      - "!src/MyBadFile.hs"
```

Alternative, you can disable the offending expression in the Haskell source

```hs
-- brittany-disable-next-binding

someFunction =
  [someQuote|
    someText
  |]

-- brittany-disable-next-binding

someOtherFunction =
  $(someOtherQuote "someText")
```

Note that `disable-next-binding` on works on top-level bindings. Attractive as it may be, you can't disable a specific binding within a `where` or `let`.

## Restyled keeps removing shebangs

If you edit a ["Stack Script"](https://docs.haskellstack.org/en/stable/GUIDE/#script-interpreter), you will find that Restyled (through Brittany) **really** wants to strip that shebang.

This is an open issue: https://github.com/lspitzner/brittany/issues/192

Unfortunately, you must exclude such files via `.restyled.yaml`.