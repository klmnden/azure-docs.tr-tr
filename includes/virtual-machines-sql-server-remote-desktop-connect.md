Uzak Masaüstü kullanarak SQL Server sanal makinesine bağlanmak için aşağıdaki adımları kullanın:

1. Azure sanal makinesi oluşturulup çalıştırıldıktan sonra VM'lerinizi görüntülemek için Azure portalında Sanal Makineler simgesine tıklayın.

1. Yeni VM’niz için üç noktaya **...** tıklayın.

1. **Bağlan**'a tıklayın.

   ![Portalda VM'ye bağlanın](./media/virtual-machines-sql-server-remote-desktop-connect/azure-virtual-machine-connect.png)

1. VM’niz için tarayıcınızın indirdiği **RDP** dosyasını açın.

1. Uzak Masaüstü Bağlantısı, bu uzak bağlantının yayımcısının tanımlanamadığı bildiriminde bulunur. Devam etmek için **Bağlan**’a tıklayın.

1. **Windows Güvenliği** iletişim kutusunda, **Farklı bir hesap kullan**’a tıklayın. Bunu görmek için **Diğer seçenekler**’e tıklamanız gerekebilir. VM oluştururken yapılandırdığınız kullanıcı adını ve parolayı belirtin. Kullanıcı adından önce ters eğik çizgi eklemeniz gerekir.

   ![Uzak masaüstü kimlik doğrulaması](./media/virtual-machines-sql-server-remote-desktop-connect/remote-desktop-connect.png)

SQL Server sanal makineye bağlandıktan sonra, SQL Server Management Studio'yu başlatabilir ve yerel yönetici kimlik bilgilerinizi kullanarak Windows Kimlik Doğrulamasına bağlanabilirsiniz. SQL Server Kimlik Doğrulamasını etkinleştirdiyseniz, sağlama işlemi sırasında yapılandırdığınız SQL oturum açma adı ve parolasını kullanarak da SQL Kimlik Doğrulamasına bağlanabilirsiniz.

Makineye erişim, gereksinimlerinize göre makineyi ve SQL Server ayarlarını doğrudan değiştirmenize olanak tanır. Örneğin, güvenlik duvarı ayarlarını yapılandırabilir veya SQL Server yapılandırma ayarlarını değiştirebilirsiniz.