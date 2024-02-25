```java
@RunWith(MockitoJUnitRunner.class)
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
    public void testFindAll() {
        // Prepare
        GroupTalkQuery query = new GroupTalkQuery();
        when(ticketQueryAdapter.findAll(any(), any(), any(), any())).thenReturn(new TicketRdoListRdo());

        // Execute
        GroupTalkListItemList result = groupTalkService.findAll(query);

        // Verify
        assertNotNull(result);
        // Add more assertions as needed
    }

    @Test
    public void testFindAllWithNullQuery() {
        // Prepare
        when(ticketQueryAdapter.findAll(any(), any(), any(), any())).thenReturn(new TicketRdoListRdo());

        // Execute
        GroupTalkListItemList result = groupTalkService.findAll(null);

        // Verify
        assertNotNull(result);
        // Add more assertions as needed
    }

    @Test
    public void testMapListRdoToGroupTalkListItems() {
        // Prepare
        TicketRdoListRdo listRdo = new TicketRdoListRdo();
        // Add some tickets to listRdo
        when(ticketQueryAdapter.findAll(any(), any(), any(), any())).thenReturn(listRdo);

        GroupTalkQuery query = new GroupTalkQuery();
        // Add necessary fields to query

        // Execute
        GroupTalkListItemList result = groupTalkService.findAll(query);

        // Verify
        // Add assertions to verify the mapping of listRdo to group talk list items
    }

    @Test
    public void testFindWithNonNullTicketRdo() {
        // Arrange
        TicketKey ticketKey = new TicketKey(/* ticket key details */);
        TicketRdo ticketRdo = new TicketRdo(/* ticket Rdo details */);
        when(ticketQueryAdapter.find(ticketKey)).thenReturn(ticketRdo);

        // Act
        GroupTalk result = groupTalkService.find(ticketKey);

        // Assert
        assertNotNull(result);
        // Add more specific assertions based on the behavior of the find(ticketKey, ticketRdo) method
    }

    @Test
    public void testFindWithNullTicketRdo() {
        // Arrange
        TicketKey ticketKey = new TicketKey(/* ticket key details */);
        when(ticketQueryAdapter.find(ticketKey)).thenReturn(null);

        // Act
        GroupTalk result = groupTalkService.find(ticketKey);

        // Assert
        assertNull(result);
        // Add more specific assertions based on the behavior of the find(ticketKey, ticketRdo) method
    }
}
```
