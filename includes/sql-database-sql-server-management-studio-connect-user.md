## <a name="connect-to-azure-sql-database-as-a-user"></a>Azure SQL Database’e kullanıcı olarak bağlanma
SSMS barındıran Azure SQL Database’e kullanıcı olarak bağlanmak için aşağıdaki adımları kullanın.

1. Windows Arama kutusuna "Microsoft SQL Server Management Studio" yazın ve SSMS’yi başlatmak için masaüstü uygulamasına tıklayın.
2. Sunucuya Bağlan penceresine şu bilgileri girin:

* **Sunucu türü**: Varsayılan veritabanı altyapısıdır; bu değeri değiştirmeyin.
  
  * **Sunucu adı**: SQL Database’i barındıran sunucunun adını şu biçimde girin: *&lt;sunucuadı >*.**database.windows.net**
  * **Kimlik doğrulaması türü**: Yeni başlıyorsanız SQL Kimlik Doğrulaması’nı seçin. SQL Database mantıksal sunucusu için Active Directory’yi etkinleştirdiyseniz, Active Directory Parola Kimlik Doğrulaması’nı veya Active Directory Tümleşik Kimlik Doğrulaması’nı seçebilirsiniz.
  * **Kullanıcı adı**: SQL Kimlik Doğrulaması’nı veya Active Directory Parola Kimlik Doğrulaması’nı seçtiyseniz, sunucudaki veritabanına erişimi olan bir kullanıcı adı girin.
  * **Parola**: SQL Kimlik Doğrulaması’nı veya Active Directory Parola Kimlik Doğrulaması’nı seçtiyseniz belirtilen kullanıcı için parola girin.
    
       ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-sql-server-management-studio-connect-user/connect-user-1.png)

1. Bağlanmak istediğiniz veritabanını belirtmek için **Seçenekler**’e tıklayın.
   
      ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-sql-server-management-studio-connect-user/connect-user-2.png)
2. **Veritabanına Bağlan**’da bağlanmak istediğiniz veritabanını seçin.
   
     ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-sql-server-management-studio-connect-user/connect-user-3.png)
3. **Bağlan**'a tıklayın.
4. İstemcinin IP adresinin SQL Database mantıksal sunucusuna erişimi yoksa, bir Azure hesabına giriş yapıp sunucu düzeyinde güvenlik duvarı kuralı oluşturmanız istenir. Azure aboneliği yöneticisiyseniz, sunucu düzeyinde güvenlik duvarı kuralı oluşturmak için **Giriş yap**’a tıklayın. Bunun dışındaki durumlarda, bir yöneticiden sunucu düzeyinde güvenlik duvarı kuralı veya bağlanmaya çalıştığınız veritabanında veritabanı düzeyinde güvenlik duvarı kuralı oluşturmasını isteyin.
   
      ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-sql-server-management-studio-connect-user/connect-user-4.png)
5. Kimlik bilgileriniz belirtilen veritabanına erişmenizi sağlıyorsa, Nesne Gezgini açılır; siz de artık kullanıcı izinlerinize bağlı olarak yönetici görevler gerçekleştirebilir ya da verileri sorgulayabilirsiniz.
   
      ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-sql-server-management-studio-connect-user/connect-user-5.png)

## <a name="troubleshoot-connection-failures"></a>Bağlantı hatalarını giderme
Bağlantı hatasının en yaygın nedeni sunucu adındaki ( <*sunucuadı*> mantıksal sunucunun adıdır, veritabanının değil), kullanıcı adındaki veya paroladaki hatalardır; bunun yanı sıra sunucuya güvenlik nedenleriyle bağlantılara izin verilmiyordur. 



<!--HONumber=Nov16_HO2-->


