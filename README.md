# RAG Tax Helper Project

## Overview
The RAG Tax Helper project implements a Retrieval-Augmented Generation (RAG) pipeline to process, store, and query a large corpus of IRS PDFs. It combines PDF scraping, embedding generation, semantic search, and a generative language model to provide accurate and contextually relevant answers to user queries.

---

## 1. PDF Scraping and Processing
Over 2000+ IRS PDFs were scraped, processed, and prepared for downstream tasks. Key steps included:

- **Scraping**: Automated downloading and organization of IRS documents.
- **Image Conversion**: PDFs were converted into images (one per page) to support visual embedding generation.
- **OCR**: Text was extracted using PyTesseract to enable BM25-based text relevance scoring. Preprocessing (grayscale, resizing, thresholding) improved OCR accuracy.
- **Batch Processing**: PDFs were processed in manageable chunks to handle memory constraints.

---

## 2. Embedding Generation
Image embeddings were generated using the COLPALI model, which produced 128-dimensional vectors capturing the semantic content of each page. These embeddings were stored in a Vespa database for efficient similarity-based retrieval.

---

## 3. Vespa Database
The Vespa database served as the backbone for retrieval, supporting both text-based and vector-based search. Key features included:

- **Chunking**: Each page was stored as a separate "chunk" for precise retrieval.
- **Hybrid Schema**: Combined OCR text (for BM25 scoring) and embeddings (for vector similarity).
- **Authentication**: Control and data plane authentication ensured secure access.

---

## 4. Semantic Search
A hybrid search approach combined BM25 (text relevance) and vector similarity (semantic relevance):

1. **BM25 Scoring**: Ranked pages based on query term frequency and importance.
2. **Vector Similarity**: Re-ranked top results using embedding similarity (dot product).
3. **Custom Ranking Profile**: Balanced BM25 and vector scores for optimal results.
4. **Chunk-Level Retrieval**: Retrieved specific pages for precise answers.

---

## 5. Integration with GEMMA-4
The GEMMA-4 LLM was used to generate detailed answers based on retrieved pages. Key features:

- **Custom Prompts**: Dynamically generated prompts included the query, retrieved text, and images.
- **Multimodal Input**: GEMMA-4 processed both text and images for accurate responses.
- **Answer Generation**: Leveraged retrieved context to provide high-quality answers.

---

## Challenges and Solutions
1. **Memory Constraints**: Batch processing ensured stability when handling large datasets.
2. **Hybrid Search Tuning**: Custom Vespa ranking profiles balanced BM25 and vector relevance.
3. **Secure Access**: Authentication mechanisms protected the database.
4. **Prompt Design**: Dynamic prompts ensured the LLM received the right context.

---

## Conclusion
The RAG Tax Helper project demonstrates a scalable and secure RAG pipeline for processing and querying large document corpora. By integrating Vespa for semantic search and GEMMA-4 for generation, the system delivers accurate, context-aware answers. This approach is adaptable to other domains, making it a versatile solution for information retrieval and question answering tasks.
