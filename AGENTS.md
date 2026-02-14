# SAM3 Agent Guidelines

This file provides guidelines for coding agents working on the SAM3 repository.

## Build/Lint/Test Commands

```bash
# Install development dependencies
pip install -e ".[dev,train]"

# Format code (use this before committing)
ufmt format .

# Check formatting without making changes
ufmt check .

# Run type checking
mypy sam3

# Run all tests
pytest

# Run a single test file
pytest sam3/perflib/tests/tests.py

# Run a single test function
pytest sam3/perflib/tests/tests.py::TestMasksToBoxes::test_masks_box

# Run tests with coverage
pytest --cov=sam3 --cov-report=html
```

## Code Style Guidelines

### File Headers
Every Python file must start with:
```python
# Copyright (c) Meta Platforms, Inc. and affiliates. All Rights Reserved

# pyre-unsafe
```

### Imports
- Imports are automatically sorted by `usort` (via ufmt)
- Use `from typing import` for type hints: `from typing import Optional, List, Dict`
- Keep external imports separate from local sam3 imports
- Order: stdlib → external (torch, numpy, PIL) → local (sam3.*)

### Formatting
- Use `black` formatter with 88 character line length
- Use `ufmt` which combines `ruff-api` formatter and `usort` for import sorting
- No inline comments except docstrings

### Type Hints
- All functions must have type hints (mypy enforces `disallow_untyped_defs`)
- Use `Optional[T]` for optional types
- Use `Dict[K, V]`, `List[T]`, `Tuple[...]` from typing module
- Use `torch.Tensor` for PyTorch tensors

### Naming Conventions
- **Classes**: PascalCase (e.g., `Sam3Image`, `BoxMode`, `Visualizer`)
- **Functions/Methods**: snake_case (e.g., `sam3_inference`, `run_single_image_inference`)
- **Private methods**: prefix with underscore (e.g., `_encode_prompt`, `_prune_messages`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `TEXT_ID_FOR_TEXT`)

### Docstrings
- Use docstrings for classes and public functions
- Keep docstrings concise and descriptive
- Format with triple quotes, first line summary

### Error Handling
- Use specific exception types (`ValueError`, `FileNotFoundError`, etc.)
- Include descriptive error messages with context
- Example: `raise ValueError(f"Invalid JSON in tool call: {tool_call_json_str}")`

### PyTorch Specific Patterns
- Use `torch.nn.Module` for model classes
- Use `torch.Tensor` for tensor type hints
- Use `torch.cuda` operations with proper device management
- Call `super().__init__()` in `__init__` methods

### Testing
- Test files located in `sam3/perflib/tests/`
- Use `pytest` framework
- Test classes named `Test*`
- Test functions named `test_*`
- Use `torch.testing.assert_close()` for tensor comparisons

### Code Organization
- Package structure:
  - `sam3/model/`: Model architectures (encoder, decoder, backbones)
  - `sam3/agent/`: Agent logic and inference helpers
  - `sam3/train/`: Training utilities
  - `sam3/perflib/`: Performance utilities and tests
- Keep related functionality together
- Use `__init__.py` files to define public APIs

### Common Patterns
- Use `from sam3.model.*` for model imports
- Use `from sam3.agent.helpers.*` for utility helpers
- Use `typing_extensions.override` for overriding methods
- Use `from copy import deepcopy` when needed
