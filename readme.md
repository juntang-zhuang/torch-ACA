# PyTorch implementation of "Adaptive Checkpoint Adjoint" (ACA) for an accurate and differentiable ODE solver
This library provides ordinary differential equation (ODE) solvers implemented in PyTorch. Backpropagation through all solvers is supported using the adjoint method with a checkpoint strategy to guarantee numerical accuracy in reverse-mode trajectory. For usage of ODE solvers in deep learning applications, see [1].

## Dependencies
PyTorch 1.0 (Will test on other versions later)
tensorboardX
Pythorn 3

## Examples
### Image classification on Cifar
A ResNet18 is modified into its corresponding ODE model, and achieve ~5% errorate (vs 10% by adjoint method and naive method).
Code is in folder ```cifar_classification```
#### How to train
```
python train.py
```
#### Train with different modes of solvers
```train.py``` uses the solver defined in ```torch_ACA/odesolver_mem/ode_solver_endtime.py```, this mode only support integration from start time t0 to end time t1, and output a tensor for time t1.

```train_mem.py``` uses the solver defined in ```torch_ACA/odesolver_mem/adjoint_mem.py```, this mode only support integration from start time t0 to end time t1, and output a tensor for time t1. Furtheremore, this mode uses \\[O(N_f + N_t)\\] memory, which is more memory-efficient than normal mode, but the running time is longer.

```train_multieval.py``` uses the solver defined in ```torch_ACA/odesolver/ode_solver.py```, this mode supports extracting outputs from multiple time points between t0 and t1. 
Note: 
(1) Evaluation time ```t_eval``` must be specified in a list. 
    e.g.  t_eval = [a1, a2, a3 ..., an]  where 
suppose \\[z\\] is of shape xxx, 

#### Warning
This repository currently only supports \\[ \frac{dz}{dt} = f(t,z) \\] where \\[z\\] is a tensor (other data types such as tuple are not supported).
If you are using a function \\[f\\] which produces many output tensors, you can concatante them into a single tensor within definition of \\[f\\].

### Three-body problem
Please run ```python three_body_problem.py ```.
The problem is: given trajectories of three stars, how to estimate their masses and predict their future trajectory.




## References
[1] Zhuang, Juntang, et al. "Adaptive Checkpoint Adjoint Method for Gradient Estimation in Neural ODE." arXiv preprint arXiv:2006.02493 (2020). [[arxiv]](https://arxiv.org/abs/2006.02493)

Please cite our paper if you find this repository useful:
```
@article{zhuang2020adaptive,
  title={Adaptive Checkpoint Adjoint Method for Gradient Estimation in Neural ODE},
  author={Zhuang, Juntang and Dvornek, Nicha and Li, Xiaoxiao and Tatikonda, Sekhar and Papademetris, Xenophon and Duncan, James},
  journal={ICML},
  year={2020}
}
```
