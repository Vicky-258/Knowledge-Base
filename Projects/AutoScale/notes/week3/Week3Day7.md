# Week 3 — Forecasting Model (Summary)

## Final Model Choice
GRU was selected due to its stability, fast convergence, and predictable failure mode.
While it smooths spikes, this behavior is safer for autoscaling when combined with buffers.

## Design Choices
- Fixed input window: 30 timesteps
- Prediction horizon: 12 steps
- Bottleneck architecture (last hidden state)
- MSE-focused evaluation

## Lessons Learned
- Architecture matters more than model family
- Predictable failures are preferable to sharp accuracy gains
- Transformers require architectural freedom to outperform RNNs

## Mistakes Made
- Over-trusting early loss improvements
- Expecting attention to shine under bottleneck constraints
- Underestimating how much failure mode matters in systems

## Ready for Integration
The predictor is now frozen, versioned, and inference-safe.
Future work will focus on scaling logic, not retraining.


[[Week4]]