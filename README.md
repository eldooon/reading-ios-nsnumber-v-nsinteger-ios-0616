# Primitives & NSNumber

## Objectives

1. Get introduced to the difference between primitives and objects, and which requires a `*` in declarations.
2. Learn the four most common primitive data types and what distinguishes them.
3. Know when to employ `NSNumber` for holding values and what distinguishes it from primitives.
4. Review common error messages when working with `NSNumber`.

## Primitives

You may have noticed in previous examples that `NSString` variables are declared with a `*` ("star" or "asterisk") while `NSInteger` variables are declared without one. This is because `NSString` is an object type (we'll cover in depth what this means in the next topic). Objects require a pointer reference (the `*`) in their declaration because, well, they point to a set of values at a specific memory address in the computer's RAM. However, `NSInteger` is a primitive, or "data type", and is itself a value, rather than a pointer to a value or set of values.

**Note:** *Entering* `NSInteger *` *will cause the compiler to generate an* `invalid integer to pointer conversion` *warning. The same goes for declaring any of the data types. This syntax does serve a purpose so the language permits it, but your application won't do what you expect.*

While you can reference the complete list of , the primitives with which you'll interact the most are:

```objc
NSInteger
NSUInteger
CGFloat
BOOL
```
A great reference on the more complete list of primitives can be found in the documentation on Apple's [Foundation Data Types](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Miscellaneous/Foundation_DataTypes/index.html#//apple_ref/doc/c_ref/NSTimeInterval).

### `NSInteger`—neither `int` nor `long`

These Objective-C types get treated at run time as either 32-bit or 64-bit C data types depending on the current device. This is important because the first mobile CPU to run a true 64-bit architecture in a smartphone is the [A7 chip](http://en.wikipedia.org/wiki/Apple_A7) in the iPhone 5S and iPad Mini 3, first released in September 2013.

| Data Type  | Description             | 64-bit            |
|:----------:|:------------------------|:------------------|
|`NSInteger` | `int` (32-bit)          | `long` (64-bit)   |
|`NSUInteger`| `unsigned int` (32-bit) | `unsigned long` (64-bit) |
|`CGFloat`   | `float` (32-bit)        | `double` (64-bit) |
|`BOOL`      | Typedef: YES = 1, NO = 0 |

### Signed vs. Unsigned Integers

You've probably been wondering what this distinction between signed and unsigned integers means. The most basic explanation is that signed integers can handle values less than zero (negative numbers), while unsigned integers can only be positive. The way signed integers accomplish this is by taking the top half of their available range and using it to count down from -1.

For numbers less than about two billion (2 147 483 647) in Objective-C, incorrectly casting integers to the wrong sign won't cause any problems, but it's good practice to be aware of the distinction. Generally, signed integers should only be used when a value can be below zero, such as a coordinate.

**Advanced:** *Signed integers use their first bit as a* `+` *or* `-` *flag. So on the lower half of the number range, signed and unsigned will convert to the same value. However, an unsigned integer in the top half of the range, if read improperly as a signed integer, will return a negative value. [Further Reading](http://en.wikipedia.org/wiki/Integer_(computer_science))*

### Floating Point Values

In programming, **floating point** values are used to represent decimal values. Objective-C handles floating points with `CGFloat`. If you were to try setting an `NSInteger` to a decimal value, you will lose all of the information following the decimal point.

```objc
NSInteger piInt = 3.14159265359;
NSLog(@"%li", piInt);
```
This will print: `3`.

However, using `CGFloat` to store the value of *pi* will retain the decimal value:

```objc
CGFloat pi = 3.14159265359;
NSLog(@"%f", pi);
```
This will print: `3.141593`.

Notice how it only printed to six decimal places? This has to do with the default setting of the `%f` format specifier, and doesn't reflect the precision of the `CGFloat` value itself. We can override the default length using `%.nf` where `n` is the number of decimal points we wish to see.

```objc
CGFloat pi = 3.14159265359;
NSLog(@"%.12f", pi);
```
This will print: `3.141592653590` since we only set *pi* to the eleventh decimal point.

#### Floating Point Arithmetic

It's important to know that integer values **do not** automatically become floating point values even if you declare the result of a fraction like this:

```objc
CGFloat elevenNinths = 11/9;
NSLog(@"%f", elevenNinths);
```
This will print: `1`.

But why did it not print `1.222222`? Well, that's because dividing (`/`) integer values can only result in another integer. However, if any part of an arithmetic operation involves a floating point value, the result will evaluate to a floating point value as well.

```objc
CGFloat elevenNinths = 11/9.0; // or 11.0/9.0 or 11.0/9
NSLog(@"%f", elevenNinths);
```
This will print: `1.222222`, just like we wanted.

### Booleans

A **Boolean** (typed `BOOL` in Objective-C) is a true or false (`YES` or `NO`) value. They are most useful in conditional (`if`) statements which we'll cover in the next reading.

You can set a `BOOL` value like this:

```objc
BOOL isTrue = YES; // bit value 1, or anything non-zero, or non-"nil"
BOOL isFalse = NO; // bit value 0, exactly zero, or "nil"

NSLog(@"yes = %d, no = %d", isTrue, isFalse);
```
This will print: `yes = 1, no = 0`.

**Top Tip:** *The C language type is* `bool` *(all-lowercase). Use* `BOOL` *(all uppercase) when programming in Objective-C.*

## NSNumber

Since Objective-C primitives are **not** objects there are certain things that can't be done with them—namely storing them in sets like arrays and dictionaries (the compiler will complain if you try  this).

The purpose of `NSNumber` is provide a way to store primitives as objects. It as an object-type "wrapper" that bundles the primitive data so it can be placed into a set. The drawback, however, is that information stored in an `NSNumber` cannot be directly modified, and any information wrapped in an `NSNumber` loses the specificity of its origin type.

For this reason, it's best to only use `NSNumber` for storing primitives in sets.

### The `NSNumber` Literal

The literal syntax for defining an `NSNumber` is `@()`. Each of the above primitive types can be defined as an `NSNumber` using the literal syntax:

```objc
NSInteger negSeven = -7;
NSUInteger eight = 8;
CGFloat pi = 3.14159265359;
BOOL isTrue = YES;

NSNumber *negSevenNumber = @(negSeven);
NSNumber *eightNumber = @(eight);
NSNumber *piNumber = @(pi);
NSNumber *isTrueNumber = @(isTrue);

NSLog(@"%@, %@, %@, %@", negSevenNumber, eightNumber, piNumber, isTrueNumber);
```
This will print: `-7, 8, 3.1419265359, 1, 29`.

##### Implicit Literal Syntax

When an `NSNumber` is defined directly to a value instead of to a variable, the parentheses `(``)` can be omitted. If this is the case, they are said to be "implicit".

```objc
NSNumber *negSeven = @-7;
NSNumber *eight = @8;
NSNumber *pi = @3.14159265359;
NSNumber *isTrue = @YES;
NSNumber *isFalse = @NO;
```

### Inability to Operate

If we can't store primitives into sets, then why do we use them at all? Well, it's because `NSNumber` can't do arithmetic.

```objc
NSNumber *sum = @1 + @1;
```
This results in a compiler error. However, arithmetic operations **can** be placed within the parenthetical:

```objc
NSNumber *sum = @(1 + 1);
```

### Common Errors

Here are some errors you might encounter when using `NSNumber` incorrectly:

```objc
NSNumber *sum = @2 + @3;
// Semantic Issue: Invalid operands to binary expression ('NSNumber *' and 'NSNumber *')
```
```objc 
NSNumber *sum = 2 + @3; 
// Semantic Issue: Arithmetic on pointer to interface 'NSNumber', which is not a constant size for this architecture and platform
```
```objc
NSArray *numbers = @[2, 3]; 
// Semantic Issue: Numeric literal must be prefixed by '@' in a collection
```
```objc
NSDictionary *numbers = @{@2: 3}; 
// Semantic Issue: Numeric literal must be prefixed by '@' in a collection
```

### Methods

Other than the initializer methods which are generally handled by the literal syntax, the methods on `NSNumber` are primarily conversion methods. The most common ones that you'll use are:

* `integerValue` - converts to an `NSInteger`
* `floatValue` - converts to a `CGFloat`
* `boolValue` - converts to a `BOOL`
* `stringValue` - converts to an `NSString`

The `isEqualToNumber:` method works similarly for comparing the equivalence of two `NSNumber`s as the `isEqualToString:` method does for two `NSString`s.

## Unsigning the Negative Sign

As a curiosity, let's see what happens when we incorrectly read a negative signed integer as an unsigned integer:

```objc
NSInteger negOne = -1;
NSNumber *negOneNum = @(negOne);
NSUInteger negOneUInt = [negOneNum integerValue];
    
NSLog(@"%lu", negOneUInt);
```

This prints: `18 446 744 073 709 551 615` (without spaces). 

That's the 64-bit decimal value of `-1`. We get the same problem if  we use `NSNumber`'s `unsignedIntegerValue` method instead, even if we capture the return with an `NSInteger`.

```objc
NSInteger negOne = -1;
NSNumber *negOneNum = @(negOne);
NSInteger negOneInt = [negOneNum unsignedIntegerValue];
    
NSLog(@"%lu", negOneInt);
```
Mix-ups like this are pretty rare, but if you see a value at run time in the 18-quintillion range, it's probably just the result of a sign conversion error.


