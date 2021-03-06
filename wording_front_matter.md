% A Unified Executors Proposal for C++ | P0443R7

----------------    -------------------------------------
Title:              A Unified Executors Proposal for C++

Authors:            Jared Hoberock, jhoberock@nvidia.com

                    Michael Garland, mgarland@nvidia.com

                    Chris Kohlhoff, chris@kohlhoff.com

                    Chris Mysen, mysen@google.com

                    Carter Edwards, hcedwar@sandia.gov

                    Gordon Brown, gordon@codeplay.com

Other Contributors: Hans Boehm, hboehm@google.com

                    Thomas Heller, thom.heller@gmail.com

                    Lee Howes, lwh@fb.com

                    Bryce Lelbach, brycelelbach@gmail.com

                    Hartmut Kaiser, hartmut.kaiser@gmail.com

                    Bryce Lelbach, brycelelbach@gmail.com

                    Gor Nishanov, gorn@microsoft.com

                    Thomas Rodgers, rodgert@twrodgers.com

                    David Hollman, dshollm@sandia.gov

                    Michael Wong, michael@codeplay.com

Document Number:    P0443R7

Date:               2018-05-07

Audience:           SG1 - Concurrency and Parallelism, LEWG

Reply-to:           sg1-exec@googlegroups.com

Abstract:           This paper proposes a programming model for executors, which are modular components for creating execution. The design of this proposal is described in paper [P0761](https://wg21.link/P0761).

------------------------------------------------------

## Changelog

### Revision 7

Revision 7 of this proposal corrects wording bugs discovered by the authors after Revision 6's publication.

* Enhanced `static_query_v` to result in a default property value for executors which do not provide a `query` function for the property of interest
* Revise `then_execute` and `bulk_then_execute`'s operational semantics to allow user functions to handle incoming exceptions thrown by preceding execution agents
* Introduce `exception_arg` to disambiguate the user function's exceptional overload from its nonexceptional overload in `then_execute` and `bulk_then_execute`

### Revision 6

Revision 6 of this proposal corrects bugs and omissions discovered by the authors after Revision 5's publication, and introduces an enhancement improving the safety of the design.

* Enforce mutual exclusion of behavioral properties via the type system instead of via convention
* Introduce missing `execution::require` adaptations
* Allow executors to opt-out of invoking factory functions when appropriate
* Various bug fixes and corrections

### Revision 5

Revision 5 of this proposal responds to feedback requested during the 2017 Albuquerque ISO C++ Standards Committee meeting and introduces changes which allow properties to better interoperate with polymorphic executor wrappers and also simplify `execution::require`'s behavior.

* Defined general property type requirements
* Elaborated specification of standard property types
* Simplified `execution::require`'s specification
* Enhanced polymorphic executor wrapper
    * Templatized `execution::executor<SupportableProperties...>`
    * Introduced `prefer_only` property adaptor
* Responded to Albuquerque feedback
    * From SG1
        * Execution contexts are now optional properties of executors
        * Eliminated ill-specified caller-agent forward progress properties
        * Elaborated `Future`'s requirements to incorporate forward progress
        * Reworded operational semantics of execution functions to use similar language as the blocking properties
        * Elaborated `static_thread_pool`'s specification to guarantee that threads in the bool boost-block their work
        * Elaborated operational semantics of execution functions to note that forward progress guarantees are specific to the concrete executor type
    * From LEWG
        * Eliminated named `BaseExecutor` concept
        * Simplified general executor requirements
        * Enhanced the `OneWayExecutor` introductory paragraph
        * Eliminated `has_*_member` type traits
* Minor changes
    * Renamed TS namespace from `concurrency_v2` to `executors_v1`
    * Introduced `static_query_v` enabling static queries
    * Eliminated unused `property_value` trait
    * Eliminated the names `allocator_wrapper_t` and `default_allocator`

### Revision 4

* Specified the guarantees implied by `bulk_sequenced_execution`, `bulk_parallel_execution`, and `bulk_unsequenced_execution`

### Revision 3

* Introduced `execution::query()` for executor property introspection
* Simplified the design of `execution::prefer()`
* `oneway`, `twoway`, `single`, and `bulk` are now `require()`-only properties
* Introduced properties allowing executors to opt into adaptations that add blocking semantics
* Introduced properties describing the forward progress relationship between caller and agents
* Various minor improvements to existing functionality based on prototyping

### Revision 2

* Separated wording from explanatory prose, now contained in paper [P0761](https://wg21.link/P0761)
* Applied the simplification proposed by paper [P0688](https://wg21.link/P0688)

### Revision 1

* Executor category simplification
* Specified executor customization points in detail
* Introduced new fine-grained executor type traits
    * Detectors for execution functions
    * Traits for introspecting cross-cutting concerns
        * Introspection of mapping of agents to threads
        * Introspection of execution function blocking behavior
* Allocator support for single agent execution functions
* Renamed `thread_pool` to `static_thread_pool`
* New introduction

### Revision 0

* Initial design

# Proposed Wording

