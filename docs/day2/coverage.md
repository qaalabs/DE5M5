# Achieve Test Coverage

## Learning Objectives

- Write comprehensive tests for ingestion module
- Reach 70%+ test coverage target
- Understand what good coverage means
- Fix any failing tests

## What is Test Coverage?

```
Coverage = (Lines of code executed by tests) / (Total lines of code)

Example:
Your code:     10 lines
Tests run:      7 lines
Coverage:      70%
```

**But**: Coverage isn't everything!

- 70% coverage with good tests > 90% coverage with meaningless tests
- Test behavior, not just lines
- Edge cases matter more than happy path

## How can you see the coverage?

### Full coverage report

`pytest --cov=src`

### See which lines aren't covered

`pytest --cov=src --cov-report=html`

Then open `htmlcov/index.html` in a browser.

---

## Guidelines for this project:

- **Stretch Goal**: 80% coverage
- **Acceptable**: 70% coverage
- **Minimum**: All critical functions tested

Share your tests with the group - if you have a test that works well, others can use it to help everyone reach 70%.

