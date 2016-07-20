
1. [Azure Portal]’ı ziyaret edin. **Tümüne Gözat** > **Mobile Apps** > henüz oluşturmuş olduğunuz arka uca tıklayın. Mobil uygulama ayarlarında, **Hızlı Başlangıç** > **Cordova** öğesine tıklayın. **İstemci uygulamanızı yapılandırın** altında **Yeni Uygulama Oluştur**’u seçin ve **İndir**’e tıklayın. Arka ucunuza bağlanacak önceden yapılandırılmış bir uygulama için tam bir Cordova projesini indirir.

2. İndirilen ZIP dosyası paketini sabit diskinizdeki bir dizine açın.

3. Projeyi **Visual Studio** kullanarak açın.  **Aç** > **Proje/Çözüm...** seçeneğine tıklayın.

4. _siteadı_.sln dosyasını bulun ve **Aç**’a tıklayın.

5. Varsayılan öykünücü olarak **Ripple - Nexus (Galaxy)** kullanılır.  Öykünücünün yanındaki açılan oka tıklayın ve **Google Android Öykünücüsü**’nü seçin.

6. **Google Android Öykünücüsü**’ne tıklayın.  Proje oluşturulup çalıştırılacaktır.  Google Android Öykünücüsü’nden gelen, ağa erişim isteyen bir ağ güvenlik uyarısı görebilirsiniz.  Sonuç olarak, Google Android Öykünücüsü gösterilecek, uygulamanız da çalıştırılacaktır.

7. Uygulamada, _Öğreticiyi tamamla_ gibi anlamlı bir metin yazın ve ardından ‘Ekle’ düğmesine basın. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)

[Azure Portal]: https://portal.azure.com/



<!--HONumber=Jun16_HO2-->


