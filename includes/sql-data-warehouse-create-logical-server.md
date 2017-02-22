### <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Azure portalında yeni bir mantıksal SQL sunucusu oluşturma

1. **Yeni**'ye tıklayın, **mantıksal sunucu** terimini arayın ve **ENTER**'a basın.

    ![mantıksal sunucu arama](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. **SQL Server (mantıksal sunucu)** girişini seçin 

    ![mantıksal sunucu seçme](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Yeni SQL Sunucusu (mantıksal sunucu) dikey penceresini açmak için **Oluştur**’a tıklayın.

   <kbd> ![açık mantıksal sunucu dikey penceresi](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd>
    <kbd>![mantıksal sunucu dikey penceresi](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd>
  
3. SQL Server (mantıksal sunucu) dikey penceresinin sunucu adı metin kutusunda, yeni mantıksal sunucu için geçerli bir ad belirtin. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![yeni sunucu adı](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > Yeni sunucunuzun tam adı <sunucu_adınız>.database.windows.net olacaktır.
    >
    
4. Sunucu yöneticisi oturum açma adı metin kutusunda, bu sunucuda SQL kimlik doğrulaması oturum açma işlemi için kullanılacak olan kullanıcı adını belirtin. Bu oturum açma işlemi, asıl sunucu oturum açma işlemi olarak bilinir. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![SQL yöneticisi oturum açma adı](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. **Parola** ve **Parolayı onaylayın** metin kutularında, sunucu sorumlusu oturum açma hesabı için parola belirtin. Yeşil onay işareti, geçerli bir parola sağladığınızı gösterir.
    
    ![SQL yönetici parolası](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. İçinde nesne oluşturma izniniz olan bir abonelik seçin.

    ![aboneliği](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. Kaynak grubu metin kutusunda **Yeni oluştur**’u seçin ve sonra kaynak grubu metin kutusunda yeni kaynak grubu için geçerli bir isim belirtin (daha önce oluşturduğunuz bir kaynak grubu varsa, bu kaynak grubunu da kullanabilirsiniz). Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.

    ![yeni kaynak grubu](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. **Konum** metin kutusunda, konumunuza uygun bir veri merkezi (örneğin, "Avustralya Doğu") seçin.
    
    ![sunucu konumu](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > **Azure hizmetlerinin sunucuya erişmesine izin ver** onay kutusu bu dikey pencerede değiştirilemez. Bu ayarı, sunucu güvenlik duvarı dikey penceresinde değiştirebilirsiniz. Daha fazla bilgi için bkz. [Güvenliğe giriş](../articles/sql-database/sql-database-manage-servers-portal.md).
    >
    
9. **Oluştur**’a tıklayın.

    ![oluştur düğmesi](./media/sql-data-warehouse-create-logical-server/create.png)



<!--HONumber=Feb17_HO3-->


