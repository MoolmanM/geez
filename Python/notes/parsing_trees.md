# Parsing Trees

Unlike tags and commits, tree objects are binary objects, but their format is actually quite
simple. A tree is the concatenation of records of the format:

`[mode] space [path] 0x00 [sha-1]`

- `[mode]` is up to six bytes and is an octal representation of of a file mode, stored in ASCII.
  For example, 100644 is encoded with byte values 49 (ASCII "1"), 48 (ASCII "0"), 48, 54, 52, 52.
  The first two digits encode the file type (file, directory, symlink or submodule), the
  permissions.
- It's follwed by 0x20, as ASCII space;
- Followed by the null-terminated (0x00) path;
- Followed by the object's SHA-1 in binary encoding, on 20 bytes.

## Tree Serialization and Sorting in Git

When serializing Git tree objects(writing them back), entires may have been added or modified.
It is crucial to **sort these entries consistently** due to Git's identity rules:

- **Git Identity Rule**: Two equivalent objects (e.g., trees describing the same directory structure) **must produce the same hash**.
- **Implication**: Differently sorted trees wtih the same contents are still equivalent in struture, but will result in **different SHA-1
identifiers**, which violates this rule.
- **Validation**: Git does **not enforce** correct sorting. However, **incorrectly sorted trees are technically invalid**.

### Real-World Consequence

- Invalid trees may not cause immediate errors but lead to **subtle, hard-to-diagnose bugs**.
- Example bug: Git status reporting a clean worktree as **fully modified**.
- Cause: Improperly sorted tree entires created invalid trees (observed in the `geez` implementation)

### Sorting Mechanism

- **Entries are sorted alphabetically by name**.
- **Special Rule**: Directories (tree entries) are considered to have a trailing `/`.
  - This means:
    - A file names `whatever` sorts **before** `whatever.c`.
    - A directory named `whatever` sorts **after** `whatever.c`, as if named `whatever/`.

This behavior is defined in the `base_name_compare` function in Git's source (`tree.c`).
