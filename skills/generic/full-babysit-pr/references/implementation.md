# Implementation

Write the smallest change that completely solves the task. Small means nothing
beyond what the task needs, never an unfinished task. Readability beats line
count.

**Understand**

- Read repository instructions, the code the task touches, its callers and
  tests. Trace the real flow: where data enters, what is stored, what leaves.
- List every place the change must reach: callers, tests, fixtures, config,
  exports, documentation. That is scope; extra features are not.
- Check what the change could break: data it could destroy or expose, callers
  or public contracts that would stop working.

**Choose the solution**

First drop what nobody asked for, then take the first option that fully works:

1. Skip what nobody asked for: options, flexibility, "for later" code. A vague
   request gets the smallest version that does the core job.
2. Reuse what the codebase has (a helper, component, service or pattern), the
   way surrounding code uses it.
3. Use the standard library or a platform feature, unless the project has its
   own equivalent.
4. Use an installed dependency. Never add a dependency for a few lines.
5. Only then write the minimum new code, following established patterns.

**Build**

- Finish every part the task needs, including callers, tests and fixtures the
  change affects. Fix a bug at its root in shared code, not in one caller.
- Add no unrequested abstraction, wrapper, configuration or type conversion.
  Keep the existing structure, layers and conventions. Prefer deletion to
  addition when it solves the task.
- Between options of similar size, take the one correct on edge cases.
- Never cut validation at trust boundaries, error handling that prevents data
  loss, security or accessibility. Moved or merged code keeps them.
- Comment only the reason the code cannot show.
- Give new non-trivial logic (a branch, loop, parser, money, security or data
  writes) one focused test that fails when it breaks. Trivial changes need none.

Resolve routine choices autonomously. Record what was reused instead of
written and what was deliberately skipped and why.
