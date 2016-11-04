---
title: Xamarin.Android için Mobile Services’ı Kullanmaya Başlama | Microsoft Docs
writer: craigd
description: Azure Mobile Services’i Xamarin.Android uygulamanız ile kullanma hakkında bilgi alın.
documentationcenter: xamarin
author: lindydonna
manager: dwrede
editor: ''
services: mobile-services

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/21/2016
ms.author: donnam

---
# <a name="getting-started"></a>Mobile Services’ı kullanmaya başlama
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Xamarin.Android uygulaması oluşturma](../app-service-mobile/app-service-mobile-xamarin-android-get-started.md).
> 
> 

Bu öğreticide, bir Xamarin.Android uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilmiştir. Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir *Yapılacaklar listesi* uygulaması oluşturacaksınız. 

Bir video izlemeyi tercih ederseniz, aşağıdaki klip de bu öğretici ile aynı adımları izlemektedir.

Video: Xamarin geliştirici destekçisi Craig Dunn ile "Xamarin ve Azure Mobile Services’i Kullanmaya Başlama" (süre: 10:05 dk)

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Getting-Started-with-Xamarin-and-Windows-Azure-Mobile-Services/player]
> 
> 

Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![][0]

Bu öğreticinin tamamlanması OS X için XCode ve Xamarin Studio veya ağa bağlı bir Mac bilgisayardaki Windows işletim sisteminde Visual Studio gerektirir. Tam yükleme yönergeleri [Visual Studio ve Xamarin için Kurulum ve Yükleme](https://msdn.microsoft.com/library/mt613162.aspx) bölümünde bulunabilir. 

İndirilen hızlı başlangıç projesi Xamarin.Android için Azure Mobile Services bileşenini içerir. Bu proje Android 4.2 veya sonraki bir sürümü hedeflese de Mobile Services SDK’sı yalnızca Android 2.2 veya sonraki bir sürümünü gerektirir.

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil hizmet edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5).
> 
> 

## <a name="create-new-service"> </a>Yeni bir mobil hizmet oluşturma
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Yeni bir Xamarin.Android uygulaması oluşturma
Mobil hizmetinizi oluşturduktan sonra yeni bir uygulama oluşturmak veya mevcut bir uygulamayı mobil hizmetinize bağlamak üzere değiştirmek için Klasik Azure Portalı’ndaki kolay bir hızlı başlangıcı izleyebilirsiniz.

Bu bölümde, mobil hizmetinize bağlanan yeni bir Xamarin.Android uygulaması oluşturacaksınız.

1. [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.
2. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Xamarin.Android**’e tıklayın ve **Yeni Android uygulaması oluştur** seçeneğini genişletin.
   
    ![][6]
   
    Burada, mobil hizmetinize bağlanan bir Xamarin.Android uygulaması oluşturmanın üç kolay adımı gösterilmiştir.
   
    ![][7]
3. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItem tablosu oluştur**’a tıklayın.
4. **Uygulama indirme ve çalıştırma** altında **İndir**’e tıklayın.
   
    Bu, mobil hizmetinize bağlanan örnek için *Yapılacaklar listesi* uygulaması için projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Android uygulamanızı çalıştırma
Bu öğreticinin son aşaması yeni uygulamanızı oluşturmak ve çalıştırmaktır.

1. Sıkıştırılmış proje dosyalarını kaydettiğiniz konuma göz atın ve bilgisayarınızdaki dosyaları genişletin.
2. Xamarin Studio veya Visual Studio’da **Dosya** ve ardından **Aç**’a tıklayın, sıkıştırılmamış örnek dosyalara gidin ve **XamarinTodoQuickStart.Android.sln** dosyasını seçerek açın.
3. Projeyi oluşturmak ve uygulamayı başlatmak için **Çalıştır** düğmesine basın. Bir öykünücü veya bağlı bir USB cihazını seçmeniz istenir.
   
   > [!NOTE]
   > Android öykünücüsünde projeyi çalıştırılabilmek için en az bir Android Sanal Cihazı (AVD) tanımlamanız gerekir. Bu cihazları oluşturmak ve yönetmek için AVD Yöneticisi’ni kullanın.
   > 
   > 
4. Uygulamada, *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından **Ekle** seçeneğine tıklayın.
   
    ![][10]
   
    Bu, Azure üzerinde barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve veriler listede görüntülenir.
   
   > [!NOTE]
   > Sorgulamak ve ToDoActivity.cs C# dosyasında bulunan verileri eklemek için, mobil hizmetinize erişen kodu gözden geçirebilirsiniz.
   > 
   > 
5. [Klasik Azure Portalı] geri dönün, **Veri** sekmesine ve sonra **TodoItems** tablosuna tıklayın.
   
    ![][11]
   
    Bu, uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.
   
    ![][12]

## <a name="next-steps"> </a>Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* [Çevrimdışı veri eşitlemeye başlama] Hızlı başlangıcın uygulamanızı esnek ve sağlam hale getirmek için çevrimdışı veri eşitlemeyi nasıl kullandığını öğrenin.
* [Kimlik doğrulamayı kullanmaya başlama] Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Anında iletme bildirimlerini kullanmaya başlama] Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Mobile Services’ı kullanmaya başlama]:#getting-started
[Yeni bir mobil hizmet oluşturma]:#create-new-service
[Mobil hizmet örneği tanımlama]:#define-mobile-service-instance
[Sonraki Adımlar]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-completed-android.png
[2]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create.png
[3]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create-page1.png
[4]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create-page2.png
[5]: ./media/partner-xamarin-mobile-services-android-get-started/obile-services-selection.png
[6]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-portal-quickstart-xamarin-android.png
[7]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-steps-xamarin-android.png
[8]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-xamarin-project-android-xs.png
[9]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-xamarin-project-android-vs.png
[10]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-data-tab.png
[12]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-data-browse.png
[13]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-services-diagram.png


<!-- URLs. -->
[Verileri kullanmaya başlama]: /develop/mobile/tutorials/get-started-with-data-xamarin-android
[Çevrimdışı veri eşitlemeye başlama]: mobile-services-xamarin-android-get-started-offline-data.md
[Kimlik doğrulamayı kullanmaya başlama]: /develop/mobile/tutorials/get-started-with-users-xamarin-android
[Anında iletme bildirimlerini kullanmaya başlama]: /develop/mobile/tutorials/get-started-with-push-xamarin-android
[Mobile Services Android SDK’sı]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Azure]: http://azure.microsoft.com/
[Klasik Azure Portalı]: https://manage.windowsazure.com/




<!--HONumber=Aug16_HO1-->


