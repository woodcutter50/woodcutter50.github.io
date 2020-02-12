---
title: "SwiftUI - SearchBar"
excerpt: "SwiftUI NavigationView 에서 SearchBar 적용하기"
date: 2020-02-04

category: SwiftUI
tags: SwiftUI SearchBar NavigationView
---

# NavigationView 에서 SearchBar 적용하기

### 안녕하세요 woodcutter 입니다.
### 이번 시간에는 SwiftUI의 `NavigationView` 에서 `SearchBar` 적용 방법을 알아보겠습니다.

## SearchBar
### 시작 하기 전 부가 설명이 필요하여 집고 넘어 가겠습니다. 🥢
#### 1. SwiftUI -> SearchBar (X)
- SwiftUI에서는 SearchBar를 제공하지 않습니다.

#### 2. UIViewRepresentable
- UIKit Views 를 SwiftUI와 연결해주는 프로토콜 입니다.

- UIViewRepresentable를 채택하고 UIKit에서 제공하는 UISearchBar를 불러와 구현 해야합니다.


#### 3. UISearchBarDelegate
- UISearchBar의 delegate 프로퍼티는 UISearchBarDelegate 타입입니다.
- delegate 활용하여 text 처리에 필요한 기능을 사용할수 있습니다.

## Code
``` swift
struct SearchBar: UIViewRepresentable {
    
}
```
UIKit 에서 제공하는 UISearchBar를 사용하기 위해서 UIViewRepresentable 채택하면
아래와 같이 에러가 발생합니다!
### Type 'SearchBar' does not conform to protocol 'UIViewRepresentable'
가볍게 오류를 fix 해서 자동완성 해줍니다.

``` swift
struct SearchBar: UIViewRepresentable {
    typealias UIViewType = UISearchBar
}
```
자동완성 해주면 typealias UIViewType가 생성되고 UIKit 에서 제공하는 UISearchBar를 사용하려 했기때문에 UIViewType에 UISearchBar를 추가해 줍니다!

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
- 뷰 를 만들고 만든 뷰 를 리턴 하는데 리턴 타입은 UISearchBar 입니다.
2. updateUIView
- 우리가 직접 생성한 `SearchBar` 를 SwiftUI에서 사용중 변경 사항이 생기는경우 updateUIView가 호출되고 매개 변수인 context를 사용하여 업데이트 합니다.

그럼 makeUIVIew 메서드에 우리가 사용하고 싶은 UISearchBar를 만들겠습니다!
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

이제 SwiftUI의 NavigationView를 사용하여 우리가 만든 SearchBar를 적용해 보겠습니다.

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

NavigationView에는 `VStack` 을 추가 한 다음 컴포넌트의 진행방향, 간격을 설정 하고 `SearchBar` 를 추가해 줍니다.

예쁜? 화면 구성을 위해서 SearchBar 아래 List를 추가했습니다.

실행 해 보시면 정상적으로 SearchBar가 출력됩니다.

그러나 여기에는 중요한 문제가 있습니다.

바로 `UISearchBarDelegate` 를 사용하지 않아서 UISearchBar의 기능을 온전하게 사용할 수 없습니다.

문제는 SwiftUI 는 struct이기때문에 UISearchBarDelegate를 사용할수 없어요~

그래서 `makeCoordinator` 라는 메서드를 이용해서 coordinator를 만들고 coordinator가 delegate 를 구현하여 넘겨줍니다.

SwiftUI는 아직 많은 기능을 제공하지 않기때문에 UIKit의 기능을 사용하가위해선는 반드시 Coordinator 개념을 이해하고 넘어가야합니다.

그래서 다음 시간에는 Coordinator를 사용하여 좀더 완성된 SearchBar를 만들어 보겠습니다.

감사합니다. 

뿅!
