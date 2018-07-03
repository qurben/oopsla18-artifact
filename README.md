# Artifact for OOPSLA'18 Submission 'Scopes as Types'

## Artifact Description

This is the artifact accompanying the OOPSLA'18 submission 'Scopes as
Types'. This artifact consists of the following:

- The paper draft in `scopes-as-types.pdf`, which is updated to match
  the artifact. Note that the paper submitted as part of the HotCRP
  submission, is the original draft, which may deviate in places from
  this artifact.

- An implementation of the Statix language, embedded in the Spoofax
  language workbench.

- Language implementations for the three case studies in the
  paper. Each implementation is fully operational and includes a
  static semantics definition in Statix, some example programs, and a
  test suite to test the static semantics.

## Getting Started

This section explains how to get the artifact running and what can be
inspected. The artifact consists of a Spoofax Eclipse workspace with
language implementation projects for the case studies presented in the
paper. The static semantics of these languages are defined in Statix,
the constraint language defined in the paper. First we explain how to
get Spoofax, and build the language projects. Then we explain the
structure of the language projects, and what can be inspected and
tested.

### Getting Spoofax and Building the Language Projects

The case studies are implemented in the Spoofax language workbench,
which supports Statix is one of its meta-languages.

- Download the appropriate Spoofax archive for your platform from
  https://buildfarm.metaborg.org/job/metaborg/job/spoofax-releng/job/master/661/artifact/dist/spoofax/eclipse/. The
  `spoofax-*-jre.*` archives include a JRE, and do not depend on a
  compatible local Java installation. It is recommended to use a
  version with an included JRE.

- Install and run Spoofax by unpacking the archive, and starting
  `eclipse` or `eclipse.exe`. When Spoofax is started, it will ask for
  a workspace; select the artifact root directory. Workspaces can be
  changed using the `File > Switch Workspace` menu, if necessary.
  
- Build all language projects using the `Project > Build All`
  menu. After a successful language build, the console shows something
  like:
  
      Reloading language project eclipse:///lang.sysf

### Inspecting the Language Projects

The workspace contains three language projects, for the three case
studies: the Simply Typed Lambda Calculus with Structural Records in
`lang.stlcrec`, System F in `lang.sysf`, and Featherweight Generic
Java in `lang.fgj`. Each project contains a Statix specification of
the static semantics, some example programs, and a test suite.

- The Statix specification of a language is found in the file
  `trans/statics.stx`. The file can be opened in an editor by
  double-clicking.

- Example programs can be found in the `example` folder of a language
  project. Double-clicking opens an editor and starts type checking
  for the file, using the language's Statix specification. If the file
  contains errors, a red marker appears at the top of the
  editor. Depending on the size of the program, type checking can take
  some time. Only after type checking is done will the error marker be
  updated. When done, the console shows a status line similar to:

      Solved 415 constraints (103 delays) with 0 failed and 0 remaining constraint(s).

  If the number of failed or remaining constraints is not zero, the
  program did not successfully type check.

- Every language projects includes a test suite for the static
  semantics of the language, located in the `test` directory. Test
  files have a `.spt` extension, and can be opened and inspected in an
  editor by double-clicking. Failing tests are marked with a red
  marker. If a file contains many tests, it may take a while before
  the tests are finished and the success and failures are correctly
  marked. Use the console or the progress window to check if the tests
  are still running.

- All tests in a directory can be run by selected the directory in the
  `Package Explorer`, and selecting the `Spoofax (meta) > Run all
  selected tests` menu. A test runner will appear listing all tests,
  showing progress and which tests success or failure. Running all
  tests (especially for FGJ) takes quite a while. For faster results,
  open individual files, or run fewer tests by selecting
  subdirectories of the `test` directory.

## Claims Supported by the Artifact

This artifact supports the following claims from the paper:

> "We show that viewing scopes as types enables us to model the
> internal structure of types in a range of non-simple type systems
> (including structural records and generic classes) using the generic
> representation of scopes. Further, we show that relations between
> such types can be expressed in terms of generalized scope graph
> queries. We extend scope graphs with scoped relations and queries."

The specifications of all three languages model non-syntactic aspects
of types, such as record and class structure, or type variables and
lazy substitution, using the scope graph model.

> "We extend the scope graph model with scoped relations to model the
> association of types with declarations and explicit substitutions in
> the instantiation of parameterized types. We generalize name
> resolution in scope graph from resolution from references to
> declarations to general queries for scoped relations. This enables
> flexible definition of queries for reachable or visible declarations
> and other properties, such as the visible record fields in the
> definition of subtyping of structural record types."

This extended model and a resolution algorithm are implemented as part
of the Statix implementation. A variety of queries are used
through-out the Statix specifications of the case studies.

> "We introduce Statix, a declarative, language for specifying type
> systems. The language provides simple guarded rules for definition
> of user-defined constraints with unification and scope graph
> construction and resolution as built-in theories. We provide a
> declarative and an operational semantics of Statix."

The Statix language, including a type checker and a solver, are
implemented and included as part of Spoofax.

> "We simplify the resolution calculus and algorithm of Néron et
> al. [2015a] and Van Antwerpen et al. [2016] by not including imports
> as a primitive. We demonstrate how imports (and other name- and
> type-dependent name resolution schemas) can be encoded using the
> scopes as types approach. We discuss how these patterns depend on
> resolution in incomplete scope graphs, and how the algorithm
> guarantees soundness of resolution in incomplete graphs. We further
> generalize resolution by namespace/query-specific parameterization
> with visibility policies instead of global policies."

This specs implement binding patterns -- such as class inheritance --
using the simplified model that is part of Statix. In earlier work,
these patterns were implemented using the import mechanism. Global
resolution policies, for example how to resolve methods or variables
in FGJ, are provided in the Statix language for convenience, but every
query can redefine every parameter of the resolution calculus.

> "We have evaluated the Statix language in three case studies: the
> simply-typed lambda calculus with records [Pierce 2002] (STLC-REC),
> System F [Girard 1972; Reynolds 1974], and Featherweight Generic
> Java [Igarashi et al. 2001]."

The artifact contains fully functional language implementations of the
case studies. The test suites give us trust in the correctness of the
given specifications.

The following claims from the paper are not supported by this artifact:

> "We extend the visual notation of scope graph diagrams with scoped
> relations, which provides a useful language for explaining patterns
> of names and types in programming languages. We also extend the
> visual notation with unresolved (constraint) nodes for illustrating
> the resolution process."

The visual notation is not part of Statix or of its output.

## Case study languages

### General remarks:

- Focus is on expressivity and correctness

- Not on performance

- Not on good error messages

### STLC+Rec

- STLC, let-expressions

- Records with structural subtyping

- Record extension

- Type let binders

### System F

- STLC, let-expressions

- Type binders (`Fun`)

- Type let binders

### FGJ

- Classes

- Generics

Differences from FGJ:

- Constructor arguments do not have to match fields.

- Fields can be initialized with arbitrary expressions, instead of just variable references.

- No check that every field is initialized.

## Evaluation Instructions

- review tests suites on success and coverage

- write own tests (in example programs or SPT files)

- tinker with the specs (advanced?!)
