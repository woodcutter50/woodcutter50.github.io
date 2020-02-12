---
title: "SwiftUI SearchBar"
excerpt: "SwiftUI NavigationView 에서 SearchBar 적용하기"
date: 2020-02-04

category: SwiftUI
tags: SwiftUI SearchBar NavigationView
---

# NavigationView 에서 SearchBar 적용하기

### 안녕하세요 woodcutter 입니다.

이번 시간에는 SwiftUI의 `NavigationView` 에서 `SearchBar` 적용 방법을 알아보겠습니다.

## Check
### 시작 하기 전 부가 설명이 필요하여 짚고 넘어 가겠습니다. 🥢
#### 1. SwiftUI -> SearchBar (X)
- SwiftUI에서는 SearchBar를 제공하지 않습니다.

#### 2. UIViewRepresentable
- UIKit Views 를 SwiftUI와 연결해주는 프로토콜 입니다.

- UIViewRepresentable를 채택하고 UIKit에서 제공하는 UISearchBar를 불러와 구현 해야합니다.


#### 3. UISearchBarDelegate
- UISearchBar의 delegate 프로퍼티는 UISearchBarDelegate 타입입니다.
- delegate 활용하여 필요한 추가 기능을 사용할수 있습니다.

## SearchBar
``` swift
struct SearchBar: UIViewRepresentable {
    
}
```
UIKit 에서 제공하는 UISearchBar를 사용하기 위해서 UIViewRepresentable 채택하면!
짠! 시작부터 에러가 발생하네요...ㅋㅋㅋ

### Type 'SearchBar' does not conform to protocol 'UIViewRepresentable'
가볍게 오류를 fix 해서 자동완성 해줍니다.

``` swift
struct SearchBar: UIViewRepresentable {
    typealias UIViewType = UISearchBar
}
```
fix 하면 typealias UIViewType가 생성되고 UIKit 에서 제공하는 UISearchBar를 추가해 줍니다!

그런데 타입을 지정해 주어도 똑같은 오류가 발생합니다...
### Type 'SearchBar' does not conform to protocol 'UIViewRepresentable'
한번 더 fix 해줍니다.

``` swift
struct SearchBar: UIViewRepresentable {
    typealias UIViewType = UISearchBar
    
    func makeUIView(context: UIViewRepresentableContext<SearchBar>) -> UISearchBar {
        
    }
    
    func updateUIView(_ uiView: UISearchBar, context: UIViewRepresentableContext<SearchBar>) {
        
    }
}
```
두개의 메서드가 생성되었네요 
1. makeUIView
- View를 만들고 View를 리턴 하는데 리턴 타입은 UIViewType에 추가한 UISearchBar 입니다.
2. updateUIView
- 우리가 만든 `SearchBar` 를 SwiftUI에서 사용중 업데이트 가 발생하면 updateUIView가 호출되고 매개 변수인 context를 사용하여 업데이트 할수 있습니다.

그럼 makeUIVIew 메서드에 UISearchBar를 만들겠습니다!
``` swift
func makeUIView(context: UIViewRepresentableContext<SearchBar>) -> UISearchBar {
	let searchBar = UISearchBar(frame: .zero)
	searchBar.autocapitalizationType = .none
	searchBar.showsCancelButton = false
	searchBar.searchBarStyle = .minimal
	return searchBar
}

```
UISearchBar를 선언하고 기본설정을 해주었습니다.

다음은 SwiftUI의 NavigationView를 사용하여 우리가 만든 SearchBar를 적용해 보겠습니다.

## NavigationView

``` swift
struct MainView: View {
    var body: some View {
        NavigationView {
            VStack(alignment: .leading, spacing: 0) {
                SearchBar()
                List {
                    ForEach((1...10), id: \.self) { index in
                        Text("\(index)")
                    }
                }
            }
            .navigationBarTitle("타이틀")
        }
    }
}
```
`MainView` 라는 View를 만들었고 body 부분에 `NavigationView` 추가 했습니다

avigationView에는 `VStack` 을 추가 한 다음 컴포넌트의 진행방향, 간격을 설정 하고 `SearchBar` 를 추가해 줍니다.

예쁜? 화면 구성을 위해서 SearchBar 아래 List를 추가했습니다.

실행 해 보시면 정상적으로 SearchBar가 출력됩니다.

완성!!!!👏🏻👏🏻👏🏻 ...이면 좋겠지만 중요한 문제가 있습니다!

바로 `UISearchBarDelegate` 를 사용하지 않아 추가 기능을 구현할 수 없어요!

그래서 `makeCoordinator` 라는 메서드를 이용해서 coordinator 인스턴스를 만들고 delegate를 구현해야 합니다.

``` swift
func makeCoordinator() -> SearchBar.Coordinator {
	return Coordinator(text: $text, showCancelButton: $showCancelButton)
}
```
대략 이런식으로 작성하는데 

Coordinator 사용법은 다음 시간에 이어서 설명 하겠습니다.

감사합니다.

뿅!
