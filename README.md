# Context Engine Examples

Example integrations and usage patterns for the [Context Engine](https://github.com/contextenginehq/context-engine).

## Examples

### CLI Usage

```bash
# Build a cache from markdown documentation
context build --sources ./docs --cache ./my-cache

# Query for relevant context within a 4000-token budget
context resolve --cache ./my-cache --query "how to deploy" --budget 4000

# Inspect cache metadata
context inspect --cache ./my-cache
```

### MCP Server with Claude Desktop

Add to your Claude Desktop MCP configuration:

```json
{
  "mcpServers": {
    "context": {
      "command": "/path/to/mcp-context-server",
      "args": ["--cache-root", "/path/to/caches"]
    }
  }
}
```

The server exposes three tools:
- `context.resolve` — Query for relevant documents within a token budget
- `context.list_caches` — List available context caches
- `context.inspect_cache` — Inspect cache metadata and validity

### Programmatic Usage (Rust)

```rust
use context_core::{CacheReader, Scorer, Selector};

// Read a pre-built cache
let cache = CacheReader::open("./my-cache")?;

// Score documents against a query
let scored = Scorer::score(&cache, "deployment");

// Select documents within budget
let selection = Selector::select(scored, 4000);

for doc in &selection.documents {
    println!("{}: {} tokens (score: {})", doc.id, doc.tokens, doc.score);
}
```

## More Examples Coming

- CI/CD integration patterns
- Multi-cache workflows
- Custom scoring pipelines
- SDK usage (JavaScript, Python)

## License

Apache-2.0

---

"Context Engine" is a trademark of Context Engine Contributors. The software is open source under the [MIT License](LICENSE). The trademark is not licensed for use by third parties to market competing products or services without prior written permission.
