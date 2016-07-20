

## SSMS kullanarak yeni veritabanı kullanıcısı oluşturma

SSMS kullanarak var olan bir veritabanında yeni veritabanı kullanıcısı oluşturmak için aşağıdaki adımları kullanın. 

Bu adımlar SSMS kullanan Nesne Gezgini'nde SQL Database’e bağlı olduğunuzu varsayar ve SQL Database mantıksal sunucunuza ya sunucu düzeyinde sorumlu yönetici olarak ya da yeni kullanıcı oluşturacak izinlere sahip kullanıcı hesabıyla bağlanırlar. 

1. Nesne Gezgini'nde Veritabanları düğümünü genişletin ve yeni kullanıcı hesabını oluşturmak istediğiniz veritabanını seçin.

     ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Seçilen veritabanına sağ tıklayın ve **Sorgu**’ya tıklayın.

     ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. Sorgu penceresinde, kullanıcı veritabanınızda içerilen kullanıcı oluşturmak için aşağıdaki Transact-SQL deyimini düzenleyin ve kullanın. 

    ```CREATE USER user1 WITH PASSWORD ='p@ssw0rd1';

     ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-3.png)






<!--HONumber=Jun16_HO2-->


