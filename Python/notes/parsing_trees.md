# Parsing Trees

Unlike tags and commits, tree objects are binary objects, but their format is actually quite
simple. A tree is the concatenation of records of the format:

`[mode] space [path] 0x00 [sha-1]`

- `[mode]` is up to six bytes and is an octal representation of of a file mode, stored in ASCII.

  For example, 100644 is encoded with byte values 49 (ASCII "1"), 48 (ASCII "0"), 48, 54, 52,
  52. The first two digits encode the file type (file, directory, symlink or submodule), the
  permissions.
- It's follwed by 0x20, as ASCII space;
- Followed by the null-terminated (0x00) path;
- Followed by the object's SHA-1 in binary encoding, on 20 bytes.
