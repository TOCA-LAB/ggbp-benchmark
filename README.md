# GGBP-Instances

## Overview

This repository provides benchmark instances for the Generalized Graph Burning
Problem (GGBP). 
The instances are organized into three families: real-world graphs, generated
planar graphs, and generated random graphs.


The benchmark contains 296 instances:
+ 56 real instances
+ 40 planar generated instances
+ 200 random generated instances

## Dataset Structure

The dataset is structured in the following way:
```sh
ggbp-instances/
├── instances
│   ├── planar
│   │   └── planar-n<n>-i<i>.mtx
│   ├── random
│   │   └── gnp-n<n>-p<p>-i<i>.mtx
│   └── real
│       └── name.mtx
└── metadata
    └── real-sources.csv
```

## File Format

All instances are stored in MatrixMarket (`.mtx`) format. Each file represents a sparse
adjacency matrix in coordinate form, where each coordinate entry indicates an edge.
The instances are unweighted.

All instances are stored in MatrixMarket (`.mtx`) coordinate format as sparse
adjacency matrices. 
All instances are treated as undirected, unweighted graphs.


## Instance Naming

For the real instances, the name of the graph is presented. 
For the generated instances, the parameters are:
+ $n$: Order of the graph
+ $p$: Probability of adding an edge between two vertices, represented as a percentage in the file name (for the random instances only)
+ $i$: Number of the instance (there are several instances with each parameter combination)

For example, `p10` means $p = 0.10$.

Examples:
+ `planar-n50-i3.mtx`: Planar graph with 50 vertices, instance 3
+ `gnp-n100-p20-i7.mtx`: Random graph $G(n,p)$ with $n = 100$, $p = 0.20$, instance 7

## Sources

The real instances were obtained from the 
[Network Repository](https://networkrepository.com/) and from the 
[Stanford Large Network Dataset Collection](https://snap.stanford.edu/data/).
Their original graph names were preserved in the file names.

Per-instance source metadata is available in `metadata/real-sources.csv`.

The random and planar instances were generated using [SageMath](https://www.sagemath.org/).

## License

The generated instances in `instances/planar` and `instances/random` are released under
the Creative Commons Attribution-ShareAlike 4.0 International license (CC BY-SA 4.0).

The real instances in `instances/real` are derived from third-party graph datasets and are
not covered by the license above. 
Per-instance source and license metadata is available in `metadata/real-sources.csv`.

Instances derived from the Network Repository are subject to its data policy, which
licenses data under a Creative Commons Attribution-ShareAlike license.

Instances derived from the Stanford Large Network Dataset Collection should be cited
according to SNAP's citation instructions. No general dataset license was identified on
the SNAP dataset page; users should verify the terms of each original dataset before
redistribution or reuse.

## Citation

If you use this benchmark dataset, please cite it as:

```bibtex
@misc{ggbp_instances,
  title        = {GGBP-Instances: A Benchmark Dataset for the Generalized Graph Burning Problem},
  author       = {Guido, Lucas and Sambinelli, Maycon},
  year         = {2026},
  howpublished = {\url{https://github.com/student-lucas/ggbp-instances}},
  note         = {Accessed: 2026-06-08}
}
```

## Authors

+ Lucas Guido
+ Maycon Sambinelli
