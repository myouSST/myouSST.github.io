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
    private GroupTalkServiceImpl groupTalkService;

    @Test
    void findAll() {
        TicketRdoListRdo ticketRdoListRdo = TicketRdoListRdo.sample();
        when(ticketQueryAdapter.findAll(any(TenantKey.class), any(TicketQuery.class), any(Pageable.class), any(Order.class)))
            .thenReturn(ticketRdoListRdo);

        GroupTalkQuery groupTalkQuery = GroupTalkQuery.builder()
            .tenantKey(TenantKey.sample())
            .ticketQuery(TicketQuery.sample())
            .pageable(Pageable.all())
            .order(Order.CREATED)
            .build();

        GroupTalkListItemList actual = groupTalkService.findAll(groupTalkQuery);

        GroupTalkListItemList expected = new GroupTalkListItemList(
            ticketRdoListRdo.getList().stream()
                .map(GroupTalkUtil::newTalk)
                .map(GroupTalkListItem::new)
                .collect(Collectors.toList()),
            -1,
            TicketRdoListRdo.sample().getTotal()
        );

        assertEquals(expected, actual);
    }

    @Test
    void find() {
        TicketRdo ticketRdo = TicketRdo.sample();
        MemberList memberList = MemberList.sample();
        PartnerList partnerList = PartnerList.sample();

        when(ticketQueryAdapter.find(any(TicketKey.class)))
            .thenReturn(ticketRdo);
        when(groupTalkMemberService.findAll(any(TicketKey.class)))
            .thenReturn(memberList);
        when(groupTalkPartnerService.findAll(any(TicketKey.class)))
            .thenReturn(partnerList);


        GroupTalk actual = groupTalkService.find(TicketKey.sample());

        GroupTalk expected = new GroupTalk(GroupTalkUtil.newTalk(ticketRdo), memberList, partnerList, GroupTalkUtil.newGroupTalkOption(ticketRdo));

        assertEquals(expected, actual);
    }

    @Test
    void findForUpdate() {
        TicketRdo ticketRdo = TicketRdo.sample();
        MemberList memberList = MemberList.sample();
        PartnerList partnerList = PartnerList.sample();

        when(ticketQueryAdapter.findForUpdate(any(TicketKey.class)))
            .thenReturn(ticketRdo);
        when(groupTalkMemberService.findAll(any(TicketKey.class)))
            .thenReturn(memberList);
        when(groupTalkPartnerService.findAll(any(TicketKey.class)))
            .thenReturn(partnerList);


        GroupTalk actual = groupTalkService.findForUpdate(TicketKey.sample());

        GroupTalk expected = new GroupTalk(GroupTalkUtil.newTalk(ticketRdo), memberList, partnerList, GroupTalkUtil.newGroupTalkOption(ticketRdo));

        assertEquals(expected, actual);
    }

    @Test
    void register() {
        groupTalkService.register(TicketKey.sample(), GroupTalkCdo.sample());

        verify(ticketAdapter).register(any(TicketKey.class), any(TicketCdo.class));
        verify(groupTalkMemberService).joinAll(any(TicketKey.class), any(MemberList.class));
        verify(groupTalkPartnerService).inviteAll(any(TicketKey.class), any(PartnerList.class));
        verify(groupTalkPartnerService).joinAll(any(TicketKey.class), any(PartnerList.class));
    }
}
```
