---
title: "Copilot 평가 Level1 - 도메인객체"
date: 2023-01-18 18:10:00 -0400
categories: copilot
---

## Level1 - 도메인객체

제가 개발하고 유지보수 하는 제품은 DDD 개발 방법론으로 개발을 하고 있고 기본이 되는 도메인 로직에 대해 테스트 케이스 생성을 먼저 테스트합니다.

Spring 기반의 DI나 어노테이션이 없어 상대적으로 잘 작성해줄 것으로 예상해 level1 으로 선정했습니다.

수행전 궁금한 결과는 lombok 으로 생성되는 함수들에 대해서도 잘 대응 해주는지와 오브젝트에 대해 어떤방식으로 mocking 을 해 주는가 입니다.

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


