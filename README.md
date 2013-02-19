# What is this?

This is meant to be a guide for new code. The guidelines here are intended
to improve the readability of code and make it consistent.

A style gude is about consistency. Consistency with this style guide is
important. Consistency within a project is more important. Consistency
within one module or function is most important.

But most importantly: know when to be inconsistent -- sometimes the
style guide just doesn't apply. When in doubt, use your best judgment.
Look at other examples and decide what looks best. And don't hesitate
to ask!

Two good reasons to break a particular rule:

1. When applying the rule would make the code less readable, even for
   someone who is used to reading code that follows the rules.
2. To be consistent with surrounding code that also breaks it (maybe for
   historic reasons) -- although this is also an opportunity to clean up
   someone else's mess (in true XP style).

_Source: Python's PEP 8, [A Foolish Consistency is the Hobgoblin of Little Minds][pep8]_


These guidelines build on Apple's existing [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html).
Unless explicitly contradicted below, assume that all of Apple's guidelines apply as well.

## Whitespace

 * Spaces, not tabs. Use 4 spaces for indenting.
 * End files with a newline.
 * Make liberal use of vertical whitespace to divide code into logical chunks.

## Documentation and Organization

 * All method declarations should be documented.
 * Comments should be hard-wrapped at 80 characters.
 * Comments should be [Tomdoc](http://tomdoc.org/)-style.
 * Comments should be like this:

```objc
// some comment
// that takes several
// lines

/* or if you have an inline comment */
```

 * Use `#pragma mark`s to categorize methods into functional groupings and protocol implementations, following this general structure:

```objc
#pragma mark Properties

@dynamic someProperty;

- (void)setCustomProperty:(id)value {}

#pragma mark Lifecycle

+ (id)objectWithThing:(id)thing {}
- (id)init {}

#pragma mark Drawing

- (void)drawRect:(CGRect) {}

#pragma mark Another functional grouping

#pragma mark GHSuperclass

- (void)someOverriddenMethod {}

#pragma mark NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark NSObject

- (NSString *)description {}
```

## Imports

 * Sort `#import` and `#include` alphabetically
 * Frameworks first
 * Counterparts second
 * Everything else third

```objc
#import <UIKit/UIKit.h>

#import "MyClass.h"

#import "bar.h"
#import "foo.h"
```

## Declarations

 * Don’t use line breaks in method declarations.
 * Never declare an ivar.
 * If exposing an immutable type for a mutable property is required, write a custom setter that to writes to the auto-generated ivar.
 * Always declare memory-management semantics even on `readonly` properties.
 * Declare properties `readonly` if they are only set once in `-init`.
 * Declare properties `copy` if they return immutable objects and aren't ever mutated in the implementation.
 * Don't use `@synthesize` unless the compiler requires it. Note that optional properties in protocols must be explicitly synthesized in order to exist.
 * Instance variables should be prefixed with an underscore (just like when implicitly synthesized).
 * Don't put a space between an object type and the protocol it conforms to.

```objc
@property (attributes) id<Protocol> object;
@property (nonatomic, strong) NSObject<Protocol> *object;
```

 * C function declarations should have no space before the opening parenthesis, and should be namespaced just like a class.

```objc
void GHAwesomeFunction(BOOL hasSomeArgs);
```

 * Constructors should generally return `instancetype` rather than `id`. Especially class constructors. [More on `instancetype`](http://nshipster.com/instancetype/).
 * Prefer C99 struct initialiser syntax to helper functions (such as `CGRectMake()`).

```objc
  CGRect rect = { .origin.x = 3.0, .origin.y = 12.0, .size.width = 15.0, size.height = 80.0 };
   ```

## Expressions

 * Don't access a property directly unless you're in `-init` or `-dealloc` or a custom setter.
 * Use dot-syntax when invoking idempotent methods, including setters and class methods (like `NSFileManager.defaultManager`).
 * Use object literals, boxed expressions, and subscripting over the older, grosser alternatives.
 * Comparisons should be explicit for everything except `BOOL`s.
 * Prefer positive comparisons to negative.
 * Long form ternary operators should be wrapped in parentheses and only used for assignment and arguments.
 * Use `nil` instead of `NULL`.

```objc
Blah *a = (stuff == thing ? foo : bar);
```

* Short form, `nil` coalescing ternary operators should avoid parentheses.

```objc
Blah *b = thingThatCouldBeNil ?: defaultValue;
```

 * There shouldn't be a space between a cast and the variable being cast.

``` objc
NewType a = (NewType)b;
```

## Control Structures

 * Always surround `if` bodies with curly braces.
 * Never use single-line `if` bodies.
 * All curly braces should begin on the same line as their associated statement, except for method implementations. They should end on a new line.
 * Put a single space after keywords and before their parentheses.
 * Break early.
 * Avoid a return inside a method.
 * No spaces between parentheses and their contents.

```objc
if (shitIsBad) {
	// do stuff
}

if (something == nil) {
	// do stuff
} else {
	// do other stuff
}
```

## Blocks

 * Blocks should have a space between their return type and name.
 * Block definitions should omit their return type when possible.
 * Block definitions should omit their arguments if they are `void`.

```objc
void (^blockName1)(void) = ^{
    // do some things
};

id (^blockName2)(id) = ^ id (id args) {
    // do some things
};
```

## Literals

 * Avoid making numbers a specific type unless necessary (for example, prefer `5` to `5.0`, and `5.3` to `5.3f`).
 * The contents of array and dictionary literals should have a space on both sides.
 * Dictionary literals should have no space between the key and the colon, and a single space between colon and value.

``` objc
NSArray *theShit = @[ @1, @2, @3 ];

NSDictionary *keyedShit = @{ GHDidCreateStyleGuide: @YES };
```

 * Longer or more complex literals should be split over multiple lines (optionally with a terminating comma):

``` objc
NSArray *theShit = @[
    @"Got some long string objects in here.",
    [AndSomeModelObjects too],
    @"Moar strings."
];

NSDictionary *keyedShit = @{
    @"this.key": @"corresponds to this value",
    @"otherKey": @"remoteData.payload",
    @"some": @"more",
    @"JSON": @"keys",
    @"and": @"stuff",
};
```

## Categories

 * Categories should be named for the sort of functionality they provide. Don't create umbrella categories.
 * Category methods should always be prefixed.
 * If you need to expose private methods for subclasses or unit testing, create a class extension named `Class+Private`.


[pep8]: http://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds
