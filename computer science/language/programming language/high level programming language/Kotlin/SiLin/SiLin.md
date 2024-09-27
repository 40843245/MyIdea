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
1. Exception to Result instance at high order function.
2. Exception to Result instance at high order function and then cancel at exception.
3. define some new keywords for existing state of identifier and type of identifier at present.
4. default value for `Nullable` type.
5. define new class, and extension of new methods or properties etc.
6. modifier definition of an identifier
7. by keyword omitting

### Exception to Result instance at high order function

The Result instance is always returned.

When exception is thrown in a high order function, then Result is returned and NOT continue to execute the high order function.

#### Symbol

```
.?
```

#### Examples
##### Example 1

```
val list1 = listOf(2,3,4,5,6,7)
val result1 : Result<Int> = list1.?forEach{
  println(it)
}

println("-")
println(result1.isFailure)
```

should output

```
2
3
4
5
6
7
-
false
```

##### Example 2

```
val list1 = listOf(2,3,4,5,6,7)
val result1 : Result<Int> = list1.?forEach{
  println(it)
  if(it == 4){
    throw Exception("New Exception")
  }
}

println("-")
println(result1.isFailure)
```

should output

```
2
3
4
-
true
```

### Exception to Result instance at high order function and then cancel at exception

The Result instance is always returned.

When exception is thrown in a high order function, then Result is returned and NOT continue to execute of the high order function. The execution before

exception throws will be cancelled.

#### Symbol

```
.??
```

#### Examples
##### Example 1

```
val list1 = listOf(2,3,4,5,6,7)
val result1 : Result<Int> = list1.??forEach{
  println(it)
}

println("-")
println(result1.isFailure)
```

should output

```
2
3
4
5
6
7
-
false
```

##### Example 2

```
val list1 = listOf(2,3,4,5,6,7)
val result1 : Result<Int> = list1.??forEach{
  println(it)
  if(it == 4){
    throw Exception("New Exception")
  }
}

println("-")
println(result1.isFailure)
```

should output

```
-
true
```

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

- class

+ Type
+ ProtectionLevel
+ Static
+ Final
+ Annotation
+ Idenifier
+ Variable
+ Property
+ Function
+ Method
+ Class
+ Parameter
+ Signature

- enum

+ CaseEnum

+ ProtectionLevelEnum
  
+ ParameterEnum

-----------------------------

The hierarchy of type are shown as follows.

```
Any <-- Type

Type <-- Identifier

Any <-- ProtectionLevel

Any <-- Static

Any <-- Final

Any <-- Annotation

Any <-- Returned

Any <-- Body

Any <-- Modifier

Modifier <-- VariableModifier

VariableModifier,Static,Final <-- PropertyModifier

Modifier <-- FunctionModifier

FunctionModifier,Static,Final <-- MethodModifier

PropertyModifier, MethodModifier <-- MemberModifier

Modifier,Static,Final <-- ClassModifier

Any <-- Instance

Instance <-- VariableInstance

Instance <-- FunctionInstance

Instance <-- ClassInstance

ProtectionLevel <-- Property

ProtectionLevel <-- Method

Identifier <-- Variable <-- Property

Identifier <-- Function <-- Method

Method,Property <-- Class

```

> [!NOTE]
> `<--` means inherited
> 
> For example, `Any <-- Identifier` means `Idenfier` inherits `Any` Class

#### `CaseEnum`

`CaseEnum` is an enum class that wraps all constants about case.

+ `CaseEnum.CAMELCASE`

+ `CaseEnum.BIGCAMELCASE`

+ `CaseEnum.UPPERCASE`

+ `CaseEnum.LOWERCASE`

+ `CaseEnum.CONSTANT_CASE`

+ `CaseEnum.MACRO_CASE`

#### `ProtectionLevelEnum`

`ProtectionLevelEnum` is an enum class that wraps all constants about protection level.

+ `ProtectionLevelEnum.PRIVATE`

+ `ProtectionLevelEnum.PUBLIC`

+ `ProtectionLevelEnum.PROTECTED`

#### `Type`

- methods

+ getName
+ isType
+ getTypeName
+ getParentName
+ getRootSignature

+ getName() returns the non-qualified name of the type.

```
Type.getName() = this:class.SimpleName
```

+ getQualifiedName() returns the qualified name of the type.

```
Type.getQualifiedName() = this:class.qualifiedName
```
  
+ isType(other:Type) is another form of `is` keyword.

```
Type.isType(other:Type) : Boolean = this is other 
```

+ getTypeName() will return a String that represents type of `this` value. For example: `2.getTypeName()` will return `Int`

It is equivalent to

```
Type.getTypeName() : String = this::class.simpleName
```

+ getParent() return its parent node. If it has no parent, it will return null.

+ getParentName() return the name of its parent node. If it has no parent, it will return null.

```
Type.getParent.getName()
```
  
+ getRootSignature() will return Signature Instance that is root node of `this` value. If it has no parent, it will return itself node. For example, if it refers an instance of Class Type, then .getRootSignature() will return the instance of Class Type.

If it does NOT refer anything, it getRootSignature() will return the instance of itself.
  
- properties

+ signature
  
+ signature will return the Signature Instance that represents the signature of this.

#### `ProtectionLevel`

- methods

+ isPrivate
+ isPublic
+ isProtected

+ isPrivate() returns true iff it is private.

```
Property.isPrivate() : Boolean = this.protectionLevel == ProtectionLevelEnum.PRIVATE
```

+ isPublic() returns true iff it is public.

```
Property.isPublic() : Boolean = this.protectionLevel == ProtectionLevelEnum.PUBLIC
```

+ isProtected() returns true iff it is protected.

```
Property.isProtected() : Boolean = this.protectionLevel == ProtectionLevelEnum.PROTECTED
```

- properties

+ protectionLevel

+ protectionLevel returns `ProtectionLevelEnum` instance indicates that its protection level (including `private`,`public`, `protected`).

#### `Static`

- methods

+ isStatic
+ isNotStatic

+ isStatic() returns true iff it is static.

```
Property.isStatic() : Boolean = this.static
```

+ isNotStatic() returns true iff it is not static. It is opposite of `isStatic` method

```
Property.isNotStatic() : Boolean = !this.isStatic()
```

- properties

+ static

+ static returns it is static.

#### `Final`

- methods

+ isFinal

+ isFinal() returns true iff it is final.

```
Property.isFinal() : Boolean = this.final
```

- properties

+ final
  
+ final returns it is final.

#### `Annotation`

- property

+ name
+ entries
+ keys
+ values

+ name returns name of `Annotation` instance.
  
+ entries returns all entries formed in key-value pair of `Annotation` instance.
  
+ keys returns all keys of `Annotation` instance.

##### Examples
###### Example 1

Consider the following code snippets.

```
@Preview ( background = true )
fun mainActivity(){
  
}
```

The function `mainActivity` has an annotation named `Preview`.

The @Preview annotation has a key-value pair `background = true`. So 

```
val previewAnnotation = Function(mainActivity).getAnnotation("Preview")
println(previewAnnotation.entries)
println(previewAnnotation.keys)
println(previewAnnotation.values)
```

should output

```
[background = true]
[background]
[true]
```

+ values returns all values of `Annotation` instance.

#### `Modifier`
None

#### `VariableModifier`

- properties

+ val
+ var

+ val returns true iff the identifier is defined with `val` keyword.
+ var returns true iff the identifier is defined with `var` keyword.

#### `FunctionModifier`
None

#### `PropertyModifier`

- properties

+ protectionLevel
+ static
+ final

+ protectionLevel returns `ProtectionLevel` instance that indicates the protection level of property in class.
  
+ static returns true iff it is static. Exactly said, the property is defined with `static` keyword, or property is in the object or in static class or in companion object of the class.
  
+ final returns true iff it is final. Exactly said, the property is defined with `final` keyword.

#### `MethodModifier`

- properties

+ protectionLevel
+ static
+ final

+ protectionLevel returns `ProtectionLevel` instance that indicates the protection level of method in class.
  
+ static returns true iff it is static. Exactly said, the method is defined with `static` keyword, or method is in the object or in static class or in companion object of the class.
  
+ final returns true iff it is final. Exactly said, the method is defined with `final` keyword.
  
#### `ClassModifier`

- properties

+ protectionLevel
+ static
+ final
+ class
+ object
+ data 

+ protectionLevel returns `ProtectionLevel` instance that indicates the protection level of the class.
  
+ static returns true iff it is static. Exactly said, it is the class that is defined with `static` keyword, or it is an object.
  
+ final returns true iff it is final. Exactly said, it is the class or object is NOT defined with `open` keyword or defined with `final` keyword.

+ class returns true iff it is a class. Exactly said, it is defined with `class` keyword.
+ object returns true iff it is a object. Exactly said, it is defined with `object` keyword.
+ data returns true iff it is a data class or a data object. Exactly said, it is defined with `data` keyword.

#### `Returned`

- property

+ returnedValue
+ returnedType

+ returnedValue returns the returned value from the last call of function or method.
  
+ returnedType returns the `Type` instance that indicates the type of `returnedValue` property.

#### `Body`

- properties

+ statements
+ variables

+ statements returns the statements in the body.

+ variables returns the variable that is defined in this body. (i.e. Whose scope is in the body)

#### `Idenifier`

- methods

+ isCamelcased
+ isBigCamelcased
+ isUppercased
+ isLowercased
+ isConstantCased

+ isCamelcased() returns true iff the identifier consists of english letter and is small camel cased (such as `isNumeric`)

```
Idenifier.isCamelcased() = Idenifier.case == CaseEnum.CAMELCASE
```

+ isBigCamelcased() returns true iff the identifier consists of english letter and is big camel cased (such as `IsNumeric`)

```
Idenifier.isBigCamelcased() = Idenifier.case == CaseEnum.BIGCAMELCASE
```

+ isUppercased() returns true iff the identifier consists of english letter and is all uppercased. (such as `ISNUMERIC`)

```
Idenifier.isUppercased() = Idenifier.case == CaseEnum.UPPERCASE
```

+ isLowercased() returns true iff the identifier consists of english letter and is all lowercased. (such as `isnumeric`)

```
Idenifier.isLowercased() = Idenifier.case == CaseEnum.LOWERCASE
```

+ isConstantCased() returns true iff the identifier looks like a constant in enum class (i.e. only consists of english letter and underscore `_`, and is all uppercased, and it does NOT start and end with underscore.  (such as `MAGICAL_NUMBER`) )

```
Idenifier.isConstantCased() = Idenifier.case == CaseEnum.CONSTANT_CASE
```

+ isMacroCased() returns true iff the identifier looks like a macro in enum class (i.e. only consists of english letter and underscore `_`, and is all uppercased (such as `__MAGICAL_NUMBER__` , `__IFDEFINED__` ) )

```
Idenifier.isMacroCased() = Idenifier.case == CaseEnum.MACRO_CASE
```

- properties

+ case

+ case returns the type of case, it may one of these values defined in `CaseEnum` enum class.

#### `Variable`
None 

#### `Property` 

- methods

+ getModifiers

+ getModifiers returns a list of `Modifier` instance that represents all modifiers.

```
Property.getModifiers() = this.modifiers
```

- properties

+ modifiers
  
+ modifiers returns a list of `Modifier` instance that represents all modifiers.

#### `Function`

- methods

+ hasParameters
+ getNumberOfParameters
+ getParameters
+ getParametersName
+ getParametersType
+ getParameterName
+ getParameterType
+ getAnnotations
+ getAnnotationsName
+ getAnnotation
+ getAnnotationName

+ hasParameters returns true iff it has one or more parameters.
  
```
Function.hasParameters() = this.getNumberOfParameters() > 0 
```

+ getNumberOfParameters returns the number of parameters. 

```
Function.getNumberOfParameters() = this.numberOfParameters
```

+ getParameters returns a list of `Parameter` class. All infos about parameters can be known through this method.

+ getParametersName returns a list of String that represents the name of all parameters.

```
Function.getParametersName() = this.getParameters.map{ parameter -> parameter.name }
```

+ getParametersType returns a list of `Type` class that represents the type of all parameters.

```
Function.getParametersName() = this.getParameters.map{ parameter -> parameter.type }
```

+ getParameter(index:Int) returns `Parameter` instance that indicates the parameter according to the given argument index. If it is out of bound, then it throws OutOfIndexException.
  
+ getParameter(parameter:Parameter) returns  `Parameter` instance that indicates the parameter according to the given argument Parameter.That is, find the parameter argument parameter, then return the `Parameter` instance.  If the parameter argument is NOT in the function, then it throws NoSuchElementException.

+ getParameter(name:String) returns `Parameter` instance that indicates the parameter according to the given argument name. That is, find the parameter whose name equals to the argument name, then return the `Parameter` instance.  If the name argument is NOT in the function, then it throws NoSuchElementException.

+ getParameterName(index:Int) returns the name of parameter to the given argument index. If it is out of bound, then it throws OutOfIndexException.

+ getParameterName(parameter:Parameter) returns  String instance that indicates the name of parameter according to the given argument Parameter.That is, find the parameter whose name equals to the name of argument parameter, then return the String instance.  If the parameter argument is NOT in the function, then it throws NoSuchElementException.

+ getParameterName(name:String) returns  String instance that indicates the name of parameter according to the given argument Parameter. That is, find the parameter whose name equals to the argument name, then return the String instance.  If the parameter argument is NOT in the function, then it throws NoSuchElementException.

+ getParameterType(index:Int) returns `Type` instance that indicates the type of parameter to the given argument index. If it is out of bound, then it throws OutOfIndexException.

+ getParameterType(name:String) returns `Type` instance that indicates the type of parameter to the given argument name. That is, find the parameter whose name equals to the argument name, then return the `Type` instance. If the name argument is NOT in the function, then it throws NoSuchElementException.

+ getAnnotations() returns a list of `Annotation` instance.
  
+ getAnnotationsName() returns a list of String instance that indicates the name of all `Annotations`.
   
+ getAnnotation(index:Int) returns an `Annotation` instance according to the given argument index. If it is out of bound, then it throws OutOfIndexException.

+ getAnnotation(annotation:Annotation) returns `Annotation` instance according to the given argument Annotation.That is, find the annotation whose name equals to the name of argument annotation, then return the String instance. If the annotation argument is NOT in the function, then it throws NoSuchElementException.
  
+ getAnnotation(name:String)  returns an `Annotation` instance according to the given argument name. That is, find the parameter whose name equals to the argument name, then return the String instance.  If the parameter argument is NOT in the function, then it throws NoSuchElementException.
  
+ getAnnotationName(index Int) returns an name of the annotation according to the given argument index. If it is out of bound, then it throws OutOfIndexException.

+ getAnnotationName(annotation:Annotation) returns String instance that indicates the name of annotation according to the given argument Annotation.That is, find the annotation whose name equals to the name of argument annotation, then return the String instance.  If the annotation argument is NOT in the function, then it throws NoSuchElementException.
  
+ getAnnotationName(name:String) returns an name of the annotation according to the given argument  name. That is, find the annotation whose name equals to the argument name, then return the String instance.  If the parameter argument is NOT in the function, then it throws NoSuchElementException.
  
- properties

+ numberOfParameters
+ parameters
+ returned
+ annotations
+ body

+ numberOfParameters returns the number of parameters.

+ parameters returns the list of `Parameter` instance.

+ returned returns `Returned` instance.

+ annotations returns the list of `Annotation` instance.

+ body returns the `Body` instance that indicates the body of function.

#### `Method`

- methods

+ getModifiers

+ getModifiers returns a list of `Modifier` instance that represents all modifiers.

```
Property.getModifiers() = this.modifiers
```

- properties

+ modifiers
  
+ modifiers returns a list of `Modifier` instance that represents all modifiers.

#### `Class`

- properties

+ members
+ properties
+ methods

+ members returns a list of `Member`.
  
+ properties returns a list of `Property`.
  
+ methods returns a list of `Method`.

#### `Instance`

- property

+ name 
+ value
+ type

+ name returns the name of the Instance. 
+ value returns the value the Instance currently stored. 
+ type returns the type of the Instance.

#### `VariableInstance`

- methods

+ isNullable
+ isNull
+ isNotNull
+ hasNull
+ hasNotNull
+ hasInitialized
+ needToInitializedFirst
+ canBeLateInitialized
+ getVariableModifier

+ isNullable() will return true iff the variable is nullable (or exactly said, is `Nullable` type)

+ isNull() will return true iff the variable is null.

+ isNotNull() is opposite of `isNull` method.

+ hasNull() will return true iff the variable has been set to null.

+ hasNotNull is opposite of `hasNull` method.
  
+ hasInitialized() will return true iff the variable has been initialized.
  
+ needToInitializedFirst() will return true iff the variable needs to initialized first before accessing it.
  
+ canBeLateInitialized() will return true iff the variable can be late initialized (or exactly said, is declare with `lateinit` or it alias `LateInit`). It is opposite of `needToInitializedFirst` method.

+ getVariableModifier returns `VariableModifier` instance that indicate the modifier of the `VariableInstance` instance.
  
#### `FunctionInstance`

- methods

+ hasInvoked
+ hasNotInvoked
+ getFunctionModifier

+ hasInvoked returns true iff it has been invoked.
  
+ hasNotInvoked is the opposite of `hasInvoked` method

+ getFunctionModifier returns `FunctionModifier` instance that indicate the modifier of the `FunctionInstance` instance.
  
- properties

+ invokedTimes

+ invokedTimes returns the times it is invoked.
  
#### `ClassInstance`

- methods

+ getClassModifier

+ getClassModifier returns `ClassModifier` instance that indicate the modifier of the `ClassInstance` instance.


- property

+ class

+ class returns `Class` instance that indicates the class of the `ClassInstance` instance.
 
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

