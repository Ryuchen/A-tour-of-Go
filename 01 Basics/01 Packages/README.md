## Packages

Every Go program is made up of packages.
> 所有的Go语言程序都是由 `package` 组成的。

Programs start running in package `main`.
> 程序入口为 `package main`。

This program is using the packages with import paths "fmt" and "math/rand".
> 这个程序通过导入 `fmt` 和 `math/rand` 路径来使用 `packages`。

**Program**
> 程序示例

```gotemplate
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```

By convention, the package name is the same as the last element of the import path. For instance, the `"math/rand"` package comprises files that begin with the statement `package rand`.
> 按照管理，`package` 的名称往往是导入路径的最后一个元素。例如，导入的 `"math/rand"` 就包含着陈述 `package rand` 的意思。

**Note**: The environment in which these programs are executed is deterministic, so each time you run the example program rand.Intn will return the same number.
>**注意**：这些程序的运行环境是确定性的，所以每次你运行示例程序 `rand.Intn` 都会返回相同的数字。

(To see a different number, seed the number generator, see `rand.Seed`. Time is constant in the playground, so you will need to use something else as the seed.)
> 为了看到不同的数字，需要为数字生成器配置种子，查看 `rand.Seed`。在游乐场（页面上的运行环境）时间是常量，所以你需要使用其他的东西作为种子。

---

## 补充内容 - Part One

### What is the `rand.Seed()` function in Golang?

In Golang, the `rand.Seed()` function is used to set a seed value to generate pseudo-random numbers.
> 在Golang中，`rand.Seed()` 方法被当做配置一个种子参数然后来生成伪随机数。

If the same seed value is used in every execution, then the same set of pseudo-random numbers is generated. In order to get a different set of pseudo-random numbers，we need to update the seed value.
> 如果每次执行都使用相同的随机种子，则生成相同的伪随机数集。为了获得不同的伪随机数集，我们需要更新随机种子的值。

#### Syntax

```gotemplate
rand.Seed(value)
```

#### Parameters
The `rand.Seed()` function accepts the following parameter:

- `value`: This is the value that is set as the seed value

> `rand.Seed()` 方法接收以下参数：
> - `value`：这个值是设置为随机种子

#### Example
The code given below will show us how to use the `rand.Seed()` function to set the seed value:
> 下面的代码示例，告诉我们如何为 `rand.Seed()` 函数设置随机种子

```gotemplate
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	// seed to get different result every time
	rand.Seed(time.Now().UnixNano())
	fmt.Println(rand.Intn(50))
	fmt.Println(rand.Float64())
}
```

## 补充内容 - Part Two

### Golang生成随机数 - 真随机数与伪随机数
在Golang中可以通过 `math/rand` 的方法来生成伪随机数集，通常情况下使用时间戳生成的随机数就可以满足要求。但是，伪随机数其实是有周期的。这就意味着在高安全要求下的身份验证、加密等情况使用伪随机数有风险。所以一般企业对产品的加密秘钥的生成必须采用真随机数生成器，这样才能保证万无一失，杜绝了被破解的可能性。

#### 伪随机数
Golang的`math/rand`库会使用默认的随机种子 —— `1`来生成随机数。

**伪随机数**：是使用一个确定性的算法计算出来的似乎是随机的数序，因此伪随机数实际上并不随机。

- Example：使用时间戳来生成伪随机数

```gotemplate
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	rand.Seed(int64(time.Now().UnixNano()))
	fmt.Println(rand.Int())
}
```

#### 真随机数
Golang的`crypto/rand`库可以用来生成真随机数

**真随机数**：利用当前系统的熵池来计算出固定一定数量的随机比特，然后将这些比特作为字节流返回。

- Example：使用物理环境来生成真随机数

```gotemplate
package main

import (
	"crypto/rand"
	"fmt"
	"math/big"
)

func main() {
	//生成 20 个 [0, 100) 范围的真随机数
	for i := 0; i < 20; i++ {
		result, _ := rand.Int(rand.Reader, big.NewInt(100))
		fmt.Println(result)
    }
}
```

#### 总结
真正的随机数是使用物理现象产生的：比如掷钱币、骰子、转轮、使用电子元件的噪音、核裂变等等，这样的随机数发生器叫做物理性随机数发生器，它们的缺点是技术要求比较高。

按照 `crypto/rand` 的源文件来看，这些数据源自于每台设备：

```gotemplate
// Package rand implements a cryptographically secure
// pseudorandom number generator.
package rand

import "io"

// Reader is a global, shared instance of a cryptographically
// strong pseudo-random generator.
//
// On Linux, Reader uses getrandom(2) if available, /dev/urandom otherwise.
// On OpenBSD, Reader uses getentropy(2).
// On other Unix-like systems, Reader reads from /dev/urandom.
// On Windows systems, Reader uses the CryptGenRandom API.
```
以Linux为例，优先调用getrandom(2)，其实就是/dev/random优先。与/dev/urandom两个文件，他们产生随机数的原理其实是差不多的，本质相同：都是利用当前系统的熵池来计算出固定一定数量的随机比特，然后将这些比特作为字节流返回。

> 熵池就是当前系统的环境噪音，熵指的是一个系统的混乱程度，系统噪音可以通过很多参数来评估，如内存的使用，文件的使用量，不同类型的进程数量等等。

简单来说，这个随机数的随机种子为(进程数+内存占用长度+时间戳)（只是举例解释，并非实际算法），而系统中存在多少线程完全就是随机的，无周期的。

但要注意，通过这种方式，要比math.rand慢大约10倍。

## 网页答案

```gotemplate
package main

import (
	"crypto/rand"
	"fmt"
	"math/big"
)

func main() {
	result, _ := rand.Int(rand.Reader, big.NewInt(100))
	fmt.Println(result)
}
```

## Reference: 
- [how-to-properly-seed-random-number-generator](https://stackoverflow.com/questions/12321133/how-to-properly-seed-random-number-generator)
- [what-is-the-randseed-function-in-golang](https://www.educative.io/answers/what-is-the-randseed-function-in-golang)
- [golang-生成随机数-真随机数与伪随机数](https://xzhsh.ch/2021/09/golang-%E7%94%9F%E6%88%90%E9%9A%8F%E6%9C%BA%E6%95%B0-%E7%9C%9F%E9%9A%8F%E6%9C%BA%E6%95%B0%E4%B8%8E%E4%BC%AA%E9%9A%8F%E6%9C%BA%E6%95%B0/)
