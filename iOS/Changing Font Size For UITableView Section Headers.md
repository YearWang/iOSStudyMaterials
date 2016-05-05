# Changing Font Size For UITableView Section Headers


---
```objective-c
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section {

    UILabel *myLabel = [[UILabel alloc] init];
    myLabel.frame = CGRectMake(20, 8, 320, 20);
    myLabel.font = [UIFont boldSystemFontOfSize:18];
    myLabel.text = [self tableView:tableView titleForHeaderInSection:section];

    UIView *headerView = [[UIView alloc] init];
    [headerView addSubview:myLabel];

    return headerView;
}
```




