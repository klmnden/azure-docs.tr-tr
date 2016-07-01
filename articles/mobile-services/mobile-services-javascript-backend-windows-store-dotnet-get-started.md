<properties
    pageTitle="Windows Mağazası uygulamaları (C#) için Mobile Services’ı Kullanmaya Başlama | Microsoft Azure"
    description="C#’da Windows Mağazası geliştirme için Azure Mobile Services’ı kullanmaya başlamak üzere bu öğreticiden yararlanın."
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="get-started-article" 
    ms.date="05/11/2016"
    ms.author="glenga"/>

# <a name="getting-started"> </a>Mobile Services’ı kullanmaya başlama

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Bir Windows uygulaması oluşturma](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).

Bu öğreticide, bir evrensel Windows uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Evrensel Windows uygulaması çözümleri, Windows Mağazası 8.1 ve Windows Phone Mağazası 8.1 uygulamaları ve projelerde ve ortak paylaşılan bir proje için projeleri içerir. Daha fazla bilgi için bkz. [Windows ve Windows Phone’u hedefleyen evrensel Windows uygulamaları oluşturma](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx).

Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir *Yapılacaklar listesi* uygulaması oluşturacaksınız.  Oluşturacağınız mobil hizmet, sunucu tarafı iş mantığı için JavaScript kullanır. Visual Studio kullanarak desteklenen .NET dillerinde sunucu tarafı iş mantığınızı yazmanızı sağlayan bir mobil hizmeti oluşturmak için bu konunun .NET arka uç sürümüne bakın.

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil hizmet edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F).
* [Windows için Visual Studio 2013 Express]

## Yeni bir mobil hizmet oluşturma

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Yeni bir Evrensel Windows uygulaması oluşturma

Mobil hizmetinizi oluşturduktan sonra yeni bir evrensel Windows uygulaması oluşturmak veya mevcut bir Windows Mağazası veya Windows Phone uygulama projesini mobil hizmetinize bağlamak üzere değiştirmek için klasik Azure portalındaki kolay bir hızlı başlangıcı izleyebilirsiniz.

Bu bölümde, mobil hizmetinize bağlanan yeni bir evrensel Windows uygulaması oluşturacaksınız.

1.  [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.


2. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Windows**’a tıklayın ve **Yeni Windows Mağazası uygulaması oluştur** seçeneğini genişletin.

    Burada, mobil hizmetinize bağlanan bir Windows Mağazası uygulaması oluşturmanın üç kolay adımı gösterilmiştir.

    ![Mobile Services hızlı başlangıç adımları](./media/mobile-services-javascript-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. Henüz yapmadıysanız, yerel bilgisayarınıza veya sanal makinenize [Windows için Visual Studio 2013 Express]’i indirin ve yükleyin.

4. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItem tablosu oluştur**’a tıklayın.

5. **Uygulamanızı indirme ve çalıştırma** altında uygulamanız için bir dil seçin ve sonra **İndir**’e tıklayın.

    Bu, mobil hizmetinize bağlanan örnek için *Yapılacaklar listesi* uygulaması için projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Windows uygulamanızı çalıştırma

[AZURE.INCLUDE [mobile-services-javascript-backend-run-app](../../includes/mobile-services-javascript-backend-run-app.md)]

>[AZURE.NOTE]Sorgulamak ve MainPage.xaml.cs dosyasında bulunan verileri eklemek için, mobil hizmetinize erişen kodu gözden geçirebilirsiniz.

## Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* [Çevrimdışı veri eşitlemeye başlama] Uygulamanızı esnek ve sağlam hale getirmek için çevrimdışı veri eşitlemeyi nasıl kullanacağınızı öğrenin.

* [Mobile Services uygulamanıza kimlik doğrulaması ekleme][Kimlik doğrulamayı kullanmaya başlama]  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.

* [Uygulamanıza anında iletme bildirimleri ekleme][Anında iletme bildirimlerini kullanmaya başlama]  
  Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.

* [.NET istemci kitaplığını kullanma](mobile-services-dotnet-how-to-use-client-library.md)  
 Mobil hizmeti sorgulama, verilerle çalışma ve özel API'lere erişim hakkında bilgi edinin.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Mobile Services’ı kullanmaya başlama]:#getting-started
[Yeni bir mobil hizmet oluşturma]:#create-new-service
[Mobil hizmet örneği tanımlama]:#define-mobile-service-instance
[Sonraki Adımlar]:#next-steps

<!-- Images. -->



<!-- URLs. -->
[Çevrimdışı veri eşitlemeye başlama]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[Kimlik doğrulamayı kullanmaya başlama]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-users.md
[Anında iletme bildirimlerini kullanmaya başlama]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md
[Windows için Visual Studio 2013 Express]: http://go.microsoft.com/fwlink/?LinkId=257546
[Mobile Services SDK’sı]: http://go.microsoft.com/fwlink/?LinkId=257545
[Klasik Azure Portalı]: https://manage.windowsazure.com/
 


<!--HONumber=Jun16_HO2-->


