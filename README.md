# Android-Interview-Study-2022 [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fsungbin5304%2FAndroid-Interview-QnA-2020&count_bg=%2340B7EA&title_bg=%23555555&icon=iconify.svg&icon_color=%23FFFFFF&title=study+with+this+repo&edge_flat=false)](https://hits.seeyoufarm.com)

🤓 2021~ 안드로이드 취직 대비 공부 기록터 📖

> [[`공부할거야!` 시리즈 모음]](https://github.com/jisungbin/x-study)
>
> **가독성 정리 필요**

-----

# 안드로이드 `Life-Cycle`에 대하여 설명하시오

사용자가 앱을 `탐색하고`, `나가고`, `다시 들어올 때` 등등의 **상태 변화**에 따라서 이를 앱에서 알아차릴 수 있게 제공하는 `Callback`.

## Activity

1. `onCreate` : `Activity`가 **최초** 생성되고, 필요한 작업의 초기화를 수행하기 위해 호출됨

> **만약 A - > B 갔다가 다시 B -> A 로 이전 `stack`으로 돌아오는 `activity`일 경우, `onCreate` 대신 `onRestart`가 호출됨**

2. `onStart` : `Activity`가 보여지기 위한 모든 작업들의 초기화(`onCreate`)가 끝났고, 상호작용 하기 전에 호출됨
3. `onResume` : `Activity`가 사용자와 상호작용이 시작됐을 때 호출됨
4. `onPause` : 다른 `Activity`가 보여져서 현재의 `Activity`가 가려졌을 때(보이지 않을 때) 호출됨
5. `onStop` : `Activity`가 더 이상 보여지지 않을 떄 호출됨

> 이는 메모리가 부족하여 종료될 시에는 호출되지 **않을** 수 있음<br/>
> 또한 이는 **다른 `Activity`들의 생명주기 작업이 끝나야 호출**됨<br/>
> 마지막으로 이 상태에서 `onSaveInstanceState()`가 호출됨

6. [`onDestroy`](https://stackoverflow.com/a/4449988/14299073) : 시스템 리소스 자원(CPU, 메모리 등등)이 부족해져 앱이 종료되거나, `finish()` 코드를 실행했을 때 호출됨

### Foreground

현재 `Activity` **밖에서** 실행되는 것

### Background

현재 `Activity` **안에서** 다른 작업과 **동시**에 뒤에서 실행되는 것

### 앱 사용 도중에 카카오톡으로 부터 알림이 오면,<br/>사용중인 앱의 `TopActivity`의 `Life-Cycle` 상태는 어떻게 되나요?

`onPause` 상태가 됨

### `onRestart`는 언제 호출되나요?

**`onStart` 호출 전에**, 가려진 `Activity`가 다시 보여질 때 호출됨

## Application

1. `onCreate`: 다른 클래스들이 실행되기 전에 제일 먼저 호출됨
2. `onLowMemory`: 안드로이드의 메모리가 부족할 때 호출됨
3. `onTrimMemory`: 앱이 잘 실행되고 있는데, 안드로이드가 메모리가 부족하여 필요없는 메모리의 정리가 필요할 때 호출됨
4. `onTerminuate`: 안드로이드 에뮬레이터 환경에서만 실행되며, 에뮬레이터가 꺼졌을 때 호출됨
5. `onConfigurationChanged`: 안드로이드 [`구성요소`](https://support.google.com/android/answer/9021432?hl=en-GB)가 변경되었을 때 호출됨

## Layout

1. `onAttachedToWindow` : `View`가 `Window`에 연결되면 호출됨

> `Drawing`할 표면을 알고있는 단계 -> `listener` 설정 가능

2. `measure`
3. `onMeasure` : `View`의 `Size`를 확인하기 위해 호출됨

> `ViewGroup`일 경우 계속해서 `Child View`에 대한 `Size` 측정을 하고, 그에 대한 결과로 자신의 `Size`를 결정

4. `layout`
5. `onLayout` : `View`의 `Size`를 측정하여 `position` 설정 후 호출됨
6. `dispatchToDraw`
7. `draw`
8. `onDraw` : `View`가 그려질 준비가 되었을 때 호출됨

> `onDraw`는 여러번 호출되므로, 여기서는 `객체`를 만들면 안됨<br/>**`Canvas`는 무엇을 그릴지를 담당하고, `Paint`는 어떻게 그릴지를 담당함**

### onDetechedFromWindow

`View`가 `Window`에서 분리될 때 호출됨

> 더 이상 `Drawing`할 `View`가 없음을 알림 -> 모든 작업들을 정리해야 하는 시기

### onFinishInflate

`View` 전개가 끝난 후 호출됨

> `Layout`일 경우, 모든 `Child View`가 추가 된 후에 호출됨

### MeasureSpec

`MeasureSpec`은 `parent`에서 `child`으로 전달되는 `Layout`들의 요구 사항을 캡출화함

#### MeasureSpec.EXACTLY

`Parent View`가 `Child View`의 정확한 `Size`를 결정함.

> `Child View`의 `Size`에 관계없이 무조건 정해진 `Size`로 설정함

#### MeasureSpec.AT_MOST

`Child View`는 지정된 `Size`까지의 원하는 만큼 `Size`를 정할 수 있음

#### MeasureSpec.UNSPECIFIED

`Parent View`가 `Child View`의 `Size`에 대해 제한을 두지 않음

> `Child View`는 원하는 만큼의 `Size`를 정할 수 있음

### `requestLayout()`과 `invalidate()`의 차이점

#### requestLayout()

`measure`부터 `Layout`을 재호출

#### invalidate()

`dispatchToDraw()`부터 `Layout`을 재호출

## Fragment

1. `onAttach` : `Fragment`가 `Activity`에 `attach`될 때

> `argument`로 `Context`가 들어옴

2. `onCreate` : `Fragment`가 생성될 때 호출되며 사용자 `interface` 초기화에 사용됨

> `UI` 초기화는 할 수 **없음**

3. `onCreateView` : `Layout`을 `inflate`하는 곳

> `UI` 초기화를 할 수 **있음**

4. `onActivityCreated` : `Activity`의 `Fragment`의 `View`가 모두 생성된 상태

> `UI` 변경 작업을 하는 곳

5. `onStart` : `Fragment`가 보여지기 **바로 전**에 호출됨
6. `onResume` : `Fragment`가 사용자와 상호 작용하기 **바로 전**에 호출됨
7. `onPause` : 다른 `Activity`가 보여질 때 호출됨

> `Fragment`의 `Parent Activity`가 아닌 다른 `Activity`가 `ForeGround`로 나오게 될 때 호출됨<br/>
> **또한 `BackStack`으로 들어감**

8. `onStop` : 다른 `Activity`가 화면을 완전히 가림
9. `onDestroyView` : `Fragment`와 관련된 `View`들이 제거될 때 호출됨
10. `onDestroy` : `Fragment`가 소멸될 때 호출됨
11. `onDetach` : `Fragment`가 `Activity`로 부터 해제되어질 때

# `ViewHolder`에 대하여 설명하시오

 `Inflate`를 최소화 하기 위해서 `View`들의 각 요소에 바로 접근할 수 있게 저장해두고 사용하기 위한 객체

## `ListView`에서의 `ViewHolder` 패턴

> `ListView`는 `ViewHolder` 사용이 **선택임**

1. `getView` 에서 `convertView`가 `null` 일 때만 **최초 한 번** `inflate`하고 `ViewHolder`룰 생성해 각 요소를 `findViewById`로 연결시켜 저장하고 `convertView`에 `tag`로 저장시킴
2. 그리고 다음부터는 `ViewHolder`를 `convertView`의 `tag`로 불러와서 재사용함

## `RecyclerView`에서의 `ViewHolder` 패턴

> `ListView`와는 달리, `RecyclerView`는 `ViewHolder`의 사용이 **필수임**

1. `createViewHolder`에서 새로운 `View`를 생성함과 동시에 `ViewHolder`를 만들고 `return`함
2. `onBindViewHolder`에서 `paramater`로 제공받은 `ViewHolder`의 `value`를 변경함

# 안드로이드 `Task`에 대하여 설명하시오

`Stack` 형식으로 차곡차곡 쌓이는 `Activity`들의 집합

#### Root Activity : `task`에 쌓이기 시작한 최하위 `activity`

#### Top Activity : `task`에 가장 최근에 쌓인 최상위 `activity`

> 만약 이 `task`에 쌓이지 않게 `activity`를 만들고 싶다면, `manifest`에 다음과 같이 추가하면 된다.<br/><br/>
> `android:excludeFromRecents="true"`

# 안드로이드 `Affinity`에 대하여 설명하시오

`Activity` 실행 시 신규 `task`에 만들 경우, 지정할 `task`의 이름<br/>
**이때 `Custom Affinity`의 이름에는 최소 한 개 이상의 `.`가 들어가야 한다.**

# `Intent`에 대하여 설명하시오

`Component`(`Activity`, `Service`, `Broadcast Receiver`)간에 통신을 하기 위한 메시지

## Intent Type

1. `Explicit Intent` : 시작할 `component`의 `class-name`을 지정

> `명시적 인텐드`<br/>
> 일반적으로 본인이 만든 `component`를 실행할 때 사용<br/><br/>
> EX) `Intent(context, MainActivity::class.java)`

2. `Implicit Intent` : 특정 `component`의 `class-name` 없이 어떠한 작업을 수행할지만 선언함

> `암시적 인텐드`<br/>
> 해당 `Intent`를 처리할 수 있는 `component`를 `system`이 필터링하여 수행하거나, 사용자에게 선택하도록 함<br/><br/>
> EX) [클릭](https://developer.android.com/guide/components/intents-common?hl=ko)

## Intent 구성요소

| **구성요소**     | **내용**                                                     |
| ---------------- | ------------------------------------------------------------ |
| `Component-Name` | 시작할 `component`의 `name`<br/>**`Explicit Intent`와 `Implicit Intent`의 구분 수단** |
| `Action`         | 수행할 작업을 설명하는 문자열<br/>일반적인 `Action`은 `Intent`안에 상수로 정의되어 있음 |
| `Data`           | 작업을 수행할 `data` 및 해당 데이터의 `MIME` 유형을 참조하는 `Uri` 객체 |
| `Category`       | `Intent`를 처리해야 하는 `component`의 종류에 대한 정보를 담은 문자열 |
| `Extras`         | 요청한 작업을 수행하기 위해 필요한 추가 정보들을 담고 있는 `Key:Value` 객체 |
| `Flags`          | `Intent`에 대한 `Meta-Data`같은 역할                         |

## MIME란?

이 데이터가 어떤 형식의 데이터인지 설명하는 명칭

| **타입**      | **설명**           | **예시**                                |
| ------------- | ------------------ | --------------------------------------- |
| `text`        | 모든 종류의 텍스트 | `text/plane`<br/>`text/html`            |
| `image`       | 모든 종류의 이미지 | `image/gif`<br/>`image/png`             |
| `audio`       | 모든 종류의 오디오 | `audio/midi`<br/>`audio/wav`            |
| `video`       | 모든 종류의 비디오 | `video/webm`<br/>`video/ogg`            |
| `application` | 모든 종류의 데이터 | `application/xml`<br/>`application/pdf` |

> 더 많은 정보) [[방문]](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

# `Pending Intent`에 대하여 설명하시오

다른 `Process`(`Application`)에서 권한을 허가하여 가지고 있는 `Intent`를 본인 `Process`에서 실행하는 것 처럼 해주는 것

> EX) `Notification`으로 작업을 수행할 때 `PendingIntent`를 사용한다.<br/>`Notification`은 `NotifcationManager`라는 다른 `process`에서 실행되기 때문.

# `Call by Value`/`Call by Reference`/`Call by Assignment`에 대하여 설명하시오

> 함수의 호출 방식

## CbV

> 함수에 값을 복사해서 전달하는 방식

함수의 인자로 전달되는 변수를 함수의 매개변수로 복사해서 실행함.<br/>
즉, 함수에 사용되는 매개변수들은 별도의 `scope`를 가지며 함수의 매개변수를 바꿔도 함수로 전달한 원래 변수의 값은 바뀌지 않음.

## CbR

> 함수에 값의 주소값을 전달하는 방식

함수에 인자로 전될되는 변수를 변수의 주소값으로 바꿔서 함수의 매개변수로 전달함.<br/>
즉, 함수로 전달되는 매개변수들과 원래의 변수들과 동일 `scope`를 가져서 함수의 매개변수 값이 바뀌면 원래 변수의 값도 바뀜.

## CbA

> 전달되는 객체에 따라 참조방식이 결정되는 방식

`Mutable Object`는 `CbR`을 따르고, `Immutable Object`는 `CbV`를 따른다.

## `Java`는 `CbV`/`CbR`/`CbA`중 어떤것인지 설명하시오

`Call by Value`를 따릅니다.

# `Java`의 자료형을 설명하시오

## `Primitive Type`(원시 자료형)

1. Boolean Type<br/>
   &nbsp;* **`boolean`**<br/>
2. `Numeric Type`<br/>
   &nbsp;2-1. `Integral Type`<br/>
   &nbsp;&nbsp;&nbsp;&nbsp;* `Integer Type` : **`short`**, **`int`**, **`long`**<br/>
   &nbsp;&nbsp;&nbsp;&nbsp;* `Floating Type` : **`float`**, **`double`**<br/>
   &nbsp;2-2. `Character Type` : **`char`**<br/>

> 자료형의 길이가 항상 `고정적`임<br/>
> **`비객체`** 타입 -> 즉 `null`을 가질 수 없음.

## `Reference Type`(참조 자료형)

1. **`Class Type`**<br/>
   &nbsp;* `String Class`<br/>
   &nbsp;* `Wrapper Class`<br/>
   &nbsp;&nbsp;&nbsp;&nbsp;* `void`의 `Wrapper Class`는 `Void`<br/>
2. **`Interface Type`**<br/>
3. **`Array Type`**<br/>
4. **`Enum Type`**<br/>
5. `etc...`<br/>

> 기본적으로 `Object`를 상속받으면 `Reference Type`이 됨<br/>
> 또한 선언한 자료형이 `Primitive Type`이 아니면 `Reference Type`이 됨

# `String` vs `StringBuilder` vs `StringBuffer`

## String

기본적으로 `immutable`(`불변`)이다.<br/>
즉, 값이 바뀌게 되면 **새로운 메모리영역을 가르키는 새로운 변수를 만든다.**<br/>
값이 바뀌기 전 이전 변수는 `Garbage`로 남아있다가 `GC`(`Garbage Collection`)에 의해 사라지게 된다.<br/>
**그리고 `쓰레드 동기화`가 가능하다**.

> 문자열 연산이 **적고** `멀티쓰레드`일 경우 사용

## StringBuffer

기본적으로 `mutable`이다.<br/>
**그리고 `쓰레드 동기화`가 가능하다**.

> 문자열 연산이 **많고** `멀티쓰레드` 환경일 경우 사용

## StringBuilder

기본적으로 `mutable`이다.<br/>
**그리고 `쓰레드 동기화`가 불가능하다**.

> 문자열 연산이 **많고** `단일쓰레드` 이거나, `동기화`를 고려하지 않는 환경일 때 사용

## 정리

| **구분**           | **`String`**  | **`StringBuffer`** | **`StringBuilder`** |
| ------------------ | ------------- | ------------------ | ------------------- |
| **`Memory`**       | `String pool` | `Heap`             | `Heap`              |
| **`Modifiable`**   | No            | Yes                | Yes                 |
| **`Thread-Safe`**  | Yes           | Yes                | No                  |
| **`Synchronized`** | Yes           | Yes                | No                  |
| **`Performance`**  | Fast          | Slow               | Fast                |

# 안드로이드 4대 컴포너트에 대하여 설명하시오

| **`Intent`**         | **Description**                                              |
| -------------------- | ------------------------------------------------------------ |
| `Activity`           | 사용자와 상호작용 하는 `interface`                           |
| `Service`            | 백그라운드 처리용<br/>**사용자와 직접 상호작용을 하지 않는다.** |
| `Broadcast Receiver` | 안드로이드에서 발생하는 각종 `event`를 받아옴<br/>시스템에서 시작됨 |
| `Content Provider`   | 데이터를 관리하고 다른 앱에 데이터를 제공<br/>**데이터 공유를 위해 표준화된 `SQLite interface` 사용 (대용량 가능)** |

> 안드로이드 4대 컴포너트는 `Intent` 속성에 포함된다.

# `Manifest`에 대해 설명하시오

`Manifest`(`매니페스트`)는 `Application`에 대한 각종 정보를 기록하는 파일이다.

| **Tag**             | **Description**                                              |
| ------------------- | ------------------------------------------------------------ |
| `<Manifest>`        | `Application`에 대한 전반적인 정보를 정의한다.<br/>EX) 버전코드 |
| `<application>`     | `Application`의 이름, 아이콘등을 정의한다.                   |
| `<activity>`        | `Application`에서 사용될 `Activity`들를 정의한다.            |
| `<service>`         | `Application`에서 사용될 `service`들을 정의한다.             |
| `<provider>`        | `Application` 안에서 사용되는 데이터들을 다른 `Application`으로 공유할 수 있게 해주는 `Content-Provider`들을 정의한다. |
| `<receiver>`        | `Application`에서 사용될 `Broadcast-Receiver`를 정의한다.    |
| `<uses-permission>` | `Application`에서 사용될 `permission`들을 정의한다.          |
| `<uses-library>`    | `Application`에서 사용될 외부 라이브러리들을 정의한다.       |
| `<uses-sdk>`        | `Application`에서 최소로 필요로 하는 `SDK` 버전을 정의한다.  |


# `Inflate`에 대해 설명하시오

`XML`에 정의되있는 `View`를 실제 `View` 객체로 만드는 역할을 수행한다.

# `JVM`/`DVM`/`ART`에 대해 설명하시오

## JVM

> `ByteCode` -> `interpret` -> 각 `OS`에 맞는 기계어로 번역 -> 실행

**장점** : `OS`에 구애받지 않고, 해당 `OS`에 맞는 기계어로 번역됨<br/>
**단점** : `Native`언어들에 비해 속도가 느림

## DVM

> `dex-file`을 `Dalvik Machine` 위에 올리는 방식

`ByteCode` -> `interpret` -> 실행 -> `Native`코드로 변환(`JIT-compile`)<br/>
`License` 문제로 `JVM` 대신 `Java`코드를 이용할 수 있게 개발됨

**장점** : `OS`에 구애받지 않고, 해당 `OS`에 맞는 기계어로 번역됨<br/>
**단점** : 성능과 배터리에 안좋음, `JIT-compile`된 코드가 메모리에 올라가 메모리 사용량이 높음

### JIT-compile

실행시점에 소스코드를 번역함<br/>
따라서 설치는 빠르지만, 실행시점에 느리게 됨<br/>
또한 번역한 정보를 메모리에 올려야 하기 때문에 실행 시 메모리를 많이 먹음

## ART

> `Machine` 위에서 `OAT-file`을 돌리는 방식<br/>`VM`이 아닌 `runtime`시 사용됨

앱을 설치할 때 완전히 `native`코드로 변환되어 설치됨(`AOT-compile`)

**장점** : 코드 `interpret` 및 `JIT-compile`을 제거하여 성능이 향상됨<br/>
**단점** : 설치시점에 코드를 번역하여 설치가 느리고, 파일을 따로 저장하기 때문에 용량이 커짐

### AOT-compile

설치시점에 소스코드를 번역함<br/>
따라서 설치는 느리고, 번역된 소스코드를 따로 파일로 저장하기 때문애 용량을 많이 먹음<br/>
하지만 실행시점에 미리 번역한 파일을 실행하므로 실행속도는 빠름

## `Android`는 `JVM`/`DVM`/`ART` 중 어느것인지 설명하시오

### Android 4.4 (`KitKat`) 이전

`DVM` 사용

### Android 4.4 (`KitKat`) 이후

`DVM`에서 `ART`로 컴파일 방식을 바꾸기 시작함

> `Dalvik`에서 돌아가는 앱들이 `ART`에서 죽는 현상이 발생함

### Android 5.0 (`Lolipop`) 이후

`ART`가 기본 `runtime`이 됨

### Android 7.0 (`Nougat`) 이후

`AOT-compile` 방식과 `JIT-compile` 방식을 조합해서 사용하기 시작함

### Android 8.0 (`Oreo`) 이후

`ART-compile` 방식의 많은 개선이 이뤄져 많은 문제들  해결

# `ListView` vs `RecyclerView`

| **분류**             | **`ListView`**                                               | **`RecyclerView`**                                           |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **`ViewHolder`**     | 선택                                                         | 필수                                                         |
| **`Item Layout`**    | 세로 방향만 지원                                             | `LayoutManager`을 통해 다양하게 설정 가능                    |
| **`Item Animation`** | 별도의 `animation`이 없다                                    | `RecyclerView.ItemAnimator` 클래스를 통해 제어할 수 있다.    |
| **`Adapter`**        | 다양한 분류의 어뎁터가 존재한다.<br/>EX) `ArrayAdapter`, `CursorAdapter`, etc... | `Item`을 보여주기 위한 `Adapter`를 직접 정의해서 사용해야 한다. |
| **`Decoration`**     | `android:divider` 속성을 이용하여 `item` 들을 관리할 수 있다. | `RecycerView.ItemDecoration` 를 사용하여 구분선을 설정해야 한다. |
| **`Click Event`**    | `AdapterView.OnItemClickListener` `interface`를 통해 설정할 수 있다. | `RecyclerVier.OnItemClickListener` 이라는 개별 터치를 관리하는 `interface`는 있지만, `OnClickListener`는 따로 존재하지 않아 직접 정의해서 사용해야 한다. |

# `Flowable`과 `Observable`의 차이점을 설명하시오

`Flowable`은 `Backpressure Buffer`가 **기본적으로 내장되어 있지만**, `Observable`은 `Backpressure Buffer`가 **기본적으로 내장되어있지 않다.** (따라서 직접 추가해서 등록해줘야 한다)

## `Backpressure Buffer`란?

`소비`되는 데이터에 비해 `생산`되는 데이터의 양이 너무 많을 경우 `OutOfMemory` 같은 `exception`이 발생 할 수 있기 때문에, `생산`되는 데이터의 양을 `소비`되는 데이터의 양에 비례해서 조절해주는 역할을 해 준다.

# `REST`에 대하여 설명하시오

> Representational State Transfer

자원의 `이름`으로 구분하여 해당 `정보`를 주고 받는 모든 것<br/>
**웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일**

## `CRUD` Operation

`Create` : 생성 (`POST`)<br/>
`Read` : 조회 (`GET`)<br/>
`Update` : 수정 (`PUT`)<br/>
`Delete` : 삭제 (`DELETE`)

**장점** : `Server`와 `Client`의 역할을 명확하게 구분한다.<br/>
**단점** : `REST`는 설계 가이드일 뿐이라, 표준이 존재하지 않는다.

## REST API란?

`REST` 기반으로 서비스 `API`를 구현한 것

#### `API`란?

> Application Programming Interface

데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며, 서로 정보를 교환 가능 하도록 해주는 것

### `REST API` 설계 기본 규칙

> * `도큐먼트` : 객체 인스턴스와 비슷한 개념
> * `컬렉션` : `서버`에서 관리하는 `디텍터리` 저장소
> * `스토어` : `클라이언트`에서 관리하는 `리소스` 저장소

1. `URI`의 정보는 자원을 표현해야 한다.<br/>
2. 자원에 대한 행위는 `CRUD Operation`으로 표현한다.<br/>
3. `/`는 계층 관계를 나타내는데 사용한다.<br/>
4. `URI` 마지막 문자로 `/`를 포함하지 않는다.<br/>
5. `파일 확장자`는 `URI`에 포함시키지 않는다.<br/>
6. `_`는 `URI`에 사용하지 않는다.<br/>

## RESTful이란?

`REST` 원리를 따르는 시스템

# `URI`와 `URL`의 차이점에 대하여 설명하시오

## URI (`Uniform Resource Indetifier`)

전체 주소

> EX) `https://test.com/test.pdf?index=5`

## URL (`Uniform Resource Locator`)

파일의 위치(구분자)를 표시하는 주소

> EX) `https://test.com/test.pdf`?index=5  

```
https://test.com/test.pdf?index=5
https://test.com/test.pdf?index=15
```

위와 같은 주소는 같은 `URL`을 가지지만 `URI`는 서로 다름<br/>

> `URL`은 `URI` 안에 있음

# `Process`에 대하여 설명하시오

**실행중에 있는** 프로그램<br/>
프로그램을 실행하면, 실행하기 위해서 `메모리 할당`이 이루어지고, 할당된 메모리 공긴으로 바이너리 코드가 올라간다. **이 순간부터 `process`라고 불린다.**

## `Process`의 메모리 구조

1. `Code 영역` : 프로그램을 실행시키는 소스코드들이 올라간다.<br/>
2. `Data 영역` : `전역변수`, `static` 변수들이 할당된다.<br/>
3. `Heap 영역` : `동적할당`을 위한 메모리 영역이다.<br/>
4. `Stack 영역` : `지역변수`와 `인자`들을 위한 메모리 영역이다.<br/>

## `Process Scheduling`

`Process`의 `CPU` 할당 순서 및 방법을 결정하는 일

# `Thread`에 대하여 설명하시오

`Process` 내부의 작업 단위

## 특징

1. `Thread`는 한 `process`에서 **최소 하나 이상 존재한다.**<br/>
2. `Multi-Thread`에서 각 `thread`끼리는 서로 `process`의 **메모리 영역을 공유한다.**

> `Thread`가 하나의 `process`안에서 여러개가 존재하는걸 `Multi-Thread`라고 한다.

## `Thread Pool`에 대하여 설명하시오

`Thread`를 **미리 만들어놓는것**

# `Handler`에 대하여 설명하시오

서로 다른 `thread`간의 통신을 위한 장치

## `Looper`에 대하여 설명하시오

`MessageQueue`로부터 `message`를 꺼내와서 해당 `message`와 연결된 `handler`를 호출하는 역할을 함

## `MessageQueue`에 대하여 설명하시오

`Message`를 `Queue` 형태로 저장하여 관리하는 객체이다.<br/>

> `FIFO`(`First In First Out`) 방식으로 동작하기 때문에, `message`는 `queue`에 들어온 순서에 따라 차례대로 저장됨<br/><br/>
> 그리고 가장 먼저 들어온 `message` 객체부터 차례대로 처리됨

## `Message`에 대하여 설명하시오

`Thread`로 전달할 데이터를 한 곳에 저장할 객체이다.

# `Wrapper`에 대하여 설명하시오

> 필요한 데이터를 받거나 쓰기 위해 데이터 형태를 설정해 주는 역할을 함

`Primitive Type`에 대한 클래스 표현이라고 할 수 있다.

# `View.post()`와 `Handler`와의 차이점을 설명하시오

`View.post()`는 `handler`와는 달리 `View`가 `attach` 되지 않으면 **`post`된 모든 `runnable`들을 `View`가 `attach` 될 때 까지 연기한다.**

# `Thread`간 통신 방법에 대하여 설명하시오

// TODO

# `Activity`간 통신에 대하여 설명하시오

[`Singleton`](https://github.com/sungbin5304/Programming-Study#%EC%8B%B1%EA%B8%80%ED%86%A4)을 이용하거나 `intent`를 사용한다.

# `FCM`의 원리에 대하여 설명하시오

## 1. 어플리케이션을 위한 `key`를 `FCM 서버`에서 얻음

1. 어플리케이션이 설치되는 순간 `firebase 서버`에 `key` 휙득을 위한 요청을 보냄 <br/>
2. `Firebase 서버`에서 `key`를 만들어서 어플리케이션으로 전달함 <br/>
3. 어플리케이션으로 전달된 `key`를 `google server`에 전송함 <br/>
4. `Google server`에서 이 `key`를 `database`에 저장함 <br/>

## 2. `Google 서버`에서 어플리케이션으로 `notification`을 발생시킴

1. `Google server`에서 어플리케이션으로 데이터를 전달하기 위해 `key`를 얻음 <br/>
2. `Key`와 데이터를 `firebase server`로 보냄 (`http 통신`이 이용됨) <br/>
3. `Firebase server`에서 받은 `key`로 어떤 어플리케이션인지 구분 후, 데이터를 `notification`으로 호출함 <br/>

# `ABI`에 대하여 설명하시오

> `Application Binary Interface`


`Runtime`에 시스템과 앱의 머신코드가 어떻게 상호작용할지를 기술한 인터페이스.

# `HTTP` 라이브러리들의 역사에 대하여 설명하시오

2009년 : `Android` 발표, `HttpClient`, 아파치의 `DefaultHttpClient` 사용<br/>
2011년 : `Google Developer` 블로그에서 `HttpUrlConnection` 사용 권장<br/>
이후 : `Volley`나 `OkHttp`의 등장으로 `HTTP` 통신이 쉬워짐<br/>
2014년 : `HttpClient`가 `Deprecated`되면서 이를 베이스로 하는 `Volley`도 같이 `Deprecated`됨<br/>
이후 : `OkHttp`와 그 `wrapper`인 `Retrofit`이 등장하여 인기를 가짐<br/>

## `Retrofit`이 인기를 받게된 이유?

`Client` 부분과 `Callback` 형식들을 플러그로 설정 할 수 있기 때문

# `Context`에 대하여 설명하시오

새로 생성된 `객체`에서 지금 어떤 일이 일어나고 있는지 알 수 있게 해줌<br/>
또한 `resource`, `database`, `preferences` 등에 접근을 제공해줌<br/>

**`Context`는 기본적으로 `gc`에 의해 수집되지 않음**

## `Application Context`

**`Singleton-instance`** 이며, `application life-cycle`과 연결되어 있음<br/>

현재의 `context`와 분리된 `life-cycle`을 가진 `context`나, **`activity`의 범위를 넘어서 `context`를 전달할 때 사용함**<br/>

## `Activity Context`

`Activity`에서 사용가능하며, `activity life-cycle`과 연결되어 있음<br/>

`Activity` 범위 내에서 `context`를 전달하거나 `lifecycle`이 현재의 `context`에 붙은 `context`가 필요할 때 사용함<br/>

### `Context` 얻는 방법들

#### View.getContext()

`activity context`를 반환한다.

### this

`View.getContext()`와 같은 역할을 한다.

#### Activity.getApplicationContext()

`application context`를 반환한다.

#### ContextWrapper.getBaseContext()

자신의 `context`가 아닌 다른 `context`에 접근할 때 사용된다.<br/>
`Context`를 생성할 때 만들어지는 `context`들의 기본 `context`를 반환한다.

# `LMK`에 대하여 설명하시오

## 목적

## Priority

## `OOM KILLER`와의 차이점을 설명하시오

# `**Big** Size Image` 처리 방법을 설명하시오

# `Callable`과 `Runnable`의 차이점을 설명하시오

# `Interceptor`에 대하여 설명하시오

# `GC`(`Garbage Collection`)에 대해 설명하시오

## `GC`의 종류에 대해서 설명하시오

# `drawable`과 `mipmap`의 차이점을 설명하시오

| **구분**        | **`drawable`**                      | **`mipmap`**                  |
| --------------- | ----------------------------------- | ----------------------------- |
| **목적**        | **모든 이미지** 저장                | **아이콘 이미지** 저장        |
| **지원 버전**   | 모든 버전                           | API 2.3 이상                  |
| **이미지 크기** | **디바이스 픽셀밀도에 따라 달라짐** | **이미지 크기에 따라 달라짐** |

이미지를 확대하여 보여주는 기능을 구현할 때 이미지 파일을 `drawable`에 넣으면 이미지 자체가 확대되서 이미지가 손상되지만, 이미지 파일을 `mipmap`에 넣어주게 되면 **확대됐을때 적당한 크기를 가지는 이미지 파일을 보여준다.**

# `ReactiveX` 관련

## `Cold`에 대해서 설명하시오

> 대부분의 `operator`의 기본값

`Stream`의 전과 후를 **연결만 시켜준다.**<br/>
`Observer`가 등록되야 작동을 시작한다.<br/>

## `Hot`에 대해서 설명하시오

`Stream`에서 값을 계속 발생시킨다.<br/>
`Observer`가 등록되있지 않아도 작동된다.<br/>

# `Coroutines` 관련

## `Flow`에 관하여 설명하시오

`비동기`로 동작하면서 여러가지의 값을 반환하는 `Coroutines Builder` 이다.

## `suspend`란?

> [[stackoverflow]](https://stackoverflow.com/questions/47871868/what-does-the-suspend-function-mean-in-a-kotlin-coroutine) <br/>
> 추후 정리 예정

# `AndroidX`가 무엇인지 설명하시오

기존 `Android` 지원 라이브러리의 성능을 크게 개선시킨 것<br/>
`support`라이브러리와 `AppCompoat` 라이브러리들이 `androidx`로 하나로 합쳐졌다.<br/>
`AndroidX` 패키지는 기존 `Android` 지원 라이브러리의 동일한 기능을 제공하여 기존 라이브러리를 완전히 대체한다.

# 코틀린 관련

## `also`, `apply`, `with`, `let`, `run`의 차이점을 설명하시오

## `object`와 `companion object`에 대해서 설명하시오

-----

# `Issue`를 통한 오타/오류/내용 추가 지적 환영합니다 :)

## 잘 보셨다면, `Star`와 `Fork` 부탁드려요!

### 저와 이 난잡한 `repository`를 보시는 분들의 `좋은 결과`를 기원합니다.
