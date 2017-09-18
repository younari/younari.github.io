---
layout: post
title: "Swift Property"
author: "younari"
---

# Swift Terminology - Property(속성)
##### Sources from [The Swift Programming Language (Swift 4)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html)

> *Properties associate values with a particular class, structure, or enumeration. Stored properties store constant and variable values as part of an instance, whereas computed properties calculate (rather than store) a value. Computed properties are provided by classes, structures, and enumerations. Stored properties are provided only by classes and structures.*

# Stored Properties
- 클래스, 구조체에서만 사용 가능
- 인스턴스와 연관된 값을 **저장**

**👋🏻 Lazy property**

- **lazy stored property**: 지연 저장 속성, 초기화 하는데 오래걸리거나 복잡한 초기화 과정이 있는 변수의 경우 지연저장을 사용하면 좋다.
- A lazy property does not get initialized until someone accesses it

# Computed Properties
- 클래스, 구조체, 열거형에서 모두 사용 가능
- **실제로 값을 저장하지 않지만,** get, set키워드를 통해서 값을 간접적으로 설정하거나 받을 수 있다.
- setter를 선언할 때 값을 넣지 않았다면 디폴트 네임은 newValue로 사용한다.

### Sample Code
{% highlight swift %}
struct Point {
{% endhighlight %}

# Read-Only Computed Properties
- A computed property with a getter but no setter is known as a read-only computed property. A read-only computed property always returns a value, and can be accessed through dot syntax, but cannot be set to a different value.

{% highlight swift %}
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}

let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
{% endhighlight %}

# Property Oberver
- **willSet** is called just before the value is stored.
- **didSet** is called immediately after the new value is stored.
- **Default value**: newValue VS. oldValue

### Sample Code
{% highlight swift %}
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
{% endhighlight %}


# Type Property
- 타입 자체에 대한 속성
- 값을 가져올때는 **클래스의 이름**을 통해서 가져올 수 있다.
- `let color: UIColor = UIColor.red`

### 👍🏻 Both types and instances can have methods & properties
- You can also define **properties that belong to the type itself,** not to any one instance of that type. There will only ever be one copy of these properties, no matter how many instances of that type you create. These kinds of properties are called **type properties.**
- Type methods and properties are denoted with the keyword **static.**


### 👍🏻 Sample Code - 01

{% highlight swift %}
staticfuncabs(d:Double) -> Double {
	if d < 0 { 
		return -d
	}else{
		return d
	}
} 
{% endhighlight %}

- `static var pi: Double`

{% highlight swift %}
    static let thresholdLevel = 10
    static var maxInputLevelForAllChannels = 0
    var currentLevel: Int = 0 {
        didSet {
            if currentLevel > AudioChannel.thresholdLevel {
                // cap the new audio level to the threshold level
                currentLevel = AudioChannel.thresholdLevel
            }
            if currentLevel > AudioChannel.maxInputLevelForAllChannels {
                // store this as the new overall maximum input level
                AudioChannel.maxInputLevelForAllChannels = currentLevel
            }
        }
    }
}
{% endhighlight %}

# Memory
- **Code, Data, Heap, Stack**
- Class는 Code 영역에 저장
- Static Type Property는 Data 영역에 저장
- Instance의 속성들은 Heap 영역에 저장

# Value VS. Reference
### Value (struct and enum)

