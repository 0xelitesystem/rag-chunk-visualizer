# rag-chunk-visualizer

See how four different chunking strategies split your text. Side by side. With overlap highlighted.

**Live demo:** https://0xelitesystem.github.io/rag-chunk-visualizer/

Browser-only, single HTML file.

## Use

1. Paste a passage into the Source text box (a sample is already loaded).
2. Set the target chunk size, the overlap in tokens, and the chars-per-token ratio.
3. Compare the four strategy panels. They update as you type, with overlapping text highlighted.
4. Check each panel's chunk count and mean, min and max tokens before picking a strategy.

## What it does

Paste a passage. Set chunk size, overlap, and chars-per-token ratio. Get four parallel views:

- **Fixed-size**: slice every N tokens
- **Recursive**: fall through paragraph -> line -> sentence -> word
- **Sentence**: group sentences until target size
- **Paragraph**: double-newline-separated atomic units

Each view shows chunk count, mean/min/max tokens, total tokens (including overlap inflation), and renders every chunk with overlapping regions highlighted.

## Why this exists

Picking a chunking strategy is one of the most underestimated decisions in RAG. Most projects start with "fixed 500 tokens, 50 overlap" because that is what a tutorial used, then discover months later that retrieval quality is mediocre because the strategy doesn't match the source content.

Reading about chunking is one thing. Seeing what each strategy actually produces on your specific text is another. This tool gives you that view immediately, without writing code or running a notebook. It is one HTML file with no tracking and no dependencies, MIT licensed.

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

## Privacy

Everything runs in your browser. The page makes no network requests, and the text you paste is never sent anywhere or stored. The only thing saved is your light or dark theme choice, kept in localStorage under the key `theme`.

## Run locally

```
git clone https://github.com/0xelitesystem/rag-chunk-visualizer
cd rag-chunk-visualizer
```

Open `index.html` in a browser, or serve the folder with `python -m http.server` and visit http://localhost:8000.

## Build

No build. Open `index.html`, or deploy via GitHub Pages.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT.

## Related

- [embedding-cost-estimator](https://github.com/0xelitesystem/embedding-cost-estimator): cost matrix across providers
- [rag-evaluation-rubrics](https://github.com/0xelitesystem/rag-evaluation-rubrics): measure retrieval quality
- [prompt-cost-calculator](https://github.com/0xelitesystem/prompt-cost-calculator): per-call cost estimator
