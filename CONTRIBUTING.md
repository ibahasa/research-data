# Contributing

This repository does not accept pull requests that add or change data.

Everything under `raw/` is generated output. The data is exported from our
private research repository by tooling that also writes each dataset's
`MANIFEST.json` and `PROVENANCE.md` and regenerates `INDEX.md`. A file edited
here by hand would be overwritten on the next export, and its `sha256` in
`MANIFEST.json` would stop matching in the meantime — which is the one thing
this repository exists to guarantee.

## What is useful to us

**Tell us a number looks wrong.** Open an issue naming the dataset, the file,
and the row or figure you are looking at. Include the commit SHA you read it at,
since files change between exports and permalinks do not.

**Tell us a `PROVENANCE.md` is unclear or overstates something.** Those files are
where we declare what a dataset cannot be used for, and they are the part most
worth arguing with.

**Tell us a link is broken.** Especially a link from an article at ibahasa.com
that lands here and 404s.

We would rather hear that a published figure is wrong than have it stay up.
Corrections are recorded in the open, in the article and in the dataset.

## Citing

Use a permalink pinned to a commit, never to `main`. See "Citing a file" in
[`README.md`](./README.md). Datasets carry a DOI through Zenodo; the concept DOI
always resolves to the latest version, and the version DOI is the reproducible
one.

## Conventions, for anyone reading a MANIFEST

`cited_by` lists the articles that use a dataset. It is added after an article
goes live, not at export time, because the article does not exist yet when the
data is exported. It is an array, since one dataset can support several
articles, and it is additive — the fields that outside citations depend on
(`sha256`, `source_commit`, `fingerprint`) are never rewritten.

Releases are cut when they mean something — after several articles accumulate,
or when someone wants to cite this archive formally — not once per article. A
new Release mints a new version DOI, and a long list of near-identical versions
would make the DOIs less useful, not more. Per-article reproducibility already
rests on the commit-pinned links.

## Language

The root of this repository is in English, since readers arrive here from a DOI
or a citation. The `PROVENANCE.md` inside each dataset is in Indonesian, since it
is written alongside the research itself. That split is deliberate.
