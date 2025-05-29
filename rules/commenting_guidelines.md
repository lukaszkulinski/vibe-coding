# Codebase Commenting Guidelines (PHP/Symfony)

## Philosophy

Our primary goal for commenting is **clarity and maintainability**. In a PHP/Symfony codebase, type hints and PHPDoc provide information about the *what*. Therefore, comments should focus on the **why**, the **intent**, and the **complexities** that are not immediately obvious from the code or type hints alone.

We use **PHPDoc** as the standard format for multi-line comments. PHPDoc ensures structure, integrates with IDEs for enhanced code navigation and type inference, and offers a consistent format for both human developers and AI systems interacting with our code.

Comments should always add value. Avoid redundant comments that simply restate what the code or type declarations already make clear. Prioritize commenting on public APIs, complex logic, and non-intuitive decisions.

---

## Core Standard: PHPDoc

All multi-line comments explaining classes, functions, methods, services, controllers, or complex blocks must use the PHPDoc format (`/** ... */`).

---

## When to Comment

Focus commenting efforts where they provide the most value:

1. **Public APIs and Services:**
    - **Functions/Methods:** Explain the purpose, parameters, return values, side effects, and any potential exceptions (`@throws`).
    - **Controllers/Services:** Document the intent of the controller/service, key actions, expected input/output, and any business rules enforced.
    - **Entities/DTOs:** Explain the role of complex or widely-used classes, especially if the name or usage is not immediately self-explanatory.

2. **Complex Logic:**
    - If an algorithm, calculation, or piece of business logic is intricate or non-obvious, add comments explaining the *why* and the reasoning behind the approach.

3. **Non-Obvious Decisions & Trade-offs:**
    - If a particular implementation choice was made for specific reasons (performance, framework limitation, compatibility, workaround for a bug), document it. This context is crucial for future maintainers.

4. **Important Constants or Configuration:**
    - If the purpose of a constant, configuration value, or environment variable is not clear from its name and value, add a brief explanation.

5. **Workarounds and `TODO`s:**
    - Use `// HACK:` or `// WORKAROUND:` for temporary fixes, explaining why the workaround is necessary and potentially linking to an issue tracker or documentation.
    - Use `// TODO:` for planned improvements or missing features, ideally with context or a link to an issue.

6. **Type Clarifications (Sparingly):**
    - If PHP’s type hints or PHPDoc might be ambiguous, or a variable requires further semantic explanation, add a clarifying comment or a `@var` PHPDoc tag. Howev
