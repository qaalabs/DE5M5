## Testing Demo

- Why test: tests catch regressions, not just new bugs - without them you won't know until production breaks
- The pandas problem: `assert df1 == df2` raises a ValueError - use `pandas.testing.assert_frame_equal()`
- Fixtures provide reusable test data - show the `@pytest.fixture` pattern
- Walk through the full test file in `python-testing.md` - learners follow along
