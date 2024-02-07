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

github copilot 은 사용 경험이 충분히 좋지 않았다..
로컬 캐싱을 너무 강하게 하는건지 기존에 작성했던 주석이나 샘플 코드들이 지속적으로 추천됐다.

codeium 은 무료 서비스만으로도 결과는 충분히 만족스럽게 나타났다.

다만 가끔 결과를 보여주는 ui 가 갑자기 사라져 보이지 않는 증상이 있었다.

생산성을 올리기 위한 기능인데 갑자기 이러니 오히려 생산성이 떨어지는 느낌을..

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

