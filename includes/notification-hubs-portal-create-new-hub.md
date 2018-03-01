

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Kaynak oluştur** > **Web ve Mobil** > **Bildirim Hub'ı** seçeneğini belirleyin.
   
      ![Azure portalı - Bildirim hub'ı oluşturma](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. **Bildirim Hub'ı** kutusuna benzersiz bir ad yazın. **Bölge**, **Abonelik** ve **Kaynak Grubu** (zaten varsa) seçimi yapın. 
   
      Hizmet veri yolu ad alanınız yoksa hub adına göre oluşturulan (ad alanı adı varsa) varsayılan adı kullanabilirsiniz.
    
      Hub'ı oluşturmak istediğiniz bir hizmet veri yolu ad alanı varsa aşağıdaki adımları izleyin

    a. **Ad Alanı** alanında **Var Olanı Seç** bağlantısını seçin. 
   
    b. **Oluştur**’u seçin.
   
      ![Azure portalı - Bildirim hub'ı özelliklerini ayarlama](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Ad alanını ve bildirim hub'ını oluşturduktan sonra **Tüm kaynaklar**'ı seçerek açın ve ardından listeden oluşturulan bildirim hub'ını seçin. 
   
      ![Azure portalı - Bildirim hub'ı portal sayfası](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-resources.png)

5. Listeden **Erişim İlkeleri**'ni seçin. Verilen iki bağlantı dizesini not edin. Bu dizelere daha sonra anında iletme bildirimleri için ihtiyaç duyacaksınız.

      >[!IMPORTANT]
      >Uygulamanızda DefaultFullSharedAccessSignature **KULLANMAYIN**. Bunu yalnızca arka ucunuzda kullanmanız gerekir.
      >
   
      ![Azure portalı - Bildirim hub'ı bağlantı dizeleri](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

