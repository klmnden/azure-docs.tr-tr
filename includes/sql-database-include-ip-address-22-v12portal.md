
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, the following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through the new Azure portal
-->


1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Sol taraftaki listeden seçin **tüm hizmetleri**. 

3. Kaydırın ve seçin **SQL sunucuları**. 
   
    ![Portalda Azure SQL veritabanı sunucunuzun Bul][b21-FindServerInPortal]
5. Filtre metin kutusuna, sunucunuzun adını yazmaya başlayın. Satır görüntülenir.

6. Sunucunuz için satırı seçin. Sunucunuz için bir dikey pencerede görüntülenir.

7. Sunucu dikey penceresinde seçin **ayarları**. 

8. Seçin **Güvenlik Duvarı**. 
   
    ![Ayarları seçin > Güvenlik Duvarı][b31-SettingsFirewallNavig]
9. Seçin **istemcisi ekleme IP**. İlk metin kutusunda, yeni kural için bir ad yazın.

10. Etkinleştirmek istediğiniz aralığı için düşük ve yüksek IP adresi değerlerini yazın.
    
    * Düşük değer end ile sağlamak için kullanışlı olabilir **.0** ve sonunda yüksek bir değer **.255**.
    
    ![İzin vermek için bir IP adres aralığı Ekle][b41-AddRange]
11. **Kaydet**’i seçin.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
