
1. **Uygulama Hizmetleri**'ne tıklayın > Mobil Uygulama arka ucunuzu seçin > **Hızlı Başlangıç** > istemci platformunuza (iOS, Android, Xamarin, Cordova) tıklayın.

![Mobile Apps Hızlı Başlangıcın vurgulandığı Azure Portal][quickstart]

2. Veritabanı bağlantısı yapılandırılmamışsa bir Veri Bağlantısı oluşturmanız gerekir.

![BD'ye Mobile Apps Connect ile Azure Portal][connect]

  * Yeni SQL Veritabanı'nı ve sunucusunu oluşturun.

  ![Mobile Apps ile Azure Portal yeni BD ve sunucu oluşturma][server]

  * Veri bağlantısı başarıyla oluşturulana kadar bekleyin.

  ![Mobile Apps ile Azure Portal veri bağlantısı oluşturma bildirimi][notification]

  * Veri bağlantısının başarılı olması gerekir.

  ![Mobile Apps ile Azure Portal veri bağlantısı oluşturma bildirimi][already-connection]

3. **2. Tablo API'si oluştur**'un altında **Arka uç dili** olarak Node.js seçeneğini belirleyin. Bildirimi kabul edin ve **TodoItem tablosu oluştur**’a tıklayın. Böylece veritabanınızda yeni bir *TodoItem* tablosu oluşturulur. Var olan bir arka ucun Node.js’ye geçirilmesinin tüm içeriğin üzerine yazacağını unutmayın! Bunun yerine bir .NET arka ucu oluşturmak için [bu yönergeleri izleyin][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
