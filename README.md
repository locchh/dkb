# dkb


## Features

- Support CLI

- Support python API

- Zero cost: No external dependencies, fully local

- Embedded (in-file) database including in memory and on-disk

- Core functions

- **Add**: Folders, files, text

- **Search**: Keyword, tags, path, hybrid

- **Chunking**: By headings, paragraphs, character's length, separators

- **CRUD**: Full create, read, update, delete

- **Export**: folder, file, text

- **Tags**: Add, remove, search by tags

- **Filters**: Path patterns, date ranges, tag exclusion

- **Stats**: Document counts, tag counts



```python

from dkb import DKB


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

db.add_document('notes/idea.md', tags=['ideas'])


# Add direct text

db.add_text('Just a thought...', tags=['ideas'])



# ============================================

# 3. SEARCH (Keyword + Tags + Filters)

# ============================================


# Simple keyword search

results = db.search(query=['authentication'])


# Search with limit

results = db.search(query=['machine learning'], limit=5)


# Search by tag only

results = db.search(tags=['work', 'important']) # AND

results = db.search(tags=['python', 'javascript'], match='any') # OR


# Combined: keyword + tags

results = db.search(

query=['authentication'],

tags=['backend', 'security']

)


# Search by path pattern

results = db.search(path='docs/api/*')



# ============================================

# 4. READ / RETRIEVE

# ============================================


# List all documents

all_docs = db.list_documents()


# Get single document

doc = db.get('notes/idea.md')


# Get document metadata

metadata = db.get_metadata('notes/idea.md')

# Returns: {path, created_at, modified_at, tags, length}



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

db.update_metadata('notes/idea.md', metadata={'author': 'John', 'priority': 'high'})



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

db.export('./output_folder')


# Export specific documents

db.export('./output', tags=['important'])



# ============================================

# 8. CHUNKING OPERATIONS

# ============================================


# Chunk document by headings

chunks = db.chunk(

path='docs/api.md',

strategy='headings',

)


# Chunk document by paragraphs

chunks = db.chunk(

path='docs/api.md',

strategy='paragraphs',

)


# Chunk document by separator

chunks = db.chunk(

path='docs/api.md',

strategy='separator',

separator='----',

)



# ============================================

# 9. UTILITY / STATS

# ============================================


# Get statistics

stats = db.stats()

# Returns: {

# total_documents: 150,

# total_length: 125000,

# total_tags: 23,

# size_bytes: 2048000

# }


# List all tags

all_tags = db.list_tags()


# Count documents by tag

tag_counts = db.count_by_tags(['work', 'ideas', 'draft'])

# Returns: {'work': 45, 'ideas': 23, 'draft': 12}



# ============================================

# 10. CONTEXT MANAGER (Auto-close)

# ============================================


with DKB('knowledge.dkb') as db:

db.add_folder('./docs')

results = db.search(query='query', limit=5)

# Automatically closes connection



# ============================================

# 11. RESULT OBJECT

# ============================================


results = db.search(query='authentication', limit=5)


for result in results:

print(result.path) # "docs/api.md#authentication"

print(result.content) # Chunk or full content

print(result.score) # 0.87 (relevance)

print(result.length) # 245

print(result.tags) # ['backend', 'security']

print(result.metadata) # {created_at, modified_at, ...}

```



## Related Projects


- [LEANN](https://github.com/yichuan-w/LEANN)


- [CocoIndex](https://github.com/cocoindex-io/cocoindex)


- [RAGFlow](https://github.com/infiniflow/ragflow)


- [Txtai](https://github.com/neuml/txtai)


- [KB MCP Server](https://github.com/Geeksfino/kb-mcp-server)


- [SQLite](https://docs.python.org/3.12/library/sqlite3.html#)


- [KuzuDB](https://kuzudb.github.io/docs/)

