---
layout: post
title: "Android: Kotlin Flow vs LiveData"
date: 2022-07-14
excerpt: "Kotlin Flow vs LiveData"
comments: true
categories : Android-심화
---

# kotlin Flow 및 LiveData
- [사용법](https://developer.android.com/codelabs/advanced-kotlin-coroutines?hl=ko#0)
- [reference](https://medium.com/@lukaszburcon/flow-and-livedata-in-mvvm-architecture-6f8879b96ce0)

## [LiveData](https://developer.android.com/topic/libraries/architecture/livedata)
- LiveData는 아키텍처 컴포턴트 중 일부이며, 일반적으로 데이터 전송을 위한 용도이다.
### LiveData 개요
- 관찰 가능한 데이터 홀더 클래스
- 다른 관찰 가능한 일반 클래스와는 달리 생명주기를 인식한다.
    - 따라서, 관찰하던 컴포넌트가 destroy가 되거나 비활성화 된다면, Obserer에게 데이터를 알리는 것을 중단한다.
- 위의 동작은 아래 장점들을 야기한다.

### 장점
- UI와 데이터 상태의 일치 보장
    - LiveData는 관찰자 패턴을 따른다.
        - 이는 기본 데이터가 변경될 때 Observer에게 알리면서 이 Observer가 UI를 변경할 수 있다.
- 메모리 누수가 없다.
    - LiveData는 생명주기를 인식하며, 연결된 Observer 또한 생명주기가 다함에 따라 같이 소멸된다.
- 중지된 활동으로 인한 비정상 종료가 없다.
    - activity가 백 스택에 있을 때, 등 관찰자의 생명주기가 비활성 상태라면 관찰자는 어떤 LiveData 이벤트도 받지 않아 비정상 문제가 일어나지 않는다.
- 생명주기를 더 이상 수동으로 처리하지 않는다.
    - UI 구성요소는 관련 데이터를 관찰만 한다.
    - 따라서, 생명주기에 따른 상태 변경에 대응한 작업을 직접할 필요가 없다.
- 최신 데이터 유지
    - 생명주기가 비활성화되고 다시 활성상태가 될 때 최신 데이터를 수신한다.
- 적절한 구성 변경
    - 기기 회전과 같은 구성 변경으로 인해 activiy 또는 fragment가 다시 생성되면 최신 데이터를 즉시 받게 된다.
- 리소스 공유
    - 앱에서 시스템 서비스를 공유할 수 있도록 싱글톤 패턴을 사용하는 LiveData를 확장해 시스템 서비스를 래핑할 수 있다.
    - LiveData 객체가 시스템 서비스에 한 번 연결되면 모든 관찰자가 해당 객체를 볼 수 있다.

## [Kotlin Flow](https://developer.android.com/kotlin/flow)
- Flow는 아키텍처의 더 낮은 레벨에서 동작하도록 설계되었다.

### Flow 개요
- 값이 지연 생성되는 컬렉션 유형인 시퀀스의 비동기 버전이다.
- Flow는 Iterator와 매우 유사하지만, suspend를 사용해서 값을 비동기적으로 처리가능하게끔 한다.
    - 따라서, Network Request를 Main 스레드 blocking 없이 다음 값을 만드는데 더 안전하게 할 수 있다.
- 코루틴에 대해 전체 지원을 포함한다.
    - 이는 코루틴을 잉용해서 flow를 빌드, 변환, 생산을 할 수 있음을 의미한다.
- flow는 cold stream이다.(<-> hot stream)


## LiveData와 Flow의 차이
- LiveData는 생명주기 문제를 해결하며 데이터를 관찰하는 데 사용된다.
- Flow는 지속적인 데이터 통합에 사용되며 비동기 프로그래밍에도 단순화되어있다.
- 위에서 볼 수 있듯 LiveData는 View와 밀접하게 연관되어 관련된 데이터를 관측하는데 있어 더 유용하게 사용할 수 있고, Flow는 비동기 처리에 관해 유연하게 대처할 수 있음을 알 수 있다.
- 또한 LiveData는 비동기 데이터 스트림을 처리하도록 설계되지 않았기에 
    - 결합 기능이 제한적이고, 변환을 통해 만들어진 객체를 포함한 모든 LiveData가 Main Thread에서 관측된다.
    - 즉, 다른 쓰레드에서 동작하지 않기에 많은 양의 데이터 처리를 메인 쓰레드에서 수행하고 anr 문제 등을 야기할 수 있다.
- Flow는 스트림 변환을 할 수 있기에 비동기 처리가 단순화된 상태로 사용할 수 있다.
    - 하지만 lifecycle을 인식하지 않기에 이에 대한 대응이 필요할 것이다.
- 위의 두 문제를 결합해 LiveDat와 Flow를 하나의 프로젝트에서 공존을 할 수 있고 함께 사용하는 것에서 이익을 취할 수 있다.
    - UI의 Observer는 LiveData를 사용하고 앱의 하위 레이어 등과 같은 곳에서 Flow를 통해 스트림 변환을 쉽게 수행해 두 개의 장점을 모두 가져올 수 있다.

### 정리
- LiveData와 Flow는 상호보완적인 관계를 맺을 수 있다.
- LiveData는 구성 변경시에 안정성을 제공하고 항상 최신 데이터를 유지하며 View에 데이터를 전달하는 역할을 한다.
- Flow는 앱의 하위 레이어에서 동작하여 데이터를 수집 및 처리해 서로 다른 코루틴 범위에서 작업을 실행할 수 있다.
- 따라서, View와 ViewModel 사이에서는 LiveData가 관계를 맺어주고, 그 아래의 더 복잡한 처리는 Flow가 도맡는 역할 분담을 하면 더 좋다.