<properties
    pageTitle="Windows Evrensel uygulamaları için Mobile Services’ı Kullanmaya Başlama | Microsoft Azure"
    description="C#’da Evrensel Windows uygulama geliştirme için Azure Mobile Services’ı kullanmaya başlamak için bu öğreticiden yararlanın."
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/11/2016"
    ms.author="glenga"/>


# <a name="getting-started"> </a>Mobile Services’ı kullanmaya başlama

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Azure Mobile Apps’te bir Windows uygulaması oluşturma](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).

Bu öğreticide, bir evrensel Windows uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Evrensel Windows uygulaması çözümleri, Windows Mağazası 8.1 ve Windows Phone Mağazası 8.1 uygulamaları ve projelerde ve ortak paylaşılan bir proje için projeleri içerir. Daha fazla bilgi için bkz. [Windows ve Windows Phone’u hedefleyen evrensel Windows uygulamaları oluşturma](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx).

Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir *Yapılacaklar listesi* uygulaması oluşturacaksınız. Oluşturacağınız mobil hizmet sunucu tarafı iş mantığı ve mobil hizmeti yönetmek için Visual Studio’yu kullanarak desteklenen .NET dillerini kullanır. JavaScript'te, sunucu tarafı iş mantığınızı yazmanıza olanak veren bir mobil hizmet oluşturmak için, bu konudaki JavaScript arka uç sürümü bölümüne bakın.

>[AZURE.NOTE]Bu konu, Klasik Azure Portalı’nı kullanarak yeni bir mobil hizmet projesi ve evrensel Windows uygulaması oluşturmayı gösterir. Visual Studio 2013 Update 3’ü kullanarak, varolan bir Visual Studio çözümüne yeni mobil hizmet projesi de ekleyebilirsiniz. Daha fazla bilgi için bkz. [Varolan bir uygulamaya Mobile Services’ı ekleme](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md).

>Windows Phone 8.0 ya da Windows Phone Silverlight 8.1 projesine bir mobil hizmet eklemek için bkz. [Varolan bir Windows Phone uygulamasına Mobile Services’ı ekleme](mobile-services-windows-phone-get-started-data.md).

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil hizmet edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-windows-store-dotnet-get-started%2F).
* [Visual Studio 2013].

## Yeni bir mobil hizmet oluşturma

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Yeni bir Evrensel Windows uygulaması oluşturma

Mobil hizmetinizi oluşturduktan sonra yeni bir uygulama oluşturmak veya mevcut bir uygulamayı mobil hizmetinize bağlamak üzere değiştirmek için Klasik Azure Portalı’ndaki kolay bir hızlı başlangıcı izleyebilirsiniz.

Bu bölümde, mobil hizmetinize bağlanan yeni bir evrensel Windows uygulaması oluşturacaksınız.

1. [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.

2. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Windows**’a tıklayın ve **Yeni Windows Mağazası uygulaması oluştur** seçeneğini genişletin.

    Burada, mobil hizmetinize bağlanan bir Windows Mağazası uygulaması oluşturmanın üç kolay adımı gösterilmiştir.

    ![Mobile Services hızlı başlangıç adımları](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. Henüz yapmadıysanız, yerel bilgisayarınıza veya sanal makinenize [Visual Studio 2013]’ü indirin ve yükleyin.

4. **Uygulamanızı ve hizmetinizi yerel olarak indirme ve çalıştırma** altında, Windows Mağazası uygulamanız için bir dil seçin ve sonra **İndir**’e tıklayın.

    Bu, hem mobil hizmet hem de mobil hizmetinize bağlanan örnek _Yapılacaklar listesi_ uygulaması için projeleri içeren bir çözümü indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Uygulamayı yerel mobil hizmete karşı test etme

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-dotnet](../../includes/mobile-services-dotnet-backend-test-local-service-dotnet.md)]

>[AZURE.NOTE]Sorgulamak ve MainPage.xaml.cs dosyasında bulunan verileri eklemek için, mobil hizmetinize erişen kodu gözden geçirebilirsiniz.


## Mobil hizmetinizi yayımlama

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]


<ol start="4">
<li><p>Paylaşılan kod projesinde, App.xaml.cs dosyasını açın, bir <a href="http://msdn.microsoft.com/library/Windowsazure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx" target="_blank">MobileServiceClient</a> örneği oluşturan kodu bulun, <em>localhost</em>’u kullanarak bu istemciyi oluşturan koda açıklama ekleyin ve aşağıdaki gibi görünen uzak mobil hizmet URL’sini kullanarak istemciyi oluşturan kodun açıklamasını kaldırın:</p>

        <pre><code>public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://todolist.azure-mobile.net/",
            "XXXX-APPLICATION-KEY-XXXXX");</code></pre>

    <p>The client will now access the mobile service published to Azure.</p></li>
</ol>

## Uygulamayı Azure’da barındırılan yerel mobil hizmete karşı test etme

Mobil hizmet yayımlandığına ve istemci Azure’da barındırılan uzak mobil hizmete bağlandığına göre, öğe depolamak için Azure’u kullanarak uygulamayı çalıştırabiliriz.

[AZURE.INCLUDE [mobile-services-windows-universal-test-app](../../includes/mobile-services-windows-universal-test-app.md)]


## Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

*  [Varolan bir uygulamaya Mobile Services’ı ekleme][Veri kullanmaya başlama].
  <br/>Mobile Services’ı kullanarak verileri depolama ve sorgulama hakkında daha fazla bilgi edinin.

* [Çevrimdışı veri eşitlemeye başlama]
  <br/>Uygulamanızı esnek ve sağlam hale getirmek için çevrimdışı veri eşitlemeyi nasıl kullanacağınızı öğrenin.

* [Mobile Services uygulamanıza kimlik doğrulaması ekleme][Kimlik doğrulamayı kullanmaya başlama]
  <br/>Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.

* [Uygulamanıza anında iletme bildirimleri ekleme][Anında iletme bildirimlerini kullanmaya başlama]
  <br/>Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.

* [Mobile Services .NET arka uç sorunlarını giderme]
  <br/> Mobile Services .NET arka uçta ortaya çıkabilecek sorunları tanılamayı ve düzeltmeyi öğrenin.

Evrensel Windows uygulamaları hakkında daha fazla bilgi için bkz. [tek bir mobil hizmette birden çok cihaz platformunu destekleme](mobile-services-how-to-use-multiple-clients-single-service.md#shared-vs).

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->

<!-- Images. -->



<!-- URLs. -->
[Visual Studio 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Veri kullanmaya başlama]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md
[Çevrimdışı veri eşitlemeye başlama]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[Kimlik doğrulamayı kullanmaya başlama]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md
[Anında iletme bildirimlerini kullanmaya başlama]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile Services SDK’sı]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript ve HTML]: mobile-services-win8-javascript/
[Klasik Azure Portalı]: https://manage.windowsazure.com/
[Mobile Services .NET arka uç sorunlarını giderme]: mobile-services-dotnet-backend-how-to-troubleshoot.md



<!--HONumber=Jun16_HO2-->


