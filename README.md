# How-Full-Text-search-works-in-mysql


`please read complete document to understand how full-text-search works in mysql and when you have to use it or when not ?`


To perform full-text search on a column, you need to create a full-text index on the column. You can use the CREATE INDEX statement to create the index or define it when creating the table.
### Option 1: Using ALTER TABLE to Add Full-Text Index
```
ALTER TABLE documents ADD FULLTEXT(text);
```
This will add a full-text index to the text column in the documents table.

Option 2: Using CREATE INDEX
Alternatively, you can create the full-text index using the CREATE INDEX statement like this:
```
CREATE FULLTEXT INDEX idx_text ON documents(text);
```
This will create a full-text index named idx_text on the text column of the documents table.

once you create a full text index for the column on which you want to perform full text search enter some dummy data 
```
INSERT INTO documents (text) VALUES
('Artificial intelligence is a branch of computer science that deals with creating intelligent machines capable of learning and reasoning.'),
('Machine learning is a subset of artificial intelligence that allows systems to learn from data without being explicitly programmed.'),
('The quick brown fox jumps over the lazy dog.'),
('AI is revolutionizing industries like healthcare, finance, and transportation.');
```

### Perform a `Full-Text Search` on the Column
```
SELECT * FROM documents WHERE MATCH(text) AGAINST ('AI and machine learning' IN NATURAL LANGUAGE MODE);
```

this query return the output something like that :

| id | text                                                                                                                                                                                                                      |
| -- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 5  | AI and machine learning are transforming industries such as healthcare, finance, and transportation by providing powerful tools for data analysis and decision-making.                                                    |
| 2  | Machine learning is a branch of artificial intelligence where systems learn from data and improve their performance over time without being explicitly programmed.                                                        |
| 1  | Artificial intelligence is a field of computer science that focuses on creating intelligent machines capable of performing tasks that normally require human intelligence, like learning, reasoning, and problem-solving. |
| 4  | In this document, we will discuss the ethical implications of AI in modern society, including privacy concerns and the impact on jobs.                                                                                    |
| 3  | The quick brown fox jumps over the lazy dog.                                                                                                                                                                              |


## Explanation of Output:
- Document 5 is the most relevant because it contains both "AI" and "machine learning" as key terms in the context of the query. This document is likely ranked the highest because it has these terms in close proximity and is directly related to the query.
- Document 2 comes next, because it discusses machine learning in the context of artificial intelligence, which is still relevant but might not have both terms as prominently as Document 5.
- Document 1 talks about artificial intelligence, and while it is highly relevant, it does not focus specifically on "machine learning" in the same way as Document 5 or Document 2. However, it is still returned due to its relevance to the query.
- Document 4 discusses AI in general and is returned because it mentions AI and machine learning, but it is ranked lower due to the lesser focus on these keywords.
- Document 3 is completely unrelated to the query, as it doesn't mention AI or machine learning, but it is still included because it doesn't violate any constraints. However, it will be ranked lowest because it doesn't have any relevance to the search terms.

## Relevance and Ranking:
- MySQL's full-text search will rank documents based on relevance using an internal ranking system. It looks at factors such as:
- Word frequency: How often the search terms appear in the document.
- Proximity: How close the search terms are to each other in the document.
- Document length: Shorter documents with search terms near the beginning may rank higher.

  ### How MySQL Handles the Full-Text Search:
Natural Language Mode: MySQL's MATCH() ... AGAINST() in natural language mode ranks results by relevance and ignores common words (like "the", "and", etc.). It's designed to handle simple textual similarity based on keyword matching and word frequency.


## Key Takeaways:
- Exact Matching: The MATCH() function finds documents that match the search terms but is primarily focused on keyword matching.
- Relevance Ranking: The results are ranked based on their relevance to the search terms, considering factors like word frequency and proximity of terms.
- Not Semantic Matching: This is not as advanced as semantic similarity—for example, it won't find documents that talk about "AI" and "machine learning" using completely different phrasing (like "artificial intelligence" and "deep learning") unless those specific words are present in the document.

# In Summary:
- Full-text search in MySQL can handle some degree of textual similarity (keyword matching) but does not handle vector embeddings well.
- Manual cosine similarity or Euclidean distance can be implemented, but MySQL is not optimized for vector-based similarity search, so performance can be an issue.
- For scalable vector similarity search, you should consider using specialized libraries like FAISS, Annoy, or a vector database like `ChromaDB` for better performance.
- MySQL can store embeddings and metadata, but you would need to handle the vector search computations in a more efficient system like FAISS or use a hybrid approach combining MySQL and a vector database.
- # In short, if you want to search for a song with Hindi lyrics and enter a substring in Hinglish, MySQL won't be able to fully understand the user's intent due to its limitations in processing such queries. In this case, you would need to use sentence embeddings and a vector database to accurately match the query to the relevant song.

