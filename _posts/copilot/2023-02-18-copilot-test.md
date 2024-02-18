

```java
@ExtendWith(MockitoExtension.class)
class GroupTalkServiceImplTest {

    @Mock
    private TicketAdapter ticketAdapter;

    @Mock
    private TicketQueryAdapter ticketQueryAdapter;

    @Mock
    private TicketCustomMetadataAdapter ticketCustomMetadataAdapter;

    @Mock
    private GroupTalkMemberService groupTalkMemberService;

    @Mock
    private GroupTalkPartnerService groupTalkPartnerService;

    @Mock
    private TalkQueryAuthorizedFilterService talkQueryAuthorizedFilterService;

    @Mock
    private TalkCustomerQueryAuthorizedFilterService talkCustomerQueryAuthorizedFilterService;

    @InjectMocks
    private GroupTalkServiceImpl groupTalkService;

    @Test
    void shouldReturnGroupTalkListItemListWhenExists() {
        GroupTalkQuery query = GroupTalkQuery.builder().build();
        GroupTalkListItemList expectedList = GroupTalkListItemList.sample();
        when(ticketQueryAdapter.findAll(any(), any(), any(), any())).thenReturn(TicketRdoListRdo.sample());

        GroupTalkListItemList actualList = groupTalkService.findAll(query);

        assertEquals(expectedList, actualList);
    }

    @Test
    void shouldReturnGroupTalkWhenExists() {
        TicketKey ticketKey = TicketKey.sample();
        when(ticketQueryAdapter.find(any())).thenReturn(TicketRdo.sample());

        GroupTalk actualGroupTalk = groupTalkService.find(ticketKey);

        assertNotNull(actualGroupTalk);
    }

    @Test
    void shouldReturnGroupTalkForUpdateWhenExists() {
        TicketKey ticketKey = TicketKey.sample();
        when(ticketQueryAdapter.findForUpdate(any())).thenReturn(TicketRdo.sample());

        GroupTalk actualGroupTalk = groupTalkService.findForUpdate(ticketKey);

        assertNotNull(actualGroupTalk);
    }

    @Test
    void shouldRegisterGroupTalkSuccessfully() {
        TicketKey ticketKey = TicketKey.sample();
        GroupTalkCdo groupTalkCdo = GroupTalkCdo.sample();

        groupTalkService.register(ticketKey, groupTalkCdo);

        verify(ticketAdapter, times(1)).register(any(), any());
    }

    @Test
    void shouldModifyGroupTalkSuccessfully() {
        TicketKey ticketKey = TicketKey.sample();
        GroupTalkUdo groupTalkUdo = GroupTalkUdo.sample();

        groupTalkService.modify(ticketKey, groupTalkUdo);

        verify(ticketAdapter, times(1)).modify(any(), any());
    }

    @Test
    void shouldRemoveGroupTalkSuccessfully() {
        TicketKey ticketKey = TicketKey.sample();

        groupTalkService.remove(ticketKey);

        verify(ticketAdapter, times(1)).remove(any());
    }
}
```

```java
@ExtendWith(MockitoExtension.class)
public class GroupTalkServiceImplTest {
    
    @Mock
    private TicketAdapter ticketAdapter;
    @Mock
    private TicketQueryAdapter ticketQueryAdapter;
    @Mock
    private TicketCustomMetadataAdapter ticketCustomMetadataAdapter;
    @Mock
    private GroupTalkMemberService groupTalkMemberService;
    @Mock
    private GroupTalkPartnerService groupTalkPartnerService;
    @Mock
    private TalkQueryAuthorizedFilterService talkQueryAuthorizedFilterService;
    @Mock
    private TalkCustomerQueryAuthorizedFilterService talkCustomerQueryAuthorizedFilterService;

    @InjectMocks
    GroupTalkServiceImpl service;
    
    @Test
    public void testFindAll() {
        GroupTalkQuery query = new GroupTalkQuery();
        service.findAll(query);
        verify(ticketQueryAdapter).findAll(any(), any(), any(), any());
    }

    @Test
    public void testFind() {
        TicketKey ticketKey = new TicketKey();
        service.find(ticketKey);
        verify(ticketQueryAdapter).find(any());
    }

    @Test
    public void testFindForUpdate() {
        TicketKey ticketKey = new TicketKey();
        service.findForUpdate(ticketKey);
        verify(ticketQueryAdapter).findForUpdate(any());
    }

    @Test
    public void testRegister() {
        TicketKey ticketKey = new TicketKey();
        GroupTalkCdo groupTalkCdo = new GroupTalkCdo();
        service.register(ticketKey, groupTalkCdo);
        verify(ticketAdapter).register(any(), any());
    }

    @Test
    public void testModify() {
        TicketKey ticketKey = new TicketKey();
        GroupTalkUdo groupTalkUdo = new GroupTalkUdo();
        service.modify(ticketKey, groupTalkUdo);
        verify(ticketAdapter).modify(any(), any());
    }

    @Test
    public void testRemove() {
        TicketKey ticketKey = new TicketKey();
        service.remove(ticketKey);
        verify(ticketAdapter).remove(any());
    }
}

```
