---
title: "Copilot 평가 Level3 - Controller"
date: 2023-01-18 18:10:00 -0400
categories: copilot
---

## Level3 - remove 케이스 커버 테스트


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

