1. [Azure Portal]’da oturum açın.

2. **+YENİ** > **Web + Mobil** > **Mobil Uygulama**’ya tıklayıp Mobile Uygulama arka ucu için bir ad verin.

3. **Kaynak Grubu** için yeni bir kaynak grubu seçin ya da yeni bir tane oluşturun (uygulamanızla aynı adı kullanarak). 
 

 Başka bir App Service planı seçin veya yeni bir tane oluşturun. Uygulama Hizmetleri planları, farklı fiyatlandırma katmanında ve tercih ettiğiniz konumda yeni plan oluşturma hakkında daha fazla bilgi için bkz. [Azure App Service planlarına ayrıntılı genel bakış](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **App Service planı** için, varsayılan plan ([Standart katman](https://azure.microsoft.com/pricing/details/app-service/)’da) seçilidir. Aynı zamanda başka bir plan da seçebilir veya [yeni bir tane oluşturabilirsiniz](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). App Service planının ayarları, uygulamanızla ilişkili [konumu, özellikleri, maliyeti ve işlem kaynaklarını](https://azure.microsoft.com/pricing/details/app-service/) saptar. 

    Planla ilgili kararı verdikten sonra **Oluştur**’a tıklayın. Böylece Mobil Uygulama arka uç oluşturulur. 
    
6. Yeni Mobil Uygulama arka ucunun **Ayarlar** dikey penceresinde **Hızlı başlangıç** > istemci uygulaması platformunuz > **Veritabanına bağlan**’a tıklayın. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. **Veri bağlantısı ekle** dikey penceresinde **SQL Database** > **Yeni veritabanı oluşturun**’a tıklayın, veritabanı için **Ad** yazın, fiyatlandırma katmanını seçin ve ardından **Sunucu**’ya tıklayın.  Bu yeni veritabanını yeniden kullanabilirsiniz. Aynı konumda zaten bir veritabanınız varsa, bunun yerine **Mevcut veritabanlarından birini kullanın**’ı seçebilirsiniz. Farklı bir konumdaki veritabanının kullanımı, bant genişliği maliyetleri ve daha yüksek gecikme nedeniyle önerilmez.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. **Yeni Sunucu** dikey penceresinin **sunucu adı** alanına benzersiz bir sunucu adın yazın, oturum açma ve parola verin, **Azure hizmetlerinin sunucuya erişmesine izin ver**’i denetleyin ve **Tamam**’a tıklayın. Böylece yeni bir veritabanı oluşturulur.

9. **Veri bağlantısı ekle** dikey penceresine dönün, **Bağlantı dizesi**’ne tıklayın, oturum açma ve parola değerlerini yazın ve **Tamam**’a tıklayın. Devam etmeden önce veritabanın sorunsuz dağıtılması için birkaç dakika bekleyin.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/



<!--HONumber=Jun16_HO2-->


