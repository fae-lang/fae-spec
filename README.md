# Fae Language Spec

## Primitives overview
The most basic unit of data in Fae is the struct. Structs are immutable values that have attributes and values. Even a integer is considered a struct (although its representation inside the runtime's internals may be that of a unboxed int). There are 4 main functions uses with manipulating structs:

#### (vm.struct/has-attr? struct attr)
Returns a truthy value if the given struct contains the attribute. 

Examples:

```clojure
(vm.struct/has-attr? {.foo.bar/baz 42} .foo.bar/baz) => vm.struct/contains-value
```
