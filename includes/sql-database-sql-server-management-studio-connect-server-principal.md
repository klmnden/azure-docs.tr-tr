

## Sunucu düzeyinde asıl oturum açmayı kullanılarak Azure SQL Database’e bağlanma

Sunucu düzeyinde oturum açmayı kullanarak SSMS barındıran Azure SQL Database’e bağlanmak için aşağıdaki adımları kullanın.

1. Windows Arama kutusuna "Microsoft SQL Server Management Studio" yazın ve SSMS’yi başlatmak için masaüstü uygulamasına tıklayın.

2. Sunucuya Bağlan penceresine şu bilgileri girin:

 - **Sunucu türü**: Varsayılan veritabanı altyapısıdır; bu değeri değiştirmeyin.
 - **Sunucu adı**: SQL Database’i barındıran sunucunun adını şu biçimde girin: *&lt;sunucuadı >*.**database.windows.net**
 - **Kimlik doğrulaması türü**: Yeni başlıyorsanız SQL Kimlik Doğrulaması’nı seçin. SQL Database mantıksal sunucusu için Active Directory’yi etkinleştirdiyseniz, Active Directory Parola Kimlik Doğrulaması’nı veya Active Directory Tümleşik Kimlik Doğrulaması’nı seçebilirsiniz.
 - **Kullanıcı adı**: SQL Kimlik Doğrulaması’nı veya Active Directory Parola Kimlik Doğrulaması’nı seçtiyseniz, sunucudaki veritabanına erişimi olan bir kullanıcı adı girin.
 - **Parola**: SQL Kimlik Doğrulaması’nı veya Active Directory Parola Kimlik Doğrulaması’nı seçtiyseniz belirtilen kullanıcı için parola girin.
   
       ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-1.png)

3. **Bağlan**'a tıklayın.
 
4. İstemcinin IP adresinin SQL Database mantıksal sunucusuna erişimi yoksa, bir Azure hesabına giriş yapıp sunucu düzeyinde güvenlik duvarı kuralı oluşturmanız istenir. Azure aboneliği yöneticisiyseniz, sunucu düzeyinde güvenlik duvarı kuralı oluşturmak için **Giriş yap**’a tıklayın. Bu yapılmazsa, bir Azure yöneticisinin sunucu düzeyinde güvenlik duvarı kuralı oluşturmasını sağlayın.
 
      ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-2.png)
 
1. Azure aboneliği yöneticisiyseniz ve oturum açmanız gerekiyorsa, oturum açma sayfası göründüğünde aboneliğiniz için kimlik bilgilerini sağlayın ve giriş yapın.

      ![oturum aç](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-3.png)
 
1. Azure girişini sorunsuz yaptıktan sonra önerilen sunucu düzeyinde güvenlik duvarı kuralını gözden geçirin (bir dizi IP adresine izin vermek için değiştirebilirsiniz), sonra da güvenlik duvarı kuralı oluşturmak ve SQL Database bağlantısını tamamlamak için **Tamam**’a tıklayın.
 
      ![yeni sunucu düzeyinde güvenlik duvarı](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-4.png)
 
5. Kimlik bilgileriniz size erişim sağlıyorsa, Nesne Gezgini açılır; siz de artık yönetici görevleri gerçekleştirebilir ya da verileri sorgulayabilirsiniz. 
 
     ![yeni sunucu düzeyinde güvenlik duvarı](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-5.png)
 
     
## Bağlantı hatalarını giderme

Bağlantı hatasının en yaygın nedeni sunucu adındaki ( <*sunucuadı*> mantıksal sunucunun adıdır, veritabanının değil), kullanıcı adındaki veya paroladaki hatalardır; bunun yanı sıra sunucuya güvenlik nedenleriyle bağlantılara izin verilmiyordur. 





<!--HONumber=Aug16_HO1-->


