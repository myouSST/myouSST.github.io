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

대상 코드
``` java
@Service
@RequiredArgsConstructor
public class UserQueryService {
    private final UserService userService;

    private final TeamService teamService;

    public UserRdo find(String userId) {
        User user = userService.find(userId);
        List<Team> teams = user.getTeamIds().stream()
            .map(this::getTeam)
            .toList();

        return new UserRdo(user, teams);
    }

    public List<UserRdo> findAll() {
        List<User> users = userService.findAll();
        Map<String, Team> teamMap = ListUtil.toMap(teamService.findAll(), Team::getId);

        return users.stream()
            .map(user -> new UserRdo(user, getTeams(user, teamMap)))
            .toList();
    }

    private List<Team> getTeams(User user, Map<String, Team> teamMap) {
        return user.getTeamIds().stream()
            .map(teamMap::get)
            .toList();
    }

    private Team getTeam(String teamId) {
        return teamService.find(teamId);
    }
}

```
---
gpt - 100% 메서드,  100% 라인, 2/1 성공
``` java
@ExtendWith(MockitoExtension.class)
class UserQueryServiceTest {

    @Mock
    private UserService userService;

    @Mock
    private TeamService teamService;

    @InjectMocks
    private UserQueryService userQueryService;

    @Test
    void find() {
        // 가짜 데이터 및 가짜 동작 정의
        String userId = "testUserId";
        User user = User.sample();
        when(userService.find(userId)).thenReturn(user);
        when(teamService.find(anyString())).thenReturn(Team.sample());

        // 테스트
        UserRdo userRdo = userQueryService.find(userId);

        // 검증
        assertEquals(user.getId(), userRdo.getId());
        verify(userService, times(1)).find(userId);
        verify(teamService, times(1)).find(anyString());
    }

    @Test
    void findAll() {
        // 가짜 데이터 및 가짜 동작 정의
        List<User> users = Arrays.asList(User.sample(), User.sample2());
        List<Team> teams = Arrays.asList(Team.sample(), Team.sample2());
        when(userService.findAll()).thenReturn(users);
        when(teamService.findAll()).thenReturn(teams);

        // 테스트
        List<UserRdo> userRdos = userQueryService.findAll();

        // 검증
        assertEquals(users.size(), userRdos.size());
        verify(userService, times(1)).findAll();
        verify(teamService, times(1)).findAll();
    }

    // 추가적인 테스트 케이스를 필요에 따라 작성할 수 있습니다.

}
```
---
jetbrain ai assistant - 100% 메서드, 100% 라인, 3/3 성공
```java
@ExtendWith(MockitoExtension.class)
class UserQueryServiceTest {
    @Mock
    UserService userService;

    @Mock
    TeamService teamService;

    @InjectMocks
    UserQueryService userQueryService;

    @Test
    void shouldReturnUserRdoForGivenUserId() {
        // Given
        Team team = Team.sample("c-team");
        Team team2 = Team.sample("b-team");
        User user = User.sample("userID");
        user.getTeamIds().add("b-team");
        when(userService.find("userID")).thenReturn(user);
        when(teamService.find("c-team")).thenReturn(team);
        when(teamService.find("b-team")).thenReturn(team2);

        // When
        UserRdo resultUserRdo = userQueryService.find("userID");

        // Then
        assertThat(resultUserRdo.getId()).isEqualTo(user.getId());
        assertThat(resultUserRdo.getTeams()).containsExactly(new TeamRdo(team), new TeamRdo(team2));
    }

    @Test
    void testFindAll() {
        // Setting up mock values
        User user1 = User.sample("user1");
        User user2 = User.sample("user2");

        Team team1 = Team.sample("c-team");
        Team team2 = Team.sample2();

        given(userService.findAll())
            .willReturn(Arrays.asList(user1, user2));
        given(teamService.findAll())
            .willReturn(Arrays.asList(team1, team2));

        // Creating the instance to be tested
        UserQueryService userQueryService = new UserQueryService(userService, teamService);

        // Act
        List<UserRdo> result = userQueryService.findAll();

        // Assert
        assertThat(result).isNotNull();
        assertThat(result.size()).isEqualTo(2);
        assertThat(result.get(0)).extracting(UserRdo::getId, UserRdo::getName)
            .containsExactly(user1.getId(), user1.getName());
        assertThat(result.get(1)).extracting(UserRdo::getId, UserRdo::getName)
            .containsExactly(user2.getId(), user2.getName());
    }
}

```
---
git hub copilot intelliJ 에서 테스트 케이스 생성 포기.. intelliJ 와의 호환성이 너무 낮다..


