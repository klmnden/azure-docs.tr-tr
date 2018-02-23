1. Adresinde oturum açın [Azure portal].
2. Seçin **+ yeni** > **Web + mobil** > **mobil uygulama**ve mobil uygulamalarınızı arka uç için bir ad sağlayın.
3. İçin **kaynak grubu**, varolan bir kaynak grubu seçin veya (aynı adı kullanarak uygulamanızı) yeni bir tane oluşturun. 
4. İçin **uygulama hizmeti planı**, varsayılan plan (içinde [standart katmanı](https://azure.microsoft.com/pricing/details/app-service/)) seçilidir. Ayrıca, farklı bir plan seçebilirsiniz veya [yeni bir tane oluşturun](../articles/app-service/app-service-plan-manage.md#create-an-app-service-plan). 

   Uygulama hizmeti planının ayarları belirlemek [konumu, özellikleri, maliyet ve işlem kaynaklarını](https://azure.microsoft.com/pricing/details/app-service/) uygulamanızla ilişkili. Uygulama hizmeti hakkında daha fazla bilgi için planları ve farklı fiyatlandırma yeni bir plan oluşturmak nasıl katmanı ve tercih ettiğiniz konumda bkz [Azure App Service planlarına ayrıntılı genel bakış](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
5. **Oluştur**’u seçin. Bu adımda, Mobile Apps arka uç oluşturulur. 
6. İçinde **ayarları** yeni Mobile Apps arka uç, bölmesinde seçin **Hızlı Başlangıç** > istemci uygulaması platformunuz > **bir veritabanına bağlanmak**. 
   
   ![Bir veritabanına bağlanmak için seçimleri](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
7. İçinde **veri bağlantısı Ekle** bölmesinde, **SQL veritabanı** > **yeni bir veritabanı oluşturmak**. Veritabanı adı girin, bir fiyatlandırma katmanı seçin ve ardından **Server**. Bu yeni veritabanını yeniden kullanabilirsiniz. Aynı konumda zaten bir veritabanınız varsa, bunun yerine **Mevcut veritabanlarından birini kullanın**’ı seçebilirsiniz. Bant genişliği maliyetleri ve daha yüksek gecikme nedeniyle farklı bir konumda veritabanının kullanılmasını öneririz yok.
   
   ![Bir veritabanı seçme](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
8. İçinde **yeni sunucu** bölmesinde, bir benzersiz sunucu adı girin **sunucu adı** kutusunda, bir oturum açma ve parola seçin sağlayın **izin Azure Hizmetleri erişim sunucusuna**, seçin **Tamam**. Bu adım, yeni bir veritabanı oluşturur.
9. Geri **veri bağlantısı Ekle** bölmesinde, **bağlantı dizesi**, veritabanınız için oturum açma ve parola değerlerini girin ve seçin **Tamam**. 

   Veritabanı devam etmeden önce başarılı bir şekilde dağıtılması için birkaç dakika bekleyin.

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/
