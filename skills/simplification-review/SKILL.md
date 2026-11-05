---
name: simplification-review
description: Rank simplifications in supported scope, concepts, file structure, and implementation. Use when assessing overengineering, fragmented modules, or what can be consolidated or removed.
---

# Simplification review

Produce evidence-backed simplification options. Review only unless implementation is requested.

## Vocabulary

**Module**: anything with an interface and an implementation — a function, class, package, or tier-spanning slice. _Avoid_: unit, component, service.

**Interface**: everything a caller must know to use the module correctly: the type signature, but also invariants, ordering constraints, error modes, required configuration, and performance characteristics.

**Leverage**: what callers get from a module. More capability per unit of interface they learn.

**Locality**: what maintainers get from a module. Change, bugs, knowledge, and verification concentrate in one place rather than spreading across callers. Fix once, fixed everywhere.

**Shallow module**: large interface, thin implementation — the opposite of a module that earns its keep.

**The deletion test.** Imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across N callers, it was earning its keep.

**Pass-through**: an interface that adds nothing — callers could reach the behaviour directly.

## Process

1. **Scope.** State the core behaviour and guarantees to preserve from the request and relevant specifications. Treat simplicity claims and line budgets as hypotheses to investigate.

2. **Trace.** Follow the actual workflow from inputs through execution to outputs. Inspect callers, configuration, and tests for each candidate. Search for unused interfaces, pass-throughs, duplicated state or work, lifecycle constraints, speculative extensibility, compatibility scaffolding, and tests pinning implementation details. Record what each mechanism currently enables or protects; qualify uncertain dynamic or external usage.

   Inspect the file tree alongside dependency and workflow traces. Look for responsibilities scattered across technical-layer directories, files repeatedly read or changed together, one-consumer helpers separated from their owner, redundant nesting, and re-export chains without a useful interface. Identify the cohesive module each proposed grouping would create.

3. **Challenge.** Apply the deletion test to every candidate. Prefer changes that remove concepts and caller obligations while retaining leverage and locality. Classify each finding by one primary **kind**, in priority order:
   1. **Scope reduction:** remove optional features, configuration modes, compatibility paths, or guarantees.
   2. **Conceptual simplification:** replace competing terminology, concepts, or representations with one coherent model.
   3. **Structural simplification:** colocate related files, collapse unnecessary directory layers, or consolidate fragmented modules.
   4. **Implementation simplification:** remove pass-throughs, duplicate state, or unnecessary machinery.

   Add a secondary kind only when independently useful. Separately label every applicable **impact**:
   - **Behaviour-preserving:** supported behaviour and contracts remain intact.
   - **Breaking change:** callers, configuration, or persisted data require migration.
   - **Capability removal:** supported functionality disappears.
   - **Guarantee reduction:** safety, correctness, or operational protection weakens.

   Prefer layouts that reduce navigation, knowledge scattering, and cross-module coordination. Fewer files is not itself a simplification: preserve useful ownership, independent change, and test seams. Check public exports, imports, path-based discovery, and tooling before classifying file moves as behaviour-preserving.

   Mark conflicts with documented requirements. When a feature's value depends on actual use, make the recommendation conditional. Judge dispensability by present needs and obligations, not implementation length; useful abstractions may be worth keeping.

4. **Rank.** Group findings by primary kind in priority order: scope reduction, conceptual simplification, structural simplification, then implementation simplification. Within each group, rank by decreasing superfluousness, including navigation and coordination burden, not estimated line savings. Priority determines presentation, not permission to discard required behaviour or guarantees. Each item includes a file/symbol reference, kind, impact labels, concrete evidence, the smallest change, the complexity removed, and how to validate retained behaviour and contracts. Label speculative findings explicitly; report only supported candidates rather than filling a quota. Omit empty groups.

Close with what earns its keep and the smallest plausible remaining workflow. Qualify size estimates by their scope and retained guarantees.
