---
title: "[Spring] 컨트롤러 테스트 가이드 in Spring Boot"
date: 2019-03-16 18:09:28 -0400
categories:study Spring

---

## Spring Boot에서 RestController에 대한 단위 테스트, 통합 테스트를 하는 방법

> 이글은 [The Practical Developer](https://www.facebook.com/thepracticaldeveloper/)님의 게시글인  **"[Guide to Testing Controllers in Spring Boot](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#comment-401)"**을 허가받아 번역한 글임을 밝힙니다.
>
> 번역을 반겨주셨던 The Practical Developer님께 감사를 표합니다.

#### 목차

1. Spring Boot에서 RestController에 대한 단위 테스트, 통합 테스트를 하는 방법

2. 개요

   2.1. [The sample application](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#The_sample_application)

   2.2. Server and Client Side Tests](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Server_and_Client_Side_Tests)

- [Server-Side Tests](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Server-Side_Tests)
- Inside-Server Tests
  - Strategy 1: MockMVC in Standalone Mode
    - MockMVC standalone code example
      - [MockitoJUnitRunner and MockMVC](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#MockitoJUnitRunner_and_MockMVC)
      - [JacksonTester initialization](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#JacksonTester_initialization)
      - [Configure the Standalone Setup in MockMVC](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Configure_the_Standalone_Setup_in_MockMVC)
      - [Testing ControllerAdvices and Filters with MockMVC](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Testing_ControllerAdvices_and_Filters_with_MockMVC)
      - [Better Assertions with BDDMockito and AssertJ](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Better_Assertions_with_BDDMockito_and_AssertJ)
  - Strategy 2: MockMVC with WebApplicationContext
    - MockMVC and WebMvcTest code example
      - [SpringRunner](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#SpringRunner)
      - [MockMVC Autoconfiguration](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#MockMVC_Autoconfiguration)
      - [Overriding beans for testing using MockBean](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Overriding_beans_for_testing_using_MockBean)
      - [No server calls](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#No_server_calls)
    - [Using MockMVC with a Web Application Context – Conclusions](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Using_MockMVC_with_a_Web_Application_Context_Conclusions)
- Outside-Server Tests
  - [Strategy 3: SpringBootTest with a MOCK WebEnvironment value](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Strategy_3_SpringBootTest_with_a_MOCK_WebEnvironment_value)
  - Strategy 4: SpringBootTest with a Real Web Server
    - Spring Boot Test Code Example
      - [Web Server Testing](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Web_Server_Testing)
      - [Mocking layers](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Mocking_layers)
      - [TestRestTemplate](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#TestRestTemplate)
    - [SpringBootTest approach – Conclusions](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#SpringBootTest_approach_Conclusions)
  - [Performance and Context Caching](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Performance_and_Context_Caching)
- [Conclusion](https://thepracticaldeveloper.com/2017/07/31/guide-spring-boot-controller-tests/#Conclusion)ty



스프링 부트에서 컨트롤러(웹 또는 API  계층)를 테스트하기위한 여러 방법들이 있다. 몇몇은 순수한 단위테스트를 작성하기위한 지원들을 해주고 다른 몇몇은 통합테스트에 유용하다. 이 포스트에서는 standalone모드에서 MockMVC를 사용하여 스프링에서 사용가능한 크게 세가지의 테스트를 다룰 것이다.

### Introduction 도입

**스프링 부트**에서는 테스팅을 위한 몇가지 접근법들이 있다. 이것들은 지속적으로 진화하는 framework이고, 새로운 버전에는 더 많은 옵션들이 추가된다. 동시에 오래된 것들은 (at the same time that old ones are kept for the sake of backward compatibility.). 그 결과로 코드의 일부를 테스트 하기위한 수많은 방법들이 생겼고, and some unclarity about when to use what.  이 글에서는 독자들이 테스트를 위한 다른 관점들을 이해하도록 돕고, 각 방법들이 왜 유용한지, 또 언제 써야하는지 설명할 것이다. 

이 글에서는 가장 모호한 부분인 Controller 테스팅에 초점을 맞춘다. 컨트롤러 테스트는 mocking 객체가 각기 다른 레벨들에서 쓰여질수 있는 등의 이유로 가장 모호하기 때문에.

### The sample application 예시 프로그램

우리는 각기 다른 개념들을 연습하기위해  이글에서 몇몇의 예시 코드를 사용할 것이다. 

> 이글의 모든 코드는 [GitHub: Spring Boot Testing Strategies](https://github.com/mechero/spring-boot-testing-strategies) 에서 이용가능하다. ( 이 코드가 유용하다고 생각하면 star을 눌러주세요! )

요약하자면, 위 코드는 REST API를 통해 공개된 entity들(superheroes)의 저장소이다.  다른 전략들을 사용할때 발생하는 일들에 대해 더 나은 이해를 하기위해각 예시의 몇몇 특징들을 나열하는것도 중요하다.

- 만약에 슈퍼히어로가 식별자를 통해 찾아지지 않는다면, NonExistingHeroException이 던져진다. 이 안에는 예외를 발생시켜주고, 이것을 404코드(NOT_FOUND)로 변환해주는 스프링의 @RestControllerAdvice 어노테이션이 있다.

- SuperHeroFilter 클래스는 우리의 HTTP통신에서 HTTP Response 헤더에 `X-SUPERHERO-APP` 을 추가하기 위해 사용될 것이다.



### Server and Client Side Tests 서버 사이드 테스트 vs 클라이언트 사이드 테스트

먼저, 서버사이드와 클라이언트 사이드를 분리해보자.

서버사이드 테스트는 가장 확장된 방식의 테스팅이다. 우리는 request를 보내보고 서버가 어떻게 행동하는지, response의 구성과 컨텐츠등을 체크 할 수 있다.

클라이언트 사이드 테스트는 자주 쓰이지는 않지만, 당신이 request의 구성과 동작(http method)를 검증하고 싶을때 유용하다. 이러한 테스트들에서 당신은 서버의 행동을 mock(가짜로 구성)하고, 몇몇 코드를 호출하여 서버로 간접적으로 request를 보내볼 수있다. 이를 통해 우리는 정확하게 request요청이 있음을 알수 있고, 그 내용을 확인하고 테스트 할 수 있다. 당신은 response의 내용을 신경쓸 필요가 없다( 가짜로 구성 했었음). 아쉽게도 이 내용에 대한 좋은 예시는 많이 없다.  [공식 문서](https://github.com/spring-projects/spring-framework/blob/master/spring-test/src/test/java/org/springframework/test/web/client/samples/SampleTests.java) 를 봐도 크게 도움되지 않는다. 어쨋든, 중요한점은 이것들이 우리가 client application을 작성 할때, 우리쪽에서 세계로 나가는 request들을 검증할때 사용될 수 있다는 것이다.

우리는 서버사이드 테스트에 초첨을 맞출것이다. 이는 어떻게 서버 로직이 동작하는지 확인하기 위한 것이다. 이때 당신은 보통 request를 mock(가짜로 구성)하고, 어떻게 서버가 반응하는지 체크하려고 할것이다. 스프링에서 이러한 종류의 테스트들은 어플리케이션에서 Controller Layer과 밀접한 연관이 있다. (Controller Layer가 Spring에서 HTTP handling을 담당하는 부분이기 때문에)



### Server-Side Tests

서버사이드 테스트의 안을 들여다보면, 우리가 스프링에서 구별할 수 있는 두가지 방식이 있다. 첫번째는 MockMVC접근을 사용하여 Controller 테스트를 작성하는 것이고, 두번째는 RestTemplate을 이용하는 것이다. 만약에 당신이 Real 단위 테스트를 작성하고 싶다면, 첫번째 방식(MockMVC)를 택해야한다. 반면 통합테스트를 작성하고 싶다면 RestTemplate을 사용해야한다. 왜냐면 MockMVC를 이용하면 Controller에 대한 assertion(참이라고 가정되는 조건문)들을 [fine-grain](https://icthuman.tistory.com/entry/FineGrained-vs-CoarseGrained) 할수있다 (= 컨트롤러에 대한 테스트를 세분화 할수 있다는 의미). 반면 RestTemplate은 Spring의 WebApplicationContext를 (Standalone모드 여부에 따라 부분적으로 또는 완전히) 이용한다. 이 두가지에 대해 더 자세히 설명해보자.



### Inside-Server Tests

우리는 우리의 Controller의 로직을 웹서버를 구동하지 않고도 직접 테스트 할 수 있다. 이것을 바로 "inside-server testing"이라 부르고 이 테스팅은 단위 테스트의 개념에 더 가깝다. 이를 가능하게 하려면, 당신은 전체 웹서버의 행동을 mock해야한다. 그래서 어느정도는 테스트 되어야할 부분들을 놓치곤 할 것이다. 하지만 여기서 놓친 부분들은 통합 테스트에서 완전히 커버되기 때문에 걱정하지 않아도 된다. 



### **Strategy 1: MockMVC in Standalone Mode**

![Test MockMVC Standalone](https://thepracticaldeveloper.com/wp-content/uploads/2017/07/tests_mockmvc_wm.png)

 스프링 standalone 모드에서 MockMVC를 사용한다면, inside-server test를 작성할 수 있다. 이렇게 함으로써 어떠한 contect도 로딩하지 않고 테스트 할 수 있게 된다. 아래 예시를 봐보자

### MockMVC standalone code example

```java
@RunWith(MockitoJUnitRunner.class)
public class SuperHeroControllerMockMvcStandaloneTest {
 
    private MockMvc mvc;
 
    @Mock
    private SuperHeroRepository superHeroRepository;
 
    @InjectMocks
    private SuperHeroController superHeroController;
 
    // This object will be magically initialized by the initFields method below.
    private JacksonTester<SuperHero> jsonSuperHero;
 
    @Before
    public void setup() {
        // We would need this line if we would not use MockitoJUnitRunner
        // MockitoAnnotations.initMocks(this);
        // Initializes the JacksonTester
        JacksonTester.initFields(this, new ObjectMapper());
        // MockMvc standalone approach
        mvc = MockMvcBuilders.standaloneSetup(superHeroController)
                .setControllerAdvice(new SuperHeroExceptionHandler())
                .addFilters(new SuperHeroFilter())
                .build();
    }
 
    @Test
    public void canRetrieveByIdWhenExists() throws Exception {
        // given
        given(superHeroRepository.getSuperHero(2))
                .willReturn(new SuperHero("Rob", "Mannon", "RobotMan"));
 
        // when
        MockHttpServletResponse response = mvc.perform(
                get("/superheroes/2")
                        .accept(MediaType.APPLICATION_JSON))
                .andReturn().getResponse();
 
        // then
        assertThat(response.getStatus()).isEqualTo(HttpStatus.OK.value());
        assertThat(response.getContentAsString()).isEqualTo(
                jsonSuperHero.write(new SuperHero("Rob", "Mannon", "RobotMan")).getJson()
        );
    }
 
    @Test
    public void canRetrieveByIdWhenDoesNotExist() throws Exception {
        // given
        given(superHeroRepository.getSuperHero(2))
                .willThrow(new NonExistingHeroException());
 
        // when
        MockHttpServletResponse response = mvc.perform(
                get("/superheroes/2")
                        .accept(MediaType.APPLICATION_JSON))
                .andReturn().getResponse();
 
        // then
        assertThat(response.getStatus()).isEqualTo(HttpStatus.NOT_FOUND.value());
        assertThat(response.getContentAsString()).isEmpty();
    }
 
    @Test
    public void canRetrieveByNameWhenExists() throws Exception {
        // given
        given(superHeroRepository.getSuperHero("RobotMan"))
                .willReturn(Optional.of(new SuperHero("Rob", "Mannon", "RobotMan")));
 
        // when
        MockHttpServletResponse response = mvc.perform(
                get("/superheroes/?name=RobotMan")
                        .accept(MediaType.APPLICATION_JSON))
                .andReturn().getResponse();
 
        // then
        assertThat(response.getStatus()).isEqualTo(HttpStatus.OK.value());
        assertThat(response.getContentAsString()).isEqualTo(
                jsonSuperHero.write(new SuperHero("Rob", "Mannon", "RobotMan")).getJson()
        );
    }
 
    @Test
    public void canRetrieveByNameWhenDoesNotExist() throws Exception {
        // given
        given(superHeroRepository.getSuperHero("RobotMan"))
                .willReturn(Optional.empty());
 
        // when
        MockHttpServletResponse response = mvc.perform(
                get("/superheroes/?name=RobotMan")
                        .accept(MediaType.APPLICATION_JSON))
                .andReturn().getResponse();
 
        // then
        assertThat(response.getStatus()).isEqualTo(HttpStatus.OK.value());
        assertThat(response.getContentAsString()).isEqualTo("null");
    }
 
    @Test
    public void canCreateANewSuperHero() throws Exception {
        // when
        MockHttpServletResponse response = mvc.perform(
                post("/superheroes/").contentType(MediaType.APPLICATION_JSON).content(
                        jsonSuperHero.write(new SuperHero("Rob", "Mannon", "RobotMan")).getJson()
                )).andReturn().getResponse();
 
        // then
        assertThat(response.getStatus()).isEqualTo(HttpStatus.CREATED.value());
    }
 
    @Test
    public void headerIsPresent() throws Exception {
        // when
        MockHttpServletResponse response = mvc.perform(
                get("/superheroes/2")
                        .accept(MediaType.APPLICATION_JSON))
                .andReturn().getResponse();
 
        // then
        assertThat(response.getStatus()).isEqualTo(HttpStatus.OK.value());
        assertThat(response.getHeaders("X-SUPERHERO-APP")).containsOnly("super-header");
    }
}
```

코드에 대한 자세한 설명은 다음 섹션에서 하겠다.

