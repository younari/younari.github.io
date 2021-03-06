---
layout: post
title: "Swift 탐구 생활"
author: "Amy"
---

> 사수님께 배운 내용, RayWenderlich DevCon을 통해 학습한 내용, 구글링한 내용 등 - 각종 문제 상황을 해결하면서 남겨진 스위프트 지식들을 기록하는 포스트 입니다. 비정기적으로 새로운 내용을 학습할 경우 업데이트 됩니다.

# 01. Memory Leak 

- **질문**: init은 불렸는데, Deinit이 안불렸어요!
- ↳ **Retain count**
- ↳ **상호 참조되는 상황 주의**
- ↳ 눈에 보이지는 않지만 뒤에서 줄줄 새고 있는 메모리를 관리하자.
- **↳  요약**: **Heap Allocation**은 **Deallocation**될 때 **Reference Cycle**이 발생되어 **Memory Leak**이 일어나지 않도록 주의해야 한다.
- **흔히 발생하는 문제**: **프로토콜 Delegate 프로퍼티, Closure 블럭 내부**


<hr>

- ***"Memory is a limited resource on mobile. Use too much and the jetsam system deamon will kill your app."*** @RayWenderlich, 2017 DevCon, Session 8
- [참고 링크 RWDevCon 2017 Vault - Swift Memory Management](https://videos.raywenderlich.com/courses/81-rwdevcon-2017-vault-tutorials/lessons/8)


<hr>


<hr>




<br>

# 02. Frame과 Bounds
- "너가 어디에 있든 관심이 없고, 너의 보여지는 중앙에 위치하고 싶을 때" -> Bounds! (대상 뷰의 바운드 mid)
- "나는 너의 종속된 좌표로 들어가고 싶어!" -> 뷰와 뷰의 구성에는 Frame!
- 콜렉션뷰 인셋을 주었을 때 바운드 값이 변한다 -> 바운드는 카메라의 위치라고 생각해보자.

<br>

# 03. Copy와 DeepCopy
- (objc) `DeepCopy`는 클래스 내부의 프로퍼티까지 메모리 주소를 싹 다 새로 만든다.

<br>

# 04. 스크롤 퍼포먼스
- **현상**: 핀터레스트 레이아웃으로, CollectionView에서 2단 그리드 형태로 Cell을 그려낼 때 스크롤 하면서 Cell의 움직임이 버벅이는 현상
- **원인**: 아이폰은 1초에 60프레임을 그린다. 다른 작업 때문에 Main Thread에서 초당 20프레임 이하로 떨어지면 UI가 버벅이는 현상이 발생한다. UI 작업 이외의 dataRequest 하는 부분은 concurrent로 돌리고, attributedString 등 연산이 들어가는 행위는 Cell마다 하지 않고 1번만 할 수 있도록 분리시킨다.
- **인사이트**: 메인 스레드에서 UI를 그리는 데 시간을 몰아주어야 한다. 연산은 1번만 한다. 

<br>

# 05. AutoLayout 적용 시점
- **질문**: setNeedsLayout / layoutIfNeeded / layoutSubviews의 차이점은 무엇인가요?
- **setNeedsLayout**: asynchronous update (비동기 업데이트)
- **layoutIfNeeded**: synchronous update (동기 업데이트 = 바로 실행)
- **layoutSubviews**: override method
- (발췌) So, stated succinctly, layoutIfNeeded says update immediately please, whereas setNeedsLayout says please update but you can wait until the next update cycle.
- [참고자료01](http://www.iosinsight.com/setneedslayout-vs-layoutifneeded-explained/)
- [참고자료02](https://medium.com/@abhimuralidharan/ios-swift-setneedslayout-vs-layoutifneeded-vs-layoutsubviews-5a2b486da31c)

<br>

# 06. init rect & coder
- `init(rect:)` 뷰를 코드로 생성할때 사용하는 방식
- `init(coder:)` 뷰를 xib로 생성할때 사용하는 방식
- 예시로 아래에서는 스토리보드를 통해 init 했으므로, init(coder:)가 불림

```
collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: "MyIdentifier", for: indexPath) 
```

<br>

# 07. Xcode config
- [Using xcconfig files](http://www.jontolof.com/cocoa/using-xcconfig-files-for-you-xcode-project/)

<br>

# 08. ScrollView
- API - `scrollRectToVisible(_:animated:)`
- [Apple Doc](https://developer.apple.com/documentation/uikit/uiscrollview/1619439-scrollrecttovisible#declarations)
- [stackoverflow](https://stackoverflow.com/questions/1446536/uiscrollview-works-as-expected-but-scrollrecttovisible-does-nothing)

<br>

# 09. Data에 didSet을 통해 UI를 변경하기
- [참고 링크 RWDevCon 2017 Vault - Advanced Auto Layout](https://videos.raywenderlich.com/courses/81-rwdevcon-2017-vault-tutorials/lessons/2)

```
var separatorsVisible: Bool = false {
  didSet {
    horizontalSeparator.isHidden = !separatorsVisible
    verticalSeparator.isHidden = !separatorsVisible
  }
}
```

<br>

# 10. 오토레이아웃 우선순위를 이용하는 애니메이션 트릭 (Lyft)
- 새로운 데이터가 Set 되는 순간, 특정 constraint를 deActive 하고, 다음차 우선순위 constraint가 적용되게끔 하는 테크닉
- [참고 링크 RWDevCon 2017 Vault - Advanced Auto Layout](https://videos.raywenderlich.com/courses/81-rwdevcon-2017-vault-tutorials/lessons/2)

```
func setTextLabel(_ textData: String?, animated: Bool = true) {
  label.text = textData
  labelContainer.layoutIfNeeded()
  labelWidth.isActive = textData != nil
  UIView.animate(withDuration: animated ? 0.25 : 0, animations: layoutIfNeeded)
}
```

<br>

# 11. Machine Learning in iOS
- [참고 링크 RWDevCon 2017 Vault - Machine Learning in iOS](https://videos.raywenderlich.com/courses/81-rwdevcon-2017-vault-tutorials/lessons/3)


<br>


# 12. iOS Concurrency
- [참고 링크 RWDevCon 2017 Vault - iOS Concurrency](https://videos.raywenderlich.com/courses/81-rwdevcon-2017-vault-tutorials/lessons/4)

### DispatchGroup
- 일련의 concurrent task를 하나의 DispatchGroup 내로 묶어놓을 수 있다.
- 일련의 concurrent task가 끝나면 DispatchGroup을 통해 **MainThread에 Notify**하여 가장 마지막 Task 처리
- ex) `let animationGroup = DispatchGroup()`

```
extension UIView {
  static func animate(withDuration duration: TimeInterval, animations: @escaping () -> Void, group: DispatchGroup, completion: ((Bool) -> Void)?) {
    group.enter()
    animate(withDuration: duration, animations: animations) { (success) in
      completion?(success)
      group.leave()
    }
  }
}
```

```
animationGroup.notify(queue: DispatchQueue.main) {
  //Do Something
}
```

### Race Condition
- concurrent로 multi thread가 돌 때 같은 변수를 서로 다른 스레드에서 동시에 read / write 할 경우 Race Condition이 발생할 수 있다.

```
let workerQueue = DispatchQueue(label: "a", attributes: .concurrent)
let group = DispatchGroup()
```

### ThreadSafe with DispatchBarrier
- Race Condition을 피하기 위해서는 동시에 같은 변수를 read/write하지 못하도록 barrier 한다.
- `.async(flags: .barrier)`

```
let isolationQueue = DispatchQueue(label: "isolation", attributes: .concurrent)

override func changeProperty(first: String, second: String) {
    isolationQueue.async(flags: .barrier) {
        super.changeName(first: firstName, second: lastName)
    }
}
    
override var first: String {
    return isolationQueue.sync {
        return super.first
    }
}
``` 


<br>

# 13. Reconstructing Popular iOS Animations
- [참고 링크 RWDevCon 2017 Vault - Reconstructing Popular iOS Animations](https://videos.raywenderlich.com/courses/81-rwdevcon-2017-vault-tutorials/lessons/5)

<br>

# 14. Server Side Swift with Vapor
- [Server Side Swift with Vapor](https://videos.raywenderlich.com/courses/115-server-side-swift-with-vapor/lessons/1)

<br>

# 15. 콜렉션뷰 Cell의 ContentView
- 기본적으로 다른 설정을 하지 않는다면 cell의 `contentView.translatesAutoresizingMaskIntoConstraints = true` 이다.
- 그런데 `contentView.translatesAutoresizingMaskIntoConstraints` 값을 커스텀 Cell에서 false 로 바꿔서 오토레이아웃을 사용하겠다고 하게되면, Cell의 Height과 Width 컨스트레인트가 없기 때문에 ambiguous layout이 된다.
- 이 때 스토리보드에서 Cell에 subView들을 오토레이아웃을 걸어두었다면 이 subView들의 오토레이아웃 또한 ambiguous layout이 된다.
- 때문에 커스텀 Cell에서 `self.contentView.translatesAutoresizingMaskIntoConstraints = false` 해주게 될 경우에는 contentView의 constraint를 무조건 잡아줘야 스토리보드에서 걸어둔 subView들의 오토레이아웃을 유지할 수 있다.

```
// Example
self.addConstraint(self and contentView top)
self.addConstraint(self and contentView bottom)
self.addConstraint(self and contentView left)
self.addConstraint(self and contentView right)
```
 

# 16. OptionSet, 비트 연산
- [A type that presents a mathematical set interface to a bit set](https://developer.apple.com/documentation/swift/optionset)
- [Advanced Operators](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html)

```
struct ShadowDirection: OptionSet {
    let rawValue: Int
    static let left  = ShadowDirection(rawValue: 1 << 0)
    static let right = ShadowDirection(rawValue: 1 << 1)
    static let up  = ShadowDirection(rawValue: 1 << 2)
    static let down  = ShadowDirection(rawValue: 1 << 3)
    static let all: ShadowDirection = [.left, .right, .up, .down]
}

```

# 17. KVO
- [Key Value Observing](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- Key-value observing is a mechanism that allows objects to be notified of changes to specified properties of other objects.


```
//Example
scrollView.addObserver(self, forKeyPath: #keyPath(UIScrollView.contentOffset), options: [.new, .old], context: &OffsetContext)
```

```
override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
}
```

# 18. 자료형과 버스 크기
- 32비트 컴퓨터 -> 4바이트 / 64비트 컴퓨터 -> 8바이트
- In computer architecture, **a bus is a communication system that transfers data between components inside a computer**, or between computers. This expression covers all related hardware components (wire, optical fiber, etc.) and software, including communication protocols.
- Early computer buses were parallel electrical wires with multiple connections, but the term is now used for any physical arrangement that provides the same logical function as a parallel electrical bus. Modern computer buses can use both parallel and bit serial connections, and can be wired in either a multidrop (electrical parallel) or daisy chain topology, or connected by switched hubs, as in the case of USB.

# 19. Accessibility
- `UIAccessibilityIsVoiceOverRunning`
- Returns a Boolean value indicating whether VoiceOver is running.
- You can use this function to customize your application’s UI specifically for VoiceOver users. For example, you might want UI elements that usually disappear quickly to persist onscreen for VoiceOver users. Note that you can also listen for the UIAccessibilityVoiceOverStatusChanged notification to find out when VoiceOver starts and stops.
- 참고: [UIAccessibilityVoiceOverStatusChanged](https://developer.apple.com/documentation/uikit/uiaccessibilityvoiceoverstatuschanged)

# 20. inline function
- CPU는 메모리를 alloc할 때 연산을 가장 많이 하기 때문에, 메모리 alloc을 줄여주는 것이 성능 향상에 좋다.
- 함수의 동작 원리: 함수 또한 결국 메모리를 alloc 하는 것인데, 함수 내부에서 다른 함수를 호출 할 때 계속 메모리를 할당하게 된다.
- 매우 간단한 연산을 하는 함수의 경우 inline을 사용하면 함수 내부에서 다른 함수를 호출할 때 메모리를 alloc하지 않고 인라인 함수들을 복붙 형태로 내부에 임베드 하게 된다.


# 21. Animation
- [Animation - Transform / Spring](https://medium.com/written-code/ui-animations-with-swift-2ebb5e6d2292)
- [YYImage](https://juejin.im/post/5a2df947f265da43163d03ca)


# 22. Extra
- [Regular Expression](https://koenig-media.raywenderlich.com/downloads/RW-NSRegularExpression-Cheatsheet.pdf)
- [Blurred Effect](https://stackoverflow.com/questions/17041669/creating-a-blurring-overlay-view)

```
//Stackoverflow
//https://stackoverflow.com/questions/17041669/creating-a-blurring-overlay-view
//only apply the blur if the user hasn't disabled transparency effects

if !UIAccessibilityIsReduceTransparencyEnabled() {
    view.backgroundColor = .clear

    let blurEffect = UIBlurEffect(style: .dark)
    let blurEffectView = UIVisualEffectView(effect: blurEffect)
    //always fill the view
    blurEffectView.frame = self.view.bounds
    blurEffectView.autoresizingMask = [.flexibleWidth, .flexibleHeight]

    view.addSubview(blurEffectView) //if you have more UIViews, use an insertSubview API to place it where needed
} else {
    view.backgroundColor = .black
}
```

# 23. Layout guide
- [Identifiers to Debugging NSLayoutConstraints](https://useyourloaf.com/blog/using-identifiers-to-debug-autolayout/)
- [`automaticallyAdjustsScrollViewInsets`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621372-automaticallyadjustsscrollviewin)
- [`edgesForExtendedLayout`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621515-edgesforextendedlayout)
- [	`extendedLayoutIncludesOpaqueBars`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621404-extendedlayoutincludesopaquebars)
