```java
    @DisplayName("findAll returns GroupTalkListItemList when TicketQueryAdapter returns non-empty TicketRdoListRdo")
@Test
void findAllReturnsGroupTalkListItemListWhenTicketQueryAdapterReturnsNonEmptyTicketRdoListRdo() {
    TicketRdoListRdo ticketRdoListRdo = new TicketRdoListRdo();
    ticketRdoListRdo.setList(Arrays.asList(new TicketRdo(), new TicketRdo()));
    when(ticketQueryAdapter.findAll(any(), any(), any(), any())).thenReturn(ticketRdoListRdo);

    GroupTalkListItemList result = groupTalkService.findAll(groupTalkQuery);

    assertThat(result).isNotNull();
    assertThat(result.getList()).isNotEmpty();
}

@DisplayName("findAll returns GroupTalkListItemList with empty list when TicketQueryAdapter returns empty TicketRdoListRdo")
@Test
void findAllReturnsGroupTalkListItemListWithEmptyListWhenTicketQueryAdapterReturnsEmptyTicketRdoListRdo() {
    TicketRdoListRdo ticketRdoListRdo = new TicketRdoListRdo();
    ticketRdoListRdo.setList(Collections.emptyList());
    when(ticketQueryAdapter.findAll(any(), any(), any(), any())).thenReturn(ticketRdoListRdo);

    GroupTalkListItemList result = groupTalkService.findAll(groupTalkQuery);

    assertThat(result).isNotNull();
    assertThat(result.getList()).isEmpty();
}

@DisplayName("findAll throws IllegalArgumentException when GroupTalkQuery has invalid ReadAuthorityType")
@Test
void findAllThrowsIllegalArgumentExceptionWhenGroupTalkQueryHasInvalidReadAuthorityType() {
    groupTalkQuery.setReadAuthorityType(ReadAuthorityType.UNKNOWN);
    assertThrows(IllegalArgumentException.class, () -> groupTalkService.findAll(groupTalkQuery));
}
```
