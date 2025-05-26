# KVLM Module

##  Purpose 

`kvlm_parse` reads the raw byte content of Git commit/tag objects and converts it into a structured Python dictionary
`kvlm_serialize` does the reverse: it converts the dictionary back to raw Git object format.

## kvlm_parse

- Splits headers and message body in a Git object.
- Key-value pairs (like `author`, `committer`) become dictionary entries.
- Multi-line values (continuation lines starting with a space) are normalized.
- Duplicate keys (e.g., multiple 'parent') become lists.
- Message body after the blank line is stored under the key 'None'.

## kvlm_serialize

- Converts the dictionary back to raw Git object format.
- All key-value pairs are formatted as `key value\n`.
- Lists (from duplicate keys) are serialized as repeated lines.
- Continuation lines are prepended with a space after each newline.
- The message body (`kvlm[None]`) is appended after a blank line.

### Example Input (bytes)

```
tree 012345...
parent abcdef...
author Some One <someone@example.com> 1234567890 +0000
committer Some One <someone@example.com> 1234567890 +0000

This is a commit message. 
```

### Parsed Output
```

{
  b'tree': b'012345...',
  b'parent': b'abcdef...',
  b'author': b'Some One <someone@example.com> 1234567890 +0000',
  b'committer': b'Some One <someone@example.com> 1234567890 +0000',
  None: b'This is a commit message.\n'
}

