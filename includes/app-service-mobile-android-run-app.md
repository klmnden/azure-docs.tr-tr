
1. [Azure Portal]’ı ziyaret edin. **Tümüne Gözat** > **Mobile Apps** > henüz oluşturmuş olduğunuz arka uca tıklayın. Mobil uygulama ayarlarında, **Hızlı Başlangıç** > **Android** öğesine tıklayın. **İstemci uygulamanızı yapılandırın**’ın altında **İndir**’e tıklayın. Arka ucunuza bağlanacak önceden yapılandırılmış bir uygulama için tam bir Android projesini indirir. 
2. **Android Studio** kullanarak, **Projeyi içeri aktar (Eclipse ADT, Gradle, vb.)** kullanarak projeyi açın. JDK hatalarını önlemek için bu içeri aktarma seçimini yaptığınızdan emin olun.
3. Projeyi oluşturmak için **’Uygulamayı’ çalıştır** düğmesine basın ve uygulamayı Android benzeticisinde başlatın.
4. Uygulamada, *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından ‘Ekle’ düğmesine basın. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler. 
   
    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure Portal]: https://portal.azure.com/
