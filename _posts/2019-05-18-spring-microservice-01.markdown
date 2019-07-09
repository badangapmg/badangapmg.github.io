---
layout: post
title:  "Spring Cloud"
date:   2015-04-18 08:43:59
author: SimfleLipe
categories: MicroService
tags: Spring MicroService(acorn)
---

## 스프링 클라우드를 활용한 마이크로 서비스 확장

스프링 클라우드 프로젝트는 스프링 부트의 범위를 넘어서는 클라우드 환경에서 필요한 기능들을 쉽게 추가 할 수 있는 맞춤형 컴포넌트 제품군을 포함하고 있다.
`Eureka, Zuul, Ribbon, Spring Config` 스프링 클라우드 다양한 컴포넌트를 살펴 보자.

### 스프링 클라우드란
스프링 클라우드 프로젝트는 스프링 팀에서 만든 일종의 포괄적 프로젝트로, 분산 시스템 개발에 필요한 공통적인 패턴들을 모아서 사용하기 쉬운 스프링 라이브러리
형태로 구현해서 제공한다. 스프링 클라우드를 사용하면 개발자들은 분산, 장애 대응, 자체 치유 기능은 스프링 클라우드에게 맡기고, 스프링 부트를 바탕으로 비즈니스
기능을 만드는 데만 집중 할 수 있다.

스프링 클라우드가 보유하고 있고 지원해 줄 수 있는 역량은 다음과 같다.

* 분산 환경 설정
* 라우팅
* 부하 분산
* 서비스 등록 및 발견
* 서비스 대 서비스 호출
* 서킷 브레이커
* 전역 잠금, 주도권 선발 및 클러스터 상태
* 보안
* 빅데이터 지원
* 분산 추적
* 분산 메시징
* 클라우드 지원

먼저 스프링 클라우드 프로젝트가 제공하는 기능을 이용해서 클라우드 환경에서 확장설을 확보 할 수 있게 살펴보고 아래의 단계별로 Spring Config을 구현 해 보자
1. Spring Config 를 이용한 환경설정
2. Ribbon 기반의 서비스 부하분산
3. Eureka를 이용한 서비스 탐색
4. Zuul을 이용한 API 게이트웨이

### Spring Cloud Config 
`Spring Cloud Config` 서버는 애플리케이션과 서비스의 모든 환경설정 속성 정보를 저장하고, 조회 하고 관리 할 수 있게 해주는 외부화된 `환경설정` 서버다.

#### Config 서버 셋업

{% highlight javascript %}
/* 자바 */
int strLen(String s) {
  return s.length()
}
{% endhighlight %}

위의 함수에 null 을 넘기면 NullPointerException이 발생 한다.

이 함수를 코틀린으로 다시 작성 해보자.

{% highlight javascript %}
fun strLen(s:String) = s.length
{% endhighlight %}

코틀린에서 이런 함수를 작성 할 때 가장 먼저 답을 알아야 할 질문은 `이 함수가 널을 인자로 받을 수 있는가?`이다.
null이 인자로 들어올 수 없다면 위와 같이 작성 할 수 있으며, 혹시 null 값을 넘기면 컴파일 시 오류가 발생한다.

{% highlight javascript %}
ERROR: Null can not be a value of a non-null type String
{% endhighlight %}

위의 함수가 널과 문자열을 인자로 받을 수 있게 하려면 타입 이름 뒤에 물음표(?)를 명시해야 한다.

{% highlight javascript %}
fun strLen(s:String?) = ...
{% endhighlight %}

널이 될 수 있는 타입의 변수가 있다면, 수행할 수 있는 연산이 제한된다.
{% highlight javascript %}
/* 메소드의 직접 호출이 제한된다. */
fun strLen(s:String?) = s.length()

ERROR: only safe(?.) or non-null asserted(!!.) ...
{% endhighlight %}

{% highlight javascript %}
/* null이 될 수 있는 값을 널이 될 수 없는 타입의 변수에 대입할 수 없다.*/
val x:String? = null
val y:String = x
ERROR: Type mismatch : inferred type is String? but String was expected
{% endhighlight %}

{% highlight javascript %}
/* null 이 될수 있는 타입의 값을 null이 될수 없는 타입의 파리미터를 받는 함수에 전달 할 수 없다.*/
strLen(x)
ERROR: Type mismatch : inferred type is String? but String was expected
{% endhighlight %}

위와 같이 null 이 될 수 있는 타입은 많은 제약이 따른다. 이러한 제약을 피하려면 일단 null 과 비교하고 나면 컴파일러는 그 사실을
기억하고 null이 아님이 확실한 영역에서는 해당값을 널이 될수 없는 타입의 값처럼 사용할 수 있다.

{% highlight javascript %}
/* null 이 될수 있는 타입의 값을 null이 될수 없는 타입의 파리미터를 받는 함수에 전달 할 수 없다.*/
fun strLenSafe(s: String?) : Int = 
    if (s!=null) s.length else 0
{% endhighlight %}

null 검사를 추가하면 코드가 컴파일 된다.

### 안전한 호출 연산자 : ?.
`안전한 호출 연산자 (?.)` 는 null 검사와 메소드 호출을 한 번의 연산으로 수행한다.
예를 들어 `s?.toUpperCase()` 는 `if (s!=null) s.toUpperCase() else null` 과 같다.

{% highlight javascript %}
class Address(val country:String)
class Company(val address:Address?)
class Person(val company:Company?)

fun Person.countryName() : String {
    val country = this.company?.address?.country
    return if (country !=null) country else "Unknown" //엘비스 연산(?:)으로 더욱 간결하게 표현 가능
}
{% endhighlight %}

### 엘비스 연산자: ?:
코틀린은 null 대신 사용할 디폴트 값을 지정할 때 편리하게 사용할 수 있는 연산자를 제공한다.
그 연산자는 `엘비스(?:)` 연산자라고 한다.

{% highlight javascript %}
fun foo(s: String) {
    val t:String = s ?: ""  //s가 null이면 결과는 빈문자열("")이다.
}
{% endhighlight %}

`안전한 연산자(?.)` 에서 살표본 `Person.countryName 함수`를 좀더 간결하게 표현 할 수 있다.
{% highlight javascript %}
fun Person.countryName() = company?.address?.country ?: "Unknown"
{% endhighlight %}

### 안전한 캐스트: as?
`as? 연산자`는 어떤 값을 지정한 타입으로 캐스트한다. `as?`는 값을 대상 타입으로 변환할 수 없으면 null을 반환한다.
{% highlight javascript %}
class castType(o:Any?) {
    val other = o as? Person ?: return false
    ...
    ..
}
{% endhighlight %}

### 널 아님 단언: !!
`널 아님 단언 (!!)` 은 코틀린에서 널이 될 수 있는 타입의 값을 다룰 때 사용할 수 있는 도구이다.
`널 아님 단언 (!!)` 을 사용하면 어떤 값이든 널이 될 수 없는 타입으로(강제로) 바꿀 수 있다.
{% highlight javascript %}
fun ignoreNulls(s: String?){
    val sNotNull : String = s!!  //예외는 이 지점을 가르킨다.
    println(sNotNull.length)
} 
{% endhighlight %}
s가 null 이면 NullPointerException 이 발생한다. 특이한 점은 발생한 예외는 null 값을 사용하는 코드가 아니라
`val sNotNull:String = s!!` 이 지점을 가르킨다.
