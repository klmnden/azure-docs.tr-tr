

## Yeni veritabanı kullanıcı db_owner izinlerini verin

Varolan bir veritabanı kullanıcı db_owner izinleri vermek için aşağıdaki adımları kullanın

Bu adımlar SSMS’deki Nesne Gezgini'nde SQL Database’e bağlı olduğunuzu varsayar ve SQL Database mantıksal sunucunuza ya sunucu düzeyinde sorumlu yönetici olarak ya da kullanıcı izinleri verecek izinlere sahip kullanıcı hesabıyla bağlanırlar. 

1. Nesne Gezgini'nde Veritabanları düğümünü genişletin ve dbo izinleri vermek istediğiniz kullanıcının bulunduğu veritabanını seçin.

     ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Seçilen veritabanına sağ tıklayın ve **Sorgu**’ya tıklayın.

     ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. Sorgu penceresinde, belirtilen kullanıcıya dbo izinlerini vermek için aşağıdaki Transact-SQL deyimini düzenleyin ve kullanın. 

    '''ALTER ROLE db_owner ADD MEMBER user1;

     ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-grant-database-user-dbo-permissions/sql-database-grant-database-user-dbo-permissions-1.png)





<!--HONumber=Jun16_HO2-->


