# Git References

Git Referecnes (refs) are simple text files stored under `.git/refs`. They
contain either:
- A SHA-1 object hash (direct reference).
- A reference to another ref (indirect reference), like `refs/heads/mean`.

Refs always resolve to a specific object. No cycles are allowed.

## Tags as References

Tags are user-defined names pointing to Git objects, typically commits. They're
commonly used to mark software releases.

### Example:

`git tag v1.2.3 6071c08`

This makes `v1.2.3` an alias for the commit `6071c08`. After this,
`git checkout v1.2.3` is the same as `git checkout 6071c08`.

Tags have no intrinsic meaning in Git and can point to any object.

## Lightweight Tags vs Tag objects

- Lightweight Tags
  Simple refs pointing directly to an object. No extra data.

- Tag Objects
  Full Git objects of type `tag` (same format as commits), containing:
  - Author
  - Date
  - Optional annotation
  - Optional PGP signature

Implemented by subclassing `GitCommit` with `fmt = b'tag'`.

## `git tag` Command

- `git tag` 
  List all existing tags.
- `git tag NAME [OBJECT]` 
  Create a new leightweight tag pointing to `OBJECT` (defaults to `HEAD`).
- `git tag -a NAME [OBJECT]`
  Create a tag object with metadata pointing to `OBJECT` (defaults to `HEAD`).
