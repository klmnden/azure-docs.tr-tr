<properties
    pageTitle="Xamarin iOS uygulamaları için Mobile Services’ı Kullanmaya Başlama | Microsoft Azure"
    description="Xamarin iOS geliştirme için Azure Mobile Services’ı kullanmaya başlamak için bu öğreticiden yararlanın"
    services="mobile-services"
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="07/21/2016"
    ms.author="donnam"/>

# <a name="getting-started"> </a>Mobile Services’ı kullanmaya başlama

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Xamarin.iOS uygulaması oluşturma](../app-service-mobile/app-service-mobile-xamarin-ios-get-started.md).

Bu öğreticide, bir Xamarin iOS uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilmiştir. Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir _Yapılacaklar listesi_ uygulaması oluşturacaksınız. Oluşturacağınız mobil hizmet sunucu tarafı iş mantığı ve mobil hizmeti yönetmek için Visual Studio’yu kullanarak desteklenen .NET dillerini kullanır. JavaScript'te, sunucu tarafı iş mantığınızı yazmanıza olanak veren bir mobil hizmet oluşturmak için, bu konudaki [JavaScript arka uç sürümü] bölümüne bakın.

>[AZURE.NOTE]Bu konu, Klasik Azure Portalı’nı kullanarak yeni bir mobil hizmet projesi oluşturmayı gösterir. Visual Studio 2013 Update 2’yi kullanarak, varolan bir Visual Studio çözümüne yeni mobil hizmet projesi de ekleyebilirsiniz. Daha fazla bilgi için bkz. [Hızlı Başlangıç: Mobil hizmet (.NET arka uç) Ekleme](http://msdn.microsoft.com/library/windows/apps/dn629482.aspx)

Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![][0]


Bu öğreticiyi tamamlamak Xamarin iOS uygulamalarına ilişkin tüm Mobile Services öğreticileri için ön koşuldur.

>[AZURE.NOTE]Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil hizmet edinebilirsiniz. Ayrıntılar için bkz. <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-xamarin-ios-get-started" target="_blank">Azure Ücretsiz Deneme Sürümü</a>.<br />Bu öğretici <a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio Professional 2013</a> gerektirir. Ücretsiz deneme sürümü kullanılabilir.

## Yeni bir mobil hizmet oluşturma

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Yeni bir Xamarin iOS uygulaması oluşturma

Mobil hizmetinizi oluşturduktan sonra yeni bir uygulama oluşturmak veya mevcut bir uygulamayı mobil hizmetinize bağlamak üzere değiştirmek için Klasik Azure Portalı’ndaki kolay bir hızlı başlangıcı izleyebilirsiniz.

Bu bölümde, mobil hizmetiniz için yeni bir Xamarin iOS uygulaması ve hizmet projesi indireceksiniz.

1. Önceden yapmadıysanız, Visual Studio ile Xamarin'i yükleyin. Yönergeler için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](https://msdn.microsoft.com/library/mt613162.aspx). Xamarin Studio'yu bir Mac OS X makinede de kullanabilirsiniz bkz. [Mac kullanıcıları için kurulum, yükleme ve doğrulamalar](https://msdn.microsoft.com/library/mt488770.aspx).

2. [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.

3. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Xamarin**’e tıklayın ve **Yeni Xamarin uygulaması oluştur** seçeneğini genişletin.

    ![][6]

    Burada, mobil hizmetinize bağlanan bir Xamarin iOS uygulaması oluşturmanın üç kolay adımı gösterilmiştir.

    ![][7]

4. **Hizmetinizi buluta indirme yayımlama** altında **iOS**’a ve **İndir**’e tıklayın.

    Bu, hem mobil hizmet hem de mobil hizmetinize bağlanan örnek _Yapılacaklar listesi_ uygulaması için projeleri içeren bir çözümü indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

5. Yayımlama profilinizi indirin, indirilen dosyayı yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Mobil hizmeti test etme

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Mobil hizmetinizi yayımlama

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Xamarin iOS uygulamasını çalıştırma

Bu öğreticinin son aşaması yeni uygulamanızı oluşturmak ve çalıştırmaktır.

1. Visual Studio veya Xamarin Studio'da, mobil hizmet çözümündeki istemci projesine gidin.

    ![][8]

    ![][9]

2. **Çalıştır** düğmesine basarak istemci projesinin oluşturun ve uygulamayı iPhone öykünücüsünde başlatın.

3. Uygulamada, _Öğreticiyi tamamla_ gibi anlamlı bir metin yazın ve ardından artı (**+**) simgesine basın.

    ![][10]

    Bu, Azure üzerinde barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve veriler listede görüntülenir.

>[AZURE.NOTE]Sorgulamak ve QSTodoService.cs C# dosyasına veri eklemek için, mobil hizmetinize erişen kodu gözden geçirebilirsiniz.


## Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* [Çevrimdışı veri eşitlemeye başlama]
  <br/>Uygulamayı esnek ve sağlam hale getirmek için çevrimdışı veri eşitlemeye nasıl hızlı başlayacağınızı öğrenin.

* [Kimlik doğrulamayı kullanmaya başlama]
  <br/>Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.

* [Anında iletme bildirimlerini kullanmaya başlama]
  <br/>Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.

* [Mobile Services .NET arka uç sorunlarını giderme]
  <br/> Mobile Services .NET arka uçta ortaya çıkabilecek sorunları tanılamayı ve düzeltmeyi öğrenin.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Mobile Services’ı kullanmaya başlama]:#getting-started
[Yeni bir mobil hizmet oluşturma]:#create-new-service
[Sonraki Adımlar]:#next-steps



<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-completed-ios.png
[6]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-portal-quickstart-xamarin-ios.png
[7]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-steps-xamarin-ios.png
[8]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Çevrimdışı veri eşitlemeye başlama]: mobile-services-xamarin-ios-get-started-offline-data.md
[Kimlik doğrulamayı kullanmaya başlama]: mobile-services-dotnet-backend-xamarin-ios-get-started-users.md
[Anında iletme bildirimlerini kullanmaya başlama]: mobile-services-dotnet-backend-xamarin-ios-get-started-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile Services SDK’sı]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript ve HTML]: mobile-services-win8-javascript/
[Klasik Azure Portalı]: https://manage.windowsazure.com/
[JavaScript arka uç sürümü]: mobile-services-ios-get-started.md
[Mobile Services .NET arka uç sorunlarını giderme]: mobile-services-dotnet-backend-how-to-troubleshoot.md


<!--HONumber=Aug16_HO1-->


