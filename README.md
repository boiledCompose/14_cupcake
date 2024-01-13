<h2>Navigation</h2>

<h3>Navigation의 구성요소의 부분</h3>

<p>

1. **NavController:** 앱의 화면 간 이동을 담당한다.
2. **NavGraph:** 이동할 `Composable` 대상을 매핑한다.
3. **NavHost:** `NavGraph`의 현재 대상을 표시하는 컨테이너 역할을 한다.
4. 
</p>

<h3>Navigation의 경로</h3>

<p>

Composable 앱에서 탐색의 기본 개념 중 하나는 경로다. 경로는 대상에 매핑되어 고유한 식별자
역할을 하는 문자열이다. 대상이 되는 것들은 일반적으로 **사용자에게 표시되는 항목에 상응하는 단일 
Composable이거나 Composable 그룹**이다. 

</p>

<h3>NavHost 추가<h3>

<src img="https://developer.android.com/static/codelabs/basic-android-kotlin-compose-navigation/img/fae7688d6dd53de9.png?hl=ko" width=500 height=500/>




