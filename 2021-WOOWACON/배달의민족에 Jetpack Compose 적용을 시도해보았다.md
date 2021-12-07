## 배달의민족에 Jetpack Compose 적용을 시도해보았다.

### 목차
1. [Jetpack Compose is now 1.0](#)
2. [Jetpack Compose](#)
3. [권장사항](#)
4. [Compose로 앱 만드는 방법](#)
5. [배달의민족에 Compose 적용 해보기](#)
6. [주소 뷰 변경하기](#)
7. [RecyclerView를 Compose로 재구성하기](#)
8. [지도 화면을 전환하기](#)
9. [LottieAnimation을 사용해보자](#)
10. [몇몇 아쉬운 점들](#)
11. [그래서 앞으로는요?](#)

### Jetpack Compose is now 1.0
Jetpack이란, 모든 안드로이드 버전과 기기에 일관되게 작동하는 라이브러리의 모음이라는 뜻이고, 실제로 Compose도 모든 버전과 기기에서 일관되게 작동한다는 장점이 있음. 현재 ```View``` 클래스는 Target SDK 버전에 따라서 동작이 조금씩 다를 수 있고 ```View``` 클래스 자체도 굉장히 오랜 시간 누적된 구조로 이루어져 있기 때문에 본래 구조를 싹 갈아엎고 Compose를 개발했다고 함.

### Jetpack Compose
- Jetpack Compose의 장점
  1. 코드 감소
  2. 직관적
  3. 빠른 개발과정
  4. 강력한 성능
- Jetpack Compose와 기존 안드로이드 UI가 가지는 차이점
  - 명령형 프로그래밍 모델인 **기존 안드로이드 UI**
    - 뷰를 생성한 **이후**에 데이터 설정
    - 사용자 인터렉션을 통해 업데이트되는 새로운 데이터를 **매번** UI에 동기화 해야 함.
    - 결합도가 높아져 실수를 유발하거나 유지 관리 측면에서 복잡성이 증가할 수 있음.
    ```kotlin
    // 명령형 프로그래밍
    val layout = LinearLayout(context)
    layout.setBackgroundColor(red)
    // 생성 이후 데이터 설정
    layout.removeAllViews()
    val childTextView = TextView(context)
    childTextView.text = "childTextView"
    // 생성 이후 데이터 설정
    layout.addView(childTextView)
    ```
  - 선언형 프로그래밍 모델인 **Jetpack Compose**
    - 생성과 동시에 화면 전체를 생성하고 데이터를 설정
    - 필요한 부분이 있으면 그 부분만 변경하도록 함.
    - 적은 코드로 직관적인 UI를 구성할 수 있음.
    - UI 속성을 순차적으로 나열하는 명령형과는 다르게 파라미터로 속성을 지정함.
    ```kotlin
    // 선언형 프로그래밍
    Column(
        modifier = Modifier
            .background(red)
        // 파라미터로 속성 지정
    ) {
        Text("childText")
    }
    // 모두 생성과 동시에 데이터가 설정됨
    ```
### 권장사항
- **MVVM** 등 단방향 데이터 흐름 아키텍쳐 권장
- targetSdkVersion **30**
- Gradle **7.0**
- Kotlin Version **1.5.21**
- minSdkVersion **21**
- Android Studio **Arctic Fox 사용**

### Compose로 앱 만드는 방법
- 기존 View에 Compose 요소 추가
- 새 화면을 Compose로 생성
- 처음부터 Compose로 구성

### 배달의민족에 Jetpack Compose 적용 해보기
- 기존 View에 Compose 요소를 추가하는 방식을 채택
- 가장 작은 요소부터 재구성하기(```View``` -> ```Layout``` 순으로 변경)

### 주소 뷰 변경하기</br>
<img src="https://user-images.githubusercontent.com/18213322/144961395-f24e8906-361d-4a15-b79b-85adde2c9c53.png" width="300" height="135"/></br>
이것을 Compose로 변경

   * **주소 TextView를 Compose로 변환하기**</br>
     - 기존 코드 
         ```xml
         <LinearLayout
             android:id="@+id/locationLayout"
             ... >
             <TextView
                 android:id="@+id/locationTextView"
                 android:layout_width="wrap_content"
                 android:layout_height="match_parent"
                 android:textColor="@color/text_white"
                 tools:text="올림픽로 295"
                 />
             <ImageView
                 ... />
         </LinearLayoout>
         ```
     - 변경된 코드
         ```xml
         <LinearLayout
             android:id="@+id/locationLayout"
             ... >
             <androidx.compose.ui.platform.ComposeView
                 android:id="@+id/locationTextComposeView"
                 android:layout_width="wrap_content"
                 android:layout_height="match_parent"/>
             <ImageView
                 ... />
         </LinearLayoout>
         ```

         ```kotlin
         binding?.locationTextComposeView?.setContent {
             LocationText(viewModel.getTitle)
         }

         @Composable
         fun LocationText(title: LiveData<String>) {
             val text = title.observeAsState()

             title.value?.let {
                 Text(
                     text = it,
                     fontSize = dpToSp(dp = 14.dp),
                     fontWeight = FontWeight.Bold,
                     color = colorResource(id = R.color.text),
                     textAlign = TextAlign.Center,
                     overflow = TextOverflow.Ellipsis,
                     ...
                 )
             }
         }
         ```
         * androidx.compose.ui.platform.ComposeView는 XML 안에 Compose를 사용할 수 있도록 하는 태그임
         * ComposeView 태그를 사용한 XML 코드는 '이 영역만 Compose를 사용하겠다' 라고 지정해놓은 것이기 때문에 setContent라는 메서드를 사용해서 Composable 함수를 넣어주어야 함 
         * LocationText Composable 함수에 주소 title을 viewModel에서 가져와서 파라미터로 넘겨줌
         * LocationText 함수 안에서 title LiveData를 StateString 타입으로 만들어서 Text를 지정해 주었음
   </br>
   </br>

   * **주소 설정 LinearLayout을 Compose로 변경하기**</br>
     - Compose Basic Layout
         - Compose에는 아래의 총 3개 레이아웃이 존재함
           * **Column**(세로 LinearLayout)
           * **Row**(가로 LinearLayout)
           * **Box**(FrameLayout)
         - ConstraintLayout은 복잡한 레이아웃을 구성할 때 주로 사용했었음. 중첩된 뷰 계층구조보다 플랫 뷰 계층구조가 성능이 우수했기 때문임.
         - 그러나 Compose는 바뀐 데이터만 다시 불러오기 때문에 한번 그린 후에 초기화가 되지 않아 깊은 계층 구조에도 효율적으로 처리할 수 있음.

     - 기존 코드
         ```xml
         <LinearLayout
             android:id="@+id/locationLayout"
             ... >
             <androidx.compose.ui.platform.ComposeView
                 android:id="@+id/locationTextComposeView"
                 android:layout_width="wrap_content"
                 android:layout_height="match_parent"/>
             <ImageView
                 ... />
         </LinearLayoout>
         ```
         ```kotlin
         binding?.locationTextComposeView?.setContent {
             LocationText(viewModel.getTitle)
         }

         @Composable
         fun LocationText(title: LiveData<String>) {
             val text = title.observeAsState()

             title.value?.let {
                 Text(
                     text = it,
                     fontSize = dpToSp(dp = 14.dp),
                     fontWeight = FontWeight.Bold,
                     color = colorResource(id = R.color.text),
                     textAlign = TextAlign.Center,
                     overflow = TextOverflow.Ellipsis,
                     ...
                 )
             }
         }
         ```
     - 변경된 코드
         ```kotlin
         @Composable
         fun LocationLayout(title: LiveData<String>) {
             Row(
                 modifier = Modifier.fillMaxSize(),
                 vertialAlignment = Alignment.CenterVertically,
             ) {
                 LocationText(text = title)
                 Spacer(modifier = Modifier.size(4.dp))
                 LocationIconImage()
             }
         }

         @Composable
         fun LocationIconImage() {
             Image(
                 painter = painterResource(R.drawable.icon_open),
                 ...
             )
         }
         ```
         * setContent 람다 안에 LocationText를 넣어준 것을 삭제하고 LocationLayout이라는 새로운 Composable 함수를 만들어 람다에 설정해주었음
         * LocationLayout 안에서 가로 LinearLayout을 Compose로 변환하기 위해서 Row 레이아웃을 사용함.
         * 그 안에 기존에 만들어두었던 LocationText와 Spacer, ImageView를 바꿔줄 Image Compose가 있는 LocationIconImage Composable 함수를 가로로 쌓아 주었음.
  
### RecyclerView를 Compose로 재구성하기
  - Compose는 뷰를 재활용하는 것이 더 많은 비용이 든다는 판단을 하여, 뷰를 재활용하지 않음.
  - 하지만, 모든 부분을 한번에 그리는 것은 당연히 훨씬 많은 비용이 들기 때문에, 뷰를 Lazy하게 그린다는 개념으로 보이는 것만 그려주는 ```LazyRow```/```LazyColumn```이라는 Compose가 있음.</br>
  [![스크린샷 2021-12-07 오후 2 17 27](https://user-images.githubusercontent.com/18213322/144970868-fa2ed618-120f-4e09-b01d-fd6459e5cb1e.png)](https://developer.android.com/jetpack/compose/lists#lazylistscope)
  - ```RecyclerView``` Compose로 재구성할 때 모든 ```Adapter``` 함수와 ```ViewHolder``` 클래스를 변경해주어야 하기 때문에 시간이 굉장히 오래 걸림.
  - 하지만 리사이클러뷰를 몇개의 ViewHolder만 Composable로 변환할 수 있는 방법이 없진 않았음.
  - Compose 뷰가 재활용되지 않기 때문에 계속 쌓이면서 성능이 저하되는 문제점이 있었는데 RecyclerView의 Delegate인 onViewRecycled가 호출될 때 마다 Compose 뷰의 disposeCompsition을 불러 주도록 수정했음.
  - 의외로 Compose로 구성된 ```ViewHolder```와 ```Adapter```을 함께 사용했을 때 성능 저하가 크지 않았음.

### 지도 화면을 전환하기
  - 네이버 지도 라이브러리를 사용 중
  - 네이버 지도 SDK는 Compose를 지원하지 않으므로 ```AndroidView```로 감싸서 사용하였음.
  ```kotlin
  val coroutineScope = rememberCoroutineScope()
  val savedInstanceState = rememberSavedInstanceState()
  val mapView = rememberMapViewWithLifecycle(savedInstanceState)
  AndroidView(
      factory = {
          mapView.apply {
              coroutineScope.launch {
                  // 네이버 지도 객체를 코루틴으로 가져온다
                  val naverMap = suspendCoroutine<NaverMap> { continuation ->
                    getMapAsync {
                        continuation.resume(it)
                    }
                  }

                  // 지도에 관한 기본 설정(줌 설정을 하고 있음)
                  naverMap.apply {
                      minZoom = Constants.ZOOM_LEVEL_MIN.toDouble()
                      maxZoom = Constants.ZOOM_LEVEL_MAX.toDouble()
                      uiSettings.defaultSetting()
                  }
                  
                  // 지도에 관련된 콜백 지정
                  naverMap.addOnCameraIdleListener { onCameraIdle() }
                  naverMap.addOnCameraChangeListener { i, b -> onCameraChange(i, b) }
                  onMapReady(naverMap)
              }
          }
      }
  )
  ```

  ```kotlin
  // 지도의 Lifecycle 설정
  val lifecycle = LocalLifecycleOwner.current.lifecycle
    DisposableEffect(lifecycle, mapView, savedInstanceState) {
        val lifecycleObserver = LifecycleEventObserver { _, event ->
            when (event) {
                Lifecycle.Event.ON_CREATE -> mapView.onCreate(
                    savedInstanceState.takeUnless { it.isEmpty })
                Lifecycle.Event.ON_START -> mapView.onStart()
                Lifecycle.Event.ON_RESUME -> mapView.onResume()
                Lifecycle.Event.ON_PAUSE -> mapView.onPause()
                Lifecycle.Event.ON_STOP -> mapView.onStop()
                Lifecycle.Event.ON_DESTROY -> mapView.onDestroy()
                else -> throw IllegalStateException()
            }
        }
        lifecycle.addObserver(lifecycleObserver)
        onDispose {
            // Compose Dispose 시
            mapView.onSaveInstanceState(savedInstanceState)
            lifecycle.removeObserver(lifecycleObserver)
        }
    }
  ```

### LottieAnimation을 사용해보자
- 다양한 애니메이션으로 활용
- 4.2.0 버전부터 Compose 지원
- 띠링띠링 움직이는 종을 Compose로 바꿔보자</br>
![image](https://user-images.githubusercontent.com/18213322/144978230-eb05f95f-6157-46e9-b96d-f61392094c74.png)

- LottieView in View(기존 구조)
  ```kotlin
  // 커스텀 뷰 사용
  class FloatingCartView {
      fun updateCartCount() {
          getCartItemCountUseCase?.excute(onNext = { newCartCount ->
             // Lifecycle에 의해 실행
             val foodCount = cartCount.count
             val newCount = newCartCount.count
             val differenceCount = newCount - foodCount

             setCartCount(newCount)
             playAddAnimation(differenceCount)
            
             cartCount = newCartCount
          })
      }

      
      private fun playAddAnimation(differenceCount: Int) {
          if (floatingCartImageView.isAnimating) {
              floatingCartImageView.cancelAnimation()
          }
          floatingCartImageView.setAnimation("cart_ani.json")
          floatingCartImageView.playAnimation()
          animateCountPlusCount(differenceCount)
          // 아이템 변화가 있을 때 애니메이션 실행
      }
  }

  ```

- LottieView in Compose(Lottie 4.2.0 이하 버전)
  ```kotlin
  @Composable
  fun FloatingActionCartViewCompose(
      getCartItemCountUseCase = GetCartItemCountUseCase(),
      clickable: () -> Unit = {},
  ) {
      Box(
          modifier = Modifier
            ...
            .clickable { clickable() }
            // 클릭 이벤트 처리
      ) {
          ...
          val eventHandler = remember { mutableStateOf(Lifecycle.Event.ON_ANY) }
          val lifecycleOwner = rememberUpdatedState(LocalLifecycleOwner.current)
          DisposableEffect(lifecycleOwner.value) {
              val lifecycle = LifecycleEventObserver { owner, event ->
                  eventHandler.value = event
              }
              lifecycle.addObserver(observer)
              onDispose {
                  // Compose Dispose 시
                  lifecycle.removeObserver(observer)
              }
          }
      }

      eventHandler.value.let {
          AndroidView(factory = {
              val view = LottieAnimationView(it).apply {}
              return@AndroidView view
          }, update = {
              // 뷰 Recomposition 시 불러짐
              if (event == Lifecycle.Event.ON_RESUME) {
                  getCartItemCountUseCase.excute(onNext = { newCartCount ->
                      // 숫자 업데이트 하는 로직을 넣어줌
                      ...
                      playAddAnimation(differenceCount, it)
                  })
              }
          })
      }
  }
  ```

- LottieCompose (Lottie 4.2.0 이상)
  ```kotlin
  val composition by rememberLottieComposition(
      LottieCompositionSpec.Asset("cart_ani.json")
  )
  val state = remember { CartAniState() }
  LaunchedEffect(eventHandler.value) {
      if (eventHandler.value = Lifecycle.Event.ON_RESUME) {
          state.animatable.animate(
              composition,
              iterations = 1,
              initalProgress = 0f,
              speed = 1f,
              continueFromPreviousAnimate = false,
          )
      }
  }

  LottieAnimation(
      composition,
      progress = state.animatable.progress
  )

  ```
  * ```AndroidView```로 되어있던 Compose만 ```LottieCompose```로 변환해줌
  * 기존에 Recomposition이 일어날 때 ```update``` 람다가 사용되었는데, Compose를 Recomposition 시켜주기 위해 Coroutine을 통해서 ```eventHandler```를 설정.
  * ```eventHandler``` 값이 변할 때 마다 ```state.animatable.animate``` 함수를 불러 데이터를 변환해줌
  * Lottie 관련 코드가 3줄로 간략해지는 것을 볼 수 있음

### 몇몇 아쉬운 점들

    - 몇몇 라이브러리는 사용할 수 없음
      * ExoPlayer => ```AndroidView```를 사용하거나 나중에 전환하기
      * 이미지 라이브러리(Glide, Piccaso) => 지원 계획 없음, Coil로 대체하기
      * ```MotionLayout```
      * ```ViewPager```
    - Resource 관련 사용할 수 없는 점들
      * Shape Drawable 사용 불가능 (xml)
      * Selector Drawable 사용 불가능
      * ColorStateList 사용 불가능
      * 나인패치 이미지 사용 불가능
    - 개발 생산성은??
      * MVVM으로 대체 언제 옮겨갈까
      * 러닝 커브가 확실히 존재
      * Preview 만으로는 xml에서 작성하는 것보다는 불편하다 -> 생산성 저하
      * Build 시간, apk 사이즈

### 그래서 앞으로는요?
    - 그래도 이제는 조금씩 도입할 때
      * 팀 내에서 학습하며 러닝커브를 극복
      * 간단한 화면부터 적용해 볼 예정
      * 간결하고 UI 위주의 코드는 👍
      * 아키텍쳐부터 천천히 고민해보자
