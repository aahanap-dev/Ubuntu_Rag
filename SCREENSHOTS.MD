![img.png](img.png)

![img_2.png](img_2.png)

![img_1.png](img_1.png)

![img_3.png](img_3.png)

![img_4.png](img_4.png)


Types of Chunking strategy Explored:

## Semantic Chunking
Semantic chunking treats the text as a conceptual unit rather than dividing it into unrelated blocks. Since the corpus consisted of technical documentation, which includes interconnected components like implementation details, examples, and use cases, semantic chunking proved to be effective in maintaining these relationships.

### Key Advantages in this scenerio:
- Preserves complex, nested technical concepts.

- Maintains the interdependence of explanatory text.

- Was able to capture precise technical context within documents.


### Limitations:
- Resource-intensive and computationally heavy, making it less efficient for large-scale applications.

## Customized Header-Based Chunking (finalized)
Another method explored was header-based chunking. The documents were fairly structured and well-organized, making this approach effective. Header-based chunking is simple, fast, and efficient, as it groups related content under specific headers, maintaining the context of each section. To retain the larger context, both the title and headers were included within the chunk, which enhanced the relevance of the retrieved information.

### Key Advantages:
- Simple and computationally efficient.

- Keeps related content together in a single chunk (assuming headers are well-structured).

- Retains high-level context by including titles and headers in the chunk.

### Limitations:
May miss nuanced semantic relationships between different sections, leading to possible gaps in context.


## Better Retrival Approach:
- If not for the time construct, i would have experimented with the semantic + lexical chunking combination.
Lexical would have helped in keyword queries and semantic would have been helpful in well structured queries.
To combine these two, weigheted combination can be tested for example
0.4 * similarity score of lexical algorithm (eg bm25, tfidf) + 0.6 * similarity score of computed from vector db
Based on the results, the weights can be adjusted .

- Another approach which can be tested is reranking 
Cross encoders would be computationally heavy , so i would have gone with colbert based reranking, which efficiently gives the similarity score using max sim measure