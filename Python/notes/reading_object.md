# Git Object Storage Internals

- Git uses SHA-1 hashes (40 hex chars) for object names.
- First 2 chars -> subdirectory (256 possibilities): `.git/objects/a7/...`
- Files are zlib-compressed binary data.

## Parsing Steps

1. Read file in binary mode.
2. Decompress with `zlib`.
3. Header = `<type> <size>\0`
4. Validate type and size.
5. Instantiate corresponding object class.

# Git Commit Object Breakdown (Example)

## Example SHA-1 Hash

e68ff0a3c6cdbb4e846dbb5b79a2e75f0c9c1633

- **Type**: 40-character SHA:1 hash (hexadecimal)
- **Generated From**: Commit object contents
- **Format**: `SHA-1(commit_header + tree + parent + author + committer + message)`

---

## File Storage Location

Git stores this object in:

.git/objects/e6/8ff0a3c6cdbb4e846dbb5b79a2e75f0c9c1633

- **Directory**: `e6` -> first two characters
- **Filename**: `8ff0a3c6cdbb4e846dbb5b79a2e75f0c9c1633` -> remaining 38 characters

---

## Decompressed Object Content

Assume running:

```
zlib.decompress(open(".git/objects/e6/8f...", "rb").read())
```

Yields raw commit data like:

`commit 220\x00`
Header: type = commit, size = 220 bytes

`tree d8329fc1cc938780ffdd9f94e0d364e0ea74f579`
Reference to the tree object (directory state at this commit)

`parent 7e3f3e8a81cf8b8e35a5c9fdb2963fcd9f3e3ec3`
Previous commit hash (can be multiple for merges)

`author John Doe <john@example.com> 1699655237 +0000`
Create info and UNIX timestamp

`committer John Doe <john@example.com> 1699655237 +0000`
Committer info (may differ in rebases)

Blank line + message
The actual commit message text

### Summary

- Git hashes the content, not the files names
- Commit objects point to tree objects, not file content directly
- Everything is content-addressed and stored in .git/objects/
