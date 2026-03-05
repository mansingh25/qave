<p align="center">
  <img src="assets/qave_logo.png" alt="QAVE logo" width="420" />
</p>

# Quantum Algorithm Visualization Engine (QAVE)

QAVE is a scientifically rigorous quantum circuit and algorithm visualization engine built for teaching and conceptual clarity. The Python backend emits a **versioned** `trace.json` that includes **physically computed in-gate evolution samples** instead of endpoint-only tweening. The reference viewer is a local Processing Java-mode sketch (`P3D`) that replays traces deterministically with synchronized views.

![3-qubit GHZ demo](assets/ghz3.gif)

3-qubit GHZ, deterministic `seed=24`.

## Key capabilities

- Qiskit + OpenQASM ingestion
- Deterministic timeline playback from `trace.json`
- Measurement `collapse` mode with deterministic seed
- Synchronized circuit, amplitude, and probability views
- Versioned contracts under `contracts/schemas/`
- Processing viewer as the reference renderer

## Quickstart

Generate an animation from a Qiskit circuit (requires Processing, PeasyCam, and ffmpeg; see [`docs/setup/runtime_dependencies.md`](docs/setup/runtime_dependencies.md)):

```python
from pathlib import Path

from qiskit import QuantumCircuit

from qave import ArtifactOptions, RenderOptions, SimulationOptions, generate_animation_from_qiskit

circuit = QuantumCircuit(3, 3)
circuit.h(0)
circuit.cx(0, 1)
circuit.cx(0, 2)
circuit.measure(range(3), range(3))

result = generate_animation_from_qiskit(
    circuit,
    options=SimulationOptions(algorithm_id="ghz", mode="validation", seed=24, shot_count=100),
    render=RenderOptions(width=960, height=540, fps=20, keep_frames=False, emit_mp4=False, emit_gif=True),
    artifacts=ArtifactOptions(out_dir=Path("artifacts/ghz3")),
)

print(result.gif_path)
```

## Docs

- Runtime dependencies (Processing / PeasyCam / ffmpeg): [`docs/setup/runtime_dependencies.md`](docs/setup/runtime_dependencies.md)
- Testing: [`backend/tests/README.md`](backend/tests/README.md)
- Tutorial: [`docs/tutorials/ghz3_with_QAVE.ipynb`](docs/tutorials/ghz3_with_QAVE.ipynb)

## Repo layout

- `backend/`: Python package (`qave`) and integration tests
- `viewer/`: Processing viewer + local scripts
- `contracts/`: exported JSON schemas used at the viewer/backend boundary
- `docs/`: focused documentation and tutorials
- `assets/`: README media (tracked)

## License

MIT. See [`LICENSE`](LICENSE).
