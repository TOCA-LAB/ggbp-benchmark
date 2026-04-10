# GGBP-Instances

This is a instance dataset for the Generalized Graph Burning Problem (GGBP), split between
real, planar and random instances.

The dataset is structured in the following way:
```sh
ggbp-instances/
├── graph6
│   ├── planar
│   │   └── planar_n_i.txt
│   └── random
│       └── gnp_n_p_i.txt
└── mtx
    └── real
        └── name.mtx
```

There are files in two different formats: MatrixMarket (mtx) and Graph6, each of them is stored in it's own folder.

For the real instances, the name of the graph is presented. \
For the generated instances, the parameters are:
+ $n$: Order of the graph
+ $p$: Probability of adding an edge between two vertices (for the random instances only)
+ $i$: Number of the instance (there are several instances with each parameter combination)

The real instances were obtained from the [Network Repository](https://networkrepository.com/) and from the
[Stanford Large Network Dataset Collection](https://snap.stanford.edu/data/). \
The random and planar instances were generated using [SageMath](https://www.sagemath.org/).
