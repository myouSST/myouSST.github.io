---
title: "Copilot 편의 기능과 선택"
date: 2023-01-18 18:10:00 -0400
categories: copilot
---

## Copilot 선택

1. [ ] 추가적으로 제공하는 기능들
2. [ ] 문제점 추론
3. [ ] 리팩토링
4. [ ] docs 생성

---
# github copilot

테스트 케이스 생성을 기준으로는 사용 경험이 충분히 좋지 않았다.  
기본 적으론 코드를 명시적으로 생산해내는 능력은 떨어지거나 제공하지 않는다.  
로컬 캐싱을 너무 강하게 하는건지 기존에 작성했던 주석이나 샘플 코드들이 지속적으로 추천됐다.  
md 을 작성중에도 끊임없이 추천을한다.  
![자동 완성이 끊임없이 추천되는 모습](https://github.com/myouSST/myouSST.github.io/assets/images/copilot/github/github1.png)  
![자동 완성이 끊임없이 추천되는 모습](https://github.com/myouSST/myouSST.github.io/assets/images/copilot/github/github2.png)  
![자동 완성이 끊임없이 추천되는 모습](https://github.com/myouSST/myouSST.github.io/assets/images/copilot/github/github3.png)  

### 다만 chat 기능은 beta 버전으로 waitlist 대기 후 사용 가능하다.  
약 2주 정도 기다렸으며, 개인별로 편차가 좀 있는 듯하다.
![제약사항!](https://github.com/myouSST/myouSST.github.io/assets/images/copilot/github/github-impo.png)

chat 기능 활성화 이후엔 github copilot 압도적으로 사용감이 좋았다.
커서를 올려놓고 무엇을 하고 싶은지 물어보면 그에 맞는 코드를 제안해준다.
![챗 사용](https://github.com/myouSST/myouSST.github.io/assets/images/copilot/github/github-chat.png)


### 평가
	생성 속도 - 👍👍👍
	자동완성 도움 - 👍👍👍
	prompt - N/A
	custom prompt - N/A
	chat 기능 - 👍👍👍 (waitlist)

---

# codeium  
무료 서비스만으로도 결과는 충분히 만족스럽게 나타났다.   
다만 가끔 결과를 보여주는 ui 가 갑자기 사라져 보이지 않는 먹통이되는 증상이 있었다.  
intelliJ 프로젝트가 많이 떠있는 상황에서 발생시 상당히 불편한 경험이었다.
![codeium_disabled.png](../../assets/images/copilot/codeium_disabled.png)
### 평가
	생성 속도 - 👍👍
	자동완성 도움 - 👍
	custom prompt - N/A
	prompt - 👍
	chat 기능 - 👍👍👍
    버그 - 👎👎

---


# jetbrain ai assistant
기본적으로 제공해주는 기능들이 만족도가 괜찮았다.  
놓쳤던 처리 부분이나 npe 가능성이 있는 로직에 대해 충분히 안내해준다.  
예를 들면 map 객체에서 get 으로 가져오는데 빈값일 경우 NPE 발생을 막기위해 아래 처럼 필터 코드를 제안한다.  
Chat 기능이 있지만 사용하지 않아도 될정도로 기본적인 기능들을 잘 제공했다.
```java
    private List<Team> getTeams(User user, Map<String, Team> teamMap) {
        return user.getTeamIds().stream()
            .map(teamMap::get)
            .filter(Objects::nonNull) // 추천 코드
            .toList();
    }
```
### 평가
	생성 속도 - 👍
	자동완성 도움 - 👍
	prompt - 👍👍👍
	custom prompt - 👍👍👍
	chat 기능 - 👍👍

---

# GPT
모두가 아는 그 맛
### 평가
	생성 속도 - 👍👍️
	자동완성 도움 - N/A
	prompt - N/A
	custom prompt - N/A
	chat 기능 - 👍👍👍



---
# 총평
