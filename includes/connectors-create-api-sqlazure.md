### <a name="prerequisites"></a>Ön koşullar
* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [Azure SQL veritabanı](../articles/sql-database/sql-database-get-started.md) ile bağlantı bilgilerini, sunucu adını, veritabanı adı ve kullanıcı adı/parola dahil olmak üzere. Bu bilgiler, SQL veritabanı bağlantı dizesine dahil edilir:
  
    Sunucu = tcp:*yoursqlservername*. database.windows.net,1433;Initial katalog =*yourqldbname*; Kalıcı güvenlik bilgisi = False; Kullanıcı Kimliği {your_username} =; Parola {your_password} =; MultipleActiveResultSets = False; Şifreleme = True; TrustServerCertificate = False; Bağlantı zaman aşımı = 30;
  
    Daha fazla bilgi edinin [Azure SQL veritabanlarını](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> Bir Azure SQL veritabanı oluşturduğunuzda, aynı zamanda SQL ile bulunan örnek veritabanları oluşturabilirsiniz. 
> 
> 

Azure SQL veritabanınıza bir mantıksal uygulama kullanmadan önce SQL veritabanına bağlayın. Kolayca Azure portalındaki mantıksal uygulama içinde bunu yapabilirsiniz.  

Aşağıdaki adımları kullanarak, Azure SQL veritabanına bağlan:  

1. Bir mantıksal uygulama oluşturun. Logic Apps Tasarımcısı'nda bir tetikleyici ekleyin ve ardından bir eylem ekleyin. Seçin **Göster Microsoft yönetilen API'ler** açılır listesinde ve ardından arama kutusuna "sql" girin. Eylemlerden birini seçin:  
   
    ![SQL Azure bağlantısı oluşturma adım](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Daha önce SQL veritabanı için herhangi bir bağlantısı oluşturmadıysanız, bağlantı ayrıntılarını istenir:  
   
    ![SQL Azure bağlantısı oluşturma adım](./media/connectors-create-api-sqlazure/connection-details.png) 
3. SQL veritabanı ayrıntılarını girin. Bir yıldız işareti özelliklerle gereklidir.
   
   | Özellik | Ayrıntılar |
   | --- | --- |
   | Ağ geçidi bağlanma |İşaretlemeden bırakın. Bu, bir şirket içi SQL Server'a bağlanırken kullanılır. |
   | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
   | SQL Server adı * |Sunucu adı girin; aşağıdakine benzer olduğu *servername.database.windows.net*. Sunucu adı, SQL veritabanı özellikleri Azure portalında görüntülenen ve ayrıca bağlantı dizesinde görüntülenir. |
   | SQL veritabanı adı * |SQL veritabanınız verdiğiniz adı girin. Bu bağlantı dizesini SQL veritabanı özelliklerinde listelenir: Initial Catalog =*yoursqldbname*. |
   | Kullanıcı adı * |SQL Database oluşturduğunuzda oluşturulan kullanıcı adı girin. Bu, Azure portalında SQL veritabanı özelliklerinde listelenir. |
   | Parola * |SQL veritabanı oluşturulduğunda, oluşturduğunuz parolayı girin. |
   
    Bu kimlik bilgileri, mantıksal uygulamanızı bağlanmak ve SQL verilerinize erişmek üzere yetkilendirmek için kullanılır. Tamamlandıktan sonra bağlantı ayrıntılarınızı şuna benzer:  
   
    ![SQL Azure bağlantısı oluşturma adım](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. **Oluştur**’u seçin. 
5. Bağlantıyı oluşturan dikkat edin. Şimdi, mantıksal uygulamanızı diğer adımlarla devam edin: 
   
    ![SQL Azure bağlantısı oluşturma adım](./media/connectors-create-api-sqlazure/table.png)

