# 계산기

## 🗒︎목차

1.[소개](#소개)
2.[개발환경 및 라이브러리](#개발환경-및-라이브러리) 
3.[팀원](#팀원)
4.[타임라인](#타임라인)
5.[트러블 슈팅 및 고민](#트러블-슈팅-및-고민)
6.[참고링크](#참고-링크)

## 소개
입력 순서대로 계산하는 계산기 앱 구현

## 개발환경 및 라이브러리
[![swift](https://img.shields.io/badge/swift-5.6-orange)]()
[![xcode](https://img.shields.io/badge/Xcode-13.4.1-blue)]()

## 팀원
[Aaron](https://github.com/Hashswim)

## 타임라인

<details>
<summary>Commit History</summary>
<div markdown="1">

<img width="1178" alt="image" src="https://user-images.githubusercontent.com/57447946/191939321-df0245c9-635d-496f-ab7b-c11aece14271.png">
<img width="1227" alt="image" src="https://user-images.githubusercontent.com/57447946/193221250-a664b0ef-055d-49da-8849-d7639d10613d.png">
<img width="1216" alt="image" src="https://user-images.githubusercontent.com/57447946/193221417-77c91fc8-6643-49d6-ae7f-5e2a6f89ab1a.png">
<img width="1212" alt="image" src="https://user-images.githubusercontent.com/57447946/193221505-a382ee49-db7d-4d11-b7e3-f57a2371abfe.png">
<img width="1204" alt="image" src="https://user-images.githubusercontent.com/57447946/193221585-6ea3a3d3-b334-482a-8206-30d119b435ab.png">

</div>
</details>

## 트러블 슈팅 및 고민
 - CalculatorItemQueue내부의 enqueue와 dequeue의 타입 고민
```swift
    var enqueue: [CalculateItem]
    var dequeue: [CalculateItem] = []
```
사용자로 부터 입력받는 연산자와 수를 함께 담기 위해,
위와 같이 CalculateItem을 채택하는 타입들을 담는 형식으로 구현

- UML에 대한 이해
 
> <img width="195" alt="image" src="https://user-images.githubusercontent.com/57447946/192817917-99578250-c489-4e96-a8bc-71cdb07a8c4f.png">
해당 부분에서 String을 확장해 split 메서드를 오버로딩 하는 요구사항이 있었습니다.
제가 처음 이해한 UML은 split의 기존 기능이 아니라 String내부의 특정 Character "하나에" 대해서 둘로 나누는 것으로
이후 ExpressionParser에서 string내부의 모든 연산자에 대해서 각각(중복되는 요소에 대해서도) split하며 저장하는 것으로 이해.

하지만 생각한 방식은 연산자가 많아진다면 불필요하게 반복되므로 중복되는 원소에 대해서도 모두 나누어지도록 아래와 같이 구현.
```swift
    func split(with target: Character) -> [String] {
        return self.components(separatedBy: String(target))
    }
```

- CalculateItem 타입에 대한 처리
step 2를 진행하기 전 구현한 부분은 Double과 Int, String. 모두 하나의 배열에 저장할 수 있도록 구현.
하지만 step 2를 진행하며 operands와 operators를 나누며 Double과 Operator타입으로 각각 담기도록 요구되어 
CalculateItemQueue를 제네릭 타입으로 받을까 고민.
```swift
//아래를
struct CalculatorItemQueue {
    var enqueue: [CalculateItem]
    var dequeue: [CalculateItem] = []
    ...
// 이렇게 바꿀까?
struct CalculatorItemQueue<T: CalculateItem> {
    var enqueue: [T]
    var dequeue: [T]
}
```
원래대로 코드 구현한다면 접근해서 내부의 원소를 사용할 때마다 타입캐스팅을 일일이 해주어야 함.
하지만 이후 step 3를 진행하면서 [String]이 아닌 [CalculateItem]으로 바로 받아올 수 있을 것 같다는 생각이 들어 수정하지 않고 그대로 진행.

- 접근 제어에 대한 고민
> <img width="239" alt="image" src="https://user-images.githubusercontent.com/57447946/192823245-8f025d05-c185-4e1a-8d05-443487b34ab4.png">
위를 보고 componentsByOperators 메서드는 static private으로 구현.
static 메서드에서만 접근이 가능하도록 하고 외부에서는 보이지 않도록 하는 것이 소프트웨어 아키텍처 관점에서 맞다고 이해.

- 고차함수 적용
     - `result()` 연산로직 `reduce()`사용
     ```swift
     let result = try operands.mergedQueue.reduce(0.0) { accumlatingResult, nextOperand in
                guard let nowOperator = operators.popFirst() as? Operator else {
                    throw CalculateError.invalidOperator
                }
                guard let operands = nextOperand as? Double else {
                    throw CalculateError.invalidOperand
                }

                return try nowOperator.calculate(lhs: accumlatingResult, rhs: operands)
            }
            return result
     ```
     - `parse()` 문자열 처리 `filter()`, `compactMap()` 사용
     ```swift
     static func parse(from input: String) -> Formula {
        let operators = input.filter {
            "0123456789".contains($0) == false }.compactMap {
                Operator(rawValue: $0) }
        
        let operands = componentsByOperators(from: input).compactMap {
            Double($0)
        }
     ```
     
## 참고 링크
https://developer.apple.com/documentation/xctest
https://yagom.github.io/swift_basic/contents/22_higher_order_function/
https://stackoverflow.com/questions/61092791/mapping-a-throwing-function-in-swift-double-try
