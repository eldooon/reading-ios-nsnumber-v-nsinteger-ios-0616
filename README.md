# `NSNumber`

## Objectives

1. Use `NSNumber` to provide an object "wrapper" for primitive values so they can be stored in a collection.
2. Use the `NSNumber` literal `@()` to store values, variables, and the results of operations.
3. Convert `NSNumber` object to primitive values and strings.
4. Review common error messages when working with `NSNumber`.
5. Use `isEqualToNumber:` to compare `NSNumber` objects directly.
6. Convert an `NSNumber` to its primitive value in order to use a comparison operator.

## NSNumber

Since Objective-C primitives are **not** objects they cannot be stored in collections such as an array. In fact, the compiler will complain if you even try. 

The primary purpose of `NSNumber` is to provide a way to store primitives as objects. It is an object-type "wrapper" that bundles the primitive data so it can be placed into a collection. The drawback, however, is that a primitive stored in an `NSNumber` cannot be directly modified, and information about the original type of primitive type can be lost once it's converted.

For this reason, **it's best to only use `NSNumber` for storing primitives in collections.**

### The `NSNumber` Literal

The literal syntax for defining an `NSNumber` is `@()`. However, when an `NSNumber` is defined directly to a value instead of to a variable, the round brackets `(``)` can be omitted. If this is the case, they are said to be "implicit". Most of the time that you'll be defining an `NSNumber`, you can use just the `@` ("at symbol") by itself: 

```objc
NSNumber *negSeven = @-7;
NSNumber *eight = @8;
NSNumber *pi = @3.14159265359;
```

The boolean values `YES` and `NO` can also be converted to `NSNumber`:

```objc
NSNumber *isYes = @YES;
NSNumber *isNo = @NO;
```
This is possible because `BOOL` is actually just another *typedef* like `NSInteger` and `NSUInteger` (`CGFloat`, however, is a little bit more complex). A "typedef" is a way of reading a binary word; it acts a kind of alias. What this means is that the boolean value `YES` is actually just another way of reading a `1`, and the boolean value `NO` is another way of reading a `0`. So to `NSNumber`, `@YES` is the same as saying `@1`, and `@NO` is the same as saying `@0`. When writing code, however, it is useful to use the boolean values as a signal to yourself and to other developers that the `NSNumber`'s value is meant to contain a boolean.

There *are* two cases in which you'll need to use the full literal syntax: (1.) converting an existing variable, and (2.) converting the result of an operation.

#### Converting A Variable

Each of the above `NSNumber` values can be converted from a primitive variable already defined:

```objc
NSInteger negSevenInt = -7;
NSUInteger eightUInt = 8;
CGFloat piFloat = 3.14159265359;
BOOL isYesBOOL = YES;

NSNumber *negSeven = @(negSevenInt);
NSNumber *eight = @(eightUInt);
NSNumber *pi = @(piFloat);
NSNumber *isYes = @(isYesBOOL);

NSLog(@"%@, %@, %@, %@", negSeven, eight, pi, isYes);
```
This will print: `-7, 8, 3.1419265359, 1`.

#### Conversion Methods

Other than the initializer methods which are generally handled by the literal syntax, the methods on `NSNumber` are primarily conversion methods. The most common ones that you'll use are:

* `integerValue` — converts to an `NSInteger` value
* `unsignedIntegerValue` — converts to an `NSUInteger` value
* `floatValue` — converts to a `CGFloat` value
* `boolValue` — converts to a `BOOL` value
* `stringValue` — converts to an `NSString` object.

We can use the conversion methods to transform our `NSNumber` objects back to primitives that can be manipulated:

```objc
negSevenInt = [negSeven integerValue];
eightUInt = [eight unsignedIntegerValue];
piFloat = [pi floatValue];
isYesBOOL = [isYes boolValue];
    
NSLog(@"%li, %lu, %f, %d", negSevenInt, eightUInt, piFloat, isYesBOOL);
```
This will print: `-7, 8, 3.141593, 1`.

#### Converting An Operation

As we mentioned above, an `NSNumber` can't participate directly in a mathematical operation. However, it *can* directly capture the result of an operation which is wrapped in the literal syntax:

```objc
NSNumber *sum = @(1 + 1);
NSNumber *product = @(piFloat * 2);
NSNumber *difference = @(eightUInt - negSevenInt);
NSNumber *quotient = @(1.618 / piFloat);
```
The round brackets can contain an equation of multiple operations, but generally this will make your code difficult to read and we consider it best practice to avoid using `NSNumber` whenever possible, except for storing values in collections.

### Common Errors

The compiler should complain if you attempt to use `NSNumber` incorrectly. Here are some errors that you might see—

1) When attempting an operation with two `NSNumber` objects:

```objc
NSNumber *sum = @2 + @3;
// Semantic Issue: Invalid operands to binary expression ('NSNumber *' and 'NSNumber *')
```
2) When attempting an operation with one `NSNumber` object:

```objc 
NSNumber *sum = 2 + @3; 
// Semantic Issue: Arithmetic on pointer to interface 'NSNumber', which is not a constant size for this architecture and platform
```
3) When attempting to insert primitives into a collection, such as an array:

```objc
NSArray *primes = @[1, 2, 3, 5, 7, 11, 13]; 
// Semantic Issue: Numeric literal must be prefixed by '@' in a collection
```

#### Comparing `NSNumber`s

The `isEqualToNumber:` method works similarly for comparing the equivalence of two `NSNumber`s as the `isEqualToString:` method does for two `NSString`s.

```objc
NSNumber *eight = @8;

if ([eight isEqualToNumber:@8]) {
    NSLog(@"%@ is equal to 8", eight);
} else {
    NSLog(@"%@ is not equal to 8", eight);
}
```
This will print: `8 is equal to 8`.

To use a comparison operator, the `NSNumber` must first be converted to a primitive. This can be done using a variable in an intermediate step, or by directly evaluated the return of the appropriate conversion method:

```objc
// intermediate variable

NSNumber *fortyTwo = @42;
NSInteger fortyTwoInt = [fortyTwo integerValue];

if (fortyTwoInt > 40) {
    NSLog(@"fortyTwo is greater than 40");
} else {
    NSLog(@"fortyTwo is not greater than 40");
}
```

```objc
// direct evaluation of conversion method

NSNumber *fortyTwo = @42;

if ([fortyTwo integerValue] > 40) {
    NSLog(@"fortyTwo is greater than 40");
} else {
    NSLog(@"fortyTwo is not greater than 40");
}
```
Both of these will print: `fortyTwo is greater than 40`.
<p data-visibility='hidden'>View <a href='https://learn.co/lessons/reading-ios-nsnumber-v-nsinteger' title='NSNumber'>NSNumber</a> on Learn.co and start learning to code for free.</p>
