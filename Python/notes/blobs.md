# Git Object Type: Blob

## Overview

Git supports four object types:
- `blob'
- 'tree'
- 'commit'
- 'tag'


### Blob: The simplest Type

- Represents **file content**
- No internal structure or metadata
- Used for source files, binaries, documents, etc.

### Characteristics

- **Format**:
`blob <size>\0<file data>`
- **No Parsing required**:
- No headers or fields beyond the initial object header
- Stored as-is (raw bytes)

- **Ideal for Direct Mapping**
- `serialize()` -> returns input as-is
- `deserialize()` -> returns data as-is

### Example

`echo "Hello" | git hash-object --stdin -w`
- Stores a blob containing "Hello"
- Internally saved as:
`blob 6\0Hello`

### Summary

Blobs are pure content with no structural complexity. Git treats them as opaque
binary data. They are the foundation of file tracking in the object database.

# Understanding Git Blobs with a Simple Example

## What's a Blob?

A blob in Git is just a file's content. no filename, no permissions,
no metadata -- just the bytes of the file itself.

Git uses blobs to store the actual contents of files in your project.

## Real example

You have a file named `hello.txt`:
hello.txt:
`Hello, world!`

### Step 1: Store it as Blob

`git hash-object -w hello.txt`

Git will:
- Read the contents of `hello.txt`
- Add a header: blob 13 (13 is the number of bytes in "Hello, world!\n")
- Combine it:
`blob 13\0Hello, world!`

- Compute a SHA-1 hash of that full data
- Compress it
- Store it in `.git/objects/<hash>`

### Step 2: View the Blob

`git cat-file -p <hash>`

Output:
`Hello, world!`

### Why Use Blobs?
- Git can store duplicate file contents only once
- Keeps content separate from filenames or directory info (handled by trees)

### Summary

Blobs = raw file content
No filenames, no structure
They're the simplest Git objects: just data
