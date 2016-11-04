---
title: Android uygulamaları için Azure Mobile Services’ı Kullanmaya Başlama (JavaScript arka ucu)
description: Android geliştirme (JavaScript arka ucu) için Azure Mobile Services’ı kullanmaya başlamak için bu öğreticiden yararlanın.
services: mobile-services
documentationcenter: android
author: RickSaling
manager: reikre
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/21/2016
ms.author: ricksal

---
# Android için Mobile Services’ı kullanmaya başlama (JavaScript arka ucu)
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Bu konudaki Mobile Apps sürümünün eşdeğeri için bkz. [Azure Mobile Apps’de bir Android uygulaması oluşturma](../app-service-mobile/app-service-mobile-android-get-started.md).
> 
> 

Bu öğreticide, bir Android uygulamasına Azure Mobile Services’ı kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Bu öğretici kapsamında, hem yeni bir mobil hizmet hem de yeni mobil hizmetteki uygulama verilerini depolayan basit bir **Yapılacaklar listesi** uygulaması oluşturacaksınız. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Android-Support-in-Windows-Azure-Mobile-Services/player]
> 
> 

Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![](./media/mobile-services-android-get-started/mobile-quickstart-completed-android.png)

## Ön koşullar
Bu öğreticiyi tamamlamak için Android Studio tümleşik geliştirme ortamını ve en son Android platformunu içeren [Android Geliştirici Araçları](https://developer.android.com/sdk/index.html) gerekir. Android 4.2 veya sonraki bir sürümü gereklidir.

İndirilen hızlı başlangıç projesi Android için Azure Mobile Services SDK'sı içerir.

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28).
> 
> 

## Yeni bir mobil hizmet oluşturma
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Yeni bir Android uygulaması oluşturma
Mobil hizmetinizi oluşturduktan sonra yeni bir uygulama oluşturmak veya mevcut bir uygulamayı mobil hizmetinize bağlamak üzere değiştirmek için Klasik Azure Portalı’ndaki kolay bir hızlı başlangıcı izleyebilirsiniz.

Bu bölümde, mobil hizmetinize bağlanan yeni bir Android uygulaması oluşturacaksınız.

1. Klasik Azure Portalı’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.
2. Hızlı başlangıç sekmesinde, **Platform seçin** altında **Android**’e tıklayın ve **Yeni Android uygulaması oluştur** seçeneğini genişletin.
   
    ![](./media/mobile-services-android-get-started/mobile-portal-quickstart-android1.png)
   
    Burada, mobil hizmetinize bağlanan bir Android uygulaması oluşturmanın üç kolay adımı gösterilir.
   
    ![](./media/mobile-services-android-get-started/mobile-quickstart-steps-android-AS.png)
3. Henüz yapmadıysanız, yerel bilgisayarınıza veya sanal makinenize [Android Geliştirici Araçları](https://go.microsoft.com/fwLink/p/?LinkID=280125)’nı indirin ve yükleyin.
4. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItem tablosu oluştur**’a tıklayın.
5. Şimdi **İndir** düğmesine basarak uygulamanızı indirin.

## Android uygulamanızı çalıştırma
[!INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]

## <a name="next-steps"> </a>Sonraki Adımlar
Hızlı başlangıcı tamamladığınıza göre, Mobile Services’daki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin:

* [Verileri kullanmaya başlama]
  <br/>Mobile Services’ı kullanarak verileri depolama ve sorgulama hakkında daha fazla bilgi edinin.
* [Kimlik doğrulamayı kullanmaya başlama]
  <br/>Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Anında iletme bildirimlerini kullanmaya başlama]
  <br/>Uygulamanıza en temel anında iletme bildirimlerini nasıl göndereceğinizi öğrenin.

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- URLs. -->
[Kullanmaya başlama (Eclipse)]: mobile-services-android-get-started-ec.md
[Verileri kullanmaya başlama]: mobile-services-android-get-started-data.md
[Kimlik doğrulamayı kullanmaya başlama]: mobile-services-android-get-started-users.md
[Anında iletme bildirimlerini kullanmaya başlama]: mobile-services-javascript-backend-android-get-started-push.md
[Mobile Services Android SDK’sı]: https://go.microsoft.com/fwLink/p/?LinkID=266533




<!--HONumber=Aug16_HO1-->


