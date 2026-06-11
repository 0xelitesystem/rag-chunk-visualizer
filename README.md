# rag-chunk-visualizer

See how four different chunking strategies split your text. Side by side. With overlap highlighted.

**Live demo:** https://0xelitesystem.github.io/rag-chunk-visualizer/

Browser-only, single HTML file.

## What it does

Paste a passage. Set chunk size, overlap, and chars-per-token ratio. Get four parallel views:

- **Fixed-size**: slice every N tokens
- **Recursive**: fall through paragraph → line → sentence → word
- **Sentence**: group sentences until target size
- **Paragraph**: double-newline-separated atomic units

Each view shows chunk count, mean/min/max tokens, total tokens (including overlap inflation), and renders every chunk with overlapping regions highlighted.

## Why this exists

Picking a chunking strategy is one of the most underestimated decisions in RAG. Most projects start with "fixed 500 tokens, 50 overlap" because that is what a tutorial used, then discover months later that retrieval quality is mediocre because the strategy doesn't match the source content.

Reading about chunking is one thing. Seeing what each strategy actually produces on your specific text is another. This tool gives you that view immediately, without writing code or running a notebook.

## Reading the output

Highlighted regions are bytes shared with the adjacent chunk (overlap). Watch how each strategy handles:

- Mid-sentence cuts (fixed-size breaks them; sentence-based preserves them)
- Heading-bounded sections (paragraph excels; fixed ignores)
- Very long or very short paragraphs (paragraph fails on both extremes)

## Tradeoffs not shown

This tool does not implement:

- **AST-based chunking** for source code (splits on functions, classes, etc.)
- **Semantic chunking** using embeddings to find topic boundaries
- **Document-aware chunking** that respects markdown headings, HTML structure, etc.

Those approaches require more than a single static file. The four shown here are the foundations that most production RAG pipelines actually use; the more sophisticated approaches are usually adaptations of these.

## Token approximation

Tokens are estimated as `chars / ratio` (default 4 chars per token, which is a reasonable English approximation). Set the ratio to:

- `4`: English prose
- `3`: code (denser)
- `5`: heavy whitespace or formatting

For exact token counts use the actual tokenizer for your target model.

## Build

No build. Open `index.html`, or deploy via GitHub Pages.

## License

MIT.

## Related

- [embedding-cost-estimator](https://github.com/0xelitesystem/embedding-cost-estimator): cost matrix across providers
- [rag-evaluation-rubrics](https://github.com/0xelitesystem/rag-evaluation-rubrics): measure retrieval quality
- [prompt-cost-calculator](https://github.com/0xelitesystem/prompt-cost-calculator): per-call cost estimator
