# Search Normalizer

## Project Overview

This is a text processing library used in the [Simple Wikipedia Search Engine](https://github.com/pandevim/search-engine). It is responsible for preparing raw text for indexing and querying.

## Pipeline

1.  **Casefolding**: Converts all text to lowercase to ensure case-insensitive matching.
2.  **Whitelist Check**: Checks tokens against a whitelist (`src/defaults/whitelist.txt`) to preserve special terms (e.g., "C++", ".NET") that would otherwise be split by the tokenizer.
3.  **Tokenization**: Splits text into individual tokens (words) based on punctuation and whitespace.
4.  **Stopword Removal**: Filters out common English filler words (e.g., "the", "is", "at") to reduce index size and improve relevance. Uses `src/defaults/stopwords-en.txt` ([NLTK Stopwords](https://github.com/nltk/nltk_data/blob/gh-pages/packages/corpora/stopwords.zip)).
5.  **Lemmatization**: Reduces words to their root form (e.g., "running" $\rightarrow$ "run") using a dictionary-based approach sourced from `src/defaults/lemmatization-en.txt` ([Source](https://github.com/michmech/lemmatization-lists)).

## Usecase

This component is critical for maintaining consistency between the [search index](https://github.com/pandevim/search-engine) and [search queries](https://github.com/pandevim/search-engine-serve):

1.  **Index Time**: Normalize document content before adding it to the inverted data structure.
2.  **Query Time**: Normalize user search queries using the exact same rules to ensure terms match the index keys.

## Running Tests

To run the unit tests for the Normalizer library (lemmatization, tokenizer, etc.):

```bash
cargo test
```

## Usage

```rust
use search_normalizer::Normalizer;

fn main() {
    let normalizer = Normalizer::new();
    let tokens = normalizer.process("The cats are running fast (in C++)!");

    // Output: ["cat", "run", "fast", "c++"]
    println!("{:?}", tokens);

    // If you ever need to run a test with a clean slate (no baked-in rules)
    let mut empty_normalizer = Normalizer::empty();
}
```
