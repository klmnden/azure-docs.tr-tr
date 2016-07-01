
<properties
    pageTitle="Android uygulamaları için Azure Mobile Services’ı Kullanmaya Başlama"
    description="Android geliştirme için Azure Mobile Services’ı kullanmaya başlamak için bu öğreticiden yararlanın."
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="03/05/2016"
    ms.author="ricksal"/>


# <a name="getting-started"> </a>Mobile Services’ı kullanmaya başlama

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Azure Mobile Apps’te bir Android uygulaması oluşturma](../app-service-mobile/app-service-mobile-android-get-started.md).

Bu öğreticide, bir Android uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir _Yapılacaklar listesi_ uygulaması oluşturacaksınız.  Oluşturacağınız mobil hizmet sunucu tarafı iş mantığı ve mobil hizmeti yönetmek için Visual Studio’yu kullanarak desteklenen .NET dillerini kullanır. JavaScript'te, sunucu tarafı iş mantığınızı yazmanıza olanak veren bir mobil hizmet oluşturmak için, bu konudaki [JavaScript arka uç sürümü](mobile-services-android-get-started.md) bölümüne bakın.

Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![](./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png)

Bu öğreticiyi tamamlamak için Android Studio tümleşik geliştirme ortamını ve en son Android platformunu içeren [Android Geliştirici Araçları][Android Studio] gerekir. Android 4.2 veya sonraki bir sürümü gereklidir.

İndirilen hızlı başlangıç projesi Android için Mobile Services SDK’sı içerir.

> [AZURE.IMPORTANT] Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil hizmet edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28).


## <a name="create-new-service"> </a>Yeni bir mobil hizmet oluşturma

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Mobil hizmeti yerel bilgisayarınıza indirin

Mobil hizmet oluşturdunuz, yerel bilgisayarınızda veya sanal makine üzerinde çalıştırabileceğiniz kişiselleştirilmiş mobil hizmet projenizi indirin.

1. Oluşturduğunuz mobil hizmete tıklayın, sonra hızlı başlangıç sekmesinde, **Platform seçin** altında **Android**’e tıklayın ve **Yeni Android uygulaması oluştur** seçeneğini genişletin.

    ![][1]

2. Önceden yapmadıysanız, [Visual Studio Professional 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) veya sonraki bir sürümünü indirin ve yükleyin.

3. 2. Adımda, **Hizmetinizi buluta indirme yayımlama** altında **İndir**’e tıklayın.

    Bu, mobil hizmetinizi uygulayan Visual Studio projesini indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Mobil hizmeti test etme

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Mobil hizmetinizi yayımlama

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Yeni bir Android uygulaması oluşturma

Bu bölümde, mobil hizmetinize bağlanan yeni bir Android uygulaması oluşturacaksınız.

1. [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.

2. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Android**’e tıklayın ve **Yeni Android uygulaması oluştur** seçeneğini genişletin.

    ![][2]

3. Henüz yapmadıysanız, yerel bilgisayarınıza veya sanal makinenize [Android Geliştirici Araçları][Android SDK]’nı indirin ve yükleyin.

4. **Uygulamanızı indirme ve çalıştırma** altında **İndir**’e tıklayın.

    Bu, mobil hizmetinize bağlanan örnek için _Yapılacaklar listesi_ uygulaması için projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Android uygulamanızı çalıştırma

[AZURE.INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]

## <a name="next-steps"> </a>Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* [Uygulamanıza anında iletme bildirimleri ekleme]
  <br/>Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.

* [Uygulamanıza kimlik doğrulaması ekleme]
  <br/>Arka uç verilerinizin erişimini, uygulamanızın belirli kayıtlı kullanıcılarına kısıtlamayı öğrenin.

* [Mobile Services .NET arka uç sorunlarını giderme]
  <br/> Mobile Services .NET arka uçta ortaya çıkabilecek sorunları tanılamayı ve düzeltmeyi öğrenin.

<!-- Anchors. -->
[Mobile Services’ı kullanmaya başlama]:#getting-started
[Yeni bir mobil hizmet oluşturma]:#create-new-service
[Mobil hizmet örneği tanımlama]:#define-mobile-service-instance
[Sonraki Adımlar]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png
[1]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-vs-AS.png
[2]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android-AS.png


[6]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-portal-quickstart-android.png
[7]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android.png
[8]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-eclipse-quickstart.png

[10]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-browse.png

[14]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-workspace.png
[15]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-project.png

<!-- URLs. -->
[Kullanmaya başlama (Eclipse)]: mobile-services-dotnet-backend-android-get-started-ec.md
[Uygulamanıza anında iletme bildirimleri ekleme]: mobile-services-dotnet-backend-android-get-started-push.md
[Uygulamanıza kimlik doğrulaması ekleme]: mobile-services-dotnet-backend-android-get-started-auth.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Android Studio]: https://developer.android.com/sdk/index.html
[Mobile Services Android SDK’sı]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Mobile Services .NET arka uç sorunlarını giderme]: mobile-services-dotnet-backend-how-to-troubleshoot.md

[Klasik Azure Portalı]: https://manage.windowsazure.com/



<!--HONumber=Jun16_HO2-->


