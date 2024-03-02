```java
@Test
void testFindAll() {
    // Arrange
    GroupTalkQuery query = new GroupTalkQuery();
    query.setTenantKey("test_tenant");
    query.setPageable(new PageRequest(0, 10));
    query.setOrder("desc");

    AuthorizedUserScopeQuery scopeQuery = new AuthorizedUserScopeQuery();
    TicketRdoListRdo ticketRdoListRdo = new TicketRdoListRdo();
    ticketRdoListRdo.setList(new ArrayList<>());
    ticketRdoListRdo.setNext(false);
    ticketRdoListRdo.setTotal(0);

    when(groupTalkService.getUserScopeQuery(any(GroupTalkQuery.class))).thenReturn(scopeQuery);
    when(ticketQueryAdapter.findAll(anyString(), any(), any(), any())).thenReturn(ticketRdoListRdo);

    // Act
    GroupTalkListItemList result = groupTalkService.findAll(query);

    // Assert
    assertEquals(0, result.getList().size());
    assertEquals(false, result.getNext());
    assertEquals(0, result.getTotal());

    verify(groupTalkService, times(1)).getUserScopeQuery(any(GroupTalkQuery.class));
    verify(ticketQueryAdapter, times(1)).findAll(eq("test_tenant"), any(), any(), eq("desc"));
    verifyNoMoreInteractions(groupTalkService, ticketQueryAdapter);
}
```
