# GGBP benchmark

A collection of **296 graph instances** for evaluating algorithms for the **Generalized Graph Burning Problem (GGBP)**. It combines generated random graphs, generated planar graphs, and reference networks collected from public repositories.

The files describe graph topology. The burning threshold `r` is selected when running an algorithm, so the same instance can be used for different thresholds, including the classical Graph Burning Problem with `r = 1`.

| Family | Directory | Instances | Number of vertices |
|---|---|---:|---:|
| Random graphs | [`instances/random/`](instances/random/) | 200 | 50–200 |
| Planar graphs | [`instances/planar/`](instances/planar/) | 40 | 10–64 |
| Reference networks | [`instances/real/`](instances/real/) | 56 | 34–196,591 |
| **Total** | | **296** | **10–196,591** |

The historical directory name `real/` identifies the third-party collection. It includes real-world networks and established benchmark graphs, such as the [DIMACS instance C125-9](https://networkrepository.com/C125-9.php); it should not be read as a claim that every graph in that directory was measured from a real-world network.

## Quick start

```sh
git clone https://github.com/TOCA-LAB/ggbp-benchmark.git
cd ggbp-benchmark
```

No compilation or dataset download step is needed: the instance files are included in the repository.

To run an instance with [ggbp-solver](https://github.com/TOCA-LAB/ggbp-solver), first build the solver following its README. Assuming the two repositories are sibling directories:

```sh
mkdir -p results

../ggbp-solver/build/bin/unburned_neighbors \
  instances/real/karate.mtx 2 \
  --json results/karate-r2.json
```

Here, `2` is the burning threshold, not a property encoded in the instance file. Batch experiment recipes are maintained in [ggbp-experiments](https://github.com/TOCA-LAB/ggbp-experiments).

## Repository layout

```text
instances/
├── random/
│   └── gnp-n<n>-p<p>-i<i>.mtx
├── planar/
│   └── planar-n<n>-i<i>.mtx
└── real/
    └── <network-name>.mtx
metadata/
└── real-sources.csv
```

## Instance families

### Random graphs

The random family uses the independent-edge model `G(n, p)`:

- Orders: `n ∈ {50, 100, 150, 200}`.
- Edge probabilities: `p ∈ {0.10, 0.15, 0.20, 0.25, 0.30}`.
- Ten instances per combination, indexed from `i0` to `i9`.

This gives `4 × 5 × 10 = 200` instances, generated with [SageMath](https://www.sagemath.org/).

The probability `p` is the **expected density**, not the exact density of an individual graph. For a simple undirected graph with `n` vertices and `m` edges, the observed density is `2m / [n(n−1)]`, while the expected number of edges in `G(n, p)` is `p·n(n−1)/2`.

### Planar graphs

There are ten instances for each order `n ∈ {10, 25, 50, 64}`, totaling 40 graphs. The project selected the first ten graphs returned by SageMath's `graphs.planar_graphs(n)` for each order.

This is a selection from the generator's enumeration, not a random sample of the entire class of planar graphs.

### Reference networks

The 56 files in `instances/real/` were collected from:

- [Network Repository](https://networkrepository.com/): 40 instances.
- [Stanford Large Network Dataset Collection (SNAP)](https://snap.stanford.edu/data/): 16 instances.

The collection includes networks from different domains and existing graph benchmarks. Source pages and recorded license information are listed individually in [`metadata/real-sources.csv`](metadata/real-sources.csv).

| Metadata column | Description |
|---|---|
| `file` | Instance path relative to the repository root |
| `source` | Repository from which the graph was obtained |
| `source_url` | Dataset page with its description and citation information |
| `license` | License information recorded for that source |

Each reference-network file has one corresponding row in this metadata file. Source pages provide the context for the original networks; the files distributed here are the computational inputs used by this project.

## File format and graph interpretation

Instances use **Matrix Market-style coordinate files** with the `.mtx` extension. Some have a Matrix Market banner; others start directly with the dimensions. The dimension line contains the graph order twice and a declared count `m`. All edge records contain two integer vertex identifiers:

```text
% Optional leading comments or a Matrix Market banner
n n m
u_1 v_1
u_2 v_2
...
```

For example, a triangle can be represented as:

```text
%%MatrixMarket matrix coordinate pattern symmetric
3 3 3
1 2
2 3
1 3
```

When loading an instance for the GGBP:

1. Skip leading lines beginning with `%`, if present.
2. Read the dimensions and preserve all `n` vertices, including vertices without edge records.
3. Read endpoint pairs until the end of the file. Vertex identifiers are **one-based**, from `1` to `n`.
4. Interpret each pair as an **undirected, unweighted** edge. For the simple-graph problem, discard self-loops and merge repeated undirected edges.

The `.mtx` suffix does not imply that every file can be passed unchanged to a strict Matrix Market reader. In particular:

- Some files omit the required Matrix Market banner.
- Reference-network files can contain self-loops or repeated edge records; the edge count of the resulting simple graph can therefore differ from the number of records.
- [`web-wiki-crocodile.mtx`](instances/real/web-wiki-crocodile.mtx) declares `170918` in its header but contains `180020` endpoint-pair records. Removing 145 self-loops and merging 9102 repeated undirected records gives 170773 simple edges. Read all records rather than stopping at the declared count.

The companion solver's coordinate reader follows this interpretation. Other readers may require an explicit conversion. The instance files are preserved here; readers and analyses should state how they normalize them.

## Naming conventions

| Pattern | Interpretation | Example |
|---|---|---|
| `gnp-n<n>-p<p>-i<i>.mtx` | Random graph; `p` is expressed as a percentage | [`gnp-n100-p20-i7.mtx`](instances/random/gnp-n100-p20-i7.mtx): order 100, edge probability 0.20, index 7 |
| `planar-n<n>-i<i>.mtx` | Planar graph of the given order and index | [`planar-n50-i3.mtx`](instances/planar/planar-n50-i3.mtx): order 50, index 3 |
| `<network-name>.mtx` | Reference-network identifier, mapped to its source in the metadata | [`karate.mtx`](instances/real/karate.mtx) |

The index `i` identifies a stored instance; it is **not** a graph-generation seed or an algorithm seed. Preserve filename case when selecting files or joining instance names with results.

## Reproducible use

The distributed files define the benchmark. Generation scripts and graph-generation seeds are not included in this repository, so generation parameters alone do not reconstruct the exact stored graphs.

Record the dataset commit and the subset of files used in each experiment. For example:

```sh
git rev-parse HEAD
sha256sum instances/random/gnp-n100-p20-i7.mtx
```

The repository contains all 296 instances. An experiment-specific exclusion or missing solver result does not remove an instance from this collection. Document such subsets and failures in the experiment results rather than silently changing the benchmark.

Optimal values, solver outputs, execution logs, and analysis tables belong to the companion experiment and analysis repositories. They are not encoded in these graph files.

## Related research artifacts

- [ggbp-solver](https://github.com/TOCA-LAB/ggbp-solver): exact methods, heuristics, and metaheuristics.
- [ggbp-experiments](https://github.com/TOCA-LAB/ggbp-experiments): recipes for the computational experiments.
- [ggbp-analisis](https://github.com/TOCA-LAB/ggbp-analisis): processing of raw results, tables, and figures.

## Research background and attribution

This repository was developed in the undergraduate research project *Queimando Grafos* at the [Federal University of ABC (UFABC)](https://www.ufabc.edu.br/), by [Lucas Guido](https://github.com/lucas-gcp) under the supervision of [Maycon Sambinelli, Ph.D.](http://professor.ufabc.edu.br/~m.sambinelli/), with support from FAPESP, [process **2025/04352-6**](https://bv.fapesp.br/pt/bolsas/232745/queimando-grafos/).

## License and source attribution

The generated graphs in `instances/random/` and `instances/planar/` are released under [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/). See [`LICENSE`](LICENSE) for the full license texts.

Copyright (C) 2026 Lucas Guido, Maycon Sambinelli / Universidade Federal do ABC

### Third-party components
Graphs in `instances/real/` are derived from third-party datasets and are not covered by that declaration. Consult their entries in [`metadata/real-sources.csv`](metadata/real-sources.csv) and the linked source pages:

- Network Repository publishes a [CC BY-SA data policy and citation requirements](https://networkrepository.com/policy.php). Its metadata entries here use `CC BY-SA` without assigning a license version.
- SNAP entries use `not specified` where no license was recorded in the metadata. This is not a public-domain designation; consult the individual dataset page for its terms and citation instructions.

## Citation

When using this collection, cite the benchmark and identify the exact commit used. Also cite the original datasets and source repositories as requested on their dataset pages.

```bibtex
@misc{ggbp_benchmark,
  author = {Guido, Lucas and Sambinelli, Maycon},
  title  = {{GGBP Benchmark}: Instances for the Generalized Graph Burning Problem},
  year   = {2026},
  url    = {https://github.com/TOCA-LAB/ggbp-benchmark}
}
```
