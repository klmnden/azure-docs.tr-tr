
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-08-01 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md
articles/sql-database/sql-database-configure-firewall-settings

-->
## Yeni bir Azure SQL sunucu düzeyinde güvenlik duvarı oluşturma
Tek bir IP adresinden (istemci bilgisayarınız) veya bir IP adresi aralığının tamamından SQL Veritabanı mantıksal sunucusuna bağlantılara izin veren sunucu düzeyinde güvenlik duvarı kuralı oluşturmak için Azure portalında aşağıdaki adımları kullanın.

1. O anda bağlı değilseniz [Azure portal](http://portal.azure.com)’a bağlanın.
2. Varsayılan dikey penceresinde **SQL sunucuları**’na tıklayın.
   
    ![Yeni sunucu güvenlik duvarı](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-1.png)
3. **SQL sunucuları** dikey penceresinde güvenlik duvarı kuralının oluşturulacağı SQL sunucusuna tıklayın.
   
    ![Yeni sunucu güvenlik duvarı](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-2.png)
4. Sunucunuz özelliklerini gözden geçirin.
   
    ![Yeni sunucu güvenlik duvarı](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-3.png)
5. **Ayarlar** dikey penceresinde **Güvenlik Duvarı**’na tıklayın.
   
    ![Yeni sunucu güvenlik duvarı](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-4.png)
   
   > [!NOTE]
   > Sunucu düzeyi **Güvenlik duvarı ayarları** dikey penceresine **Veritabanı** dikey penceresi araç çubuğundan da erişebilirsiniz.
   > 
   > 
6. Azure’un, istemcinizin IP adresi için bir kural oluşturmasını sağlamak için **İstemci IP’si ekle**’ye tıklayın.
   
      ![Yeni sunucu güvenlik duvarı](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-5.png)
7. İsteğe bağlı olarak, bir IP adresi aralığına erişime izin vermek için güvenlik duvarı adresini düzenlemek üzere eklenen IP adresine tıklayın.
   
      ![Yeni sunucu güvenlik duvarı](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-6.png)
8. Sunucu düzeyinde güvenlik duvarı kuralı oluşturmak için **Kaydet**’e tıklayın.
   
     ![Yeni sunucu güvenlik duvarı](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-7.png)
   
   > [!IMPORTANT]
   > İstemci IP adresiniz zaman zaman değişebilir; siz de yeni bir güvenlik duvarı kuralı oluşturana kadar sunucunuza erişemeyebilirsiniz. [Bing](http://www.bing.com/search?q=my%20ip%20address) kullanarak IP adresinizi denetleyebilirsiniz. Ardından tek bir IP adresi veya bir IP adresi aralığı ekleyin. Ayrıntılar için bkz. [Güvenlik duvarı ayarlarını yönetme](../articles/sql-database/sql-database-configure-firewall-settings.md#manage-existing-server-level-firewall-rules-through-the-azure-portal).
   > 
   > 

<!--HONumber=Sep16_HO3-->


