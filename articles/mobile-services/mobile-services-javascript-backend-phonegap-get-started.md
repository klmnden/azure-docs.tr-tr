<properties
    pageTitle="PhoneGap/cordova uygulamaları için Azure Mobile Services kullanmaya başlama | Microsoft Azure"
    description="iOS, Android ve Windows Phone’da PhoneGap geliştirme için Azure Mobile Services’ı kullanmaya başlamak üzere bu öğreticiden yararlanın."
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="multiple"
    ms.topic="get-started-article" 
    ms.date="02/10/2016"
    ms.author="ggailey777"/>

# Mobile Services’ı kullanmaya başlama

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

Bu öğreticide bir uygulamaya Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilmiştir. Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir _Yapılacaklar listesi_ uygulaması oluşturacaksınız. 

Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![][3]

### Ek Gereksinimler

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

+ PhoneGap araçları (Windows Phone 8 projeleri için v3.2+ gereklidir).

+ Etkin bir Microsoft Azure hesabı.

+ PhoneGap birden çok platform için geliştirmeyi destekler. PhoneGap araçlarına ek olarak, hedeflediğiniz her bir platforma yönelik araçları yüklemeniz gerekir:

    - Windows Phone: [Windows Phone için Visual Studio 2012 Express](https://go.microsoft.com/fwLink/p/?LinkID=268374)’i yükleyin
    - iOS: [Xcode] yükleyin (v4.4+ gerekli)
    - Android: [Android Geliştirici Araçları][Android SDK]’sını yükleyin
        <br/>(Android için Mobile Services SDK’sı Android 2.2 veya sonraki bir sürümünü destekler. Hızlı başlangıç uygulamasını çalıştırmak için Android 4.2 veya üzeri gerekir.)

## Yeni bir mobil hizmet oluşturma

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Yeni PhoneGap uygulaması oluşturma

Bu bölümde, mobil hizmetinize bağlanan yeni bir PhoneGap uygulaması oluşturacaksınız.

1.  [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.

2. Hızlı başlangıç sekmesindeki **Platform seçin** altında **PhoneGap**’e tıklayın ve **Yeni PhoneGap uygulaması oluştur** seçeneğini genişletin.

    ![][0]

    Burada, mobil hizmetinize bağlanan bir PhoneGap uygulaması oluşturmanın üç kolay adımı gösterilir.

    ![][1]

3. Henüz yapmadıysanız PhoneGap’i ve platform geliştirme araçlarından en az birini (Windows Phone, iOS veya Android) indirip yükleyin.

4. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItem tablosu oluştur**’a tıklayın.

5. **Uygulamanızı indirme ve çalıştırma** altında **İndir**’e tıklayın.

    Bunun yapılması, Mobile Services JavaScript SDK’sı ile birlikte mobil hizmetinize bağlanan örnek bir _Yapılacaklar listesi_ uygulaması için projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Yeni PhoneGap uygulamanızı çalıştırma

Bu öğreticinin son aşaması yeni uygulamanızı oluşturmak ve çalıştırmaktır.

1.  Sıkıştırılmış proje dosyalarını kaydettiğiniz konuma göz atın ve bilgisayarınızdaki dosyaları genişletin.

2.  Projeyi her bir platform için aşağıdaki yönergelere uygun olarak açın ve çalıştırın.

    + **Windows Phone 8**

        1. Windows Phone 8: Windows Phone için Visual Studio 2012 Express’teki **platforms\wp8** klasöründe .sln dosyasını açın.

        2. Projeyi oluşturmak ve uygulamayı başlatmak için **F5** tuşuna basın.

        ![][2]

    + **iOS**

        1. Projeyi Xcode’daki **platforms/ios** klasöründe açın.

        2. **Çalıştır** düğmesine basarak projeyi oluşturun ve uygulamayı bu projenin varsayılan seçeneği olan iPhone öykünücüsünde başlatın.

        ![][3]

    + **Android**

        1. Eclipse’te **Dosya** ve ardından **İçeri Aktar**’a tıklayın, **Android**’i genişletin, **Çalışma Alanına Var Olan Android Kodu** ve ardından **İleri** seçeneğine tıklayın.

        2. **Gözat**’a tıklayın, genişletilmiş proje dosyalarının konumuna göz atın, **Tamam**’a tıklayın, TodoActivity projesinin işaretlendiğinden emin olun ve ardından **Son**’a tıklayın. <p>Bu işlem proje dosyalarını geçerli çalışma alanına aktarır.</p>

        3. **Çalıştır** menüsünde **Çalıştır** seçeneğine tıklayarak projeyi Android öykünücüsünde başlatın.

            ![][4]

        >[AZURE.NOTE]Android öykünücüsünde projeyi çalıştırılabilmek için en az bir Android Sanal Cihazı (AVD) tanımlamanız gerekir. Bu cihazları oluşturmak ve yönetmek için AVD Yöneticisi’ni kullanın.


3. Uygulamayı yukarıdaki mobil öykünücülerinden birinde başlattıktan sonra metin kutusuna bir metin yazın ve ardından **Ekle**’ye tıklayın.

    Bu, Azure üzerinde barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekten alınan veriler **TodoItem** tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve veriler listede görüntülenir.

    > [AZURE.IMPORTANT] Ana proje PhoneGap araçları ile yeniden oluşturulursa bu platform projesinde yapılan değişikliklerin üzerine yazılır. Bunun yerine, projenin kök www dizininde aşağıdaki bölümde açıklanan değişiklikleri yapın.

4. [Klasik Azure Portalı] geri dönün, **Veri** sekmesine ve sonra **TodoItem** tablosuna tıklayın.

    ![](./media/mobile-services-javascript-backend-phonegap-get-started/mobile-data-tab.png)

    Bu, uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.

    ![](./media/mobile-services-javascript-backend-phonegap-get-started/mobile-data-browse.png)


## Her bir platform için uygulama güncelleştirmeleri yapma ve projeleri yeniden oluşturma

1. Bu örnekte ´todolist/www´ olan ´www´ dizinindeki kod dosyalarında değişiklikler yapın.

2. Tüm hedef platform araçlarına sistem yolundan erişilebildiğini doğrulayın.

2. Kök proje dizininde bir komut istemi açın ve platforma özgü aşağıdaki komutlardan birini çalıştırın:

    + **Windows Phone**

        Visual Studio Geliştirici komut isteminden aşağıdaki komutu çalıştırın:

            phonegap local build wp8

    + **iOS**

        Terminali açın ve aşağıdaki komutu çalıştırın:

            phonegap local build ios

    + **Android**

        Bir komut istemi veya terminal penceresi açın ve aşağıdaki komutu çalıştırın.

            phonegap local build android

4. Her bir projeyi önceki bölümde açıklandığı gibi uygun geliştirme ortamında açın.

>[AZURE.NOTE]Sorgulamak ve js/index.js dosyasında bulunan verileri eklemek için, mobil hizmetinize erişen kodu gözden geçirebilirsiniz.

## Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* **[Uygulamanıza kimlik doğrulaması ekleme]**  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.  

* **[Uygulamanıza anında iletme bildirimleri ekleme](https://msdn.microsoft.com/magazine/dn879353.aspx)**  
  Uygulamanıza kaydolma ve anında iletme bildirimleri gönderme hakkında bilgi edinin.

* **[Mobile Services HTML/JavaScript Kavramsal Başvurusu Nasıl Yapılır?](mobile-services-html-how-to-use-client-library.md)**  
  Verilere erişmek, özel API’leri çağırmak ve kimlik doğrulaması gerçekleştirmek için JavaScript istemci kitaplığını kullanma hakkında bilgi edinin.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Images. -->
[0]: ./media/mobile-services-javascript-backend-phonegap-get-started/portal-screenshot1.png
[1]: ./media/mobile-services-javascript-backend-phonegap-get-started/portal-screenshot2.png
[2]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-wp8.png
[3]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-ios.png
[4]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-android.png

<!-- URLs. -->
[Uygulamanıza kimlik doğrulaması ekleme]: mobile-services-html-get-started-users.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Klasik Azure Portalı]: https://manage.windowsazure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Windows Phone için Visual Studio 2012 Express]: https://go.microsoft.com/fwLink/p/?LinkID=268374
 


<!--HONumber=Jun16_HO2-->


