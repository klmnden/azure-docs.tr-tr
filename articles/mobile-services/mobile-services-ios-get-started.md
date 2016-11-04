---
title: iOS uygulamaları için Azure Mobile Services’ı kullanmaya başlama | Microsoft Docs
description: iOS geliştirme için Azure Mobile Services’ı kullanmaya başlamak için bu öğreticiyi izleyin.
services: mobile-services
documentationcenter: ios
author: krisragh
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 07/21/2016
ms.author: krisragh

---
# <a name="getting-started"> </a>Mobile Services’ı kullanmaya başlama
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Azure Mobile Apps’de bir iOS uygulaması oluşturma](../app-service-mobile/app-service-mobile-ios-get-started.md).
> 
> 

Bu öğreticide, bir iOS uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilmiştir.

Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir *Yapılacaklar listesi* uygulaması oluşturacaksınız.  Oluşturacağınız mobil hizmet, sunucu tarafı iş mantığı için JavaScript kullanır. .NET içinde sunucu tarafı iş mantığı ile bir mobil hizmet oluşturmak için bu konunun [.NET arka uç sürümü] bölümüne bakın.

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, Azure deneme sürümüne kaydolabilir ve [deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz ücretsiz mobil hizmetler](https://azure.microsoft.com/pricing/details/mobile-services/) edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-ios%2F%20).
> 
> 

## <a name="create-new-service"> </a>Yeni bir mobil hizmet oluşturma
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Yeni bir iOS uygulaması oluşturma
Mobil hizmetinize bağlı yeni bir uygulama oluşturmak için klasik Azure portalında kolay bir Hızlı Başlangıç kılavuzunu izleyebilirsiniz:

1. [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.
2. Hızlı Başlangıç sekmesinde **Platform seçin** altındaki **iOS** seçeneğine tıklayın ve **Yeni iOS uygulaması oluştur** seçeneğini genişletin. Burada, mobil hizmetinize bağlanan bir iOS uygulaması oluşturmanın adımları gösterilir.
3. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItem tablosu oluştur**’a tıklayın.
4. **Uygulamanızı indirme ve çalıştırma** altında **İndir**’e tıklayın. Bunun yapılması, Mobile Services iOS SDK’sı ile birlikte mobil hizmetinize bağlanan örnek bir *Yapılacaklar listesi* uygulaması için projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

## Yeni iOS uygulamanızı çalıştırma
[!INCLUDE [mobile-services-ios-run-app](../../includes/mobile-services-ios-run-app.md)]

<ol start="4">

<li><p>[Klasik Azure Portalı]’na geri dönün, **VERİ** sekmesine ve sonra **TodoItem** tablosuna tıklayın. Bu, uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.<p></li></ol></p>

## <a name="next-steps"> </a>Sonraki Adımlar
Mobile Services’deki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* [Çevrimdışı veri eşitlemeye başlama]
    <br/>Uygulamanızı esnek ve sağlam hale getirmek için çevrimdışı veri eşitlemeyi nasıl kullanacağınızı öğrenin.
* [Var olan bir uygulamaya kimlik doğrulaması ekleme]
    <br/>Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Var olan bir uygulamaya anında iletme bildirimleri ekleme]
    <br/>Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Mobile Services’ı kullanmaya başlama]:#getting-started
[Yeni bir mobil hizmet oluşturma]:#create-new-service
[Mobil hizmet örneği tanımlama]:#define-mobile-service-instance
[Sonraki Adımlar]:#next-steps

<!-- Images. -->
[6]: ./media/mobile-services-ios-get-started/mobile-portal-quickstart-ios.png
[7]: ./media/mobile-services-ios-get-started/mobile-quickstart-steps-ios.png
[8]: ./media/mobile-services-ios-get-started/mobile-xcode-project.png

[10]: ./media/mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Çevrimdışı veri eşitlemeye başlama]: mobile-services-ios-get-started-offline-data.md
[Var olan bir uygulamaya kimlik doğrulaması ekleme]: mobile-services-dotnet-backend-ios-get-started-users.md
[Var olan bir uygulamaya anında iletme bildirimleri ekleme]: mobile-services-dotnet-backend-ios-get-started-push.md


[Mobile Services iOS SDK’sı]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Klasik Azure Portalı]: https://manage.windowsazure.com/
[XCode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[.NET arka uç sürümü]: mobile-services-dotnet-backend-ios-get-started.md



<!--HONumber=Aug16_HO1-->


