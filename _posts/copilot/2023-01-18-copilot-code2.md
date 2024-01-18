---
title: "Copilot 평가 Level2 - Service"
date: 2023-01-18 18:10:00 -0400
categories: copilot
---

## Level2 - Service

제가 관리하고 있는 제품은 단순한 curd 를 담당하는 service 와 좀더 복잡한 업무를 다루는  flow Service 가 있습니다.

제품 복잡도와 의존성 관리를 위해 나눠놓은 단위이고 일반적으로 flow service 는 다수의 service 를 이용해 다양한 서비스를 제공합니다.

flow service 는 service 에 대한 mocking 이 얼마나 잘되며 로직 분기나 예외 처리에 대한 커버리지가 얼마나 되는지를 중심으로 평가할 예정입니다.

수행전 궁금한 결과는 spring 의 DI 통해 생성된 서비스들이 mocking 이 잘될지

분기와 예외 처리에 대한 커버리지를 높게 생성해 주는지입니다.


---

테스트를 위한 도메인 생성
``` java
@AllArgsConstructor
@NoArgsConstructor(access = AccessLevel.PRIVATE)
@Getter
@EqualsAndHashCode
@ToString
public class User {

    private String id;

    private String name;

    private String profileUrl;

    private Organization organization;

    public static User sample() {
        return new User("myou", "유민", "http://test-profile.co.kr", Organization.sample());
    }

    public static void main(String[] args) {
        System.out.println(sample());
    }
}
```
