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
    - AppConfig는 생성한 객체 인스턴스의 참조를 **생성자를 통해서 주입(연결)** 해준다.
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
    - 애플리케이션 **실행 시점(런타임)** 에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것
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

- 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지(stateful)하게 설계하면 안된다. **무상태(stateless)** 로 설계해야 한다! ⭐️⭐️⭐️
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

  # 6. 컴포넌트 스캔

### 컴포넌트 스캔과 의존관계 자동 주입 시작하기

- 스프링 빈 등록
    - 지금까지 스프링 빈을 등록할 때는 자바 코드의 @Bean이나 XML의 <bean> 등을 통해서 설정 정보에 직접 등록할 스프링 빈을 나열했다.
    - 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공한다.
    - 의존관계도 자동으로 주입하는 `@Autowired` 라는 기능도 제공한다.

- 컴포넌트 스캔과 의존관계 자동 주입
    - AutoAppConfig
        
        ```java
        package hello.core;
        
        import org.springframework.context.annotation.ComponentScan;
        import org.springframework.context.annotation.Configuration;
        import org.springframework.context.annotation.FilterType;
        
        @Configuration
        @ComponentScan(excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class))
        public class AutoAppConfig {
            
        }
        ```
        
        - 컴포넌트 스캔을 사용하려면 설정 정보에 `@ComponentScan`을 붙여준다.
        - 기존의 AppConfig와 달리 @Bean으로 등록한 클래스가 하나도 없다.
        
        ** `excludeFilters`를 이용해서 설정정보는 컴포넌트 스캔 대상에서 제외했다. (기존 예제 코드를 유지하기 위함)
        
    - MemoryMemberRepository, RateDiscountPolicy, MemberServiceImpl, OrderServiceImpl → 컴포넌트 스캔의 대상이 되도록 `@Component`를 붙여준다.
    - 의존관계 주입 → `@Autowired`를 붙여준다.
        
        ```java
        package hello.core.member;
        
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Component;
        
        @Component
        public class MemberServiceImpl implements MemberService {
        
            private final MemberRepository memberRepository;
        
            @Autowired
            public MemberServiceImpl(MemberRepository memberRepository) {
                this.memberRepository = memberRepository;
            }
        
        		...
        
        }
        ```
        

- 동작 과정
    1. @ComponentScan
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/52c94bf6-58c2-41e2-8b75-0384201f9945)

        
        - `@ComponentScan`은 `@Component`가 붙은 모든 클래스를 스프링 빈으로 등록한다.
    2. @Autowired 의존관계 자동 주입
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/1727b826-3f69-4e33-9ee1-51529ad5a909)

        
        - 생성자에 `@Autowired` 를 지정하면, 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 주입한다.
        - 기본 조회 전략은 타입이 같은 빈을 찾아서 주입한다.

### 탐색 위치와 기본 스캔 대상

- 탐색할 패키지의 시작 위치 지정
    
    ```java
    @ComponentScan(
            basePackages = "hello.core.member",
            basePackageClasses = AutoAppConfig.class,
            excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
    )
    ```
    
    - `basePackages`: 탐색할 패키지의 시작 위치 지정 (여러 개 가능)
    - `basePackageClasses`: 지정한 클래스의 패키지를 탐색 시작 위치로 지정
    - 지정하지 않으면 `@ComponentScan`이 붙은 설정 정보 클래스의 패키지가 시작 위치가 된다.
    
    ** 권장: 패키지 위치를 지정하지 않고, 설정 정보 클래스의 위치를 프로젝트 최상단에 두는 것이다.  최근 스프링 부트도 이 방법을 기본으로 제공한다.
    
    ** 스프링 부트의 대표 시작 정보인 `@SpringBootApplication` 안에 `@ComponentScan`이 들어있다.
    

- 컴포넌트 스캔 기본 대상 → 모두 스프링 빈으로 등록된다.
    - `@Component` : 컴포넌트 스캔에서 사용
    - `@Controller` : 스프링 MVC 컨트롤러에서 사용 + 스프링 MVC 컨트롤러로 인식
    - `@Service` : 스프링 비즈니스 로직에서 사용
    - `@Repository` : 스프링 데이터 접근 계층에서 사용 + 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환
    - `@Configuration` : 스프링 설정 정보에서 사용 + 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가 처리

** `useDefaultFilters` 옵션은 기본으로 켜져있는데, 이 옵션을 끄면 기본 스캔 대상들이 제외된다.

### 필터

- 필터
    - `includeFilters` : 컴포넌트 스캔 대상을 추가로 지정한다.
    - `excludeFilters` : 컴포넌트 스캔에서 제외할 대상을 지정한다.

- FilterType 옵션
    - ANNOTATION: 기본값, 애노테이션을 인식해서 동작한다.
    - ASSIGNABLE_TYPE: 지정한 타입과 자식 타입을 인식해서 동작한다.
    - ASPECTJ: AspectJ 패턴 사용
    - REGEX: 정규 표현식
    - CUSTOM: `TypeFilter`라는 인터페이스를 구현해서 처리

### 중복 등록과 충돌

- 자동 빈 등록 vs 자동 빈 등록
    - 컴포넌트 스캔에 의해 자동으로 스프링 빈이 등록되는데, 그 이름이 같은 경우 스프링은 오류를 발생시킨다.
    - `ConflictingBeanDefinitionException`

- 수동 빈 등록 vs 자동 빈 등록
    - 수동 빈 등록이 우선권을 가진다.
 
# 7. 의존관계 자동 주입

### 다양한 의존관계 주입 방법

- 생성자 주입
    
    ```java
    @Component
    public class OrderServiceImpl implements OrderService {
    
        private final MemberRepository memberRepository;
        private final DiscountPolicy discountPolicy;
    
        @Autowired
        public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
        }
    
    		...
    }
    ```
    
    - 생성자를 통해서 의존 관계를 주입 받는 방법
    - 생성자 호출시점에 딱 1번만 호출되는 것이 보장된다.
    - **불변**, **필수** 의존관계에 사용
    - **생성자가 딱 1개만 있으면 @Autowired를 생략해도 자동 주입 된다.** ⭐️⭐️⭐️ (스프링 빈에만 해당)

- 수정자 주입 (setter 주입)
    
    ```java
    @Component
    public class OrderServiceImpl implements OrderService {
    
        private MemberRepository memberRepository;
        private DiscountPolicy discountPolicy;
    
        @Autowired
        public void setMemberRepository(MemberRepository memberRepository) {
            this.memberRepository = memberRepository;
        }
    
        @Autowired
        public void setDiscountPolicy(DiscountPolicy discountPolicy) {
            this.discountPolicy = discountPolicy;
        }
    
    		...
    }
    ```
    
    - setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해서 의존관계를 주입하는 방법
    - **선택**, **변경** 가능성이 있는 의존관계에 사용
    - 자바빈 프로퍼티 규약의 수정자 메서드 방식을 사용하는 방법
    
    ** `@Autowired` 의 기본 동작은 주입할 대상이 없으면 오류가 발생한다. 주입할 대상이 없어도 동작하게 하려면 `@Autowired(required = false)` 로 지정하면 된다.
    
    ** 자바에서는 과거부터 필드의 값을 직접 변경하지 않고, setXxx, getXxx 라는 메서드를 통해서 값을 읽거나 수정하는 규칙을 만들었는데, 그것이 자바빈 프로퍼티 규약이다.
    

- 필드 주입
    
    ```java
    @Component
    public class OrderServiceImpl implements OrderService {
    
        @Autowired
        private MemberRepository memberRepository;
        @Autowired
        private DiscountPolicy discountPolicy;
    
    		...
    }
    ```
    
    - 필드에 바로 주입하는 방법
    - 외부에서 변경이 불가능해서 테스트 하기 힘들다는 치명적인 단점
    - DI 프레임워크가 없으면 아무것도 할 수 없다.
    - 사용하지 말자!

- 일반 메서드 주입
    
    ```java
    @Component
    public class OrderServiceImpl implements OrderService {
    
        private MemberRepository memberRepository;
        private DiscountPolicy discountPolicy;
    
        @Autowired
        public void init(MemberRepository memberRepository, DiscountPolicy
                discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
        }
    
    		...
    }
    ```
    
    - 일반 메서드를 통해서 주입 받을 수 있다.
    - 한 번에 여러 필드를 주입 받을 수 있다.
    - 일반적으로 잘 사용하지 않는다.

### 옵션 처리

- 자동 주입 대상을 옵션으로 처리하는 방법
    - `@Autowired(required=false)` : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출되지 않는다.
    - `org.springframework.lang.@Nullable` : 자동 주입할 대상이 없으면 null이 입력된다.
    - `Optional<>` : 자동 주입할 대상이 없으면 `Optional.empty` 가 입력된다.

- 테스트 코드
    
    ```java
    package hello.core.autowired;
    
    import hello.core.member.Member;
    import jakarta.annotation.Nullable;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    import java.util.Optional;
    
    public class AutowiredTest {
    
        @Test
        void AutowiredOption() {
            new AnnotationConfigApplicationContext(TestBean.class);
        }
    
        static class TestBean {
    
            @Autowired(required = false)
            public void setNoBean1(Member noBean1) {
                System.out.println("noBean1 = " + noBean1);
            }
    
            @Autowired
            public void setNoBean2(@Nullable Member noBean2) {
                System.out.println("noBean2 = " + noBean2);
            }
    
            @Autowired
            public void setNoBean3(Optional<Member> noBean3) {
                System.out.println("noBean3 = " + noBean3);
            }
    
        }
    }
    ```
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/586fe2c1-015b-4f5d-829f-b361ab2baac0)

    
    - noBean1 : 호출 안 됨
    - noBean2 : null 호출
    - noBean3 : Optional.empty 호출
    
    ** Member는 스프링 빈이 아니기 때문에 @Autowired(required=true)이면 예외 터짐.
    

### 생성자 주입을 선택해라!

- 불변
    - 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없다. 오히려 대부분의 의존관계는 애플리케이션 종료 전까지 변하면 안된다.(불변해야 한다.)
    - 수정자 주입을 사용하면, setXxx 메서드를 public으로 열어두어야 한다. 누군가 실수로 변경할 수도 있고, 변경하면 안되는 메서드를 열어두는 것은 좋은 설계 방법이 아니다.
    - 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없다. 따라서 불변하게 설계할 수 있다.

- 누락
    - 생성자 주입을 사용하면 주입 데이터를 누락했을 때 **컴파일 오류**가 발생한다.
    - 그리고 IDE에서 바로 어떤 값을 필수로 주입해야 하는지 알 수 있다.

- final 키워드
    - 생성자 주입을 사용하면 필드에 final 키워드를 사용할 수 있다.
    - 그래서 생성자에서 혹시라도 값이 설정되지 않는 오류를 컴파일 시점에 막아준다.

** 생성자 주입 방식은 순수한 자바 언어의 특징을 잘 살리는 방법이기도 하다.

** 기본으로 생성자 주입을 사용하고, 필수 값이 아닌 경우에는 수정자 주입 방식을 옵션으로 부여하면 된다. 생성자 주입과 수정자 주입을 동시에 사용할 수 있다. (필드 주입은 사용하지 않는게 좋다.)

### 롬복과 최신 트랜드

- 롬복 설정
    - build.gradle
        
        ```java
        // lombok 설정
        configurations {
        	compileOnly {
        		extendsFrom annotationProcessor
        	}
        }
        ```
        
        ```java
        dependencies {
        	// lombok 라이브러리
        	compileOnly 'org.projectlombok:lombok'
        	annotationProcessor 'org.projectlombok:lombok'
        	testCompileOnly 'org.projectlombok:lombok'
        	testAnnotationProcessor 'org.projectlombok:lombok'
        }
        ```
        
    - settings (롬복 플러그인 설치 및 enable annotation processing)
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/bedee626-5067-44e8-851e-7cd1d05aad2a)

        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/6989678c-80cb-41ea-a83a-d615d6416327)

        

- 예시
    
    ```java
    package hello.core;
    
    import lombok.Getter;
    import lombok.Setter;
    import lombok.ToString;
    
    @Getter
    @Setter
    @ToString
    public class HelloLombok {
    
        private String name;
        private int age;
    
        public static void main(String[] args) {
            HelloLombok helloLombok = new HelloLombok();
            helloLombok.setName("hihihi");
    
            String name = helloLombok.getName();
            System.out.println("name = " + name);
    
            System.out.println("helloLombok = " + helloLombok);
        }
    }
    ```
    

- OrderServiceImpl
    1. 생성자 주입
        
        ```java
        @Component
        public class OrderServiceImpl implements OrderService {
        
            private final MemberRepository memberRepository;
            private final DiscountPolicy discountPolicy;
        
            @Autowired
            public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
                this.memberRepository = memberRepository;
                this.discountPolicy = discountPolicy;
            }
        
        		...
        }
        ```
        
    2. 생성자가 1개 있으면 @Autowired 생략 가능
        
        ```java
        @Component
        public class OrderServiceImpl implements OrderService {
        
            private final MemberRepository memberRepository;
            private final DiscountPolicy discountPolicy;
        
            public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
                this.memberRepository = memberRepository;
                this.discountPolicy = discountPolicy;
            }
        
        		...
        }
        ```
        
    3. 롬복 적용
        - `@RequiredArgsConstructor` : final이 붙은 필드를 모아서 생성자를 자동으로 만들어준다.
        
        ```java
        @Component
        @RequiredArgsConstructor
        public class OrderServiceImpl implements OrderService {
        
            private final MemberRepository memberRepository;
            private final DiscountPolicy discountPolicy;
        
        		...
        }
        ```
        

### 조회 빈이 2개 이상 - 문제

`@Autowired`는 타입으로 조회한다. 타입으로 조회하면 선택된 빈이 2개 이상일 때 문제가 발생한다.

이때 하위 타입으로 지정할 수도 있지만, 하위 타입으로 지정하는 것은 DIP를 위배하고 유연성이 떨어진다. 그리고 이름만 다르고, 완전히 똑같은 타입의 스프링 빈이 2개 있을 때 해결이 안 된다.

스프링 빈을 수동 등록해서 문제를 해결해도 되지만, 의존 관계 자동 주입에서 해결하는 여러 방법이 있다.

### @Autowired 필드 명, @Qualifier, @Primary

- @Autowired 필드 명 매칭
    
    ```java
    // 기존 코드
    @Autowired
    private DiscountPolicy discountPolicy
    
    // 변경 코드
    @Autowired
    private DiscountPolicy rateDiscountPolicy
    ```
    
    - `@Autowired` 는 타입 매칭을 시도하고 이때 여러 빈이 있으면 필드 이름, 파라미터 이름으로 빈 이름을 추가 매칭한다.
    - 필드 명을 빈 이름으로 변경한다.

** @Autowired 매칭 정리

1. 타입 매칭
2. 타입 매칭의 결과가 2개 이상일 때 필드 명, 파라미터 명으로 빈 이름 매칭

- @Qualifier 사용
    
    ```java
    @Component
    @Qualifier("mainDiscountPolicy")
    public class RateDiscountPolicy implements DiscountPolicy {}
    
    @Component
    @Qualifier("fixDiscountPolicy")
    public class FixDiscountPolicy implements DiscountPolicy {}
    
    /***********************************************************************/
    
    @Autowired
     public OrderServiceImpl(MemberRepository memberRepository,
                             @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
         this.memberRepository = memberRepository;
         this.discountPolicy = discountPolicy;
    }
    ```
    
    - `@Qualifier` 는 추가 구분자를 붙여주는 방법이다. (주입시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것은 아니다.)
    - 빈 등록 시 @Qualifier를 붙여주고, 주입 시에 @Qualifier를 붙여주고 등록한 이름을 적어준다.

** @Qualifier 정리

1. @Qualifier끼리 매칭

2. 빈 이름 매칭

3. `NoSuchBeanDefinitionException` 예외 발생

- @Primary 사용
    
    ```java
    @Component
    @Primary
    public class RateDiscountPolicy implements DiscountPolicy {}
    
    @Component
    public class FixDiscountPolicy implements DiscountPolicy {}
    
    /***********************************************************************/
    
    // 생성자
    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository,
                             DiscountPolicy discountPolicy) {
         this.memberRepository = memberRepository;
         this.discountPolicy = discountPolicy;
    }
    
    // 수정자
    @Autowired
    public DiscountPolicy setDiscountPolicy(DiscountPolicy discountPolicy) {
         this.discountPolicy = discountPolicy;
    }
    
    // => RateDiscountPolicy가 주입 됨.
    ```
    
    - `@Primary` 는 우선순위를 정하는 방법이다. @Autowired 시에 여러 빈이 매칭되면 `@Primary` 가 우선권을 가진다.

** 메인 데이터베이스의 커넥션을 획득하는 스프링 빈은 `@Primary` 를 적용해서 조회하는 곳에서 `@Qualifier` 지정 없이 편리하게 조회하고, 서브 데이터베이스 커넥션 빈을 획득할 때는 `@Qualifier` 를 지정해서 명시적으로 획득 하는 방식으로 사용하면 코드를 깔끔하게 유지할 수 있다. 물론 이때 메인 데이터베이스의 스프링 빈을 등록할 때 `@Qualifier` 를 지정해주는 것은 상관없다.

** 우선순위: `@Qualifier` > `@Primary`

### 애노테이션 직접 만들기

- `@Qualifier("mainDiscountPolicy")` 이렇게 문자를 적으면 컴파일시 타입 체크가 안된다. 다음과 같은 애노테이션을 만들어서 문제를 해결할 수 있다.
    
    ```java
    package hello.core.annotation;
    
    import org.springframework.beans.factory.annotation.Qualifier;
    
    import java.lang.annotation.*;
    
    @Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER,
            ElementType.TYPE, ElementType.ANNOTATION_TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Qualifier("mainDiscountPolicy")
    public @interface MainDiscountPolicy {
    
    }
    ```
    
    ```java
    @Component
    public class OrderServiceImpl implements OrderService {
    
        private final MemberRepository memberRepository;
        private final DiscountPolicy discountPolicy;
    
        public OrderServiceImpl(MemberRepository memberRepository, @MainDiscountPolicy DiscountPolicy discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
        }
    
    		...
    }
    ```
    

### 조회한 빈이 모두 필요할 때, List, Map

- 의도적으로 정말 해당 타입의 스프링 빈이 다 필요한 경우도 있다. 예를 들어서 할인 서비스를 제공하는데, 클라이언트가 할인의 종류(rate, fix)를 선택할 수 있다고 가정해보자. 스프링을 사용하면 소위 말하는 전략 패턴을 매우 간단하게 구현할 수 있다.
    
    ```java
    package hello.core.autowired;
    
    import hello.core.AutoAppConfig;
    import hello.core.discount.DiscountPolicy;
    import hello.core.member.Grade;
    import hello.core.member.Member;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    import java.util.List;
    import java.util.Map;
    
    import static org.assertj.core.api.Assertions.assertThat;
    
    public class AllBeanTest {
    
        @Test
        void findAllBean() {
            ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class, DiscountService.class);
    
            DiscountService discountService = ac.getBean(DiscountService.class);
            Member member = new Member(1L, "userA", Grade.VIP);
            int fixDiscountPrice = discountService.discount(member, 10000, "fixDiscountPolicy");
    
            assertThat(discountService).isInstanceOf(DiscountService.class);
            assertThat(fixDiscountPrice).isEqualTo(1000);
    
            int rateDiscountPrice = discountService.discount(member, 20000, "rateDiscountPolicy");
            assertThat(rateDiscountPrice).isEqualTo(2000);
    
        }
    
        static class DiscountService {
            private final Map<String, DiscountPolicy> policyMap;
            private final List<DiscountPolicy> policies;
    
            @Autowired
            public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
                this.policyMap = policyMap;
                this.policies = policies;
                System.out.println("policyMap = " + policyMap);
                System.out.println("policies = " + policies);
            }
    
            public int discount(Member member, int price, String discountCode) {
                DiscountPolicy discountPolicy = policyMap.get(discountCode);
                return discountPolicy.discount(member, price);
            }
        }
    }
    ```
    

### 자동, 수동의 올바른 실무 운영 기준

- 스프링이 나오고 시간이 갈 수록 점점 자동을 선호하는 추세다. 자동 빈 등록을 사용해도 OCP, DIP를 지킬 수 있다.
- 그러면 수동 빈 등록은 언제 사용하면 좋을까?
    - 애플리케이션은 크게 업무 로직과 기술 지원 로직으로 나눌 수 있다.
        - **업무 로직 빈** : 웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 데이터 계층의 로직을 처리하는 리포지토리 등이 모두 업무 로직이다. 보통 비즈니스 요구사항을 개발할 때 추가되거나 변경된다.
        - **기술 지원 빈** : 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용된다. 데이터베이스 연결이나, 공통 로그 처리 처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술들이다.
    - 업무 로직은 가급적 자동 기능을 적극 사용하고, 기술 지원 로직은 수동 로직을 사용하는 것이 좋다.
    - 다형성을 적극 활용하는 비즈니스 로직은 수동 등록을 고민해보자.

# 8. 빈 생명주기 콜백

### 빈 생명주기 콜백 시작

- 데이터베이스 커넥션 풀이나 네트워크 소켓처럼 애플리케이션 시작 시점에 필요한 연결을 미리 해두고 애플리케이션 종료 시점에 연결을 모두 종료하는 작업을 진행하려면, 객체의 초기화와 종료 작업이 필요하다.

- 예제) 외부 네트워크에 미리 연결하는 객체를 하나 생성
    - 애플리케이션 시작 시점에 `connect()` 를 호출해서 연결을 맺어두어야 하고, 애플리케이션이 종료되면 `disConnect()` 를 호출해서 연결을 끊어야 한다.
        
        ```java
        package hello.core.lifecycle;
        
        public class NetworkClient {
        
            private String url;
        
            public NetworkClient() {
                System.out.println("생성자 호출, url = " + url);
                connect();
                call("초기화 연결 메시지");
            }
        
            public void setUrl(String url) {
                this.url = url;
            }
        
            // 서비스 시작 시 호출
            public void connect() {
                System.out.println("connect: " + url);
            }
        
            public void call(String message) {
                System.out.println("call: " + url + " / message = " + message);
            }
        
            // 서비스 종료 시 호출
            public void disconnect() {
                System.out.println("close: " + url);
            }
        
        }
        ```
        
        ```java
        package hello.core.lifecycle;
        
        import org.junit.jupiter.api.Test;
        import org.springframework.context.ConfigurableApplicationContext;
        import org.springframework.context.annotation.AnnotationConfigApplicationContext;
        import org.springframework.context.annotation.Bean;
        import org.springframework.context.annotation.Configuration;
        
        public class BeanLifeCycleTest {
        
            @Test
            public void lifeCycleTest() {
                ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
                NetworkClient client = ac.getBean(NetworkClient.class);
                ac.close();
            }
        
            @Configuration
            static class LifeCycleConfig {
                @Bean
                public NetworkClient networkClient() {
                    NetworkClient networkClient = new NetworkClient();
                    networkClient.setUrl("http://hello-spring.dev");
                    return networkClient;
                }
            }
        
        }
        ```
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/2f341130-fe21-41d9-80da-b505137c6443)

        
    - 객체를 생성하는 단계에는 url이 없고, 객체를 생성한 다음에 외부에서 수정자 주입을 통해서 `setUrl()` 이 호출되어야 url이 존재하게 된다.

- 스프링 빈의 라이프 사이클
    - **객체 생성 → 의존관계 주입**
    - 스프링 빈은 객체를 생성하고, 의존관계 주입이 다 끝난 다음에야 데이터를 사용할 수 있는 준비가 완료된다.
    - 따라서 초기화 작업은 의존관계 주입이 모두 완료되고 난 다음에 호출해야 한다.

- 스프링은 의존관계 주입이 완료되면 스프링 빈에게 콜백 메서드를 통해 초기화 시점을 알려주는 기능을 제공한다. 또한, 스프링 컨테이너가 종료되기 직전에 소멸 콜백을 준다. 따라서 안전하게 종료 작업을 진행할 수 있다.
    - 스프링 빈의 이벤트 라이프 사이클
        - **스프링 컨테이너 생성 → 스프링 빈 생성 → 의존관계 주입 → 초기화 콜백 → 사용 → 소멸 전 콜백 → 스프링 종료**
        - 초기화 콜백: 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출
        - 소멸 전 콜백: 빈이 소멸되기 직전에 호출

- 스프링의 빈 생명주기 콜백 지원 방법
    - 인터페이스 (InitializingBean, DisposableBean)
    - 설정 정보에 초기화 메서드, 종료 메서드 지정 **(외부 라이브러리에도 적용해야 할 때 사용)**
    - @PostConstruct, @PreDestroy 애노테이션 지원 **(권장)**

** 객체의 생성과 초기화를 분리하자.

### 인터페이스 InitializingBean, DisposableBean

- 초기화, 소멸 인터페이스
    
    ```java
    package hello.core.lifecycle;
    
    import org.springframework.beans.factory.DisposableBean;
    import org.springframework.beans.factory.InitializingBean;
    
    public class NetworkClient1 implements InitializingBean, DisposableBean {
    
        private String url;
    
        public NetworkClient1() {
            System.out.println("생성자 호출, url = " + url);
        }
    
        public void setUrl(String url) {
            this.url = url;
        }
    
        // 서비스 시작 시 호출
        public void connect() {
            System.out.println("connect: " + url);
        }
    
        public void call(String message) {
            System.out.println("call: " + url + " / message = " + message);
        }
    
        // 서비스 종료 시 호출
        public void disconnect() {
            System.out.println("close: " + url);
        }
    
        @Override
        public void afterPropertiesSet() throws Exception { // 초기화
            System.out.println("NetworkClient.afterPropertiesSet");
            connect();
            call("초기화 연결 메시지");
        }
    
        @Override
        public void destroy() throws Exception { // 소멸
            System.out.println("NetworkClient.destroy");
            disconnect();
        }
    
    }
    ```
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/8063c1e7-54d3-4552-a428-48661833061c)

    

- 단점
    - 이 인터페이스는 스프링 전용 인터페이스다. 해당 코드가 스프링 전용 인터페이스에 의존한다.
    - 초기화, 소멸 메서드의 이름을 변경할 수 없다.
    - 내가 코드를 고칠 수 없는 외부 라이브러리에 적용할 수 없다.

** 스프링 초창기에 나온 방법이고, 거의 사용하지 않는다.

### 빈 등록 초기화, 소멸 메서드 지정

- 설정 정보에 초기화 및 소멸 메서드 지정
    - `@Bean(initMethod = "init", destroyMethod = "close")`
    
    ```java
    package hello.core.lifecycle;
    
    public class NetworkClient2 {
    
        private String url;
    
        public NetworkClient2() {
            System.out.println("생성자 호출, url = " + url);
        }
    
        public void setUrl(String url) {
            this.url = url;
        }
    
        // 서비스 시작 시 호출
        public void connect() {
            System.out.println("connect: " + url);
        }
    
        public void call(String message) {
            System.out.println("call: " + url + " / message = " + message);
        }
    
        // 서비스 종료 시 호출
        public void disconnect() {
            System.out.println("close: " + url);
        }
    
        public void init() { // 초기화
            System.out.println("NetworkClient.afterPropertiesSet");
            connect();
            call("초기화 연결 메시지");
        }
    
        public void close() { // 소멸
            System.out.println("NetworkClient.close");
            disconnect();
        }
    
    }
    ```
    
    ```java
    package hello.core.lifecycle;
    
    import org.junit.jupiter.api.Test;
    import org.springframework.context.ConfigurableApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    public class BeanLifeCycleTest2 {
    
        @Test
        public void lifeCycleTest() {
            ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
            NetworkClient2 client = ac.getBean(NetworkClient2.class);
            ac.close();
        }
    
        @Configuration
        static class LifeCycleConfig {
            @Bean(initMethod = "init", destroyMethod = "close")
            public NetworkClient2 networkClient() {
                NetworkClient2 networkClient = new NetworkClient2();
                networkClient.setUrl("http://hello-spring.dev");
                return networkClient;
            }
        }
    
    }
    ```
    

- 장점
    - 메서드 이름을 자유롭게 줄 수 있다.
    - 스프링 빈이 스프링 코드에 의존하지 않는다.
    - 코드를 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 있다.

- 종료 메서드 추론
    - @Bean의 destroyMethod 는 기본값이 `(inferred)`으로 등록되어 있다.
    - 이 추론 기능은 `close`, `shutdown`라는 이름의 메서드를 자동으로 호출해준다.
    - 따라서 직접 스프링 빈으로 등록하면 종료 메서드는 따로 적어주지 않아도 잘 동작한다.
    - 추론 기능을 사용하기 싫으면 `destroyMethod=""` 처럼 빈 공백을 지정하면 된다.

### 애노테이션 @PostConstruct, @PreDestroy

- `@PostConstruct`, `@PreDestroy` 애노테이션 사용
    
    ```java
    package hello.core.lifecycle;
    
    import jakarta.annotation.PostConstruct;
    import jakarta.annotation.PreDestroy;
    
    public class NetworkClient3 {
    
        private String url;
    
        public NetworkClient3() {
            System.out.println("생성자 호출, url = " + url);
        }
    
        public void setUrl(String url) {
            this.url = url;
        }
    
        // 서비스 시작 시 호출
        public void connect() {
            System.out.println("connect: " + url);
        }
    
        public void call(String message) {
            System.out.println("call: " + url + " / message = " + message);
        }
    
        // 서비스 종료 시 호출
        public void disconnect() {
            System.out.println("close: " + url);
        }
    
        @PostConstruct
        public void init() { // 초기화
            System.out.println("NetworkClient.afterPropertiesSet");
            connect();
            call("초기화 연결 메시지");
        }
    
        @PreDestroy
        public void close() { // 소멸
            System.out.println("NetworkClient.close");
            disconnect();
        }
    
    }
    ```
    

- 특징
    - 최신 스프링에서 가장 권장하는 방법
    - 애노테이션 하나만 붙이면 되므로 매우 편리
    - 스프링이 아닌 다른 컨테이너에서도 동작 (스프링에 종속적인 기술이 아니라 자바 표준)
    - 컴포넌트 스캔과 잘 어울린다.
    - 유일한 단점은 외부 라이브러리에는 적용하지 못한다는 것이다.


# 9. 빈 스코프

### 빈 스코프란?

- 스프링 빈은 다양한 스코프를 지원한다.
    - 싱글톤: 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프 **(기본값)**
    - 프로토타입: 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프
    - 웹 관련 스코프
        - request: 웹 요청이 들어오고 나갈 때까지 유지되는 스코프
        - session: 웹 세션이 생성되고 종료될 때까지 유지되는 스코프
        - application: 웹의 서블릿 컨텍스와 같은 범위로 유지되는 스코프

### 프로토타입 스코프

- 싱글톤 스코프의 빈을 조회하면 스프링 컨테이너는 항상 같은 인스턴스의 스프링 빈을 반환한다.
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/08b9b043-6b4e-4b4a-9d92-dc2ed9aff406)

    
    - SingletonTest
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/f984b45a-6f38-4b3a-8bb5-580a609365e3)

        
        - 빈 초기화 메서드를 실행하고, (빈 생성 시점에 초기화 메서드 실행)
        - 같은 인스턴스의 빈을 조회하고,
        - 종료 메서드까지 정상 호출 된 것을 확인할 수 있다. (스프링 컨테이너가 종료될 때 빈의 종료 메서드 실행)
    

- 프로토타입 스코프를 스프링 컨테이너에 조회하면 스프링 컨테이너는 항상 새로운 인스턴스를 생성해서 반환한다.
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/52386b93-e003-4002-80c4-bd31f91c98ee)

    
    1. 프로토타입 스코프의 빈을 스프링 컨테이너에 요청한다.
    2. 스프링 컨테이너는 이 시점에 프로토타입 빈을 생성하고, 필요한 의존관계를 주입한다.
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/0ff80547-ae08-4747-aba2-b7c0d13e3029)

    
    3. 스프링 컨테이너는 생성한 프로토타입 빈을 클라이언트에 반환한다.
    4. 이후 스프링 컨테이너에 같은 요청이 오면 항상 새로운 프로토타입 빈을 생성해서 반환한다.
    - PrototypeTest
        
        ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/55c9df95-bd95-4c5c-a738-92adf4d8025a)

        
        - 프로토타입 스코프의 빈은 스프링 컨테이너에서 빈을 조회할 때 생성되고, 초기화 메서드도 실행된다.
        - 프로토타입 빈을 2번 조회했으므로 완전히 다른 스프링 빈이 생성되고, 초기화도 2번 실행된 것을 확인할 수 있다. (요청할 때마다 빈을 새로 생성)
        - 프로토타입 빈은 스프링 컨테이너가 생성과 의존관계 주입, 초기화까지만 관여하고 더는 관리하지 않는다. 따라서 스프링 컨테이너가 종료될 때 `@PreDestroy`와 같은 종료 메서드가 전혀 실행되지 않는다.
        - 프로토타입 빈을 관리할 책임은 프로토타입 빈을 받은 클라이언트에 있다.

<aside>
💡 **핵심은 스프링 컨테이너는 프로토타입 빈을 생성하고, 의존관계 주입, 초기화까지만 처리한다는 것이다.**

</aside>

### 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 문제점

- 스프링은 일반적으로 싱글톤 빈을 사용하므로 싱글톤 빈이 프로토타입 빈을 사용하게 된다. 그런데 싱글톤 빈은 생성 시점에만 의존관계 주입을 받기 때문에 프로토타입 빈이 새로 생성되기는 하지만, 싱글톤 빈과 함께 계속 유지되는 것이 문제다.

- 여러 빈에서 같은 프로토타입 빈을 주입 받으면, **주입 받는 시점에 각각 새로운 프로토타입 빈이 생성**된다. 예를 들어서 clientA, clientB가 각각 의존관계 주입을 받으면 각각 다른 인스턴스의 프로토타입 빈을 주입 받는
다. (물론 사용할 때 마다 새로 생성되는 것은 아니다.)
    - clientA → prototypeBean@x01
    - clientB → prototypeBean@x02

### 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 Provider로 문제 해결

- 스프링 컨테이너에 요청
    - 가장 간단한 방법 - 싱글톤 빈이 프로토타입을 사용할 때마다 스프링 컨테이너에 새로 요청하는 것이다.
    - 의존관계를 외부에서 주입받는게 아니라 직접 필요한 의존관계를 찾는 것 = 의존관계 조회(Dependency Lookup)
    - 그런데 이렇게 스프링의 애플리케이션 컨텍스트 전체를 주입받게 되면, 스프링 컨테이너에 종속적인 코드가 되고, 단위 테스트도 어려워진다.

- ObjectFactory, ObjectProvider
    - 지정한 빈을 컨테이너에서 대신 찾아주는 DL 서비스를 제공한다.
    - ObjectFactory + 편의 기능 → ObjectProvider
    - 코드
        
        ```java
        @Autowired
        private ObjectProvider<PrototypeBean> prototypeBeanProvider;
        public int logic() {
             PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
             prototypeBean.addCount();
             int count = prototypeBean.getCount();
             return count;
        }
        ```
        
        - 실행해보면 `prototypeBeanProvider.getObject()` 을 통해서 항상 새로운 프로토타입 빈이 생성되는 것을 확인할 수 있다.
        - `ObjectProvider`의 `getObject()`를 호출하면 내부에서는 스프링 컨테이너를 통해 해당 빈을 찾아서 반환한다. (**DL**)
        - 스프링이 제공하는 기능을 사용하지만, 기능이 단순하므로 단위테스트를 만들거나 mock 코드를 만들기는 훨씬 쉬워진다.
    

** ObjectFactory: 기능이 단순, 별도의 라이브러리 필요 없음, 스프링에 의존

** ObjectProvider: ObjectFactory 상속, 옵션, 스트림 처리등 편의 기능이 많고, 별도의 라이브러리 필요 없음, 스프링에 의존

- `javax.inject.Provider` 라는 JSR-330 자바 표준을 사용하는 방법
    - `jakarta.inject:jakarta.inject-api:2.0.1` 라이브러리를 gradle에 추가해야 한다.
    - 코드
        
        ```java
        @Autowired
        private Provider<PrototypeBean> provider;
        public int logic() {
             PrototypeBean prototypeBean = provider.get();
             prototypeBean.addCount();
             int count = prototypeBean.getCount();
             return count;
        }
        ```
        

<aside>
💡 프로토타입 빈은 매번 사용할 때마다 의존관계 주입이 완료된 새로운 객체가 필요할 때 사용하면 된다.

</aside>

### 웹 스코프

- 특징
    - 웹 환경에서만 동작
    - 스프링이 해당 스코프의 종료 시점까지 관리. 따라서 종료 메서드가 호출된다.

- 종류
    - request: HTTP 요청 하나가 들어오고 나갈 때 까지 유지되는 스코프, 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고, 관리된다.
    - session: HTTP Session과 동일한 생명주기를 가지는 스코프
    - application: `ServletContext`와 동일한 생명주기를 가지는 스코프
    - websocket: 웹 소켓과 동일한 생명주기를 가지는 스코프

- request 스코프
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/9000ca18-c6bc-40a4-be53-e5fae17ea18c)

    
    - HTTP request 요청 당 각각 할당된다.
    - 예제
        - build.gradle에 추가
            
            ```java
            //web 라이브러리 추가
            implementation 'org.springframework.boot:spring-boot-starter-web'
            ```
            
            ** `spring-boot-starter-web` 라이브러리를 추가하면 스프링 부트는 내장 톰켓 서버를 활용해서 웹 서버와 스프링을 함께 실행시킨다.
            
            ** 웹 라이브러리가 추가되면 웹과 관련된 추가 설정과 환경들이 필요하므로 `AnnotationConfigServletWebServerApplicationContext` 를 기반으로 애플리케이션을 구동한다.
            
        - MyLogger
            - `@Scope(value = "request")` 를 사용해서 request 스코프로 지정했다. 이 빈은 HTTP 요청 당 하나씩 생성되고, HTTP 요청이 끝나는 시점에 소멸된다.
            - 이 빈이 생성되는 시점에 자동으로 `@PostConstruct` 초기화 메서드를 사용해서 uuid를 생성해서 저장해둔다.
            - 이 빈은 HTTP 요청 당 하나씩 생성되므로, uuid를 저장해두면 다른 HTTP 요청과 구분할 수 있다.
            - 이 빈이 소멸되는 시점에 `@PreDestroy` 를 사용해서 종료 메시지를 남긴다.
            - `requestURL` 은 이 빈이 생성되는 시점에는 알 수 없으므로, 외부에서 setter로 입력 받는다.
        - LogDemoController
            - HttpServletRequest를 통해서 요청 URL을 받았다.
            - 이렇게 받은 requestURL 값을 myLogger에 저장해둔다. myLogger는 HTTP 요청 당 각각 구분되므로 다른 HTTP 요청 때문에 값이 섞이는 걱정은 하지 않아도 된다.
            - 컨트롤러에서 controller test라는 로그를 남긴다.
        - LogDemoService
            - 비즈니스 로직이 있는 서비스 계층에서도 로그를 출력해보자.
            - 여기서 중요한점이 있다. request scope를 사용하지 않고 파라미터로 이 모든 정보를 서비스 계층에 넘긴다면, 파라미터가 많아서 지저분해진다. 더 문제는 requestURL 같은 웹과 관련된 정보가 웹과 관련없는 서비스 계층까지 넘어가게 된다. 웹과 관련된 부분은 컨트롤러까지만 사용해야 한다. 서비스 계층은 웹 기술에 종속되지 않고, 가급적 순수하게 유지하는 것이 유지보수 관점에서 좋다.
            - request scope의 MyLogger 덕분에 이런 부분을 파라미터로 넘기지 않고, MyLogger의 멤버변수에 저장해서 코드와 계층을 깔끔하게 유지할 수 있다.
        - 실행 → 오류
            - 스프링 애플리케이션을 실행하는 시점에 싱글톤 빈은 생성해서 주입이 가능하지만, request 스코프 빈은 아직 생성되지 않는다. 이 빈은 실제 고객의 요청이 와야 생성할 수 있다!
            - Provider를 이용하여 해결해보자.

### 스코프와 Provider

- `ObjectProvider` 덕분에 `ObjectProvider.getObject()`를 호출하는 시점까지 request scope **빈의
생성을 지연**할 수 있다.
- `ObjectProvider.getObject()`를 호출하시는 시점에는 HTTP 요청이 진행중이므로 request scope 빈
의 생성이 정상 처리된다.
- `ObjectProvider.getObject()`를 LogDemoController, LogDemoService에서 각각 한번씩 따로
호출해도 같은 HTTP 요청이면 같은 스프링 빈이 반환된다!

### 스코프와 프록시

- 프록시 방식
    
    ```java
    @Component
    @Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class MyLogger {
    }
    ```
    
    - `proxyMode = ScopedProxyMode.TARGET_CLASS`
        - 적용 대상이 인터페이스가 아닌 클래스면 `TARGET_CLASS`를 선택
        - 적용 대상이 인터페이스면 `INTERFACES`를 선택
    - 이렇게 하면 MyLogger의 가짜 프록시 클래스를 만들어두고 HTTP request와 상관 없이 가짜 프록시 클래스를 다른 빈에 미리 주입해 둘 수 있다.
    
    ![image](https://github.com/Springdingdongrami/spring-core-principles-basic/assets/66028419/d65c2057-4a86-4719-986b-08cee80de3b6)

    
    - 가짜 프록시 객체는 요청이 오면 그때 내부에서 진짜 빈을 요청하는 위임 로직이 들어있다.
        - 가짜 프록시 객체는 내부에 진짜 myLogger를 찾는 방법을 알고 있다.
        - 클라이언트가 myLogger.log()을 호출하면 사실은 가짜 프록시 객체의 메서드를 호출한 것이다.
        - 가짜 프록시 객체는 request 스코프의 진짜 myLogger.log()를 호출한다.
        - 가짜 프록시 객체는 원본 클래스를 상속 받아서 만들어졌기 때문에 이 객체를 사용하는 클라이언트 입장에서는 사실 원본인지 아닌지도 모르게, 동일하게 사용할 수 있다. (다형성)
    - 동작 원리
        - CGLIB라는 라이브러리로 내 클래스를 상속 받은 가짜 프록시 객체를 만들어서 주입한다.
        - 이 가짜 프록시 객체는 실제 요청이 오면 그때 내부에서 실제 빈을 요청하는 위임 로직이 들어있다.
        - 가짜 프록시 객체는 실제 request scope와는 관계가 없다. 그냥 가짜이고, 내부에 단순한 위임 로직만 있고, 싱글톤 처럼 동작한다.
    - 특징
        - 프록시 객체 덕분에 클라이언트는 마치 싱글톤 빈을 사용하듯이 편리하게 request scope를 사용할 수 있다.
        - **Provider를 사용하든, 프록시를 사용하든 핵심 아이디어는 진짜 객체 조회를 꼭 필요한 시점까지 지연처리 한다는 점이다.**
        - 단지 애노테이션 설정 변경만으로 원본 객체를 프록시 객체로 대체할 수 있다. 이것이 바로 다형성과 DI 컨테이너가 가진 큰 강점이다.
        - 꼭 웹 스코프가 아니어도 프록시는 사용할 수 있다.
    - 주의점
        - 마치 싱글톤을 사용하는 것 같지만 다르게 동작하기 때문에 결국 주의해서 사용해야 한다.
        - 이런 특별한 scope는 꼭 필요한 곳에만 최소화해서 사용하자, 무분별하게 사용하면 유지보수하기 어려워진다.
