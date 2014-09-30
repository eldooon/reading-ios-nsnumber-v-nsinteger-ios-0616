---
tags: nsnumber, nsinteger, objects, collections
languages: objc
---

# NSNumber v NSInteger
When we work with integers, we typically use the `NSInteger` datatype.
NSInteger is awesome and we can do all the math we want with it. 

```objc
NSInteger x = 2;
NSInteger y = 3;
NSInteger z = x + y; // z = 5
```

However, NSInteger has some limitations; *we can't add NSIntegers to NSArrays or
NSDictionaries as those can only hold objects*. How can we hold numbers in an
array or a dictionary then? 

The answer to that is pretty easy. We turn an NSInteger into an NSNumber, which is an
object. 

Awesome! So now we can have an array of NSNumbers or a dictionary with
NSNumbers as keys or values.

```objc
NSInteger x = 2;
NSInteger y = 3;
NSNumber *objectX = [NSNumber numberWithInteger:x];
NSNumber *objectY = [NSNumber numberWithInteger:y];
NSArray *numberArray = @[objectX, objectY]; // [@2, @3]
```

Because NSNumbers are objects, we can create them the same way you would create
any object. Or we can use a convenience method shortcut. Or we can use object literals
.

```objc
// normal object creation
NSNumber *objectX = [NSNumber alloc] initWithInteger:2];

// convenience method
NSNumber *objectY = [NSNumber numberWithInteger:3];

// object literals
NSNumber *objectZ = @5;
```

Now we know how to create NSNumbers. And we know that NSNumbers can be used in
Objective-C collections like dictionaries and arrays. 

If NSNumbers are so great, why don't we just use them all the time?

*We can't use them for mathmatical operations, that's why.*

When we want to do math with NSNumbers, we have to use methods to get an
NSInteger value (in the case of integers).

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
