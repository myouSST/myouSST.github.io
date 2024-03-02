```java
    @Test
    void findAllTest(){
        // Arrange
        GroupTalkQuery query = new GroupTalkQuery();
        GroupTalkListItemList mockGroupTalkListItemList = new GroupTalkListItemList();

        Mockito.when(ticketRdoListRdoMock.getList()).thenReturn(new ArrayList<>());
        Mockito.when(groupTalkServiceImpl.findAll(query)).thenReturn(mockGroupTalkListItemList);

        // Act
        GroupTalkListItemList result = groupTalkServiceImpl.findAll(query);

        // Assert
        Mockito.verify(ticketRdoListRdoMock, Mockito.times(1)).getList();
        assert(result != null);
        assert(result.equals(mockGroupTalkListItemList));
    }
```
