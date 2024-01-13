<h2>Navigation</h2>

<h3>Navigation의 구성요소의 부분</h3>

<p>

1. **NavController:** 앱의 화면 간 이동을 담당한다.
2. **NavGraph:** 이동할 `Composable` 대상을 매핑한다.
3. **NavHost:** `NavGraph`의 현재 대상을 표시하는 컨테이너 역할을 한다.
 
</p>

<br>

<h3>Navigation의 경로</h3>

<p>

Composable 앱에서 탐색의 기본 개념 중 하나는 경로다. 경로는 대상에 매핑되어 고유한 식별자
역할을 하는 문자열이다. 대상이 되는 것들은 일반적으로 **사용자에게 표시되는 항목에 상응하는 단일 
Composable이거나 Composable** 그룹이다. 

</p>

<br>

<h3>NavHost 추가</h3>

<p align="center">

<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-compose-navigation/img/fae7688d6dd53de9.png?hl=ko" width=350, height=350/>

</p>

- **`navController`:** `NavHostController` 클래스의 인스턴스이다. `navigate()` 메서드를 호출하여
다른 대상으로 이동하는 등의 방식으로 화면 간의 이동을 위해 이 객체를 사용한다.
- **`startDestination`:** 앱에서 `NavHost`를 처음 표시할 때 기본적으로 표시되는 대상을 정의하는 문자열 경로다.

>[!NOTE]
> `rememberNavController` 메서드를 호출하여 `NavHostController` 객체를 가져올 수 있다.

```
/**
 * 예제 코드
 * rememberNavController()로 NavigationController를 반환받음
 * NavHost 함수를 선언하면서 불러온 컨트롤러 객체와 시작 화면에 보여줄 경로를 지정함
 */
@Composable
fun CupcakeApp(modifier: Modifier = Modifier) {
    val navController = rememberNavController()
    
    Scaffold( ... ) { innerPadding ->
        val uiState by viewModel.uiState.collectAsState()
        
        NavHost(
            navController = navController,
            startDestination = CupcakeScreen.Start.name,
            modifier = modifier.padding(innerPadding)
        )
    }
}
```

<br>

<h3>NavHost에서 경로 처리</h3>

<p align ="center">

<img src = "https://developer.android.com/static/codelabs/basic-android-kotlin-compose-navigation/img/f67974b7fb3f0377.png?hl=ko" width=350, height=200/>

</p>

- **route:** 경로 이름에 해당하는 문자열이다.
- **content:** 여기에서 특정 경로에 표시할 Composable을 호출한다.

>[!NOTE]
> `composable()` 함수는 **NavGraphBuilder**의 확장 함수이다.

```
/**
 * 예제 코드
 * 여러 개의 Composable 경로를 설정할 수 있음
 */
 NavHost(
   navController = navController,
   startDestination = CupcakeScreen.Start.name,
   modifier = modifier.padding(innerPadding)
) {
    composable(route = CupcakeScreen.Start.name) {
        StartOrderScreen(
            quantityOptions = quantityOptions
        )
    }
    
    composable(route = CupcakeScreen.Flavor.name) {
        val context = LocalContext.current
        SelectOptionScreen(
            subtotal = uiState.price,
            options = flavors.map { id -> context.resources.getString(id) },
            onSelectionChanged = { viewModel.setFlavor(it) }
        )
    }

}
 
```

<br>

<h3>화면 간 이동하기</h3>

화면 간 이동, 즉 경로 간 이동을 위해선 `NavHostController` 인스턴스에서 `navigate()` 메서드를 호출하면 된다.

<img src = "https://developer.android.com/static/codelabs/basic-android-kotlin-compose-navigation/img/fc8aae3911a6a25d.png?hl=ko" width=400, height=50/>

- `route`는 가고 싶은 경로에 해당하는 문자열이다.

<br> 

<h3>첫 화면으로 되돌아기기</h3>

<img src = "https://developer.android.com/static/codelabs/basic-android-kotlin-compose-navigation/img/2f382e5eb319b4b8.png?hl=ko" width=400, height=50/>

- `route`는 다시 돌아갈 대상의 경로를 나타내는 문자열
- `inclusive`는 부울 값으로, true인 경우 지정된 경로를 삭제한다. false인 경우 `route` 대상 위의 모든 대상을 삭제하고, 시작 대상을 사용자에게 표시되는 최상단 화면으로 둔다.

<br>

<h3>다른 앱으로 이동하기</h3>

앱 간 이동을 위해선 `Intent`를 사용한다. 인텐트를 문자열과 같은 일부 데이터와 함께 제공하고 해당 데이터에 적절한 작업을 할 수 있다.

다음 방법은 특정 문자열을 외부 앱에 공유하는 방법이다.

1. 인텐트 객체를 생성하고 `ACTION_SEND` 등의 인텐트를 지정한다.
2. 인텐트와 함께 전송되는 추가 데이터의 유형을 지정한다. `"text/plain`, `"image/*"`, `video/*` 등 여러 가지 유형이 존재한다.
3. `putExtra()` 메서드를 호출하는 방식으로 공유할 텍스트 또는 이미지와 같은 추가 데이터를 인텐트에 전달한다.
4. 컨텍스트의 `startActivity()` 메서드를 호출하여 인텐트에서 생성된 활동을 전달한다.

```
val context = LocalContext.current
val subject = "title"
val summary = "body"
val intent = Intent(Intent.ACTION_SEND).apply {
    type = "text/plain"
    putExtra(Intent.EXTRA_SUBJECT, subject)
    putExtra(Intent.EXTRA_TEXT, summary)
}

context.startActivity(
    Intent.createChooser (
        intent,
        context.getString(R.string.new_cupcake_order)
    )
)
```

<h3></h3>




