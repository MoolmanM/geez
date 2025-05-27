# References

**Refs** are simple text files in Git used to name and point to specific
objects (commits, branches, etc.) without using full SHA-1 hashes directly.

## Key Properties:

- Located under `.git/refs` directory.
- Contain:
  - A **SHA-1 hash** directly (direct reference):
  ```
  6071c08bcb4757d8c89a30d9755d2466cef8c1de
  ```
  - Or a **reference to another ref** (indirect reference):
  ```
  ref : refs/remotes/origin/master
  ```

### Terminology:
- **Direct Reference**: Points directly to an object ID.
- **Indirect Reference**: Points to another reference, which ultimately
resolves to an object ID.
- No loops are allowed in references.

### Summary:
- Refs are plain text files storing object hashes or references to other refs.
- They simplify access to objects using human-readable names like `HEAD`,
`main`, or `feature/*`.
