# General
- Apply functional programming paradigms wherever sensible. In particular, avoid
  global state and other side effects in the core logic like the plague and
  follow a "functional core, imperative shell" approach.
- Type safety is paramount. Disabling or working around the type checker through
  `# type: ignore`, manual type casts, or resorting to dynamic types everywhere
  (e.g. `dict.get()` in Python) etc. is a major code smell and usually indicates
  that the types and interfaces have been designed incorrectly. In such cases,
  reflect a bit on what's going on at a deeper level and what could be done
  here.
- Within a given source file, code entrypoints should be at the top; secondary
  functions (helper functions) further below.
- Avoid Java enterprise-style OOP where classes contain gigantic collections of
  methods for all kinds of purposes (no separation of concerns!) and, worse,
  many of them modify the object's internal state or produce other side effects.
  Instead, prefer simple data structures (e.g. classes with a handful methods
  with the same clear purpose) + auxiliary functions + composition.


# Writing tests
- Almost all code should be tested.
- Whenever possible (depending on the language), files containing unit tests
  should be co-located with the code-under-test, e.g. in a `*_test.py` file
  (Python) or `*.test.ts` file (TypeScript).
- For potentially long-running integration tests (or ones with significant side
  effects), tag the test accordingly.
- Each test should ideally cover a single case ("given, when, then" aka
  "arrange, act, assert") and, accordingly, test function names should follow
  the pattern `test_functionF_does_X_when_Y()`. The test function name should
  basically be a claim, written out in English. Obviously, keep those statements
  as concise as possible but, if in doubt, prefer longer & clearly phrased names
  over short, possibly misleading ones.
- Try to avoid mocking in tests. Instead, make dependencies in production code
  explicit through function parameters, so that they can be easily injected by
  tests.
- For code with external dependencies (file system, database) and other side
  effects think carefully about how we want to go about testing:
  - Can we replace the database / file system dependency with an in-memory
    database or file system at test time?
  - How can we prevent the production code from writing stuff to the hard
    drive (or carrying out other side effects) and, as a result, tests
    potentially influencing themselves or failing randomly?
  - Do we actually need to test the side effects? In a "function core,
    imperative shell" approach, the meat should be in the functional core and
    the imperative shell should be as thin as possible.


# Python-specific guidelines
- Imports:
  - Use absolute imports throughout. Absolute imports facilitate repository-wide
    string search. (Exception: In test files it makes sense to use a relative
    import to import from the code under test, since this makes moving files
    easier.)
  - Leave `__init__.py` files empty and don't manipulate subpackage exports via
    `__all__`. Imports should stay predictable and understandable. Avoid import
    magic.
- Tests:
  - Use Pytest. Don't wrap Pytest test functions in classes, unless necessary
    for some reason.
  - Avoid Pytest fixtures since they're essentially global state of the test
    application.
