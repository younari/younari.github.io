---
layout: post
title: "간단한 알고리즘 구현하기"
author: "Amy"
---

### 01. Collatz의 추측 함수 만들기
> Collatz 추측 : 입력된 수가 짝수면 2로 나누고, 홀수면 3으로 곱한 뒤 1을 더한다. 결과로 나온 수에 같은 작업을 반복하여 결과값이 1이 될 때까지 반복하면 모든 수는 결국 1이 된다. 입력된 수가 1이 될 때까지 몇 회 걸리는지를 구하고, 500회가 넘어가면 -1을 return 한다.

{% highlight swift %}
func collatz(input: Int) -> Int {
    var i: Int = input
    var count: Int = 0
    while i > 1 {
        if count <= 500 {
            if i % 2 == 0 {
                i /= 2
                count += 1
            }else if i % 2 != 0 {
                i = i * 3 + 1
                count += 1
            }
        }else if count > 500 {
            i = 0
            count = -1
        }
    }
    return count
}
{% endhighlight %}

### 02. Harshad 수 만들기
> Harshad의 수 : 양의정수 n이 Harshad의 수이려면, 각 자릿수의 합으로 n이 나누어져야 한다.

{% highlight swift %}
func harshad(number: Int) -> Bool {
    
    var returnValue: Bool = true
    var num: Int = number
    var totalNum = 0
    while num > 0 {
        totalNum += num%10
        num = num/10
    }
    
    if number % totalNum == 0 {
        returnValue = true
    }else if number % totalNum != 0 {
        returnValue = false
    }
    
    return returnValue
}
{% endhighlight %}


### 03. 황금 비율, Golden ratio
- `1+1/(1+1/(1+1/(1+...)))`

{% highlight swift %}
let depth = 20 //임의의 정수
func goldenRatio(depth:Int) -> Double {
    if depth == 0 {
        return 1.0
    }
    else {
        return 1.0 + 1.0/goldenRatio(depth: depth - 1)
    }
}

phi = goldenRatio(depth:depth)
{% endhighlight %}