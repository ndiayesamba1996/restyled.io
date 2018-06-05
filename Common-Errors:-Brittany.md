## Unknown syntactical constructs

```
ERROR: encountered unknown syntactical constructs:
HsSpliceE{}
```

Brittany (as of v0.11.0.0) doesn't handle Template Haskell (among other things), and generates the above error when it sees it. The solution is to ignore the offending files.

```yaml
---
restylers:
  - brittany:
      include:
        - "**/*.hs"
        - "!src/MyBadFile.hs"
```