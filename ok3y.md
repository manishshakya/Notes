#core Guidelines
#guidelines

# C++ Core Guidelines

Key Sections
The document is organized into categories, each with rules and rationale. Here are the most significant ones:

## Philosophy (P)

P.1: Express ideas directly in code. (e.g., Use std::vector instead of raw arrays to make intent clear.)
P.2: Write in ISO Standard C++. (Avoid vendor-specific extensions unless necessary.)
Rationale: Code should be portable, explicit, and aligned with modern standards.

## Interfaces (I)

I.3: Prefer passing by const reference (const T&) for input parameters unless copying is cheap (e.g., int).
I.11: Never return a raw pointer (T\*) or reference to a local object. (Prevents dangling pointers.)

Focus: Ensure functions are predictable and safe to use.

## Functions (F)

F.7: For cheap objects (e.g., int), pass by value; for expensive ones, use const T& or smart pointers.
F.15: Prefer simple, short functions over long, complex ones.

Rationale: Smaller functions are easier to test, debug, and optimize.

## Classes and Hierarchies (C)

C.2: Make a class member virtual only if it’s meant to be overridden.
C.9: Minimize exposure of class internals (e.g., prefer private data with accessor functions).
C.122: Use smart pointers (unique_ptr, shared_ptr) instead of raw pointers for ownership.

Focus: Encapsulation and resource management (RAII—Resource Acquisition Is Initialization).

## Resource Management (R)

R.1: Manage resources automatically using RAII (e.g., std::lock_guard for mutexes).
R.11: Avoid explicit new and delete; use make_unique or make_shared.
Rationale: Prevents leaks and simplifies exception safety.

## Expressions and Statements (ES)

ES.10: Declare variables as close to their use as possible (e.g., use auto x = foo(); in a tight scope).
ES.20: Always initialize variables to avoid undefined behavior.

Focus: Reduce bugs from uninitialized data or overly broad scopes.

## Standard Library (SL)

SL.1: Use the Standard Library (e.g., std::vector, std::string) over custom implementations unless there’s a compelling reason.
SL.4: Prefer <algorithm> functions (e.g., std::find, std::sort) over hand-written loops.

Rationale: Leverage battle-tested, optimized tools.

## Concurrency (CP)

CP.1: Think about concurrency early in design (e.g., avoid shared mutable state if possible).
CP.20: Use std::thread, std::mutex, and higher-level constructs like std::async safely.

Focus: Prevent data races and deadlocks in multi-threaded code.

## Notable Features and Recommendations

Modern C++ Emphasis: The Guidelines push for features like:

- auto for type deduction.
- Smart pointers (unique_ptr, shared_ptr) over raw pointers.
- Lambdas and ranges (C++20) for cleaner code.
- Concepts (C++20) for better template constraints.

Tooling Support: Many rules are enforceable via static analysis tools like **Clang-Tidy** or Visual Studio’s C++ Core Checkers, which integrate with the Guidelines.
Exceptions: Allowed but should be used judiciously—prefer RAII and return values for error handling where possible.

## How to Get Started

- Read It: Available online at the official GitHub repo or isocpp.org.
- Tools: Use Clang-Tidy with the -checks=cppcoreguidelines-\* flag to enforce rules.
- Practice: Start with small projects, applying RAII and Standard Library recommendations.
