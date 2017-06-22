## Reverse Integer 反转整数

```bash
Example1: x = 123, return 321
Example2: x = -123, return -321
```

> Note: The input is assumed to be a 32-bit signed integer. Your function should return 0 when the reversed integer overflows.

输入参数和返回值都为 `int32` ， 返回值 `overflows` 时 返回 0

---

## 思路

:eyes: 常规思路就是将输入的整型转换成字符串， 再反转字符串， 需要判断输入参数的正负情况， 以下是自己写的solution:

```golang
func reverse(x int) int {
        pos := true

        if x < 0 {
                x = -x
                pos = false
        }

        s := strconv.Itoa(x)
        ss := []byte(s)
        sz := len(ss)
        // 字符串翻转逻辑
        for i := 0; i < sz/2; i++ {
                mIndex := sz - i - 1
                ss[i], ss[mIndex] = ss[mIndex], ss[i]
        }

        sss, err := strconv.Atoi(string(ss))
        if err != nil {
                return 0
        }

        if !pos {
                sss = -sss
        }

        // 判断整型溢出
        if sss > math.MaxInt32 || sss < math.MinInt32 {
                return 0
        }

        return sss
}
```

`runtime` 为 6ms， 持怀疑态度， 这么挫的代码还能跑进6ms， 不得不佩服golang的强大 :joy:

上面的solution使用了类型强制转换， 在字符串转整型时可能出现溢出的情况， 所以并非是最佳的解决方案， 需要考虑其他的解决办法。

---
### Best solution

看了下投票最高的solution， java实现的， 我在下面用go 重新实现一遍， 完全的数学解题思路 (数学真是太弱了...

```golang
func reverse(x int) int {
        var result int
        for x != 0 {
                result = result*10 + x%10
                x = x / 10
        }
        if result < math.MinInt32 || result > math.MaxInt32 {
                return 0
        }
        return result
}
```

几行代码就搞定了， 比类型转换快的多的多。

另外要注意的是 ，`python` 里算 `x/10` 会返回浮点数。
