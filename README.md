# Fae Language Spec

## Primitives overview
The most basic unit of data in Fae is the struct. Structs are immutable values that have attributes and values. Even a integer is considered a struct (although its representation inside the runtime's internals may be that of a unboxed int). Some structs may be self-referencing. For example, primitive integers contain a `.vm.int/value` attribute that reffers to the primitive integer value of the integer. This allows for a integer to be given extra attributes and values while still remaining a integer. See the section on polymorphism to learn more about this process. There are 4 main functions used with manipulating structs:

#### Literal struct creation
Structs can be created via a literal format of `{attr1 k1 attr2 k2 ...}`. Symbols that start with a period, are called "attributes" and are used to define names that are stored directly in structs. 

Examples:
```clojure
{.foo.bar/baz 42}

{.shape.rectangle/width 10
 .shape.rectangle/height 5}
 
{.shape.circle/radius 20}
```

#### (vm.struct/has-attr? struct attr)
Returns a truthy value if the given struct contains the attribute. 

Examples:

```clojure
(vm.struct/has-attr? {.foo.bar/baz 42} .foo.bar/baz) => vm.struct/attr-found

(vm.struct/has-attr? {} .shape.circle/radius) => vm.struct/attr-not-found
```

#### (vm.struct/list-attrs struct attr)
Returns a hashset of the attributes found on this struct.

Examples:

```clojure
(vm.struct/list-attrs {.foo.bar/baz 42}) => #{.foo.bar/baz}
(vm.struct/list-attrs 42) => #{.vm.int/value}
```

#### (vm.struct/get-attr struct attr)
Gets the value from a struct associated with a given attribute. If the attribute is not defined in the struct an exception effect is thrown.

Examples:

```clojure
(vm.struct/get-attr {.foo.bar/baz 42} .foo.bar/baz) => 42
(vm.struct/get-attr 42 .vm.int/value) => 42

(vm.struct/get-attr 42 .foo.bar/baz) throws=> {.vm.effect/type :vm.struct/attr-not-found
                                               .vm.error.attr-not-found/attr .foo.bar/baz
                                               .vm.error.attr-not-found/struct 42}
```

#### (vm.struct/with struct attr value)
Returns a new struct with the given attr/value pair added to the struct. 

```clojure
(vm.struct/with {} .foo.bar/baz 42) => {.foo.bar/baz 42}
(vm.struct/with {.shape.circle/radius 10} .shape.circle/radius 5) => {.shape.circle/radius 5}
(vm.struct/with 33 .meta.file/line 33) => 33

```
