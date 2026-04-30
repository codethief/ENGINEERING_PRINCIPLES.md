# Programming paradigms
- Apply functional programming paradigms wherever sensible. In particular, avoid
  global state and other side effects in the core logic like the plague and
  follow a "functional core, imperative shell" approach.
- Avoid Java enterprise-style OOP where classes contain gigantic collections of
  methods for all kinds of purposes (no separation of concerns!) and, worse,
  many of them modify the object's internal state or produce other side effects.
  Instead, prefer simple data structures (e.g. classes with a handful methods
  with the same clear purpose) + auxiliary functions + composition.


# Architectural principles to draw inspiration from
- Separation of concerns
- Hexagonal architecture & onion architecture. Code architecture should be as
  simple as possible but not any simpler.
- Domain-driven design: Prefer clear domain language and corresponding domain
  model entities over ad-hoc data structures.


# Type safety
- Type safety is paramount. Disabling or working around the type checker through
  `# type: ignore` comments, manual type casts, or resorting to dynamic types
  everywhere (e.g. `dict.get()` in Python) etc. is a major code smell and
  usually indicates that the types and interfaces have been designed
  incorrectly. In such cases, reflect a bit on what's going on at a deeper level
  and what could be done here.


# Code & module structure
- Entry points: For an application, prefer a code base with a single obvious entry
  point over one with many different, non-obvious entry points. If the code does
  many different things, either provide a top-level interface (e.g. a CLI) as
  single entry point or consider splitting up the code into different packages
  and applications.
  - For libraries be transparent about what is and is not part of the public
    interface.
  - Ad-hoc scripts have their place, but not in a mature code base. Prefer a
    well-documented developer CLI over a hodgepodge of quick-and-dirty scripts.
- Within a given source file, code entrypoints and "the most important code"
  should be at the top; secondary functions (helper functions used only in that
  source file) should be near the bottom.


# Code comments
Use code comments not to repeat what the code very obviously does (trivial
comments) but
- to explain the Why behind the code,
- to explain non-obvious design & architectural designs, trade-offs,
- to document product feature decisions,
- to highlight non-obvious (e.g. non-local) dependencies between different
  pieces of code that need to be kept in sync,
- to document APIs, so that the underlying code can be treated as a black box,
- to document assumptions not covered by the type system,
- to structure the code and guide the reader.
- to mark code when critical functionality is still missing ("TODO" comments) and
  that fact is not entirely obvious.

That being said:
- Use code comments sparingly. If the total number of LoC double or triple due
  to excessive commenting, this will hinder readability and maintainability.
- A function whose body is 3 lines probably doesn't need an extensive API
  documentation. Especially not if it's just a helper function that's used in a
  single place.
- Don't repeat in a docstring what the function's parameter types already
  clearly tell anyway. (Use comments to explain the part that the types don't
  cover!)
- Internal APIs usually need less documentation than public ones.
- Prefer clear variable and function names over code comments. Prefer splitting
  up long functions into well-named smaller functions over writing guiding
  comments.
- Cross-code base references in comments age very badly since they don't get
  type-checked. Use them very sparingly.
- Don't overdo it with TODO comments. Don't write TODO comments for features
  planned for the medium- to long-term future that may or may not be implemented
  in the end.


# Writing tests
- Almost all first-party code should be tested.
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
  explicit (e.g. through function parameters), so that tests can easily inject
  fakes as needed.
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
    import to import from the module under test, since this makes moving files
    easier.)
  - Leave `__init__.py` files empty and don't manipulate subpackage exports via
    `__all__`. Imports should stay predictable and understandable. Avoid
    manipulating PYTHONPATH and related import magic.
- Tests:
  - Use Pytest. Don't wrap Pytest test functions in classes, unless necessary
    for some reason.
  - Avoid Pytest fixtures since they're essentially global state of the test
    application.
