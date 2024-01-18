---
title: "Copilot 평가 Level3 - Controller"
date: 2023-01-18 18:10:00 -0400
categories: copilot
---

## Level3 - Controller (restdoc)

제가 관리하고 있는 제품은 REST API 를 지향하며 문서화는 restdocs 를 통해 진행중입니다.

restdocs 는 테스트 코드처럼 관리가 가능하며 여기서 Controller 에서 제공하는 API spec 에 대한 명세를 합니다.

기대는 API 에 노출되는 파라미터명을 보고 추론을 통해 restdocs 를 유의미하게 작성을 해주는가 입니다.


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

