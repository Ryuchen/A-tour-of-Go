## Imports

This code groups the imports into a parenthesized, "factored" import statement.
> 这些代码将导包语句合并到一个带括号，"公式化"的导入语句中

You can also write multiple import statements, like:
> 你也可以选择写多行的导入语句，如：

```gotemplate
import "fmt"
import "math"
```

But it is good style to use the factored import statement.
> 但是使用公式化的导入语句是很好的编码习惯

---

## 补充内容 - Part One

### Types of Imports in Golang

Well, yes there are different types of imports, many of which are unknown to many users. Let’s discuss these types in detail. Let us consider one package: Let’s say math and watch the differences in import styles.
> 是的，有不同类型的导入，其中许多Golang的使用者都不知道。让我们详细讨论这些类型。让我们考虑一个包：`math` 并观察它在不同的形式下导入语句的差异。

#### 1. Direct import

Go supports the direct import of packages by following a simple syntax. Both single and multiple packages can be imported one by one using the import keyword.
> 

**Example**:
> **Single:**  
> `import "fmt"`  
> **Multiple one-by-one:**  
> `import "fmt"`  
> `import "math"`

```gotemplate
// Golang program to demonstrate the 
// application of direct import
package main

import "fmt"

// Main function
func main() {

	fmt.Println("Hello Geeks")
}
```

#### 2. Grouped import

Go also supports grouped imports. This means that you don’t have to write the import keyword multiple times; instead, you can use the keyword import followed by round braces, (), and mention all the packages inside the round braces that you wish to import. This is a direct import too but the difference is that you’ve mentioned multiple packages within one import() here. Peek at the example to get an idea of the syntax of grouped import command.
>

**Example**: 
> ```gotemplate
> import (
>   "fmt"
>   "math"
> )
> ```

```gotemplate
// Golang program to demonstrate the
// application of grouped import
package main
    
import (
    "fmt"
    "math"
)
    
// Main function
func main() {

    // math.Exp2(5) returns 
    // the value of 2^5, wiz 32
    c := math.Exp2(5)
      
    // Println is a function in fmt package 
    // which prints value of c in a new
    // line on console
    fmt.Println(c)
}
```

#### 3. Nested import

Go supports nested imports as well. Just as we hear of the name nested import, we suddenly think of the ladder if-else statements of nested loops, etc. But nested import is nothing of that sort: It’s different from those nested elements. Here nested import means, importing a sub-package from a larger package file. For instance, there are times when you only need to use one particular function from the entire package and so you do not want to import the entire package and increase your memory size of code and stuff like that, in short, you just want one sub-package. In such scenarios, we use nested import. Look at the example to follow syntax and example code for a better understanding.
>

**Example**:
> `import "math/rand"`

```gotemplate
// Golang Program to demonstrate
// application of nested import
package main
   
 import (
    "fmt"
    "math/rand"
)
  
func main() {
  
    // this generates & displays a
    // random integer value < 100
    fmt.Println(rand.Int(100))
}
```

#### 4. Aliased import

It supports aliased imports as well. Well at times we’re just tired of writing the full name again and again in our code as it may be lengthy or boring or whatsoever and so you wish to rename it. Alias import is just that. It doesn’t rename the package but it uses the name that you mention for the package and creates an alias of that package, giving you the impression that the package name has been renamed. Consider the example for syntax and example code for a better understanding of the aliased import.
> 

**Example**:
> `import m "math"`   
> `import f "fmt"`  

```gotemplate
// Golang Program to demonstrate
// the application of aliased import
package main
    
import (
    f "fmt"
    m "math"
)
    
// Main function
func main() {
  
    // this assigns value 
    // of 2^5 = 32 to var c
    c := m.Exp2(5)    
      
    // this prints the 
    // value stored in var c
    f.Println(c)                
}
```

#### 5. Dot import

Go supports dot imports. Dot import is something that most users haven’t heard of. It is basically a rare type of import that is mostly used for testing purposes. Testers use this kind of import in order to test whether their public structures/functions/package elements are functioning properly. Dot import provides the perks of using elements of a package without mentioning the name of the package and can be used directly. As many perks as it provides, it also brings along with it a couple of drawbacks such as namespace collisions. Refer the example for syntax and example code for a better understanding of the dot import.
> 

**Example**:
> `import . "math"`  

```gotemplate
// Golang Program to demonstrate 
// the application of dot import
package main
  
import (
    "fmt"
    . "math"
)
   
func main() {
  
    // this prints the value of
    // 2^5 = 32 on the console
    fmt.Println(Exp2(5))      
}
```

#### 6. Blank import

Go supports blank imports. Blank means empty. That’s right. Many times, we do not plan of all the packages we require or the blueprint of a code that we’re about to write in the future. As a result, we often import many packages that we never use in the program. Then Go arises errors as whatever we import in Go, we ought to use it. Coding is an unpredictable process, one moment we need something and in the next instance, we don’t.
> 

But go doesn’t cope up with this inconsistency and that’s why has provided the facility of blank imports. We can import the package and not using by placing a blank. This way your program runs successfully and you can remove the blank whenever you wish to use that package. Refer code for syntax and example code for a better understanding of the blank import.
> 

**Example**:
> `import _ "math"`  

```gotemplate
// Golang Program to demonstrate
// the importance of blank import

// PROGRAM1
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("Hello Geeks")
}

// -------------------------------------------------------------------------------------
// Program1 looks accurate and everything
// seems right but the compiler will throw an
// error upon building this code. Why? Because
// we imported the math/rand package but
// We didn't use it anywhere in the program.
// That's why. The following code is a solution.
//-------------------------------------------------------------------------------------

// PROGRAM2
package main

import (
	"fmt"
	_ "math/rand"
)

func main() {
	fmt.Println("Hello Geeks")
	// This program compiles successfully and
	// simply prints Hello Geeks on the console.
}
```

#### 7. Relative import

When we create our packages and place them in a local directory or on the cloud directory, ultimately the $GOPATH directory or within that, we find that we cannot directly import the package with its name unless you make that your $GOPATH. So then you mention a path where the custom package is available. These types of imports are called relative imports. We often use this type but may not know its name (Relative import). Refer to the example for syntax and example code for a better understanding of relative import.
>

**Example**:
> `import "github.com/gopherguides/greet"`  

```gotemplate
package main
	
import "github.com/gopherguides/greet"
	
// Main function
func main() {
	// The hello function is in
	// the mentioned directory
	greet.Hello()
	// This function simply prints
	// hello world on the console screen
}
```

#### 8. Circular import

Just like a circle, a loop, there exists a circular import too. This means defining a package which imports a second package implicitly and defining the second package such that it imports the first package implicitly. This creates a hidden loop that is called the “import loop“. This type, too, is unknown by many and that is because Go does not support circular imports explicitly. Upon building such packages, the Go compiler throws an error raising a warning: “import cycle not allowed”. Consider the following examples to understand this better.
> 

First package file code. Imagine that name of first package is "first"
>

```text
package first

import "second"
// Imagine that the second package's name is "second"

var a = second.b
// Assigning value of variable b from second package to a in first package
```

Second package file code. Imagine that name of second package is "second"
> 

```text
package second

import "first"
// Imagine that the first package's name is "first"

var b = first.a
// Assigning value of variable a from first package to b in second package
```

Upon building any one of these packages, you will find an error like this:
```bash
$#=> go build  
$#=> can't load package: import cycle not allowed  
$#=> ...  
```

## Reference:
- [import-in-golang](https://www.geeksforgeeks.org/import-in-golang/)