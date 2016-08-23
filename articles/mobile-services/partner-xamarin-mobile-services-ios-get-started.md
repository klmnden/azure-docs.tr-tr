<properties
    pageTitle="Xamarin iOS uygulamaları için Mobile Services’ı Kullanmaya Başlama | Microsoft Azure"
    description="Xamarin iOS geliştirme için Azure Mobile Services’ı kullanmaya başlamak için bu öğreticiden yararlanın."
    services="mobile-services"
    documentationCenter="xamarin"
    authors="conceptdev"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="07/21/2016"
    ms.author="craig.dunn@xamarin.com"/>

# <a name="getting-started"> </a>Mobile Services’ı kullanmaya başlama
[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Xamarin.iOS uygulaması oluşturma](../app-service-mobile/app-service-mobile-xamarin-ios-get-started.md).

Bu öğreticide, bir Xamarin.iOS uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilmiştir. Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir *Yapılacaklar listesi* uygulaması oluşturacaksınız. 

Bir video izlemeyi tercih ederseniz, aşağıdaki klip de bu öğretici ile aynı adımları izlemektedir.

Video: Xamarin geliştirici destekçisi Craig Dunn ile "Xamarin ve Azure Mobile Services’i Kullanmaya Başlama" (süre: 10:05 dk)

> [AZURE.VIDEO getting-started-with-xamarin-and-mobile-services]

Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![][0]

Bu öğreticinin tamamlanması OS X için XCode ve Xamarin Studio veya ağa bağlı bir Mac bilgisayardaki Windows işletim sisteminde Visual Studio gerektirir. Tam yükleme yönergeleri [Visual Studio ve Xamarin için Kurulum ve Yükleme](https://msdn.microsoft.com/library/mt613162.aspx) bölümünde bulunabilir. 

> [AZURE.IMPORTANT] Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil hizmet edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-new-service"> </a>Yeni bir mobil hizmet oluşturma

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Yeni bir Xamarin.iOS uygulaması oluşturma

Mobil hizmetinizi oluşturduktan sonra yeni bir uygulama oluşturmak veya mevcut bir uygulamayı mobil hizmetinize bağlamak üzere değiştirmek için Klasik Azure Portalı’ndaki kolay bir hızlı başlangıcı izleyebilirsiniz.

Bu bölümde, mobil hizmetinize bağlanan yeni bir Xamarin.iOS uygulaması oluşturacaksınız.

1.  [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.

2. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Xamarin.iOS**’a tıklayın ve **Yeni Xamarin.iOS uygulaması oluştur** seçeneğini genişletin.

    ![][6]

    Burada, mobil hizmetinize bağlanan bir Xamarin.iOS uygulaması oluşturmanın üç kolay adımı gösterilmiştir.

    ![][7]

3. Önceden yapmadıysanız Xcode (en son sürüm Xcode 6.0 veya daha yeni önerilir) ve [Xamarin Studio]’yu indirip yükleyin.

4. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItems tablosu oluştur**’a tıklayın.

5. **Uygulama indirme ve çalıştırma** altında **İndir**’e tıklayın.

    Bunun yapılması, mobil hizmetinize bağlı olan ve Xamarin.iOS için Azure Mobile Services bileşenine başvuran örnek _Yapılacaklar listesi_ uygulaması için projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Yeni Xamarin.iOS uygulamanızı çalıştırma

Bu öğreticinin son aşaması yeni uygulamanızı oluşturmak ve çalıştırmaktır.

1. Sıkıştırılmış proje dosyalarını kaydettiğiniz konuma göz atın, bilgisayarınızdaki dosyaları genişletin ve Xamarin Studio veya Visual Studio kullanarak **XamarinTodoQuickStart.iOS.sln** çözüm dosyasını açın.

    ![][8]

    ![][9]

2. **Çalıştır** düğmesine basarak projeyi oluşturun ve uygulamayı bu projenin varsayılan seçeneği olan iPhone öykünücüsünde başlatın.

3. Uygulamada, _Öğreticiyi tamamla_ gibi anlamlı bir metin yazın ve ardından artı (**+**) simgesine basın.

    ![][10]

    Bu, Azure üzerinde barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve veriler listede görüntülenir.

    > [AZURE.NOTE] Sorgulamak ve TodoService.cs C# dosyasında bulunan verileri eklemek için mobil hizmetinize erişen kodu gözden geçirebilirsiniz.

4. [Klasik Azure Portalı] geri dönün, **Veri** sekmesine ve sonra **TodoItems** tablosuna tıklayın.

    ![][11]

    Bu, uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.

    ![][12]


## Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* [Çevrimdışı veri eşitlemeye başlama] Hızlı başlangıcın uygulamanızı esnek ve sağlam hale getirmek için çevrimdışı veri eşitlemeyi nasıl kullandığını öğrenin.

* [Kimlik doğrulamayı kullanmaya başlama] Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.

* [Anında iletme bildirimlerini kullanmaya başlama] Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.




[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Mobile Services’ı kullanmaya başlama]:#getting-started
[Yeni bir mobil hizmet oluşturma]:#create-new-service
[Mobil hizmet örneği tanımlama]:#define-mobile-service-instance
[Sonraki Adımlar]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-completed-ios.png
[6]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-portal-quickstart-xamarin-ios.png
[7]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-steps-xamarin-ios.png
[8]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-xs.png
[9]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-vs.png
[10]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Çevrimdışı veri eşitlemeye başlama]: mobile-services-xamarin-ios-get-started-offline-data.md
[Kimlik doğrulamayı kullanmaya başlama]: partner-xamarin-mobile-services-ios-get-started-users.md
[Anında iletme bildirimlerini kullanmaya başlama]: partner-xamarin-mobile-services-ios-get-started-push.md

[Xamarin Studio]: http://xamarin.com/download
[Mobile Services iOS SDK’sı]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[Klasik Azure Portalı]: https://manage.windowsazure.com/



<!--HONumber=Aug16_HO1-->


