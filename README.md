# Primitives & NSNumber

## Objectives

1. Get introduced to the difference between primitives and objects, and which requires a `*` in declarations.
2. Learn the four most common primitive data types and what distinguishes them.
3. Know when to employ `NSNumber` for holding values and what distinguishes it from primitives.
4. Review common error messages when working with `NSNumber`.

## Primitives

You may have noticed in previous examples that `NSString` variables are declared with a `*` ("star" or "asterisk") while `NSInteger` variables are declared without one. This is because `NSString` is an object type (we'll cover in depth what this means in the next topic). Objects require a pointer reference (the `*`) in their declaration because, well, they point to a set of values at a specific memory address in the computer's RAM. However, `NSInteger` is a primitive, or "data type", and is itself a value, rather than a pointer to a value or set of values.

**Note:** *Entering* `NSInteger *` *will cause the compiler to generate an error. The same goes for declaring any of the data types.*

While you can reference the complete list, the primitives you'll interect the most are: with which you'll interact the most are:

```objc
NSInteger
NSUInteger
CGFloat
BOOL
```
A great reference on the more complete list of primitives can be found in the documentation on Apple's [Foundation Data Types](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Miscellaneous/Foundation_DataTypes/index.html#//apple_ref/doc/c_ref/NSTimeInterval).

### `NSInteger`—neither `int` nor `long`

These Objective-C types get treated at run time as either 32-bit or 64-bit C data types depending on the current device. This is important because iPhone models up to the 4S ran on 32-bit processors.

| Data Type  | Description |
|:----------:|:------------|
|`NSInteger` | `int` (32-bit), `long` (64-bit) |
|`NSUInteger`| `unsigned int` (32-bit), `unsigned long` (64-bit) |
|`CGFloat`   | `float` (32-bit), `double` (64-bit) |
|`BOOL`      | Typedef: YES = 1, NO = 0 |

### Signed vs. Unsigned Integers



http://en.wikipedia.org/wiki/Integer_(computer_science)


====Mark====

When we work with integers, we typically use the `NSInteger` datatype.
NSInteger is awesome and we can do all the math we want with it. 

```objc
NSInteger x = 2;
NSInteger y = 3;
NSInteger z = x + y; // z = 5
```

However, NSInteger has some limitations; *we can't add NSIntegers to NSArrays or NSDictionaries as those can only hold objects*. How can we hold numbers in an array or a dictionary then? 

The answer to that is pretty easy. We turn an NSInteger into an NSNumber, which is an object. 

Awesome! So now we can have an array of NSNumbers or a dictionary with NSNumbers as keys or values.

```objc
NSInteger x = 2;
NSInteger y = 3;
NSNumber *objectX = [NSNumber numberWithInteger:x];
NSNumber *objectY = [NSNumber numberWithInteger:y];
NSArray *numberArray = @[objectX, objectY]; // [@2, @3]
```

Because NSNumbers are objects, we can create them the same way you would create any object. Or we can use a convenience method shortcut. Or we can use object literals
.

```objc
// normal object creation
NSNumber *objectX = [NSNumber alloc] initWithInteger:2];

// convenience method
NSNumber *objectY = [NSNumber numberWithInteger:3];

// object literals
NSNumber *objectZ = @5;
NSNumber *objectQ = @(-15);   // You'll need parentheses for negative values...
NSNumber *objectT = @(5 - 2); // and expressions.
```

Now we know how to create NSNumbers. And we know that NSNumbers can be used in Objective-C collections like dictionaries and arrays. 

If NSNumbers are so great, why don't we just use them all the time?

*We can't use them for mathmatical operations, that's why.*

When we want to do math with NSNumbers, we have to use methods to get an NSInteger value (in the case of integers).

```objc
NSInteger integerX = [@2 integerValue];
NSInteger integerY = [@3 integerValue];
NSInteger sum = [integerX + integerY]; // sum = 5
```

####Error messages to look out for:

```objc
NSNumber *sum = @2 + @3; // Semantic Issue: Invalid operands to binary expression ('NSNumber *' and 'NSNumber *')
NSNumber *sum = 2 + @3; // Semantic Issue: Arithmetic on pointer to interface 'NSNumber', which is not a constant size for this architecture and platform
NSArray *numbers = @[2, 3]; // Semantic Issue: Numeric literal must be prefixed by '@' in a collection
NSDictionary *numbers = @{@2: 3}; // Semantic Issue: Numeric literal must be prefixed by '@' in a collection
```

####A Note on NSNumber:
NSNumber is a wrapper that can encapsulate a variety of data types (NSInteger among them). This means we can put float values into an NSNumber, and ints and doubles and any other kind of scalar value.
