---
title: "Copilot 편의 기능과 선택"
date: 2023-01-18 18:10:00 -0400
categories: copilot
---

## Copilot 선택

추가적으로 제공하는 기능들... 

문제점 추론

리팩토링

docs 생성..


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

