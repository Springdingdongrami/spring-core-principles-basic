# 1. 객체 지향 설계와 스프링

### 스프링이란?

- 자바 언어 기반의 프레임워크 - 객체 지향 언어가 가진 강력한 특징을 살려내는 프레임워크

### 좋은 객체 지향 프로그래밍이란?

- 객체 지향 프로그래밍
    - “객체”들의 모임 → 메시지를 주고받고, 데이터를 처리
    - 프로그램을 유연하고 변경이 용이하게 만든다.

- **다형성 ⭐️⭐️⭐️**
    - 역할(인터페이스)과 구현(인터페이스를 구현한 클래스, 구현 객체)을 분리
    - 오버라이딩 → 다형성으로 인터페이스를 구현한 객체를 실행 시점에 유연하게 변경할 수 있다.
    - 클라이언트를 변경하지 않고, 서버의 구현 기능을 유연하게 변경할 수 있다. ⭐️⭐️⭐️
    - 스프링의 IoC(제어의 역전), DI(의존관계 주입)는 다형성을 활용해서 역할과 구현을 편리하게 다룰 수 있도록 지원한다.

### 좋은 객체 지향 설계의 5가지 원칙(SOLID)

- SRP 단일 책임 원칙
    - 한 클래스는 하나의 책임만 가져야 한다.
    - 중요한 기준은 **변경**이다. 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것
- OCP 개방-폐쇄 원칙
    - 소프트웨어 요소는 **확장에는 열려** 있으나 **변경에는 닫혀** 있어야 한다.
    - 다형성 → 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능을 구현 (역할과 구현의 분리)
- LSP 리스코프 치환 원칙
    - 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
- ISP 인터페이스 분리 원칙
    - 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.
        - 자동차 인터페이스 → 운전 인터페이스 / 정비 인터페이스
        - 사용자 클라이언트 → 운전자 클라이언트 / 정비사 클라이언트
        - 분리하면 정비 인터페이스 자체가 변해도 운전자 클라이언트에 영향을 주지 않는다.
- DIP 의존관계 역전 원칙
    - 추상화에 의존해야지, 구체화에 의존하면 안 된다.
    - 구현 클래스에 의존하지 말고, 인터페이스에 의존하라는 뜻 (**구현이 아닌 역할에 의존**)

<aside>
💡 정리

- 객체 지향의 핵심은 **다형성**
- 다형성 만으로는 쉽게 부품을 갈아 끼우듯이 개발할 수 없다.
- 다형성 만으로는 구현 객체를 변경할 때 클라이언트 코드도 함께 변경된다.
- **다형성 만으로는 OCP, DIP를 지킬 수 없다.**
- 뭔가 더 필요하다. !!!
</aside>

### 객체 지향 설계와 스프링

- 스프링은 DI(의존관계, 의존성 주입), DI 컨테이너 기술로 다형성 + OCP, DIP를 가능하게 지원한다.
- 클라이언트 코드의 변경 없이 기능 확장이 가능하다.
- 모든 설계에 **역할**과 **구현**을 분리하자. → 인터페이스 도입

# 2. 스프링 핵심 원리 이해 1 - 예제 만들기

### 비즈니스 요구사항과 설계

- 회원
    - 회원을 가입하고 조회할 수 있다.
    - 회원은 일반과 VIP 두 가지 등급이 있다.
    - 회원 데이터는 자체 DB를 구축할 수 있고, 외부 시스템과 연동할 수 있다.
- 주문과 할인 정책
    - 회원은 상품을 주문할 수 있다.
    - 회원 등급에 따라 할인 정책을 적용할 수 있다.
    - 할할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용한다.
    - 할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지 못했고, 오픈 직전까지 고민을 미루 고 싶다.  최악의 경우 할인을 적용하지 않을 수도 있다.

→ 인터페이스를 만들고 구현체를 언제든지 갈아끼울 수 있도록 설계하자.

### 회원 도메인

- 회원 도메인 협력 관계
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/bdec87c6-e03d-46a0-8500-cc3a93c40b7f)

    
- 회원 클래스 다이어그램
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/c8eb38ea-8564-4b9b-87c1-476e2d0d7ba7)

    
- 회원 객체 다이어그램
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/4e3609b0-4204-4f10-b59f-065426fa51d9)

    

** 회원 도메인 설계의 문제점: 의존관계가 인터페이스 뿐만 아니라 구현까지 모두 의존하고 있다.

### 주문과 할인 도메인

- 주문 도메인 협력, 역할, 책임
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/be41e876-73b7-4e3f-9dc8-0f018330b1ab)

    
    1. **주문 생성**: 클라이언트는 주문 서비스에 주문 생성을 요청한다.
    2. **회원 조회**: 할인을 위해서는 회원 등급이 필요하다. 그래서 주문 서비스는 회원 저장소에서 회원을 조회한다.
    3. **할인 적용**: 주문 서비스는 회원 등급에 따른 할인 여부를 할인 정책에 위임한다.
    4. **주문 결과 반환**: 주문 서비스는 할인 결과를 포함한 주문 결과를 반환한다.
- 주문 도메인 전체
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/0530bc5f-45a6-4c83-98bd-a4041a460d16)

    
    - 역할과 구현을 분리해서 자유롭게 구현 객체를 조립할 수 있게 설계했다.
    - 덕분에 회원 저장소와 할인 정책을 유연하게 변경할 수 있다.
- 주문 도메인 클래스 다이어그램
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/cf1ea6bf-10d9-4946-9cdd-f997e3a71101)

    
- 주문 도메인 객체 다이어그램1
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/1db1ca7e-a4ae-4726-8a4a-8929eb7d0359)

    
    - 회원을 메모리에서 조회하고, 정액 할인 정책(고정 금액)을 지원해도 주문 서비스를 변경하지 않아도 된다.
    - 역할들의 협력 관계를 그대로 재사용 할 수 있다.
- 주문 도메인 객체 다이어그램2
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/c711cc96-0b65-4bb8-b83d-3b614d4c246d)

    
    - 회원을 메모리가 아닌 실제 DB에서 조회하고, 정률 할인 정책(주문 금액에 따라 % 할인)을 지원해도 주문 서비스를 변경하지 않아도 된다.
    - 협력 관계를 그대로 재사용 할 수 있다.

# 3. 스프링 핵심 원리 이해 2 - 객체 지향 원리 적용

### 새로운 할인 정책 개발

- 기존) VIP 고객은 1000원 할인
- 변경) VIP 고객은 10% 할인율 적용

### 새로운 할인 정책 적용과 문제점

- 할인 정책을 변경하려면 클라이언트인 `OrderServiceImpl` 코드를 고쳐야 한다.
    
    ```java
    public class OrderServiceImpl implements OrderService {
     //    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
         private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
     }
    ```
    
    - 역할과 구현 분리 → O
    - 다형성 활용, 인터페이스와 구현 객체 분리 → O
    - 객체지향 설계 원칙 준수 → X
        - DIP: 주문 서비스 클라이언트(`OrderServiceImpl`)는 `DiscountPolicy` 인터페이스(추상)에 의존하면서 `FixDiscountPolicy`, `RateDiscountPolicy` 클래스(구현)에도 의존하고 있다.
        - OCP: 현재 코드는 기능을 확장해서 변경하면, 클라이언트 코드에 영향을 준다. 따라서 OCP를 위반한다.

- 문제점과 해결 방법
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/35efcc4b-a909-4f79-a361-9a92a454ead6)

    
    - `FixDiscountPolicy`를 `RateDiscountPolicy`로 변경하는 순간 `OrderServiceImpl`의 소스 코드도 함께 변경해야 한다! ****OCP 위반****
    - 클라이언트 코드인 `OrderServiceImpl`은 `DiscountPolicy`의 인터페이스 뿐만 아니라 구체 클래스도 함께 의존한다. 그래서 구체 클래스를 변경할 때 클라이언트 코드도 함께 변경해야 한다. ****DIP 위반****
    - **추상에만 의존하도록 변경하자. !!!! (인터페이스에만 의존)**
    - 그런데 구현체가 없는데 어떻게 코드를 실행할 수 있을까? 이 문제를 해결하려면 누군가가 클라이언트인 `OrderServiceImpl`에 `DiscountPolicy`의 구현 객체를 대신 생성하고 주입해주어야 한다.

### 관심사의 분리

- AppConfig
    - 애플리케이션의 전체 동작 방식을 구성하기 위해 **구현 객체를 생성**하고, **연결**하는 책임을 가지는 별도의 설정 클래스를 만들자.
        
        ```java
        package hello.core;
        
        import hello.core.discount.FixDiscountPolicy;
        import hello.core.member.MemberService;
        import hello.core.member.MemberServiceImpl;
        import hello.core.member.MemoryMemberRepository;
        import hello.core.order.OrderService;
        import hello.core.order.OrderServiceImpl;
        
        public class AppConfig {
        
            public MemberService memberService() {
                return new MemberServiceImpl(new MemoryMemberRepository());
            }
        
            public OrderService orderService() {
                return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
            }
            
        }
        ```
        
    - AppConfig는 애플리케이션의 실제 동작에 필요한 **구현 객체를 생성**한다.
        - MemberServiceImpl
        - MemoryMemberRepository
        - OrderServiceImpl
        - FixDiscountPolicy
    - AppConfig는 생성한 객체 인스턴스의 참조를 **생성자를 통해서 주입(연결)**해준다.
        - MemberServiceImpl → MemoryMemberRepository
            
            ```java
            package hello.core.member;
            
            public class MemberServiceImpl implements MemberService {
            
                private final MemberRepository memberRepository;
            
                public MemberServiceImpl(MemberRepository memberRepository) {
                    this.memberRepository = memberRepository;
                }
            
                @Override
                public void join(Member member) {
                    memberRepository.save(member);
                }
            
                @Override
                public Member findMember(Long memberId) {
                    return memberRepository.findById(memberId);
                }
            }
            ```
            
        - OrderServiceImpl → MemoryMemberRepository, FixDiscountPolicy
            
            ```java
            package hello.core.order;
            
            import hello.core.discount.DiscountPolicy;
            import hello.core.member.Member;
            import hello.core.member.MemberRepository;
            
            public class OrderServiceImpl implements OrderService {
            
                private final MemberRepository memberRepository;
                private DiscountPolicy discountPolicy;
            
                public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
                    this.memberRepository = memberRepository;
                    this.discountPolicy = discountPolicy;
                }
            
                @Override
                public Order createOrder(Long memberId, String itemName, int itemPrice) {
                    Member member = memberRepository.findById(memberId);
                    int discountPrice = discountPolicy.discount(member, itemPrice); // 단일 책임 원칙
            
                    return new Order(memberId, itemName, itemPrice, discountPrice);
                }
            }
            ```
            
        - 설계 변경으로 `MemberServiceImpl`은 `MemoryMemberRepository`를 의존하지 않는다! 단지 `MemberRepository` 인터페이스만 의존한다.
        - `MemberServiceImpl` 입장에서 생성자를 통해 어떤 구현 객체가 들어올지(주입될지)는 알 수 없다.
        - `MemberServiceImpl` 의 생성자를 통해서 어떤 구현 객체를 주입할지는 오직 외부( `AppConfig` )에서 결정된다.
    
- 클래스 다이어그램
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/82c9e272-76d3-4c1e-99b7-b3dd12fb0a8a)

    
    - 객체의 생성과 연결은 `AppConfig`가 담당한다.
    - **DIP 완성**: `MemberServiceImpl`은 `MemberRepository`인 추상에만 의존하면 된다. 이제 구체 클래스를 몰라도 된다.
    - **관심사의 분리**: 객체를 생성하고 연결하는 역할과 실행하는 역할이 명확히 분리되었다.

- 회원 객체 인스턴스 다이어그램
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/0666ff46-c121-4547-bb02-87cb4cbbaf65)

    
    - `appConfig` 객체는 `memoryMemberRepository` 객체를 생성하고 그 참조값을 `memberServiceImpl`을 생성하면서 생성자로 전달한다.
    - 클라이언트인 `memberServiceImpl` 입장에서 보면 의존관계를 마치 외부에서 주입해주는 것 같다고 해서 **DI(Dependency Injection)** 우리말로 의존관계 주입 또는 의존성 주입이라 한다.

### AppConfig 리팩토링

![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/e23b850d-1cc0-4924-8278-4151228ffaf0)


- 현재 AppConfig를 보면 중복이 있고, 역할에 따른 구현이 잘 안 보인다. → 리팩토링 하자.

```java
package hello.core;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.member.MemberRepository;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.member.MemoryMemberRepository;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;

public class AppConfig {

    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public static DiscountPolicy discountPolicy() {
        return new FixDiscountPolicy();
    }

}
```

- `new MemoryMemberRepository()` 이 부분이 **중복 제거**되었다. 이제 `MemoryMemberRepository`를 다른 구현체로 변경할 때 한 부분만 변경하면 된다.
- `AppConfig` 를 보면 **역할**과 **구현** 클래스가 한눈에 들어온다. 애플리케이션 전체 구성이 어떻게 되어 있는지 빠르게 파악할 수 있다.

### 새로운 구조와 할인 정책 적용

- AppConfig의 등장으로 애플리케이션이 크게 사용 영역과 객체를 생성하고 구성하는 영역으로 분리되었다.
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/3082a615-4247-4cb8-b045-1b6116ba14cf)

    
    - 이제 할인 정책을 변경해도, 애플리케이션의 구성 역할을 담당하는 AppConfig만 변경하면 된다. 클라이언트 코드인 `OrderServiceImpl`를 포함해서 사용 영역의 어떤 코드도 변경할 필요가 없다.
    - 구성 영역은 당연히 변경된다. 
    (AppConfig - `FixDiscountPolicy` → `RateDiscountPolicy`)

### IoC, DI 그리고 컨테이너

- 제어의 역전 IoC (Inversion of Control)
    - 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것
    - AppConfig가 등장한 이후에 구현 객체는 자신의 로직을 실행하는 역할만 담당한다. 프로그램의 제어 흐 름은 이제 AppConfig가 가져간다. 예를 들어서 `OrderServiceImpl` 은 필요한 인터페이스들을 호출하지만 어떤 구현 객체들이 실행될지 모른다.
    - 프로그램에 대한 제어 흐름에 대한 권한은 모두 AppConfig가 가지고 있다. 심지어 `OrderServiceImpl`도 AppConfig가 생성한다. 그리고 AppConfig는 `OrderServiceImpl`이 아닌 OrderService 인터페이스의 다른 구현 객체를 생성하고 실행할 수 도 있다. 그런 사실도 모른 채 `OrderServiceImpl` 은 묵묵히 자신의 로직을 실행할 뿐이다.

** 프레임워크(프레임워크가 코드를 제어하고 실행) vs 라이브러리(내 코드가 직접 제어의 흐름을 담당)

- 의존관계 주입 DI (Dependency Injection)
    - 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것
    - 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결된다.
    - 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다. ⭐️⭐️⭐️
    - 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다. ⭐️⭐️⭐️

- IoC 컨테이너, DI 컨테이너
    - 객체를 생성하고 관리하면서 의존관계를 연결해주는 것 (AppConfig)

### 스프링으로 전환하기

- AppConfig
    
    ```java
    package hello.core;
    
    import hello.core.discount.DiscountPolicy;
    import hello.core.discount.RateDiscountPolicy;
    import hello.core.member.MemberRepository;
    import hello.core.member.MemberService;
    import hello.core.member.MemberServiceImpl;
    import hello.core.member.MemoryMemberRepository;
    import hello.core.order.OrderService;
    import hello.core.order.OrderServiceImpl;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    public class AppConfig {
    
        @Bean
        public MemberService memberService() {
            return new MemberServiceImpl(memberRepository());
        }
    
        @Bean
        public MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }
    
        @Bean
        public OrderService orderService() {
            return new OrderServiceImpl(memberRepository(), discountPolicy());
        }
    
        @Bean
        private static DiscountPolicy discountPolicy() {
    //        return new FixDiscountPolicy();
            return new RateDiscountPolicy();
        }
    
    }
    ```
    
    - `@Configuration` : 설정 구성
    - `@Bean` : 스프링 컨테이너에 등록

- MemberApp
    
    ```java
    package hello.core;
    
    import hello.core.member.Grade;
    import hello.core.member.Member;
    import hello.core.member.MemberService;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class MemberApp {
        public static void main(String[] args) {
    //        AppConfig appConfig = new AppConfig();
    //        MemberService memberService = appConfig.memberService();
    
            // 스프링 컨테이너
            ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
            MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
            
            Member member = new Member(1L, "memberA", Grade.VIP);
            memberService.join(member);
    
            Member findMember = memberService.findMember(1L);
    
            System.out.println("new member = " + member.getName());
            System.out.println("find member = " + findMember.getName());
        }
    }
    ```
    

- OrderApp
    
    ```java
    package hello.core;
    
    import hello.core.member.Grade;
    import hello.core.member.Member;
    import hello.core.member.MemberService;
    import hello.core.order.Order;
    import hello.core.order.OrderService;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class OrderApp {
        public static void main(String[] args) {
    //        AppConfig appConfig = new AppConfig();
    //        MemberService memberService = appConfig.memberService();
    //        OrderService orderService = appConfig.orderService();
    
            // 스프링 컨테이너
            ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
            MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
            OrderService orderService = applicationContext.getBean("orderService", OrderService.class);
    
            Long memberId = 1L;
            Member member = new Member(memberId, "itemA", Grade.VIP);
            memberService.join(member);
    
            Order order = orderService.createOrder(memberId, "itemA", 10000);
    
            System.out.println("order = " + order);
        }
    }
    ```
    

- 스프링 컨테이너
    - `ApplicationContext`
    - 스프링 컨테이너는 `@Configuration`이 붙은 AppConfig를 설정(구성) 정보로 사용한다. 여기 `@Bean`이라 적힌 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다. 이렇게 스프링 컨테이너에 등록된 객체를 스프링 빈이라 한다.
    - 스프링 빈은 `@Bean`이 붙은 메서드의 명을 스프링 빈의 이름으로 사용한다. (memberService, orderService)
    - 스프링 빈은 `applicationContext.getBean()` 메서드를 사용해서 찾을 수 있다.
    - 기존에는 개발자가 직접 자바코드로 모든 것을 했다면 이제부터는 스프링 컨테이너에 객체를 스프링 빈으로 등록하고, 스프링 컨테이너에서 스프링 빈을 찾아서 사용하도록 변경되었다.

# 4. 스프링 컨테이너와 스프링 빈

### 스프링 컨테이너 생성

- 스프링 컨테이너 생성
    
    ```java
    //스프링 컨테이너 생성
    ApplicationContext applicationContext =
                         new AnnotationConfigApplicationContext(AppConfig.class);
    ```
    
    - `ApplicationContext`를 스프링 컨테이너라고 한다.
    - `ApplicationContext`는 인터페이스이다. (다형성 적용)
    - 스프링 컨테이너는 XML을 기반으로 만들 수 있고, 애노테이션 기반의 자바 설정 클래스로 만들 수 있다.
    - 직전에 `AppConfig`를 사용했던 방식이 애노테이션 기반의 자바 설정 클래스로 스프링 컨테이너를 만든 것이다.

- 스프링 컨테이너의 생성 과정
    1. 스프링 컨테이너 생성
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/a9b21561-44e7-46f9-9b43-1390e6f0a39d)

        
        - `new AnnotationConfigApplicationContext(AppConfig.class)`
        - 스프링 컨테이너를 생성할 때는 구성 정보를 지정해주어야 한다.
        - 여기서는 AppConfig.class를 구성 정보로 지정했다.
    2. 스프링 빈 등록
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/d3ea9168-058b-4983-bdbe-51305280ef92)

        
        - 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해서 스프링 빈을 등록한다.
        - 빈 이름은 메서드 이름을 사용한다. 직접 부여도 가능 `@Bean(name="memberService2")`
        
        ** 빈 이름은 중복될 수 없다. 
        
    3. 스프링 빈 의존관계 설정
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/999f4eb6-37b9-4ca8-a5ec-a375e9d2c50e)

        
        - 스프링 컨테이너는 설정 정보를 참고해서 의존관계를 주입(DI)한다.
        
        ** 스프링은 빈을 생성하고, 의존관계를 주입하는 단계가 나누어져 있다. 그런데 이렇게 자바 코드로 스프링 빈을 등록하면 생성자를 호출하면서 의존관계 주입도 한번에 처리된다.
        

### 컨테이너에 등록된 모든 빈 조회

```java
package hello.core.beanfind;

import hello.core.AppConfig;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ApplicationContextInfoTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("모든 빈 출력하기")
    void findAllBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            Object bean = ac.getBean(beanDefinitionName);
            System.out.println("name = " + beanDefinitionName + " / object = " + bean);
        }
    }

    @Test
    @DisplayName("애플리케이션 빈 출력하기")
    void findApplicationBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

            // ROLE_APPLICATION: 직접 등록한 애플리케이션 빈
            // ROLE_INFRASTRUCTURE: 스프링이 내부에서 사용하는 빈
            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                Object bean = ac.getBean(beanDefinitionName);
                System.out.println("name = " + beanDefinitionName + " / object = " + bean);
            }
        }
    }
}
```

- `ac.getBeanDefinitionNames()` : 스프링에 등록된 모든 빈 이름을 조회한다.
- `ac.getBean()` : 빈 이름으로 빈 객체(인스턴스)를 조회한다.
- 스프링이 내부에서 사용하는 빈은 `getRole()` 로 구분할 수 있다.
    - `ROLE_APPLICATION` : 일반적으로 사용자가 정의한 빈
    - `ROLE_INFRASTRUCTURE` : 스프링이 내부에서 사용하는 빈

### 스프링 빈 조회

- 기본
    
    ```java
    package hello.core.beanfind;
    
    import hello.core.AppConfig;
    import hello.core.member.MemberService;
    import hello.core.member.MemberServiceImpl;
    import org.junit.jupiter.api.DisplayName;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.NoSuchBeanDefinitionException;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    import static org.assertj.core.api.Assertions.assertThat;
    import static org.junit.jupiter.api.Assertions.assertThrows;
    
    public class ApplicationContextBasicFindTest {
    
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    
        @Test
        @DisplayName("빈 이름으로 조회")
        void findBeanByName() {
            MemberService memberService = ac.getBean("memberService", MemberService.class);
            assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
        }
    
        @Test
        @DisplayName("이름 없이 타입으로만 조회")
        void findBeanByType() {
            MemberService memberService = ac.getBean(MemberService.class);
            assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
        }
    
        @Test
        @DisplayName("구체 타입으로 조회")
        void findBeanByName2() { // 구현에 의존하기 때문에 좋은 코드는 아니다.
            MemberService memberService = ac.getBean("memberService", MemberServiceImpl.class);
            assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
        }
    
        @Test
        @DisplayName("빈 이름으로 조회 X")
        void findBeanByNameX() {
            MemberService xxxxx = ac.getBean("xxxxx", MemberService.class);
            assertThrows(NoSuchBeanDefinitionException.class,
                    () -> ac.getBean("xxxxx", MemberService.class));
        }
        
    }
    ```
    

- 동일한 타입이 둘 이상
    
    ```java
    package hello.core.beanfind;
    
    import hello.core.member.MemberRepository;
    import hello.core.member.MemoryMemberRepository;
    import org.junit.jupiter.api.DisplayName;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.NoUniqueBeanDefinitionException;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    import java.util.Map;
    
    import static org.assertj.core.api.Assertions.assertThat;
    import static org.junit.jupiter.api.Assertions.assertThrows;
    
    public class ApplicationContextSameBeanFindTest {
    
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SameBeanConfig.class);
    
        @Test
        @DisplayName("타입으로 조회 시 같은 타입이 둘 이상 있으면, 중복 오류가 발생한다.")
        void findBeanByTypeDuplicate() {
            assertThrows(NoUniqueBeanDefinitionException.class,
                    () -> ac.getBean(MemberRepository.class));
        }
    
        @Test
        @DisplayName("타입으로 조회 시 같은 타입이 둘 이상 있으면, 빈 이름을 지정하면 된다.")
        void findBeanByName() {
            MemberRepository memberRepository = ac.getBean("memberRepository1", MemberRepository.class);
            assertThat(memberRepository).isInstanceOf(MemberRepository.class);
        }
    
        @Test
        @DisplayName("특정 타입을 모두 조회하기")
        void findAllBeanByType() {
            Map<String, MemberRepository> beansOfType = ac.getBeansOfType(MemberRepository.class);
            for (String key : beansOfType.keySet()) {
                System.out.println("key = " + key + " / value = " + beansOfType.get(key));
            }
            System.out.println("beansOfType = " + beansOfType);
            assertThat(beansOfType.size()).isEqualTo(2);
        }
    
        @Configuration
        static class SameBeanConfig {
    
            @Bean
            public MemberRepository memberRepository1() {
                return new MemoryMemberRepository();
            }
    
            @Bean
            public MemberRepository memberRepository2() {
                return new MemoryMemberRepository();
            }
        }
    }
    ```
    

- 상속 관계
    
    ```java
    package hello.core.beanfind;
    
    import hello.core.discount.DiscountPolicy;
    import hello.core.discount.FixDiscountPolicy;
    import hello.core.discount.RateDiscountPolicy;
    import org.junit.jupiter.api.DisplayName;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.NoUniqueBeanDefinitionException;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    import java.util.Map;
    
    import static org.assertj.core.api.AssertionsForClassTypes.assertThat;
    import static org.junit.jupiter.api.Assertions.assertThrows;
    
    public class ApplicationContextExtendsFindTest {
    
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
    
        @Test
        @DisplayName("부모 타입으로 조회 시, 자식이 둘 이상 있으면 중복 오류가 발생한다.")
        void findBeanByParentTypeDuplicate() {
            assertThrows(NoUniqueBeanDefinitionException.class,
                    () -> ac.getBean(DiscountPolicy.class));
        }
    
        @Test
        @DisplayName("부모 타입으로 조회 시, 자식이 둘 이상 있으면 빈 이름을 지정하면 된다.")
        void findBeanByParentTypeBeanName() {
            DiscountPolicy rateDisCountPolicy = ac.getBean("rateDiscountPolicy", DiscountPolicy.class);
            assertThat(rateDisCountPolicy).isInstanceOf(RateDiscountPolicy.class);
        }
    
        @Test
        @DisplayName("특정 하위 타입으로 조회")
        void findBeanBySubType() {
            RateDiscountPolicy bean = ac.getBean(RateDiscountPolicy.class);
            assertThat(bean).isInstanceOf(RateDiscountPolicy.class);
        }
    
        @Test
        @DisplayName("부모 타입으로 모두 조회")
        void findAllBeanByParentType() {
            Map<String, DiscountPolicy> beansOfType = ac.getBeansOfType(DiscountPolicy.class);
            assertThat(beansOfType.size()).isEqualTo(2);
            for (String key : beansOfType.keySet()) {
                System.out.println("key = " + key + " / value = " + beansOfType.get(key));
            }
        }
    
        @Test
        @DisplayName("부모 타입으로 모두 조회 - Object")
        void findAllBeanByObjectType() {
            Map<String, Object> beansOfType = ac.getBeansOfType(Object.class);
            for (String key : beansOfType.keySet()) {
                System.out.println("key = " + key + " / value = " + beansOfType.get(key));
            }
        }
    
        @Configuration
        static class TestConfig {
            @Bean
            public DiscountPolicy rateDiscountPolicy() {
                return new RateDiscountPolicy();
            }
    
            @Bean
            public DiscountPolicy fixDiscountPolicy() {
                return new FixDiscountPolicy();
            }
        }
    }
    ```
    
    - 부모 타입으로 조회하면, 자식 타입도 함께 조회한다.
    - 그래서 `Object` 타입으로 조회하면, 모든 스프링 빈을 조회한다.

### BeanFactory와 ApplicationContext

![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/34c5ea31-b1ec-4d62-8784-50e0eaa323eb)


- BeanFactory
    - 스프링 컨테이너의 최상위 인터페이스
    - 스프링 빈을 관리하고 조회
- ApplicationContext
    - BeanFactory 기능을 모두 상속 받아서 제공
    - 빈을 관리하고 검색하는 기능 + @

<< ApplicationContext가 제공하는 부가 기능>>

![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/c0365598-fac3-46c0-a57d-a13aad263731)


- 메시지소스를 활용한 국제화 기능
    - ex) 한국에서 들어오면 한국어로, 영어권에서 들어오면 영어로 출력
- 환경변수
    - 로컬, 개발, 운영 등을 구분해서 처리
- 애플리케이션 이벤트
    - 이벤트를 발행하고 구독하는 모델을 편리하게 지원
- 편리한 리소스 조회
    - 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회

### 다양한 설정 형식 지원 - 자바 코드, XML

![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/dec0012c-06c5-4681-9a40-02ff2d62fc7a)


- 애노테이션 기반 자바 코드 설정 사용
    - new AnnotationConfigApplicationContext(AppConfig.class)
    - AnnotationConfigApplicationContext 클래스를 사용하면서 자바 코드로 된 설정 정보를 넘기면 된다.
- XML 설정 사용
    - GenericXmlApplicationContext를 사용하면서 xml 설정 파일을 넘기면 된다.
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        
            <bean id="memberService" class="hello.core.member.MemberServiceImpl" >
                <constructor-arg name="memberRepository" ref="memberRepository" />
            </bean>
        
            <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository" />
        
            <bean id="orderService" class="hello.core.order.OrderServiceImpl" >
                <constructor-arg name="memberRepository" ref="memberRepository" />
                <constructor-arg name="discountPolicy" ref="discountPolicy" />
            </bean>
        
            <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy" />
        
        </beans>
        ```
        

### 스프링 빈 설정 메타 정보 - BeanDefinition

![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/fb2ac544-66ea-405b-8510-a1cf6d7d048a)


- `BeanDefinition` (빈 설정 메타 정보)
    - 스프링은 BeanDefinition이라는 추상화가 있기 때문에 다양한 설정 형식을 지원할 수 있다.
    - 쉽게 말해서 역할과 구현을 개념적으로 나눈 것이다.
    - XML을 읽어서 BeanDefinition을 만들면 되고, 자바 코드를 읽어서 BeanDefinition을 만들면 된다.
    - 스프링 컨테이너는 이 메타정보를 기반으로 스프링 빈을 생성한다.

- BeanDefinition 정보
    
    ```java
    package hello.core.beandifinition;
    
    import hello.core.AppConfig;
    import org.junit.jupiter.api.DisplayName;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.config.BeanDefinition;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class BeanDefinitionTest {
    
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    
        @Test
        @DisplayName("빈 설정 메타정보 확인")
        void findApplicationBean() {
            String[] beanDefinitionNames = ac.getBeanDefinitionNames();
            for (String beanDefinitionName : beanDefinitionNames) {
                BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);
    
                if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                    System.out.println("beanDefinitionName = " + beanDefinitionName +
                            " / beanDefinition = " + beanDefinition);
                }
            }
        }
    }
    ```
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/a20458d9-bc85-4fa8-8d32-880e7d2bfcb9)

    
    - BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
    - factoryBeanName: 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig
    - factoryMethodName: 빈을 생성할 팩토리 메서드 지정, 예) memberService
    - Scope: 싱글톤(기본값)
    - lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한 생성을 지연 처리 하는지 여부
    - InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
    - DestroyMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
    - Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정 처럼 팩토리 역할의 빈을 사용
    하면 없음)

# 5. 싱글톤 컨테이너

### 웹 애플리케이션과 싱글톤

- 스프링 없는 순수한 DI 컨테이너
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/2d397292-d354-4d06-83aa-6442cf906c64)

    
    - 우리가 만들었던 스프링 없는 순수한 DI 컨테이너인 AppConfig는 요청을 할 때마다 객체를 새로 생성한다. → 메모리 낭비가 심하다.
    - 해결 방안: 해당 객체가 딱 1개만 생성되고 공유하도록 설계하면 된다. ⇒ 싱글톤 패턴

### 싱글톤 패턴

- 싱글톤 패턴
    - 클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴이다.
    - 그래서 객체 인스턴스를 2개 이상 생성하지 못하도록 막아야 한다.
        - private 생성자를 사용해서 외부에서 임의로 new 키워드를 사용하지 못하도록 막아야 한다.
    - 싱글톤 패턴을 적용한 예제 코드
        
        ```java
        package hello.core.singleton;
        
        public class SingletonService {
        
            private static final SingletonService instance = new SingletonService();
        
            public static SingletonService getInstance() {
                return instance;
            }
        
            private SingletonService() {
        
            }
        
            public void logic() {
                System.out.println("싱글톤 객체 로직 호출");
            }
        }
        ```
        
        1. static 영역에 객체 instance를 미리 하나 생성해서 올려둔다.
        2. 이 객체 인스턴스가 필요하면 오직 `getInstance()` 메서드를 통해서만 조회할 수 있다. 이 메서드를 호출하면 항상 같은 인스턴스를 반환한다.
        3. 딱 1개의 객체 인스턴스만 존재해야 하므로, 생성자를 private으로 막아서 혹시라도 외부에서 new 키워드 로 객체 인스턴스가 생성되는 것을 막는다.
    - 싱글톤 패턴을 적용하면 고객의 요청이 올 때 마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유해서 효율적으로 사용할 수 있다.

- 싱글톤 패턴의 문제점
    - 구현 코드 자체가 많이 들어간다.
    - 의존 관계 상 클라이언트가 구체 클래스에 의존한다. → DIP 위반
    - 클라이언트가 구체 클래스에 의존해서 OCP 원칙을 위반할 가능성이 높다.
    - 테스트하기 어렵다.
    - 내부 속성을 변경하거나 초기화하기 어렵다.
    - private 생성자로 자식 클래스를 만들기 어렵다.
    - 결론적으로 유연성이 떨어진다.
    - 안티패턴으로 불리기도 한다.

### 싱글톤 컨테이너

- 싱글톤 컨테이너
    - 싱글톤 패턴의 문제점을 해결하면서, 객체 인스턴스를 싱글톤(1개만 생성)으로 관리
    - 지금까지 우리가 학습한 스프링 빈이 바로 싱글톤으로 관리되는 빈이다.

- 스프링 컨테이너
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/d5fbfc27-8ec2-4725-82f2-c52a3d6a4a90)

    
    - 스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리한다.
        - 고객의 요청이 올 때마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유해서 효율적으로 재사용할 수 있다.
    - 스프링 컨테이너는 싱글톤 컨테이너 역할을 한다. 이렇게 싱글톤 객체를 생성하고 관리하는 기능을 싱글톤 레지스트리라 한다.
    - 스프링 컨테이너의 이런 기능 덕분에 싱글톤 패턴의 모든 단점을 해결하면서 객체를 싱글톤으로 유지할 수 있다.
        - 싱글톤 패턴을 위한 지저분한 코드가 들어가지 않아도 된다.
        - DIP, OCP, 테스트, private 생성자로 부터 자유롭게 싱글톤을 사용할 수 있다.

** 스프링의 기본 빈 등록 방식은 싱글톤이지만, 싱글톤 방식만 지원하는 것은 아니다.

### 싱글톤 방식의 주의점

- 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지(stateful)하게 설계하면 안된다. **무상태(stateless)**로 설계해야 한다! ⭐️⭐️⭐️
    - 특정 클라이언트에 의존적인 필드가 있으면 안 된다.
    - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안 된다.
    - 가급적 읽기만 가능해야 한다.
    - 필드 대신에 자바에서 공유되지 않는, 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.
    - 스프링 빈의 필드에 공유 값을 설정하면 정말 큰 장애가 발생할 수 있다!!!

- 문제점 예시
    - StatefulService
        
        ```java
        package hello.core.singleton;
        
        public class StatefulService {
        
            private int price; // 상태를 유지하는 필드
        
            public void order(String name, int price) {
                System.out.println("name = " + name + " / price = " + price);
                this.price = price; // 여기가 문제!
            }
        
            public int getPrice() {
                return price;
            }
            
        }
        ```
        
    - StatefulServiceTest
        
        ```java
        package hello.core.singleton;
        
        import org.junit.jupiter.api.Test;
        import org.springframework.context.ApplicationContext;
        import org.springframework.context.annotation.AnnotationConfigApplicationContext;
        import org.springframework.context.annotation.Bean;
        
        import static org.assertj.core.api.Assertions.assertThat;
        
        class StatefulServiceTest {
        
            @Test
            void statefulServiceSingleton() {
                ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
                StatefulService statefulService1 = ac.getBean(StatefulService.class);
                StatefulService statefulService2 = ac.getBean(StatefulService.class);
        
                // ThreadA : A 사용자 10000원 주문
                statefulService1.order("userA", 10000);
                // ThreadB : B 사용자 20000원 주문
                statefulService2.order("userB", 20000);
        
                // ThreadA : 사용자A 주문 금액 조회
                int price = statefulService1.getPrice();
                System.out.println("price = " + price); // 기대: 10000원, 실제: 20000원
        
                assertThat(statefulService1.getPrice()).isEqualTo(20000);
            }
        
            static class TestConfig {
                @Bean
                public StatefulService statefulService() {
                    return new StatefulService();
                }
            }
        
        }
        ```
        

### @Configuration과 싱글톤

- AppConfig
    
    ```java
    package hello.core;
    
    import hello.core.discount.DiscountPolicy;
    import hello.core.discount.RateDiscountPolicy;
    import hello.core.member.MemberRepository;
    import hello.core.member.MemberService;
    import hello.core.member.MemberServiceImpl;
    import hello.core.member.MemoryMemberRepository;
    import hello.core.order.OrderService;
    import hello.core.order.OrderServiceImpl;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    public class AppConfig {
    
        @Bean
        public MemberService memberService() {
            return new MemberServiceImpl(memberRepository());
        }
    
        @Bean
        public MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }
    
        @Bean
        public OrderService orderService() {
            return new OrderServiceImpl(memberRepository(), discountPolicy());
        }
    
        @Bean
        private static DiscountPolicy discountPolicy() {
    //        return new FixDiscountPolicy();
            return new RateDiscountPolicy();
        }
    
    }
    ```
    
    - @Bean memberService → new MemoryMemberRepository()
    @Bean orderService → new MemoryMemberRepository()
    - 결과적으로 각각 다른 2개의 `MemoryMemberRepository` 가 생성되면서 싱글톤이 깨지는 것처럼 보인다.  스프링 컨테이너는 이 문제를 어떻게 해결할까?

- 테스트 코드
    
    ```java
    package hello.core.singleton;
    
    import hello.core.AppConfig;
    import hello.core.member.MemberRepository;
    import hello.core.member.MemberServiceImpl;
    import hello.core.order.OrderServiceImpl;
    import org.junit.jupiter.api.Test;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    import static org.assertj.core.api.Assertions.assertThat;
    
    public class ConfigurationSingletonTest {
    
        @Test
        void configurationTest() {
            ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    
            MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
            OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
            MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);
    
            MemberRepository memberRepository1 = memberService.getMemberRepository();
            MemberRepository memberRepository2 = orderService.getMemberRepository();
    
            System.out.println("memberService -> memberRepository = " + memberRepository1);
            System.out.println("orderService -> memberRepository = " + memberRepository2);
            System.out.println("memberRepository = " + memberRepository);
    
            assertThat(memberService.getMemberRepository()).isSameAs(memberRepository);
            assertThat(orderService.getMemberRepository()).isSameAs(memberRepository);
        }
        
    }
    ```
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/c62eae1c-1e95-43e2-9373-63e5dfdc9712)

    
    ⇒ 싱글톤 보장 !!!!
    

### @Configuration과 바이트코드 조작의 마법

- 스프링은 클래스의 바이트코드를 조작하는 라이브러리를 사용한다.
    - `@Configuration`을 적용한 `AppConfig`
    - `AnnotationConfigApplicationContext` 에 파라미터로 넘긴 값은 스프링 빈으로 등록된다. 그래서
    `AppConfig` 도 스프링 빈이 된다.
    - AppConfig 스프링 빈을 조회해서 클래스 정보를 출력해보자.
        
        ```java
        @Test
        void configurationDeep() {
            ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
            AppConfig bean = ac.getBean(AppConfig.class);
        
            System.out.println("bean = " + bean.getClass());
        }
        ```
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/a9bb3004-c3e6-4ec7-b0c9-cd9c4378a671)

        
        → 내가 만든 클래스가 아니라 스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것이다!
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/db017121-dd66-4ad3-be3a-ddd42ac7a911)

        
        그 임의의 다른 클래스가 바로 싱글톤이 보장되도록 해준다.
        
    - @Bean이 붙은 메서드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면 생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다.
    
    ** @Configuration을 적용하지 않고, @Bean만 적용하면 AppConfig가 CGLIB 기술 없이 순수한 AppConfig로 스프링 빈에 등록된다. 따라서 MemberRepository가 호출할 때마다 호출되고, 인스턴스가 같은지 테스트하는 코드도 실패하게 된다. (싱글톤이 깨짐!!!!!)
