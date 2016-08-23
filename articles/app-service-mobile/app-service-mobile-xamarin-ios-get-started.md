<properties
    pageTitle="Xamarin iOS uygulamaları için Azure App Service Mobile Apps’i Kullanmaya Başlama | Microsoft Azure"
    description="Xamarin.iOS geliştirme için Mobile Apps kullanmaya başlamak için bu öğreticiyi izleyin."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/28/2016"
    ms.author="normesta"/>


#Yeni bir Xamarin.iOS uygulaması oluşturma

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Genel Bakış

Bu öğreticide, bir Xamarin.iOS mobil uygulamasına Azure mobil uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir.  Yeni bir mobil uygulama arka ucu ve uygulama verilerini Azure’da depolayan basit bir _Yapılacaklar listesi_ Xamarin.iOS uygulaması oluşturacaksınız.

Bu öğreticiyi tamamlamak, Azure App Service’de Mobile Apps özelliğini kullanmayla ilgili diğer tüm Xamarin.iOS öğreticileri için ön koşuldur.

##Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/).

* Xamarin ile Visual Studio. Yönergeler için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](https://msdn.microsoft.com/library/mt613162.aspx).

* Xcode v7.0 veya daha sonraki sürümü ve Xamarin Studio Community yüklü bir Mac. Bkz. [Visual Studio ve Xamarin için kurulum ve yükleme](https://msdn.microsoft.com/library/mt613162.aspx) ve [Mac kullanıcıları için kurulum, yükleme ve doğrulamalar](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE] Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak istiyorsanız [App Service’i Deneyin](https://tryappservice.azure.com/?appServiceName=mobile)’e gidin. Burada, App Services’de hemen bir kısa süreli başlangıç mobil uygulaması oluşturabilirsiniz; kredi kartı gerekmez ve hiçbir taahhüt yoktur.

## Yeni bir Azure Mobil Uygulama arka ucu oluşturma

Yeni Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## Sunucu projesi yapılandırma

Artık, mobil istemci uygulamalarınız tarafından kullanılabilecek bir Azure Mobil Uygulama arka ucu sağladınız. Sonra, basit bir "yapılacaklar listesi" arka ucu için bir sunucu projesi indirecek ve Azure’a yayımlayacaksınız.

Node.js veya .NET arka ucu kullanmak için sunucu projesi yapılandırmak üzere aşağıdaki adımları izleyin.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]


## (İsteğe bağlı) Arka uç projeniz yerel olarak test etme

Yukarıda .NET arka ucu yapılandırmasını seçtiyseniz, arka ucu isteğe bağlı olarak yerel şekilde test edebilirsiniz.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-test-local-service](../../includes/app-service-mobile-dotnet-backend-test-local-service.md)]



## Xamarin iOS uygulamasını indirme ve çalıştırma

1. Bir tarayıcı penceresine [Azure portal]’ı açın.

2. Mobil Uygulamanızın dikey penceresinde, **Kullanmaya Başlama** > **Xamarin.iOS**.’a tıklayın. 3. Adım altında, henüz seçili değilse, **Yeni uygulama oluştur**’u seçin.  Sonra **İndir** düğmesine tıklayın.

    Bu, mobil uygulamanıza bağlı olan istemci uygulaması içeren bir projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

3. İndirdiğiniz projeyi çıkarın ve Xamarin Studio veya Visual Studio'da açın.

    ![][9]

    ![][8]

4. F5 tuşuna basarak projeyi oluşturun ve uygulamayı iPhone öykünücüsünde başlatın.

5. Uygulamada, _Xamarin öğren_ gibi anlamlı bir metin yazın ve ardından **+** düğmesine tıklayın.

    ![][10]

    Bu, Azure üzerinde barındırılan yeni mobil uygulama arka ucuna bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

>[AZURE.NOTE]Sorgulamak ve QSTodoService.cs C# dosyasına veri eklemek için, mobil uygulamanızın arka ucuna erişen kodu gözden geçirebilirsiniz.

##Sonraki adımlar

* [Uygulamanıza kimlik doğrulaması ekleme ](app-service-mobile-xamarin-ios-get-started-users.md)
  <br/>Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı kullanarak nasıl doğrulayacağınızı öğrenin.

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-ios-get-started-push.md)
  <br/>Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.

<!-- Anchors. -->
[Mobil uygulama arka ucu kullanmaya başlama]:#getting-started
[Yeni bir mobil uygulama arka ucu oluşturma]:#create-new-service
[Sonraki Adımlar]:#next-steps



<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/



<!--HONumber=Aug16_HO1-->


