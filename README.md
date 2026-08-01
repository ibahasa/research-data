# ibahasa Research Data

Raw data behind the research articles published at
[ibahasa.com/id/riset](https://ibahasa.com/id/riset).

Every article in that rubric makes one promise: **you can check the numbers
yourself.** This repository is how that promise is kept. Each article links to the
files here, pinned to the commit that existed when the article was written.

## Why a git repository and not a file host

An object-storage URL is mutable. Overwrite the file and every published article
silently points at different data, with nothing to show that it changed.

A git permalink pinned to a commit SHA is immutable. It shows the data exactly as
it stood when the article citing it was written. For a rubric whose entire value
rests on figures being checkable, that difference is the whole point.

The history matters too. Several of our findings *are* a sequence: the correction
engine audit is eight consecutive snapshots of the same corpus. In git you can diff
them yourself. On a file host they would be eight opaque blobs.

## Layout

```
raw/<dataset-slug>/
  MANIFEST.json     what the files are, where they came from, sha256 of each
  <files>           the data itself, flattened from its original paths
INDEX.md            generated table of every dataset here
```

`MANIFEST.json` records the commit of the (private) source repository the data was
exported from, so an internal reader can trace a file back to the pipeline that
produced it.

## What is deliberately not here

Held-out benchmark items. 87 of 282 items in the Benchmark Nusantara sets are
marked `held_out` and exist precisely so they never enter a training corpus.
Publishing them would destroy them permanently, and take the ability to measure
contamination with them. The publishing script refuses them, and refuses anything
under a benchmark dataset directory, regardless of what it was asked to do.

The non-held-out portion is intended for public release, but through the benchmark
team's own export tooling, which enforces the split.

## Limitations you should read before citing

- Most datasets come from a **single corpus over a single period**. Another corpus
  would very likely produce different numbers.
- Verification work behind several datasets was done by **one person, with no
  inter-annotator agreement**. Where a pass rate is reported, that rate carries an
  uncertainty we have not measured.
- Benchmark scores are **not stable at temperature 0**. The same model has scored
  75% and then 85% on identical items. Published figures use repeated runs and
  report variance; single-run numbers in raw exports should not be read as final.
- Some columns elsewhere in the ibahasa dataset are **LLM-generated candidates, not
  ground truth**. Nothing in this repository depends on them, but if that changes it
  will be labelled.

## License

Data in this repository is released under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Attribute to
**ibahasa.com**, and please link back to the article or dataset you used.

Third-party sources retain their own terms and are attributed in the relevant
`MANIFEST.json` and provenance files. Notably the Leipzig Corpora Collection
(CC BY 4.0) and Glottolog.

## Contact

Editorial and methodology questions: [mkhuda.com](https://mkhuda.com)
