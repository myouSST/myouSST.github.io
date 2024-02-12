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

## 대상 코드
```java
@AllArgsConstructor
@NoArgsConstructor(access = AccessLevel.PRIVATE)
@Getter
@ToString
public class User {

    private String id;

    private String name;

    private String profileUrl;

    private List<Skill> skills;

    private IdList teamIds;

    public static User sample() {
        return new User(
            "myou",
            "유민",
            "http://test-profile.co.kr",
            new ArrayList<>(Arrays.asList(Skill.sample(), Skill.sample2())),
            IdList.of("c-team")
        );
    }

    public static User sample2() {
        return new User(
            "hong", // 사용자 아이디
            "홍길동", // 사용자 이름
            "http://test-profile.co.kr/hong", // 프로필 URL
            new ArrayList<>(Arrays.asList(Skill.sample(), Skill.sample2())), // 사용자 스킬
            IdList.of("t1", "t2", "t3") // 사용자가 속한 팀의 ID 리스트
        );
    }

    public static User sample(String userId) {
        return new User(
            userId,
            userId,
            "http://test-profile.co.kr",
            new ArrayList<>(Arrays.asList(Skill.sample(), Skill.sample2())),
            IdList.of("c-team")
        );
    }

    public List<String> getAllSkillNames() {
        return skills.stream().map(Skill::getName).toList();
    }

    public boolean canSales() {
        return skills.stream().anyMatch(skill -> skill.getType() == SkillType.SALES);
    }

    public void addSkill(Skill skill) {
        skills.add(skill);
    }

    public void removeSkill(Skill role) {
        skills.remove(role);
    }
}
```

---
## gpt - 50% 메서드,  52% 라인
``` java
class UserTest {

    @Test
    void getAllSkillNames() {
        User user = User.sample();
        assertEquals(List.of("Java", "Python"), user.getAllSkillNames());
    }

    @Test
    void canSalesWithSalesSkill() {
        User user = User.sample();
        assertTrue(user.canSales());
    }

    @Test
    void canSalesWithoutSalesSkill() {
        User user = User.sample2();
        assertFalse(user.canSales());
    }

    @Test
    void addSkill() {
        User user = User.sample();
        assertEquals(2, user.getSkills().size());

        Skill newSkill = new Skill("New Skill", SkillType.WEB_DEVELOP);
        user.addSkill(newSkill);

        assertEquals(3, user.getSkills().size());
        assertTrue(user.getSkills().contains(newSkill));
    }

    @Test
    void removeSkill() {
        User user = User.sample2();
        assertEquals(2, user.getSkills().size());

        Skill skillToRemove = user.getSkills().get(0);
        user.removeSkill(skillToRemove);

        assertEquals(1, user.getSkills().size());
        assertFalse(user.getSkills().contains(skillToRemove));
    }

    // Additional tests can be added as needed

}
```
---
## jetbrain ai assistant - 81% 메서드, 78% 라인
```java
public class UserTest {
    @Test
    public void testUserSample() {
        User user = User.sample();
        Assertions.assertEquals("myou", user.getId());
        Assertions.assertEquals("유민", user.getName());
        Assertions.assertEquals("http://test-profile.co.kr", user.getProfileUrl());
        Assertions.assertTrue(user.getTeamIds().contains("c-team"));
    }
    @Test
    public void testUserSample2() {
        User user = User.sample2();
        Assertions.assertEquals("hong", user.getId());
        Assertions.assertEquals("홍길동", user.getName());
        Assertions.assertEquals("http://test-profile.co.kr/hong", user.getProfileUrl());
        Assertions.assertTrue(user.getTeamIds().contains("t1"));
    }
    @Test
    public void testUserSampleWithUserId() {
        String userId = "jay_TestUserId";
        User user = User.sample(userId);
        Assertions.assertEquals(userId, user.getId());
    }
    @Test
    public void testGetAllSkillNames() {
        User user = User.sample();
        Assertions.assertEquals(Arrays.asList("SampleSkill", "SampleSkill2"), user.getAllSkillNames());
    }
    @Test
    public void testCanSales() {
        User user = User.sample();
        Assertions.assertFalse(user.canSales());
    }
    @Test
    public void testAddSkill() {
        User user = User.sample();
        Skill newSkill = new Skill("Coding", SkillType.WEB_DEVELOP);
        user.addSkill(newSkill);
        Assertions.assertTrue(user.getAllSkillNames().contains(newSkill.getName()));
    }
    @Test
    public void testRemoveSkill() {
        User user = User.sample();
        Skill existingSkill = user.getSkills().get(0);
        user.removeSkill(existingSkill);
        Assertions.assertFalse(user.getAllSkillNames().contains(existingSkill.getName()));
    }
}
```
---
## github copilot - 68% 메서드, 68% 라인
```java
class UserTest {
    
    //generate tests for the methods in the User.class
    @Test
    void sample() {
        User user = User.sample();
        assertEquals("myou", user.getId());
        assertEquals("유민", user.getName());
        assertEquals("http://test-profile.co.kr", user.getProfileUrl());
    }

    @Test
    void sample2() {
        User user = User.sample2();
        assertEquals("hong", user.getId());
        assertEquals("홍길동", user.getName());
        assertEquals("http://test-profile.co.kr/hong", user.getProfileUrl());
    }

    @Test
    void sample3() {
        User user = User.sample("test");
        assertEquals("test", user.getId());
        assertEquals("test", user.getName());
        assertEquals("http://test-profile.co.kr", user.getProfileUrl());
    }

    @Test
    void getAllSkillNames() {
        User user = User.sample();
        assertEquals(2, user.getAllSkillNames().size());
    }

    @Test
    void canSales() {
        User user = User.sample();
        assertTrue(user.canSales());
    }

    @Test
    void addSkill() {
        User user = User.sample();
        user.addSkill(Skill.sample());
        assertEquals(3, user.getSkills().size());
    }

    @Test
    void removeSkill() {
        User user = User.sample();
        user.removeSkill(Skill.sample());
        assertEquals(1, user.getSkills().size());
    }
}
```
---
## codeium - 81% 메서드, 78% 라인
```java
class UserTest {

    @Test
    public void testSampleUserCreation() {
        User user = User.sample();
        assertNotNull(user);
        assertEquals("myou", user.getId());
        assertEquals("유민", user.getName());
        assertEquals("http://test-profile.co.kr", user.getProfileUrl());
        assertEquals(2, user.getSkills().size());
        assertEquals("REACT", user.getSkills().get(0).getName()); // Replace "Skill1" with the expected name of the first skill
        assertEquals("ANDROID", user.getSkills().get(1).getName()); // Replace "Skill2" with the expected name of the second skill
        assertEquals(1, user.getTeamIds().size());
        assertEquals("c-team", user.getTeamIds().get(0));
    }

    @Test
    public void testSample2UserCreation() {
        User user = User.sample2();
        assertNotNull(user);
        assertEquals("hong", user.getId());
        assertEquals("홍길동", user.getName());
        assertEquals("http://test-profile.co.kr/hong", user.getProfileUrl());
        assertEquals(2, user.getSkills().size());
        assertEquals("REACT", user.getSkills().get(0).getName()); // Replace "Skill1" with the expected name of the first skill
        assertEquals("ANDROID", user.getSkills().get(1).getName()); // Replace "Skill2" with the expected name of the second skill
        assertEquals(3, user.getTeamIds().size());
        assertEquals("t1", user.getTeamIds().get(0));
        assertEquals("t2", user.getTeamIds().get(1));
        assertEquals("t3", user.getTeamIds().get(2));
    }

    @Test
    public void testSampleWithUserIdUserCreation() {
        User user = User.sample("testId");
        assertNotNull(user);
        assertEquals("testId", user.getId());
        assertEquals("testId", user.getName());
        assertEquals("http://test-profile.co.kr", user.getProfileUrl());
        assertEquals(2, user.getSkills().size());
        assertEquals("REACT", user.getSkills().get(0).getName()); // Replace "Skill1" with the expected name of the first skill
        assertEquals("ANDROID", user.getSkills().get(1).getName()); // Replace "Skill2" with the expected name of the second skill
        assertEquals(1, user.getTeamIds().size());
        assertEquals("c-team", user.getTeamIds().get(0));
    }

    @Test
    public void testGetAllSkillNames() {
        User user = new User("testId", "Test User", "http://test-profile.co.kr", List.of(Skill.sample(), Skill.sample2()), IdList.of("c-team"));
        List<String> skillNames = user.getAllSkillNames();
        assertEquals(2, skillNames.size());
        assertEquals("REACT", skillNames.get(0)); // Replace "Skill1" with the expected name of the first skill
        assertEquals("ANDROID", skillNames.get(1)); // Replace "Skill2" with the expected name of the second skill
    }

    @Test
    public void testCanSales() {
        User user = new User(
            "testId",
            "Test User",
            "http://test-profile.co.kr",
            List.of(new Skill("Skill1", SkillType.WEB_DEVELOP), new Skill("Skill2", SkillType.SALES)),
            IdList.of("c-team")
        );
        assertTrue(user.canSales());
    }

    @Test
    public void testAddSkill() {
        User user = User.sample();
        int initialSize = user.getSkills().size();
        user.addSkill(new Skill("NewSkill", SkillType.WEB_DEVELOP));
        assertEquals(initialSize + 1, user.getSkills().size());
        assertEquals("NewSkill", user.getSkills().get(initialSize).getName());
    }

    @Test
    public void testRemoveSkill() {
        User user = User.sample();
        Skill skillToRemove = user.getSkills().get(0);
        int initialSize = user.getSkills().size();
        user.removeSkill(skillToRemove);
        assertEquals(initialSize - 1, user.getSkills().size());
        assertFalse(user.getSkills().contains(skillToRemove));
    }
}
```
---
## myou - 81% 메서드, 78% 라인
```java
class UserTest {

    @Test
    void sample() {
        User user = User.sample();
        assertEquals("myou", user.getId());
        assertEquals("유민", user.getName());
        assertEquals("http://test-profile.co.kr", user.getProfileUrl());
        assertEquals(IdList.of("c-team"), user.getTeamIds());
    }

    @Test
    void sample2() {
        User user = User.sample2();
        assertEquals("hong", user.getId());
    }

    @Test
    void testSample() {
        User user = User.sample("test");
        assertEquals("test", user.getId());
    }

    @Test
    void getAllSkillNames() {
        User user = User.sample();
        assertEquals(List.of("REACT", "ANDROID"), user.getAllSkillNames());
    }

    @Test
    void canSales() {
        User user = new User("myou", "유민", "http://test-profile.co.kr", new ArrayList<>(List.of(new Skill("sales", SkillType.SALES))), IdList.empty());
        assertTrue(user.canSales());
    }

    @Test
    void addSkill() {
        User user = User.sample();
        Skill newSkill = new Skill("test", SkillType.TEST);
        user.addSkill(newSkill);
        assertTrue(user.getSkills().contains(newSkill));
    }

    @Test
    void removeSkill() {
        User user = User.sample();
        Skill removeSkill = new Skill("REACT", SkillType.WEB_DEVELOP);
        assertTrue(user.getSkills().contains(removeSkill));

        user.removeSkill(removeSkill);
        assertFalse(user.getSkills().contains(removeSkill));
    }
}
```