```java
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
```
