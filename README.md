# 계산기

## 🗒︎목차

1.[소개](#소개)
2.[클래스 다이어그램](#클래스-다이어그램)
3.[개발환경 및 라이브러리](#개발환경-및-라이브러리) 
4.[팀원](#팀원)
5.[타임라인](#타임라인)
6.[트러블 슈팅 및 고민](#트러블-슈팅-및-고민)
7.[참고링크](#참고-링크)

## 소개
입력 순서대로 계산하는 계산기 앱 구현

## 클래스 다이어그램
<img width="1046" alt="스크린샷 2022-09-23 오후 6 57 21" src="https://user-images.githubusercontent.com/57447946/191940253-ee53d10b-35fe-4543-9201-738711b17c65.png">

## 개발환경 및 라이브러리
[![swift](https://img.shields.io/badge/swift-5.6-orange)]()
[![xcode](https://img.shields.io/badge/Xcode-13.4.1-blue)]()

## 팀원
[Aaron](https://github.com/Hashswim)

## 타임라인
[Commit History]<img width="1178" alt="image" src="https://user-images.githubusercontent.com/57447946/191939321-df0245c9-635d-496f-ab7b-c11aece14271.png">

## 트러블 슈팅 및 고민
 - CalculatorItemQueue내부의 enqueue와 dequeue의 타입 고민
```swift
    var enqueue: [CalculateItem]
    var dequeue: [CalculateItem] = []
```
사용자로 부터 입력받는 연산자와 수를 함께 담기 위해,
위와 같이 CalculateItem을 채택하는 타입들을 담는 형식으로 구현

## 참고 링크
https://developer.apple.com/documentation/xctest
https://docs.swift.org/swift-book/LanguageGuide/Protocols.html
https://docs.swift.org/swift-book/LanguageGuide/TypeCasting.html
