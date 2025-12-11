# CLAUDE.md - Physics Agent Project Guide

**Last Updated:** 2025-12-11
**Repository:** sulifang19860625-cyber/physics-agent
**Purpose:** AI-powered physics simulation and problem-solving agent

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Current State](#current-state)
3. [Planned Architecture](#planned-architecture)
4. [Development Workflows](#development-workflows)
5. [Key Conventions](#key-conventions)
6. [Common Tasks](#common-tasks)
7. [Testing Strategy](#testing-strategy)
8. [AI Assistant Guidelines](#ai-assistant-guidelines)

---

## Project Overview

### Purpose
This repository contains a physics agent designed to:
- Solve physics problems using computational methods
- Simulate physical systems and phenomena
- Provide explanations of physics concepts
- Validate solutions against known physics principles
- Interface with various physics simulation frameworks

### Technology Stack (Planned)
- **Language:** Python 3.10+ (recommended for scientific computing)
- **Core Libraries:**
  - NumPy/SciPy for numerical computations
  - SymPy for symbolic mathematics
  - Matplotlib for visualizations
  - PyTorch/TensorFlow (optional, for ML-based approaches)
- **Agent Framework:** LangGraph, CrewAI, or custom implementation
- **Testing:** pytest, hypothesis for property-based testing
- **Documentation:** Sphinx or MkDocs

---

## Current State

### Repository Status
- **Stage:** Initial setup - fresh repository
- **Files Present:** README.md only
- **Branch:** `claude/claude-md-mj0w0cf2322ply39-01GvySWSHmGPCwCn3iy8U98M`
- **Last Commit:** Initial commit (fdef82a)

### Immediate Next Steps
1. Define project requirements and scope
2. Set up Python project structure
3. Configure development environment (virtual env, dependencies)
4. Implement core agent framework
5. Add physics solvers and simulators
6. Create test suite

---

## Planned Architecture

### Recommended Directory Structure

```
physics-agent/
├── README.md                 # Project overview and quick start
├── CLAUDE.md                # This file - AI assistant guide
├── pyproject.toml           # Python project configuration
├── requirements.txt         # Python dependencies
├── setup.py                 # Package installation script
├── .gitignore              # Git ignore patterns
├── .env.example            # Environment variables template
│
├── src/
│   └── physics_agent/
│       ├── __init__.py
│       ├── agent/          # Agent core logic
│       │   ├── __init__.py
│       │   ├── base.py     # Base agent class
│       │   ├── planner.py  # Task planning
│       │   └── executor.py # Task execution
│       │
│       ├── solvers/        # Physics problem solvers
│       │   ├── __init__.py
│       │   ├── mechanics.py    # Classical mechanics
│       │   ├── thermodynamics.py
│       │   ├── electromagnetism.py
│       │   └── quantum.py      # Quantum mechanics
│       │
│       ├── simulators/     # Physics simulators
│       │   ├── __init__.py
│       │   ├── particle.py     # Particle systems
│       │   ├── field.py        # Field simulations
│       │   └── molecular.py    # Molecular dynamics
│       │
│       ├── utils/          # Utility functions
│       │   ├── __init__.py
│       │   ├── constants.py    # Physical constants
│       │   ├── units.py        # Unit conversions
│       │   └── validation.py   # Solution validation
│       │
│       └── llm/            # LLM integration
│           ├── __init__.py
│           ├── prompts.py      # Prompt templates
│           └── parser.py       # Response parsing
│
├── tests/
│   ├── __init__.py
│   ├── test_agent/
│   ├── test_solvers/
│   ├── test_simulators/
│   └── fixtures/           # Test data and fixtures
│
├── examples/               # Example usage and notebooks
│   ├── simple_mechanics.py
│   ├── trajectory_simulation.ipynb
│   └── quantum_particle.py
│
├── docs/                   # Documentation
│   ├── api/               # API documentation
│   ├── guides/            # User guides
│   └── physics/           # Physics reference
│
└── scripts/               # Utility scripts
    ├── setup_env.sh
    └── run_benchmarks.py
```

---

## Development Workflows

### Initial Setup (First-Time Development)

```bash
# 1. Clone repository (already done)
cd /home/user/physics-agent

# 2. Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -e ".[dev]"  # Editable install with dev dependencies

# 4. Set up pre-commit hooks (optional)
pre-commit install

# 5. Run tests to verify setup
pytest
```

### Standard Development Workflow

1. **Create Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Write Tests First (TDD Approach)**
   - Write failing tests in `tests/`
   - Run tests: `pytest tests/`

3. **Implement Feature**
   - Write minimal code to pass tests
   - Follow code conventions (see below)

4. **Validate**
   - Run tests: `pytest`
   - Check coverage: `pytest --cov=physics_agent`
   - Lint code: `ruff check .` or `pylint src/`
   - Type check: `mypy src/`

5. **Commit and Push**
   ```bash
   git add .
   git commit -m "feat: add particle collision solver"
   git push -u origin feature/your-feature-name
   ```

### Git Commit Convention

Use conventional commits format:
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `test:` - Test additions or modifications
- `refactor:` - Code refactoring
- `perf:` - Performance improvements
- `chore:` - Maintenance tasks

Examples:
```
feat: add thermodynamics solver for ideal gases
fix: correct momentum calculation in elastic collisions
docs: update API reference for quantum solver
test: add property-based tests for unit conversions
```

---

## Key Conventions

### Code Style

#### Python Style Guide
- Follow PEP 8 style guide
- Use type hints for all function signatures
- Maximum line length: 100 characters
- Use descriptive variable names (e.g., `velocity`, not `v`, except in well-known equations)
- Docstrings: Google or NumPy style

#### Example Function

```python
from typing import Union
import numpy as np

def calculate_kinetic_energy(
    mass: float,
    velocity: Union[float, np.ndarray]
) -> Union[float, np.ndarray]:
    """Calculate kinetic energy using classical mechanics.

    Args:
        mass: Mass of the object in kilograms
        velocity: Velocity in meters per second (scalar or vector)

    Returns:
        Kinetic energy in Joules

    Raises:
        ValueError: If mass is negative or zero

    Example:
        >>> calculate_kinetic_energy(2.0, 3.0)
        9.0
    """
    if mass <= 0:
        raise ValueError(f"Mass must be positive, got {mass}")

    if isinstance(velocity, np.ndarray):
        speed_squared = np.sum(velocity ** 2)
    else:
        speed_squared = velocity ** 2

    return 0.5 * mass * speed_squared
```

### Physics Conventions

#### Units
- **Always use SI units internally** (meters, kilograms, seconds, etc.)
- Provide conversion utilities in `utils/units.py` for input/output
- Document units clearly in docstrings and variable names when ambiguous

#### Physical Constants
- Use constants from `scipy.constants` when available
- Define custom constants in `utils/constants.py`
- Never hard-code physical constants in calculation functions

#### Numerical Precision
- Use `float64` (double precision) for physics calculations
- Be aware of numerical stability issues in iterative methods
- Document any assumptions about precision or tolerances

#### Coordinate Systems
- Default to right-handed Cartesian coordinates
- Clearly document any coordinate system transformations
- Use consistent axis conventions (z-up or y-up, specify in docs)

### Error Handling

```python
class PhysicsError(Exception):
    """Base exception for physics-related errors."""
    pass

class InvalidPhysicsStateError(PhysicsError):
    """Raised when physical state is invalid (e.g., negative mass)."""
    pass

class ConvergenceError(PhysicsError):
    """Raised when numerical method fails to converge."""
    pass
```

---

## Common Tasks

### Task 1: Adding a New Physics Solver

1. **Create solver module** in `src/physics_agent/solvers/`
2. **Define solver class** inheriting from base class
3. **Implement solve method** with clear physics documentation
4. **Add unit tests** in `tests/test_solvers/`
5. **Add example usage** in `examples/`
6. **Update documentation**

Example structure:
```python
# src/physics_agent/solvers/mechanics.py
class ProjectileMotionSolver:
    """Solver for projectile motion under constant gravity."""

    def __init__(self, gravity: float = 9.81):
        self.gravity = gravity

    def solve(
        self,
        initial_position: np.ndarray,
        initial_velocity: np.ndarray,
        time: float
    ) -> dict:
        """Solve projectile motion equations.

        Returns:
            Dictionary with 'position', 'velocity', 'time' arrays
        """
        # Implementation
        pass
```

### Task 2: Adding LLM Integration

1. **Define prompt templates** in `src/physics_agent/llm/prompts.py`
2. **Create parser** for LLM responses
3. **Add validation** to ensure physics consistency
4. **Test with example problems**

### Task 3: Running Simulations

```python
from physics_agent.simulators.particle import ParticleSimulator

# Create simulator
sim = ParticleSimulator(timestep=0.01)

# Add particles
sim.add_particle(mass=1.0, position=[0, 0, 0], velocity=[1, 0, 0])

# Run simulation
results = sim.run(duration=10.0)

# Analyze results
sim.plot_trajectories()
```

---

## Testing Strategy

### Test Types

#### 1. Unit Tests
- Test individual functions and methods
- Use pytest fixtures for common setups
- Mock external dependencies

#### 2. Integration Tests
- Test solver pipelines end-to-end
- Validate against known physics solutions
- Test agent workflow

#### 3. Property-Based Tests
- Use `hypothesis` library for property-based testing
- Verify conservation laws (energy, momentum, etc.)
- Test numerical stability

Example:
```python
from hypothesis import given, strategies as st
import numpy as np

@given(
    mass=st.floats(min_value=0.1, max_value=100.0),
    velocity=st.floats(min_value=-100.0, max_value=100.0)
)
def test_kinetic_energy_positive(mass, velocity):
    """Kinetic energy must always be non-negative."""
    ke = calculate_kinetic_energy(mass, velocity)
    assert ke >= 0
```

#### 4. Physics Validation Tests
- Compare against analytical solutions
- Verify conservation laws are maintained
- Test limiting cases (e.g., zero velocity, infinite mass ratios)

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=physics_agent --cov-report=html

# Run specific test file
pytest tests/test_solvers/test_mechanics.py

# Run tests matching pattern
pytest -k "test_energy"

# Run with verbose output
pytest -v
```

---

## AI Assistant Guidelines

### General Principles

1. **Physics First**: Always verify that solutions are physically plausible
   - Check units and dimensions
   - Verify conservation laws
   - Validate against known limits and special cases

2. **Numerical Stability**: Be aware of numerical issues
   - Avoid division by very small numbers
   - Use stable numerical methods
   - Validate convergence of iterative methods

3. **Documentation**: Provide clear physics explanations
   - Document equations in LaTeX when helpful
   - Cite physics principles being used
   - Explain assumptions and approximations

4. **Testing**: Always write tests for physics code
   - Test edge cases (zero, infinity, etc.)
   - Verify conservation laws
   - Compare against analytical solutions when available

### When Adding Physics Code

- [ ] Verify all equations against physics references
- [ ] Check dimensional consistency of all terms
- [ ] Use SI units internally
- [ ] Add docstrings with physics explanation
- [ ] Include mathematical formulation (LaTeX if complex)
- [ ] Write tests that verify physical correctness
- [ ] Add example usage
- [ ] Consider numerical stability

### When Debugging Physics Issues

1. **Check units**: Most physics bugs are unit mismatches
2. **Verify signs**: Check coordinate system and sign conventions
3. **Test conservation**: Energy, momentum, angular momentum
4. **Compare to limits**: What happens at v→0, m→∞, etc.?
5. **Dimensional analysis**: Do units work out correctly?
6. **Plot results**: Visual inspection often reveals issues

### Common Physics Pitfalls to Avoid

- **Don't** mix unit systems (SI and CGS, degrees and radians)
- **Don't** hard-code physical constants
- **Don't** ignore numerical precision issues
- **Don't** assume convergence without verification
- **Don't** forget to document coordinate system choices
- **Do** validate conservation laws in tests
- **Do** check limiting cases and special solutions
- **Do** use established numerical libraries when available

### Code Review Checklist

When reviewing or writing physics code:

- [ ] Units are consistent (all SI or clearly documented)
- [ ] Physical constants from `scipy.constants` or `utils/constants.py`
- [ ] Type hints on all function signatures
- [ ] Docstrings with physics explanation
- [ ] Conservation laws tested
- [ ] Edge cases handled (zero velocity, zero mass, etc.)
- [ ] Numerical stability considered
- [ ] Example usage provided
- [ ] Comparison to analytical solution if available

### Interaction with LLMs

When the physics agent uses LLMs:

1. **Validate LLM Physics Output**: Never trust LLM physics without verification
2. **Use Physics Constraints**: Constrain LLM outputs with physics knowledge
3. **Parse Carefully**: Extract numerical values and validate units
4. **Fallback to Classical Methods**: Use symbolic/numerical methods as ground truth
5. **Explain Reasoning**: Have LLM explain physics reasoning for debugging

---

## Quick Reference

### Essential Commands

```bash
# Setup
python -m venv venv && source venv/bin/activate
pip install -e ".[dev]"

# Testing
pytest                          # Run all tests
pytest --cov=physics_agent     # With coverage
pytest -k "mechanics"          # Run specific tests

# Code Quality
ruff check .                   # Linting
mypy src/                      # Type checking
black src/ tests/              # Auto-formatting

# Running Examples
python examples/simple_mechanics.py

# Documentation
cd docs && make html           # Build docs
```

### Key Files to Review Before Changes

- `src/physics_agent/utils/constants.py` - Physical constants
- `src/physics_agent/utils/units.py` - Unit conversions
- `tests/fixtures/` - Test data and known solutions
- `examples/` - Example usage patterns

### External Resources

- [SciPy Documentation](https://docs.scipy.org/)
- [NumPy Documentation](https://numpy.org/doc/)
- [Physics Reference](https://en.wikipedia.org/wiki/Portal:Physics)
- [Numerical Recipes](http://numerical.recipes/) - Numerical methods reference

---

## Version History

- **2025-12-11**: Initial CLAUDE.md creation for fresh repository
- Future updates should be logged here

---

## Contributing

This is the initial structure for the physics-agent project. As the codebase evolves:

1. Keep this document updated with architectural changes
2. Add specific examples from the actual implementation
3. Document any deviations from these conventions with reasoning
4. Update the directory structure to reflect actual layout
5. Add performance benchmarks and optimization notes
6. Document any external API integrations

**Remember**: This document is for AI assistants to understand the codebase. Keep it factual, specific, and focused on conventions that affect code generation and decision-making.
