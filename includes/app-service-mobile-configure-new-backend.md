
1. Tıklatın **uygulama hizmetleri** düğmesi, Mobile Apps arka ucunuz seçin, **Hızlı Başlangıç**ve ardından, istemci Platformu (iOS, Android, Xamarin, Cordova) seçin.

    ![Azure portal ile Mobile Apps vurgulanmış hızlı başlangıç][quickstart]

2. Bir veritabanı bağlantısı yapılandırılmamışsa, aşağıdakileri yaparak oluşturun:

    ![Azure portalına veritabanı Mobile Apps Connect ile][connect]

    a. Sunucu ve yeni SQL veritabanı oluşturun.

    ![Mobile Apps ile Azure portalı, yeni veritabanı ve sunucu oluşturma][server]

    b. Veri bağlantısı başarıyla oluşturulana kadar bekleyin.

    ![Veri bağlantısı başarılı oluşturulma Azure portal bildirimi][notification]

    c. Veri bağlantısının başarılı olması gerekir.

    !["Bir veri bağlantısı zaten" azure portal bildirimi][already-connection]

3. **2. Tablo API'si oluştur**'un altında **Arka uç dili** olarak Node.js seçeneğini belirleyin. 
 
4. Bildirimi kabul edin ve ardından **Todoıtem tablosu oluştur**.  
    Bu eylem, veritabanınızdaki yeni bir Yapılacaklar öğesi tablo oluşturur. 

    >[!IMPORTANT]
    > Var olan bir arka uç node.js'ye geçirilmesinin tüm içeriğinin üzerine yazar. Bunun yerine bir .NET arka ucu oluşturmak için bkz [mobil uygulamalar için .NET arka uç sunucusu SDK çalışma][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
