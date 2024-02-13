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
@ToString
public class User {

    private String id;

    private String name;

    private String profileUrl;

    private Role role;

    private List<Skill> skills;

    private IdList teamIds;

    public static User sample() {
        return new User("myou", "유민", "http://test-profile.co.kr", Role.USER, new ArrayList<>(Arrays.asList(Skill.sample(), Skill.sample2())), IdList.of("c-team"));
    }
    
    ...
    
    public void removeSkill(Skill skill, User requester) {
        if (skill.isEssential()) {
            throw new IllegalArgumentException("필수 스킬은 삭제할 수 없습니다.");
        }
    
        if (!requester.isAdministrator()) {
            throw new IllegalArgumentException("관리자만 스킬을 삭제할 수 있습니다.");
        }
    
        skills.remove(skill);
    }
}
```
---
## gpt

```java

```
---
## github copilot

```java
class UserTest {
    ... 
    
    @Test
    void shouldNotRemoveEssentialSkill() {
        User admin = User.sample2();
        User user = User.sample();
        Skill essentialSkill = new Skill("essential", SkillType.COMMUNICATION);
        user.addSkill(essentialSkill);
        assertThrows(IllegalArgumentException.class, () -> user.removeSkill(essentialSkill, admin));
    }

    @Test
    void shouldNotAllowNonAdminToRemoveSkill() {
        User nonAdmin = User.sample();
        User user = User.sample2();
        Skill skill = new Skill("skill", SkillType.SALES);
        user.addSkill(skill);
        assertThrows(IllegalArgumentException.class, () -> user.removeSkill(skill, nonAdmin));
    }

    @Test
    void shouldRemoveSkillByAdmin() {
        User admin = User.sample2();
        User user = User.sample();
        Skill skill = new Skill("skill", SkillType.SALES);
        user.addSkill(skill);
        user.removeSkill(skill, admin);
        assertFalse(user.getSkills().contains(skill));
    }
}
```