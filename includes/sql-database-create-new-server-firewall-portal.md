
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

1. SQL sunucusu dikey penceresinde, Ayarlar’ın altında; SQL sunucusunun Güvenlik Duvarı dikey penceresini açmak için **Güvenlik Duvarı**’na tıklayın.

    ![sql sunucusu güvenlik duvarı](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png)
    
2. Görüntülenen istemci IP adresini gözden geçirin ve bunun sizin IP adresiniz olduğunu, tercih ettiğiniz bir tarayıcıyı kullanarak (“IP adresim nedir” sorusuyla) İnternet üzerinden doğrulayın. IP adresleri bazen çeşitli nedenlerle eşleşmez.

    ![IP adresiniz](../articles/sql-database/media/sql-database-get-started/your-ip-address.png)

3. IP adreslerinin eşleştiğini varsayarak, araç çubuğundaki **İstemci IP’si ekle**’ye tıklayın.

    ![istemci IP’si ekle](../articles/sql-database/media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Sunucudaki SQL Veritabanı güvenlik duvarını, tek bir IP adresi veya bir adresler aralığının tamamı üzerinde açabilirsiniz. Güvenlik duvarını açmak SQL yönetici ve kullanıcılarının; sunucuda, geçerli kimlik bilgilerine sahip oldukları herhangi bir veritabanında oturum açmasını sağlar.
    >

4. Sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için, araç çubuğunda **Kaydet**’e ve sonra **Tamam**’a tıklayın.

    ![istemci IP’si ekle](../articles/sql-database/media/sql-database-get-started/save-firewall-rule.png)

> [!Tip]
> Öğretici için bkz. [SQL Veritabanı öğreticisi: Sunucu, sunucu düzeyinde güvenlik duvarı kuralı, örnek veritabanı, veritabanı düzeyinde güvenlik duvarı oluşturma ve SQL Server’a bağlanma](../articles/sql-database/sql-database-get-started.md).    
>


<!--HONumber=Jan17_HO1-->


