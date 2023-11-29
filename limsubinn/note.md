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
