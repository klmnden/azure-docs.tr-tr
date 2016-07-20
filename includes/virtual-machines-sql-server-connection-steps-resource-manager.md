### Genel IP adresi için DNS etiketi yapılandırma

SQL Server Veritabanı Altyapısına İnternet'ten bağlanmak için önce genel IP adresi için bir DNS etiketi yapılandırın.

> [AZURE.NOTE] SQL Server örneğine yalnızca aynı Sanal Ağ içinden veya yerel olarak bağlanmayı planlıyorsanız, DNS Etiketleri gerekli değildir.

DNS etiketi oluşturmak için önce portalda **Virtual Machines**’i seçin. Özelliklerini görüntülemek için SQL Server VM’yi seçin.

1. Sanal makine dikey penceresinde **Genel IP adresi**’ni seçin.

    ![genel ip adresi](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

2. Genel IP adresinizin özelliklerinde**Yapılandırma**’yı genişletin.

3. DNS etiket adı girin. Bu ad, SQL Server VM'nize bağlanmak için IP adresi yerine doğrudan kullanılabilen bir Kayıttır.

    ![dns etiketi](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### Başka bir bilgisayardan Veritabanı Altyapısına bağlanma

1. İnternet'e bağlı bir bilgisayarda SQL Server Management Studio’yu (SSMS) açın.

2. **Sunucuya Bağlan** veya **Veritabanı Altyapısına Bağlan** iletişim kutusunda **Sunucu adı** değerini düzenleyin. Sanal makinenin tam DNS adını girin (önceki görevde saptanmıştır).

3. **Kimlik Doğrulaması** kutusunda **SQL Server Kimlik Doğrulaması**’nı seçin.

5. **Oturum Aç** kutusuna geçerli bir SQL oturum açma adı yazın.

6. **Parola** kutusuna oturum açma parolasını yazın.

7. **Bağlan**'a tıklayın.

    ![ssms bağlanma](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)



<!--HONumber=Jun16_HO2-->


