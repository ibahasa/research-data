# ibahasa Research Data

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21793396.svg)](https://doi.org/10.5281/zenodo.21793396)

Raw data behind ibahasa research.

Every figure we publish links back to a file here, pinned to the commit it was
cited at. If you want to check a number, this is where you check it.

## Layout

```
raw/<dataset>/
  MANIFEST.json     file list, byte sizes, sha256 of each file, export date
  <files>           the data itself
INDEX.md            table of every dataset in this repository
```

Start at [`INDEX.md`](./INDEX.md).

Filenames are flattened from their original directory structure, so a dataset
drawn from several folders cannot produce two files with the same name. The
original path of each file is recorded in `MANIFEST.json`.

## Citing a file

Use a permalink pinned to a commit, not to `main`:

```
https://github.com/ibahasa/research-data/blob/<commit-sha>/raw/<dataset>/<file>
```

A link to `main` shows whatever the file looks like today. A link to a commit
shows what it looked like when it was cited, which is usually what you want when
you are checking someone's arithmetic.

Each file's `sha256` is in `MANIFEST.json` if you want to verify a download.

## Citing this repository

A GitHub permalink is enough to check one file, but it is not a citable
identifier on its own: it has no fixed version and nothing indexes it as a
dataset. This repository is archived on [Zenodo](https://zenodo.org), which
mints a permanent, version-pinned DOI for every GitHub Release.

- **Cite the exact version you used** (recommended, for reproducibility):
  `10.5281/zenodo.21793396` (`v0.1.0`)
- **Cite "whatever is current"** (a concept DOI that always resolves to the
  latest version): `10.5281/zenodo.21793395`

```bibtex
@dataset{huda_2026_ibahasa_research_data,
  author    = {Huda, M. Khoirul},
  title     = {ibahasa Research Data},
  month     = aug,
  year      = 2026,
  publisher = {Zenodo},
  version   = {v0.1.0},
  doi       = {10.5281/zenodo.21793396},
  url       = {https://doi.org/10.5281/zenodo.21793396}
}
```

`CITATION.cff` in this repository carries the same identifiers and is read
natively by GitHub's "Cite this repository" button on the repo's main page.

## Held-out splits

Benchmark item sets are **not** published here. A portion of those items is held
out so it stays outside training corpora and remains usable for measuring
contamination later. Releasing them here would end that.

The releasable portion is prepared separately, through tooling that enforces the
split rather than relying on anyone remembering it.

## Before you cite these numbers

- Most datasets come from a **single corpus over a single period**. Another corpus
  would very likely give different figures.
- Verification behind several datasets was done by **one person, with no
  inter-annotator agreement**. Where a pass rate appears, it carries an
  uncertainty we have not measured.
- Benchmark scores are **not stable at temperature 0**. The same model has scored
  75% and then 85% on identical items. Published figures use repeated runs and
  report variance; single-run numbers in a raw export should not be read as final.
- Some regional-language material covers **one dialect variant only**. Speakers
  elsewhere may judge differently, and that has not been tested.

None of this makes the data unusable. It makes it data, with edges. We would
rather you knew where the edges are.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Attribute to
**ibahasa.com** and link back to the dataset or article you used.

Third-party sources keep their own terms and are attributed in the relevant
`MANIFEST.json` and `PROVENANCE` files, notably the
[Leipzig Corpora Collection](https://wortschatz.uni-leipzig.de/en/download)
(CC BY 4.0) and [Glottolog](https://glottolog.org/).

## Contact

Methodology questions, corrections, or a number that looks wrong to you:
[mkhuda.com](https://mkhuda.com).

Corrections are welcome and get published. Several of our own findings are
records of us being wrong.
