# EvoPINN Competition

Black-box multiobjective optimization for physics-informed neural network training.

EvoPINN is a benchmark suite for studying whether multiobjective optimizers can
train physics-informed neural networks (PINNs) without using backpropagation or
problem-specific gradient information.  Each problem exposes a continuous
parameter vector containing the neural-network weights.  The optimizer only sees
multiple PINN loss objectives, such as PDE residual, boundary-condition loss,
initial-condition loss, data mismatch, or constitutive residual.

The goal of the competition is to develop optimizers that can genuinely reduce
PINN objectives under a fixed evaluation budget.  This benchmark is intended to
stress search mechanisms for high-dimensional, sensitive, and expensive
black-box spaces rather than to reward hand-tuned solvers for one PDE.

![Representative EvoPINN loss landscapes](docs/figures/evopinn_loss_landscape_panel.png)

**Figure.** Local 2-D parameter-space slices of representative EvoPINN loss
landscapes.  The height is the mean log-scaled objective ratio relative to a
fixed random-initialization reference.  These plots are qualitative diagnostics:
a smooth-looking random slice does not imply a globally smooth problem, because
high-dimensional PINN losses can be highly anisotropic and sharp directions can
be missed by a single sampled plane.

## Why EvoPINN?

Many standard evolutionary multiobjective algorithms can keep a diverse
population, but on PINN training tasks they often fail to produce sustained
objective decrease.  EvoPINN is designed to make that failure mode visible:
participants should report whether their method actually lowers each loss term,
not only whether it produces a visually spread nondominated set.

Useful algorithmic ideas may include adaptive step-size control, local
coordinate or direction learning, covariance adaptation, random-gradient
estimation, trust-region style sampling, archive-guided restarts, and
objective-aware perturbation scales.  The benchmark treats the evaluator as a
black box, so any such mechanism must obtain information through counted
objective evaluations.

## Platform

The competition uses MATLAB and PlatEMO.

- Recommended platform: PlatEMO v4.x
- Problem type: multiobjective, real-valued, large-scale, expensive
- Evaluator: MATLAB-only PINN loss evaluator
- Decision variables: flattened neural-network parameters in `[-1, 1]`
- Objectives: raw PINN loss components to be minimized
- Optional problem parameters: `sampleScale` and `seed`

The EvoPINN problem files are located at:

```text
PlatEMO/Problems/Multi-objective optimization/EvoPINN3M Neuroevolution MATLAB/
```

## Quick Start

Clone this repository, start MATLAB from the repository root, and add PlatEMO to
the MATLAB path:

```matlab
addpath(genpath('PlatEMO'));
```

Run a public example with NSGA-II:

```matlab
platemo( ...
    'problem',@EvoPINN1, ...
    'algorithm',@NSGAII, ...
    'N',50, ...
    'maxFE',100000, ...
    'parameter',{1,1}, ...
    'save',1);
```

For a quick smoke test, use a smaller sampling scale and a smaller budget:

```matlab
platemo( ...
    'problem',@EvoPINN1, ...
    'algorithm',@NSGAII, ...
    'N',10, ...
    'maxFE',200, ...
    'parameter',{0.01,1}, ...
    'save',1);
```

The first parameter is `sampleScale`; the second is the evaluator seed.  The
official competition setting should use the organizer-announced values.  Public
experiments normally use `sampleScale = 1`.

## Test Problems

| Problem | PDE or task | M | D | Objectives |
| :-- | :-- | --: | --: | :-- |
| `EvoPINN1` | Poisson 1D: `-u_xx = pi^2 sin(pi x)`, `x in [-1,1]` | 2 | 5,251 | `pde`, `bc` |
| `EvoPINN2` | Multiscale Poisson 1D | 2 | 20,901 | `pde`, `bc` |
| `EvoPINN3` | Heat equation 1D: `u_t - 0.4 u_xx = 0` | 3 | 921 | `pde`, `bc`, `ic` |
| `EvoPINN4` | Burgers equation 1D | 3 | 921 | `pde`, `bc`, `ic` |
| `EvoPINN5` | Allen-Cahn equation 1D | 3 | 921 | `pde`, `bc`, `ic` |
| `EvoPINN6` | Wave equation 1D: `u_tt - 100 u_xx = 0` | 3 | 21,201 | `pde`, `bc`, `ic` |
| `EvoPINN7` | Helmholtz equation 2D, `k = 4pi` | 2 | 45,901 | `pde`, `bc` |
| `EvoPINN8` | Nonlinear Schrodinger equation 1D | 3 | 30,802 | `ic`, `periodic_bc`, `pde` |
| `EvoPINN9` | Discrete-time KdV equation with `q = 50` IRK stages | 2 | 10,300 | `data_t0`, `data_t1` |
| `EvoPINN10` | Kovasznay flow 2D, steady incompressible Navier-Stokes, `Re = 20` | 3 | 7,953 | `momentum`, `continuity`, `boundary_data` |
| `EvoPINN11` | Euler-Bernoulli beam 1D: `u_xxxx + 1 = 0` | 3 | 901 | `pde`, `clamped_bc`, `free_bc` |
| `EvoPINN12` | Linear elasticity 2D, mixed displacement-stress form | 2 | 5,245 | `equilibrium`, `constitutive` |

All problems use the same public interface:

```matlab
Problem = EvoPINN1('N',50,'parameter',{1,1});
PopDec  = rand(50,Problem.D).*2 - 1;
PopObj  = Problem.CalObj(PopDec);
```

## Competition Rules

Participants should optimize the EvoPINN problems through the PlatEMO problem
interface.  The evaluator must be treated as a black box.

- Do not modify the problem definitions or loss evaluators.
- Do not use analytical, automatic-differentiation, or PDE-specific gradients
  from inside the evaluator.
- Directional, coordinate, finite-difference, or random-gradient estimates are
  allowed only when they are computed through counted calls to `CalObj`.
- Do not use reference solutions, validation MSE values, or hidden data during
  the search process.
- All submitted populations must contain feasible decision vectors within the
  provided bounds.
- All objectives are minimized.

The official ranking script and final evaluation budget will be announced by
the organizers.  A recommended public practice budget is:

```text
N = 50
maxFE = 100000
sampleScale = 1
```

## What to Submit

Please submit:

- Source code of the proposed optimizer.
- A short method description.
- Clear MATLAB instructions for running the optimizer in PlatEMO.
- Result files generated under the official settings.
- Any external dependency list needed to reproduce the run.

The organizer should be able to run a submitted method using a command of the
following form:

```matlab
platemo( ...
    'problem',@EvoPINN1, ...
    'algorithm',@YourAlgorithm, ...
    'N',50, ...
    'maxFE',100000, ...
    'parameter',{1,1}, ...
    'save',1);
```

## Baseline and Analysis Scripts

The repository includes helper scripts for generating benchmark evidence and
figures:

```text
validation/run_evopinn_front_archive.m
validation/build_evopinn_benchmark_data.py
validation/run_evopinn_ruggedness_data.m
validation/run_evopinn_landscape_slice_data.m
validation/plot_evopinn_landscape_surfaces.py
```

To regenerate the landscape figure:

```matlab
addpath(genpath('PlatEMO'));
addpath('validation');
run_evopinn_landscape_slice_data( ...
    'Problems',{'EvoPINN1','EvoPINN4','EvoPINN8','EvoPINN11'}, ...
    'sampleScale',1, ...
    'gridSize',25, ...
    'radius',0.12, ...
    'candidateCount',16, ...
    'outputDir','validation/evopinn_landscape_slices_preview');
```

```bash
python validation/plot_evopinn_landscape_surfaces.py \
  --input validation/evopinn_landscape_slices_preview/evopinn_landscape_slices.csv
```

If a landscape slice looks too smooth, increase the number of base candidates,
try multiple evaluator seeds, scan several radii, or plot objective-specific
heights.  In high dimensions, a random 2-D plane can easily miss the stiff or
chaotic directions that dominate optimizer behavior.

## Important Dates

Competition dates will be announced by the organizers.

```text
Registration deadline: TBA
Result submission deadline: TBA
Notification: TBA
```

## Citation

If you use EvoPINN in a paper, please cite the EvoPINN competition or benchmark
paper once it is available.

This repository is built on PlatEMO.  Please also acknowledge PlatEMO:

```bibtex
@article{tian2017platemo,
  title   = {PlatEMO: A MATLAB platform for evolutionary multi-objective optimization},
  author  = {Tian, Ye and Cheng, Ran and Zhang, Xingyi and Jin, Yaochu},
  journal = {IEEE Computational Intelligence Magazine},
  volume  = {12},
  number  = {4},
  pages   = {73--87},
  year    = {2017}
}
```

## Contact

Organizer contact information will be added before the official release.
