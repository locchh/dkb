# dkb

## Features
- Support CLI
- Support python API
- Zero cost: No external APIs, fully local
- Single file: Portable .dkb format
- Core functions
    - **Add**: Folders, files, text
    - **Search**: Keyword, tags, path, dates, combined
    - **Chunking**: By headings, tokens, paragraphs, custom
    - **CRUD**: Full create, read, update, delete
    - **Export**: folder, file, text
    - **Tags**: Add, remove, search by tags
    - **Filters**: Path patterns, date ranges, tag exclusion
    - **Stats**: Document counts, tag counts


```python
from dkb import DKB

# ============================================
# 1. CREATE / OPEN DATABASE
# ============================================
db = DKB('my_knowledge.dkb')


# ============================================
# 2. ADD CONTENT (Multiple Ways)
# ============================================

# Add entire folder (recursively)
db.add_folder('./docs')
db.add_folder('./docs', pattern='*.md')

# Add single document from file
db.add_document('notes/idea.md')

# Add document from string
db.add_document('notes/new.md', content='# Title\nContent...')

# Add direct text
db.add_text('Quick note', 'Just a thought...', tags=['ideas'])

# Add with chunking
db.add_document(
    'long_article.md',
    chunk_strategy='headings',  # 'headings', 'tokens', 'paragraphs'
    chunk_size=500,              # For token-based chunking
    chunk_overlap=50             # Overlap between chunks
)

db.add_folder(
    './documentation',
    chunk_strategy='headings',
    min_chunk_size=100,
    max_chunk_size=1000
)


# ============================================
# 3. SEARCH (Keyword + Tags + Filters)
# ============================================

# Simple keyword search
results = db.search('authentication')

# Search with limit
results = db.search('machine learning', limit=5)

# Search by tag only
results = db.find_by_tags(['work', 'important'])  # AND
results = db.find_by_tags(['python', 'javascript'], match='any')  # OR

# Combined: keyword + tags
results = db.search(
    query='authentication',
    tags=['backend', 'security']
)

# Advanced search with all options
results = db.search(
    query='machine learning',           # Keyword (optional)
    tags=['ai', 'research'],            # Must have these tags
    exclude_tags=['draft'],             # Must NOT have these
    path='notes/*',                     # Path pattern
    modified_after='2024-01-01',        # Date filter
    limit=10,                           # Max results
    max_tokens=2000,                    # Token budget
    order_by='relevance'                # 'relevance', 'date', 'path'
)

# Search by path pattern
results = db.find_by_path('docs/api/*')


# ============================================
# 4. READ / RETRIEVE
# ============================================

# Get single document
doc = db.get('notes/idea.md')

# List all documents
all_docs = db.list_documents()

# Find by tag
docs_by_tag = db.find_by_tags(['work'])

# Get document metadata
metadata = db.get_metadata('notes/idea.md')
# Returns: {path, created_at, modified_at, tags, token_count, chunks}


# ============================================
# 5. UPDATE
# ============================================

# Update document content
db.update('notes/idea.md', content='# Updated content...')

# Add tags
db.add_tags('notes/idea.md', ['important', 'review'])

# Remove tags
db.remove_tags('notes/idea.md', ['draft'])

# Update metadata
db.update_metadata('notes/idea.md', author='John', priority='high')


# ============================================
# 6. DELETE
# ============================================

# Remove single document
db.remove('notes/idea.md')

# Remove multiple by pattern
db.remove_by_path('drafts/*')

# Remove by tag
db.remove_by_tags(['archived'])

# Clear entire database
db.clear()


# ============================================
# 7. EXPORT
# ============================================

# Export back to markdown folder
db.export_markdown('./output_folder')

# Export as JSON
db.export_json('knowledge.json')

# Export specific documents
db.export_markdown('./output', tags=['important'])

# ============================================
# 8. CHUNKING OPERATIONS
# ============================================

# Rechunk existing documents
db.rechunk(
    strategy='headings',
    min_size=100,
    max_size=1000
)

# Get chunk info
chunks = db.get_chunks('docs/api.md')
# Returns list of chunks with their paths and token counts


# ============================================
# 8. UTILITY / STATS
# ============================================

# Get statistics
stats = db.stats()
# Returns: {
#     total_documents: 150,
#     total_chunks: 487,
#     total_tokens: 125000,
#     total_tags: 23,
#     size_bytes: 2048000
# }

# List all tags
all_tags = db.list_tags()

# Count documents by tag
tag_counts = db.count_by_tags(['work', 'ideas', 'draft'])
# Returns: {'work': 45, 'ideas': 23, 'draft': 12}

# Search within results (refine)
results = db.search('authentication')
refined = results.filter(tags=['security'])


# ============================================
# 9. CONTEXT MANAGER (Auto-close)
# ============================================

with DKB('knowledge.dkb') as db:
    db.add_folder('./docs')
    results = db.search('query')
# Automatically closes connection


# ============================================
# 10. RESULT OBJECT
# ============================================

results = db.search('authentication', limit=5)

for result in results:
    print(result.path)          # "docs/api.md#authentication"
    print(result.content)       # Chunk or full content
    print(result.score)         # 0.87 (relevance)
    print(result.token_count)   # 245
    print(result.tags)          # ['backend', 'security']
    print(result.metadata)      # {created_at, modified_at, ...}
    print(result.heading)       # "Authentication" (if chunked by heading)
```



## Related Projects

- [LEANN](https://github.com/yichuan-w/LEANN)

- [CocoIndex](https://github.com/cocoindex-io/cocoindex)

- [RAGFlow](https://github.com/infiniflow/ragflow)

- [Txtai](https://github.com/neuml/txtai)

- [KB MCP Server](https://github.com/Geeksfino/kb-mcp-server)

- [SQLite](https://docs.python.org/3.12/library/sqlite3.html#)