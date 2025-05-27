# cmd_log

Implements subcommand for a Git-like tool, which outputs a commit history in Grahpviz DOT format for visualization.

## Argument Parsing:

Adds a `log` subcommand. It accepts an optional `commit` arguement (default is `"HEAD"`), used as the starting point for history traversal.

## cmd_log(args):
Finds the Git repository, begins a DOT-format directed graph output with Graphviz syntax, then calls `log_graphviz` to recursively print commit nodes
and edges.

## log_graphviz(repo, sha, seen):
Recursively walks through commit history:
- Skips already visited commites using the `seen` set.
- Reads the commit object from disk.
- Extracts and santizes the commit message for Graphviz safety (escapes `\` and `"`)
- Displays only the first line of the message.
- Prints a node with the commit's short SHA and santized message.
- If the commit has parents, prints directed edges to each parent and recurses.

Used to visualize commit ancestry with nodes representing commits and edges showing parent relationships.

### Example Output (Graphviz DOT format):

```
digraph geezlog
 node[shape=rect]
 c_abc1234 [label="abc1234: Initial commit"]
 c_def5678 [label="def5678: Added feature"]
 c_def5678 -> c_abc1234;
}
```
