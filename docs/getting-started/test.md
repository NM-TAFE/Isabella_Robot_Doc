# Testing Guide

## Running Tests

Isabella Robot includes comprehensive test suites for validation and verification.

### Prerequisites

Ensure you have the following installed:

- pytest
- unittest (included with Python)
- Docker (for integration tests)

### Unit Tests

Run all unit tests:

```bash
pytest tests/unit/
```

Run specific test file:

```bash
pytest tests/unit/test_localizer.py -v
```

### Integration Tests

Run integration tests (requires Docker):

```bash
pytest tests/integration/
```

### Test Coverage

Generate coverage report:

```bash
pytest --cov=src tests/
```

## Test Structure

```
tests/
├── unit/              # Unit tests for individual components
├── integration/       # Integration tests for multiple systems
└── fixtures/          # Test data and configurations
```

## Continuous Integration

Tests are automatically run on every pull request via GitHub Actions.

## Troubleshooting

If tests fail:

1. Ensure all dependencies are installed: `pip install -r requirements.txt`
2. Check Docker daemon is running: `docker status`
3. Review test output for specific error messages
4. Check the [GitHub repository](https://github.com/NM-TAFE/Isabella_Robot_Doc) for known issues

## Contributing Tests

When contributing new features, please include:

- Unit tests for new functions
- Integration tests for new systems
- Documentation of test procedures
