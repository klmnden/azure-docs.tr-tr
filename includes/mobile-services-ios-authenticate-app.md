* Open **QSTodoListViewController.m** and add the following method. Change *facebook* to *microsoftaccount*, *twitter*, *google*, or *windowsazureactivedirectory* if you're not using Facebook as your identity provider.

```
        - (void) loginAndGetData
        {
            MSClient *client = self.todoService.client;
            if (client.currentUser != nil) {
                return;
            }

            [client loginWithProvider:@"facebook" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                [self refresh];
            }];
        }
```

* Replace `[self refresh]` in `viewDidLoad` with the following:

```
        [self loginAndGetData];
```

* Press  **Run** to start the app, and then log in. When you are logged in, you should be able to view the Todo list and make updates.

