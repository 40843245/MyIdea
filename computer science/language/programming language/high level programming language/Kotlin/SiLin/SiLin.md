# SiLin (a high programming language I came up with but it does exist)
## intro
It is a blend of Simple and Kotlin.

The syntax is based on Kotlin and so are functionalities. It also inherits the syntax in Kotlin.

But I refined it with more readable and maintanenble syntax.

## review 
### term
1. modifier

#### modifier
1. modifier: It modifies the identifier. It is usually placed before the identifier.

> [!CAUTION]
> There is an argue that `val` and `var` are modifiers in Kotlin.
>
> Here, I think they are.

##### Examples 
###### Example 1

In Kotlin

```
val list1 = listOf(2)
```

has modifier `val`

###### Example 2

In Kotlin

```
private val list1 = listOf(2)
```

has modifier `val` and `private`.

## refined
1.
2. 
3. define some new keywords for existing state of identifier and type of identifier at present.
4. default value for `Nullable` type.
5. define new class, and extension of new methods or properties etc.
6. modifier definition of an identifier
7. by keyword omitting

###  define some new keywords for existing state of identifier and type of identifier at present
In SiLin, these are keyword but not in Kotlin 

+ Lazy
+ LateInit
+ Nullable

#### Lazy
The Lazy modifier defines the identifier is a in lazy state.

##### Examples
###### Example 1

Take an example from [stackoverflow](https://stackoverflow.com/questions/36623177/property-initialization-using-by-lazy-vs-lateinit#:~:text=by%20lazy%20may%20be%20very%20useful%20when%20implementing%20read-only(val)%20properties)

```
val myLazyString: String by lazy { "Hello" }
```

According to `by keyword omitting` (discussed later), one can rewrite it as

```
Lazy val myLazyString: String = "Hello" 
```

Due to type inference, one can rewrite it as

```
Lazy val myLazyString = "Hello" 
```

#### LateInit
Alias of `lateinit`.

#### Nullable

The Nullable defines the variable can be nullable. 

In Kotlin, to define a nullable variable, one only can 

+ use a nullable operator `?` such as `val nullableString : String? = null`

In SiLin, to define a nullable variable, one have more way to do that

+ use a nullable operator `?` such as `val nullableString : String? = null`
+ use `Nullable` such as `Nullable nullableString: String = null`  

##### Examples
###### Example 1

```
val nullableString : String? = null
```

One can rewrite it as (NOT recommended, just for convenience to migrate it from the Kotlin style.

```
Nullable val nullableString : String? = null
```

One can also rewrite it as (highly recommend) 

```
Nullable val nullableString : String = null
```

According to the default value of Nullable type (discuss later), one can also rewrite it as (highly recommend) 

```
Nullable val nullableString : String
```

According to `modifier definition of an identifier` (discuss later), one can also rewrite it as (highly recommend) 

```
Nullable String val nullableString = null
```

and 

```
Nullable String val nullableString
```

### default value for `Nullable` type

The default value of Nullable type is null.

It is allowed only in non-strict mode to use the default value for the variable that is defined but without assignment. 

See above example.

### define new class, and extension of new methods or properties etc.

In SiLin, the new class is defined.

+ Signature
+ Idenifier
+ Class
+ Method
+ Property
+ Function
+ Variable
+ Type

The hierarchy of type are shown as follows.

```
Any <-- Identifier

Identifier <-- Function <-- Method <-- Class 

Identifier <-- Variable <-- Property <-- Class
```

`Any <-- Identifier` means `Idenfier` inherits `Any` Class

#### `Variable`

- methods

+ isNullable
+ isNull
+ isNotNull
+ hasNull
+ hasNotNull
+ hasInitialized
+ needToInitializedFirst
+ canBeLateInitialized

+ isNullable() will return true iff the variable is nullable (or exactly said, is `Nullable` type)

+ isNull() will return true iff the variable is null.

+ isNotNull() is opposite of `isNull` method.

+ hasNull() will return true iff the variable has been set to null.

+ hasNotNull is opposite of `hasNull` method.
  
+ hasInitialized() will return true iff the variable has been initialized.
  
+ needToInitializedFirst() will return true iff the variable needs to initialized first before accessing it.
  
+ canBeLateInitialized() will return true iff the variable can be late initialized (or exactly said, is declare with `lateinit` or it alias `LateInit`). It is opposite of `needToInitializedFirst` method.

#### `Type`

- methods

+ isType
+ getTypeName
+ getRootSignature

+ isType(other:Type) is another form of `is` keyword.

```
Type.isType(other:Type) : Boolean = this is other 
```

+ getTypeName() will return a String that represents type of `this` value. For example: `2.getTypeName()` will return `Int`

It is equivalent to

```
Type.getTypeName() : String = this::class.simpleName
```
+ getRootSignature() will return Signature Instance that is root elem of `this` value. For example, if it refers an instance of Class Type, then .getRootSignature() will return the instance of Class Type.

If it does NOT refer anything, it getRootSignature() will return the instance of itself.
  
- properties

+ signature
  
+ signature will return the Signature Instance that represents the signature of this.

#### extension of some type

1. For `Float` and `Double` type,

it will have new methods.

+ isInteger
+ isSafeInteger

+ isInteger() will return true iff the `this` value is an integer. That is, the distance of its value and floor of it is less than eps.

```
import kotlin.math.*;

object MathMagicalNumber{
  val EPS = 0.00001
}

Double.isInteger() : Boolean = abs( this - floor(this) ) < MathMagicalNumber.EPS
```
  
+ isSafeInteger() will return true iff the `this` value is a safe integer. A safe integer is an integer that without redundent floating point.

For example, `2.0` is NOT a safe integer. But `2` is a safe integer.

```
Double.isSafeInteger(): Boolean {
  val string1 = this.toString()
  return string1.isNumeric() && !string1.contains('.')
}
```

- For `CharSequence` type

it has new methods.

+ isNumeric

+ isNumeric() will return true iff this value can be convert to number.

```
CharSequence.isNumeric(): Boolean = this.toDoubleOrNull().isNotNull()
```

### modifier definition of an identifier

For a definition, if the identifier is defined using generic type, then one can move the the type before the variable as a modifier.

#### Examples
##### Example 1
They are equivalent(in SiLin NOT in Kotlin):

It is a simpler way of next one in Kotlin using type inference.

```
val list1 = listOf(2)
```

```
val list1 = listOf<Int>(2)
```

Since list1 is List<Int>, so in SiLin, one can rewrite it as 

```
List<Int> val list1 = (2)
```

Due to type inference, one can rewrite it as 

```
List val list1 = (2)
```

##### Example 2
They are equivalent(in SiLin NOT in Kotlin):

Take a mutable state variable which is useful in Android Compose.


```
val mutableStateVariable1 by remember { mutableIntStateOf(2) }
```

One can rewrite it in another form.

```
val mutableStateVariable1 by remember { mutableStateOf(2) }
```

and

```
val mutableStateVariable1 by remember { MutableState(2) }
```

and 

```
val mutableStateVariable1 by remember { MutableState<Int>(2) }
```

Since mutableStateVariable1 is MutableState<Int>, so in SiLin, one can rewrite it as 

```
MutableState<Int> val mutableStateVariable1 by remember { (2) }
```

Due to type inference, one can rewrite it as 

```
MutableState val mutableStateVariable1 by remember { (2) }
```

And by `by keyword omitting` (discussed in next section), one can replace the `by` , following keyword and following curl brackets with assignment operator `=` , leaving `(2)`.

```
MutableState val mutableStateVariable1 = (2) 
```

### by keyword omitting
In a statement, iff the type of identifier does NOT be determined after `by` keyword, then one can replace `by` and following keyword (even if following curl brackets) with assignment operator `=`.

(or sometimes omitting if the assigment operator `=` exists).

However, under this case, if the state of identifier is not determined or the state of identifier can not be determined only from the part before the `by` keyword or assignment operator (a good example in example 2), one must be define the state of identifier through modifier 
#### Examples
##### Example 1

In SiLin they are equivalent:

Take the example 1 in above section for example.

```
MutableState val mutableStateVariable1 by remember { (2) }
```

One can rewrite it as

```
MutableState val mutableStateVariable1 = (2)
```

##### Example 2

Take an example from [stackoverflow](https://stackoverflow.com/questions/36623177/property-initialization-using-by-lazy-vs-lateinit#:~:text=by%20lazy%20may%20be%20very%20useful%20when%20implementing%20read-only(val)%20properties)

```
val myLazyString: String by lazy { "Hello" }
```

According to `modifier definition of an identifier`, one can rewrite it as

```
String val myLazyString by lazy { "Hello" }
```

One can rewrite it as

```
Lazy String val myLazyString by lazy { "Hello" }
```

