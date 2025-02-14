# Spatially informed optimal control
This repository includes data and example code for illustrating the incorporation of spatial information into the optimal control framework.

## Background

In general, we consider the following linear dynamical system:

$\dot{x} = Ax + Bu$

In this expression, $x$ is a $N_{node} \times 1$ vector that represents the brain "state"; $A$ is a $N_{nodes} \times N_{nodes}$ coupling matrix that specifies the connectivity between pairs of nodes; $u$ is a $N_{\kappa} \times 1$ set of input signals injected in to $N_{\kappa}$ input sites (nodes); and $B$ is the $N_{nodes} \times N_{\kappa}$ input matrix that specifies the mapping of input signals to nodes.

In most applications, $B$ is made up of "canonical" column vectors. The $i$ th canonical column vector has dimensions $N_{nodes} \times 1$, and has entries of 0 except the $i$ th position, which has a value of 1. These columns imply that a given input signal is delivered exactly to one node and one node only.

In our approach, we replace the canonical column vector $i$ with a vector of nonzero entries, whose $j$ th element is equal to:

$B_{ij} = exp(-\beta \times D_{ij})$

where $D_{ij}$ is the straight-line distance from node $j$ to input node $i$. The parameter $\beta$ controls the rate of decay. In short, this model allows an input signal centered on node $i$ to also reach node $j$, though with a smaller amplitude. We argue that this spatial decay is more realistic. It accounts for imprecision and lack of spatial specificity in neurostimulation (if we imagine that control is enacted exogenously) or the fact that neuronal populations likely do not adhere to parcel boundaries (if we imagine that control is enacted endogenously). That is, if the control signal originate from some neuronal population, that population is likely represented in multiple (spatially-contiguous) parcels.

![](figs/fig.jpeg)

## What is included here?

1. <code>data/brain_states.mat</code> : $1000 \times 11$ brain states from the paper.
2. <code>data/coordinates.mat</code> : $1000 \times 3$ parcel centroids.
3. <code>data/structural_connectivity.mat</code> : $1000 \times 1000$ structural connectivity matrix
4. <code>optimalControlContinuous.m</code> : Code for obtaining the optimal inputs/trajectories. It takes as input the structural connectivity matrix $A$, the input matrix $B$, the parameters $\rho$ and $T$ (see the preprint for details), and the initial and target states, $x(t = 0)$ and $x(t = T)$, respectively. The brain states should be $1000 \times 1$ vectors.
5. <code>main.m</code> : code that reads in data, calculates the "local" and "spatial" input matrices, and calculates global energy for all $11 \times 11$ brain state transitions.

## Notes and disclaimers

The data used here is publicly available and can be downloaded [here](https://zenodo.org/records/2872624#.XOJqE99fhmM). The optimal control function was originally written by Shi Gu (University of Pennsylvania).

Note that this code is designed to be generic. It's straightforward to replace the structural connectivity, parcel centroids, and brain states with a different dataset. Note also that there are no additional dependencies; code should work "out of the box".

DISCLAIMER: All software is provide <b>as is</b>.

## References

If you use this code, please cite:

1. Betzel, R. F., Puxeddu, M. G., Seguin, C., Bazinet, V., Luppi, A., Podschun, A., ... & Parkes, L. (2024). Controlling the human connectome with spatially diffuse input signals. bioRxiv, 2024-02. [link to paper](https://www.biorxiv.org/content/10.1101/2024.02.27.581006.abstract)
2. Gu, S., Betzel, R. F., Mattar, M. G., Cieslak, M., Delio, P. R., Grafton, S. T., ... & Bassett, D. S. (2017). Optimal trajectories of brain state transitions. Neuroimage, 148, 305-317. [link to paper](https://www.sciencedirect.com/science/article/pii/S1053811917300058)
3. Betzel, R. F., Gu, S., Medaglia, J. D., Pasqualetti, F., & Bassett, D. S. (2016). Optimally controlling the human connectome: the role of network topology. Scientific reports, 6(1), 30770. [link to paper](https://www.nature.com/articles/srep30770)
