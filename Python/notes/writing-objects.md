# Writing a Git Object: Inverted Reading

Writing a GIt object is conceptually the reverse of reading one.

## Steps to Write an Object

1. **Contruct the Content**
  Format:
  '<type><size>\0<raw content>'

Example (for a blob):

blob 13\0Hello, world!

2. **Compute the Hash**
- Apply SHA-1 to the entire contructed content (including header).
- The produces the object ID.
- **Important**: The hash covers the full object *before compression*.

3. **Compress the Object**
- Use 'zlib' to compress the full header + content.

4. **Store the Object**
- Use the first 2 characters of the SHA-1 as the directory name.
- Use the remaining 38 characters as the filename.
- Write the compressed data to:
'''
.git/objects/<first 2 chars>/<remaining 38 chars>
'''

## Insights

- The hash is **not** just of the file content.
- It is not the hash of:
'<type><size>\0<raw content>'
- Git verifies and locats objects using this precise hash, not a post-compression fingerprint.

## Summary

- Reading = decompress -> parse header -> extract content
- Writing = build header + content -> hash -> compress -> store
