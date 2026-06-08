# GGBP-Instances

This is an instance dataset for the Generalized Graph Burning Problem (GGBP), split between
real, planar, and random instances.

The dataset is structured in the following way:
```sh
ggbp-instances/
└── instances
    ├── planar
    │   └── planar_n<n>_<i>.mtx
    ├── random
    │   └── gnp_n<n>_p<p>_<i>.mtx
    └── real
        └── name.mtx
```

All instances are stored in MatrixMarket (`.mtx`) format.

For the real instances, the name of the graph is presented. \
For the generated instances, the parameters are:
+ $n$: Order of the graph
+ $p$: Probability of adding an edge between two vertices, represented as a percentage in the file name (for the random instances only)
+ $i$: Number of the instance (there are several instances with each parameter combination)

The real instances were obtained from the [Network Repository](https://networkrepository.com/) and from the
[Stanford Large Network Dataset Collection](https://snap.stanford.edu/data/). \
The random and planar instances were generated using [SageMath](https://www.sagemath.org/).
