
## Why we need it here

- right now the RPS values are numbers like 1 or 150
- Model doesn't know this is spike or anything.
- This can create problems like:

## Unequal Gradient Scales

- This makes small value update gradient less than large values.
- This can result in slow convergence or exploding gradients.

## Activation:

- Activation functions expect values in a scale without a determined scale the activation
  won't be that good
- 