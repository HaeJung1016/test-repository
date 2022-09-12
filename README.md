# test-repository
for PR


## Features
autoLayout 혹은 frame을 지정하여 BackgroundOpaqueView의 사이즈 조정 가능
```Swift
// UIScreen.main.bounds에 맞게 사이즈 조정됨
let backView = BackgroundOpaqueView(useAutoLayout: false)

// translatesAutoresizingMaskIntoConstraints 활성화
// 인스턴스 생성한 곳에서 constraint 설정 필요함
let backView = BackgroundOpaqueView(useAutoLayout: true)
```

## 활용 가능한 property
```Swift
// BackgroundOpaqueView의 색상
private var backViewColor: UIColor = .black
    
// BackgroundOpaqueView의 투명도
private var opacity: CGFloat = 0.7
    
// BackgroundOpaqueView의 show / hide animation의 duration
private var animationDuration: CGFloat = 0.2
```
- IB를 통한 생성 미지원

<br>

## Usage
- 팝업창 혹은 바텀업 메뉴와 관련된 ViewController 생성
- Auto layout을 사용할지 말지 결정 후 BackgroundOpaqueView 인스턴스 생성
- 생성한 인스턴스를 view의 0번째 subview로 add
  - (Auto layout 사용할 경우) BackgroundOpaqueView에 대한 제약조건 추가
  - (배경뷰 터치 시 VC dismiss 필요한 경우) tap gesture를 BackgroundOpaqueView에 추가
- viewDidAppear()에서 배경뷰 보이도록 처리
- 팝업창일 경우 `toggle(isHidden: Bool, hideViews: [UIView?]? = nil, completion: (() -> Void)? = nil)`을 이용해 처리
```Swift
override func viewDidAppear(_ animated: Bool) {
	super.viewDidAppear(animated)
	backView.toggle(isHidden: false)
}
```

<br>

바텀업 메뉴일 경우 `toggle(isMovingToHide: Bool, y: CGFloat, moveView: UIView? = nil, completion: (() -> Void)? = nil)`을 이용해 처리

```Swift
override func viewDidAppear(_ animated: Bool) {
	super.viewDidAppear(animated)
	container.frame.origin.y = screenHeight
	let y = screenHeight - self.container.frame.height - self.view.safeAreaInsets.bottom
        
	backView.toggle(isMovingToHide: false, y: y, moveView: container)
}
```
