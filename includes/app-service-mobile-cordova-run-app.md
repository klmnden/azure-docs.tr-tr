
1. [Azure Portal]’ı ziyaret edin. **Tümüne Gözat** > **Mobile Apps** > henüz oluşturmuş olduğunuz arka uca tıklayın. Mobil uygulama ayarlarında, **Hızlı Başlangıç** > **Cordova** öğesine tıklayın. **İstemci uygulamanızı yapılandırın** altında **Yeni Uygulama Oluştur**’u seçin ve **İndir**’e tıklayın. Arka ucunuza bağlanacak önceden yapılandırılmış bir uygulama için tam bir Cordova projesini indirir.
2. İndirilen ZIP dosyasını sabit sürücünüzde bir dizine çıkarın, çözüm dosyasına (.sln) gidin ve Visual Studio'yu kullanarak dosyayı açın.
3. Visual Studio'da başlangıç okunun yanındaki açılan listeden çözüm platformunu (Android, iOS veya Windows) seçin, ardından yeşil ok üzerindeki açılan listeye tıklayarak belirli bir dağıtım cihazını veya öykünücüyü seçin. Varsayılan Android platformunu ve Ripple öykünücüsünü kullanabilirsiniz. Daha gelişmiş öğreticilerde, desteklenen bir cihazı ve öykünücüyü seçmeniz istenecek. 
4. Kendi Cordova uygulamanızı derlemek ve çalıştırmak için F5'e basın veya yeşil oka tıklayın. Öykünücüde ağa erişim isteyen bir güvenlik iletişim kutusu görürseniz erişim isteğini kabul edin.   
5. Cihazda veya öykünücüde uygulama başlatıldıktan sonra, **Enter new text (Yeni metin gir)** bölümüne *Complete the tutorial (Öğreticiyi tamamla)* gibi anlamlı bir metin girin ve **Add (Ekle)** düğmesine tıklayın.  
   Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, istekten alınan verileri SQL Veritabanı'ndaki TodoItem tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri de mobil uygulamaya geri döndürür. Mobil uygulama bu verileri listede görüntüler.
   
    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
6. Desteklemek istediğiniz her cihaz platformu için önceki üç adımı tekrarlayın.

[Azure Portal]: https://portal.azure.com/


<!--HONumber=Nov16_HO2-->


