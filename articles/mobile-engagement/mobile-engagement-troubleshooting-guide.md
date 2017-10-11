---
title: "Azure Mobile Engagement sorun giderme kılavuzları"
description: "Azure Mobile Engagement için sorun giderme kılavuzu"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 31134a29-a513-4e5e-b626-f6cf6fe04769
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 93b5e3f4892f974bf9df28955956136528470e03
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure Mobile Engagement - Sorun Giderme Kılavuzu
## <a name="introduction"></a>Giriş
Aşağıdaki sorun giderme kılavuzu nedenlerini bazı yaygın olarak görülen sorunlar ve kendi başınıza gidermenize etkinleştirmek anlamanıza yardımcı olur. 

## <a name="general"></a>Genel
Genel olarak, her zaman aşağıdakileri sağlamanız:

1. Bölümünde açıklandığı gibi tümleştirme için gereken tüm adımları ilerlemiş sağlamak bizim [başlama öğreticileri](mobile-engagement-windows-store-dotnet-get-started.md)
2. Platform SDK'ın en son sürümünü kullanıyorsunuz. 
3. Bazı sorunlar yalnızca öykünücüsünü özgü olduğundan gerçek bir cihaza hem bir öykünücü sınayın. 
4. Tüm sınırları/belgelenen kısıtlamaları Mobile Engagement gelen basarsa değil [burada](../azure-subscription-service-limits.md)
5. Mobile Engagement hizmet arka ucuna bağlanılamıyor veya görme değilseniz, sürekli olarak sonra yüklenen değil veri devam eden hizmet olay denetleyerek emin [burada](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>'İzleyici' sorunları
### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Monitör sekmesinde gösteren cihazımı görüyorum değil
İzleyici sekmesi, Mobile Engagement platformu gerçek zamanlı bağlı cihazları gösterir. Bir öykünücü ve cihaz ayıkladığınız, burada en az bir oturum görmeniz gerekir. Uygulama dağıtılmışsa, gerçek zamanlı platforma bağlı aygıtları yansıtacak etkin oturumları ölçer görürsünüz. 

Ardından, aygıtınızı monitör sekmesinde görmüyorsanız büyük olasılıkla bir SDK tümleştirme sorunu olduğunu. Sorunları çözmek için bazı ortak adımlar aşağıdaki gibidir:

1. Mobil uygulamada doğru bağlantı dizesi kullanılarak ve SDK anahtarları bölümüne ve API anahtarlarını bölümü olduğundan emin olun. Bağlantı dizesi mobil uygulamanızı monitör sekmesinde Cihazınızı yazısını görürsünüz Mobile Engagement uygulaması örneğine bağlanır. 
2. Windows platformu - sayfanızı kılıyorsa için `OnNavigatedTo` yöntemi, çağrıldığından emin olun `base.OnNavigatedTo(e)`.
3. Mevcut mobil uygulamaya Mobile Engagement tümleştirme sonra size herhangi bir adım gelişmiş tümleştirme adımları bakarak eksik değil olduğunu sağlayabilirsiniz [burada](mobile-engagement-windows-store-integrate-engagement.md)
4. Gönderdiğiniz en az bir ekran/etkinlik EngagementActivity sayfayla açıklandığı gibi çalıştığınız platforma bağlı olarak geçersiz kılarak olun [başlama öğreticileri](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Bir oturum gösteren İzleyici sekmesi görüyorum bile zaman ı bağlantısı kesilmiş veya Uygulamam kapalı / öykünücüsü.
Bu noktada platforma bağlı tek olan ve uygulamayı açmak için bir öykünücü kullanıyorsanız sonra bu öykünücüsü quirks nedeniyle olasıdır. Genel olarak, geri öykünücü başarıyla bağlantısını kesmek uygulama oturumu için giriş ekranında geldiğinizde emin olmanız gerekir. Ayrıca, Windows platformunda, Visual Studio ile hata ayıklama sırasında Visual Studio'da gitmeniz emin olmak gerekebilir **yaşam döngüsü olayları** menü çubuğu ve tıklayarak **askıya alma** gerçekten kapatmak için oturumu. Bkz: [Windows öğretici](mobile-engagement-windows-store-dotnet-get-started.md) Ayrıntılar için. 

## <a name="analytics-issues"></a>'Analytics' sorunları
### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Herhangi bir veri görmüyorum / Analytics sekmesinde verilerin yenilenmesi
Analiz verileri düzenli olarak hesaplanır ve bu yenileme 24 saatten fazla sürebilir. Bu verileri gerçek zamanlı değildir ve bu 24 saatlik süre içinde yenilenir görürsünüz.
Lütfen ancak, en az bir ekranı veya etkinliği platform arka ucuna ya da geçersiz kılma en az bir sayfayla tarafından gönderdiğinizden emin olmak `EngagementActivity` veya çağırma `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Bir aygıt için yanlış yakalanan tarih/saat Analytics sekmesinde görüyorum
Süre analiz için kullanıcı cihaz ayarları tarihinden temel alır. Bu nedenle aygıt tarihi doğru olduğundan emin olun. 

## <a name="segment-issues"></a>'Segment' sorunları
### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Bir segment oluşturup ve devre dışı olarak gösteren veya tüm veriler gösterilmiyor
Segment oluşturma, şu anda gerçek zamanlı değildir. Aynı anda analiz verileri toplanır ve 24 saatten fazla sürebilir şekilde hesaplanır. Daha sonra yeniden Buraya, ancak bu sırada ayrıca mobil uygulamalarınızı, temel alarak kesimleri oluşturan veri gerçekten gönderiyor emin olmalısınız. Örneğin bir olay 'foo' herhangi bir mobil aygıtı tarafından gönderilen değil sonra EventName ile oluşturulan bir kesim için herhangi bir segment veri olmayacaktır olması söylediğinizde foo ölçüt olarak =. Ayrıca, mobil uygulamanızı emin olmak için SDK tümleştirmesi doğru verileri gönderen denetlemeniz gerekir. 

## <a name="reach-or-push-notifications-issues"></a>'Ulaşmak' ya da anında iletme bildirimleri sorunları
### <a name="my-push-messages-are-not-being-delivered"></a>My anında iletileri teslim edilmiyor
1. Önce tüm bileşenleri - mobil uygulama ve hizmet SDK'sını doğru şekilde bağlandığını ve anında iletme bildirimleri göndermeyi mümkün olduğundan emin olmak için bir test cihazı bildirimleri göndermeyi deneyin. 
2. Her zaman en basit 'uygulamasının, çıkış bildirim' değil zamanlanan bir kampanya ilk Gönder ve ya da belirtilen tüm hedef kitle ölçütünün sahiptir. Yeniden bildirim bağlantısı düzgün çalıştığını kanıtlamak için budur. 
3. Uygulama bildirimleri teslim sorunlarla karşılaşıyorsanız, sonra da bunu önce bir uygulama bildirim göndererek denemek için bir iyi ilk adımdır. 
4. 'Yerel gönderim' doğru mobil uygulamanız için yapılandırıldığından emin olun. Platforma bağlı olarak, ya da anahtarları (Android, Windows) veya sertifikaları (iOS) içerir. Bkz: [kullanıcı arabirimi - ayarlar](mobile-engagement-user-interface-settings.md)
5. Bildirimler ayrıca mobil işletim sistemi aracılığıyla kullanıcı tarafından bu nedenle önlenebilir uygulamanın oturumunu bu durumda olmadığından emin olun. 
6. Değil ayarı olun *yoksay İzleyici itme gönderilecek API aracılığıyla kullanıcılara* seçeneğini **kampanya** bölümünde bir Reach kampanya bu anında iletme bildirimleri yalnızca olabilir sağlayacak çünkü API'leri gönderilir. 
7. Ağ bağlantısı sorunları olası bir kaynak olarak ortadan kaldırmak için WIFI ve telefon işleci ağ üzerinden bağlanan her iki bir aygıt ile anında iletme kampanyanızı test ettiğinizden emin olun.
8. Herhangi bir eşitlenmemiş aygıtı ayrıca bildirimleri göndermeyi anında iletilen bildirim servisi'nın yeteneği çünkü Aygıt/Öykünücüsü üzerindeki sistem tarih/saat doğru olduğundan emin olun. 

Daha fazla platforma aşağıdaki yönergeleri sorunlarını giderme:

1. **iOS** 
   
   * Sertifikaların geçerli ve süresi dolmamış iOS anında iletme bildirimleri için olduğundan emin olun. 
   * Doğru şekilde yapılandırdığınızdan emin olun bir *üretim* Mobile Engagement uygulamanız sertifikada. 
   * Üzerinde test ettiğinizden emin olun bir *gerçek, fiziksel aygıt.* İOS simülatörü anında iletme iletileri işleyemez.
   * Paket tanımlayıcısı mobil uygulamaya doğru şekilde yapılandırıldığından emin olun. Yönergelere bakın [burada](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
   * Test ederken "Geçici" dağıtım mobil sağlama profilinizde kullanın. "Hata ayıklama" kullanarak uygulamanızı derlenmiş ise bildirim almak mümkün olmaz
2. **Android**
   
   * \N karakterin ardından mobil uygulamanızın AndroidManifest.xml dosyasında doğru proje numarası belirttiğinizden emin olun. 
     
           <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
   * Eksik olmayan veya tüm izinleri Android derleme bildirimi dosyasında yanlış yapılandırdığınızdan emin olun 
   * GCM Server anahtarını burada aldığınız istemci uygulamanızı eklemekte olduğunuz proje numarası aynı hesaptan olduğundan emin olun. İkisi arasındaki tüm uyuşmazlığı, gönderim çıkmasını önler. 
   * Sistem alıyorsanız bildirimleri ancak değil incelediniz uygulama [bildirimler bölümü için bir simge belirtin](mobile-engagement-android-get-started.md) gibi büyük olasılıkla doğru simge Android derleme bildirimi dosyasında belirtiyorsanız değil. 
   * Bir BigPicture bildirim göndererek, ardından olması durumunda emin olun, dış görüntü sunucularına sahip sonra HTTP desteği ihtiyaç duydukları "GET" ve "HEAD".
3. **Windows**
   
   * Geçerli bir Windows mağazası uygulaması ile ilişkili uygulamayı emin olun. Visual Studio'da - projeye sağ tıklayın ve "Uygulama mağazası ile ilişkilendirme" seçeneğini belirleyin ve Windows Mağazası'nda oluşturulan uygulama seçin gerekecektir. Bu Windows mağazası uygulaması ile aynı Mobile Engagement portalında yapılandırmak için yerel gönderim kimlik bilgilerini aldığınız gelen burada olması gerekir.
   * App anında iletme bildirimleri ancak olmayan uygulama bildirimlerle alıyorsanız `EngagementOverlay` sonra tümleştirme sayfanızda bir kök kılavuz öğesi olduğundan emin olun. İki web eklemek için xaml dosyanızda bulduğu ilk "Kılavuz" öğesi sayfanızda görünümleri EngagementOverlay kullanır. Burada web görünümleri ayarlanacak bulmak isterseniz, bu görüntüler, ancak yeterli yüksekliğini ve genişliğini iki sonraki Web olduğundan emin gerekecek bildirim ve aşağıdaki duyuru gösterir gibi "EngagementGrid" adlı bir kılavuz tanımlayabilirsiniz. uygulama içi bildirim olarak:
     
           <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Bir anında iletme bildirimi/duyuru oluşturulan/kampanya ve hatta Bunu bana bildirim gönderdikten sonra 'Etkin' olarak gösteriyor. Ne anlama geliyor?
**Kampanya** Mobile Engagement içinde oluşturulan yeni cihazları mobil katılım platformunuz bağlandıkları, uzun süre çalışan bir anında iletme bildirimi anlamı olduğundan, bunlar otomatik olarak bildirimi gönderilecek şekilde çağrılır, Kampanyanın belirlediğiniz ölçütü karşılayan sürece burada yapılandırın. Tek bir çekim tek bildirim kurulu değil budur. El ile tıklayın gerekecek **son** daha fazla bildirim göndermez kampanya sonlandırılacağını düğmesi. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Anında iletme kampanya oluşturulan ve bildirimler başarıyla uygulamasını açtığınızda, hatta ı varken ancak aynı bildirim işleme alınan alıyorum alıyorum önce?
Bu test sırasında gerçekleşmesi olasıdır ve Öykünücüler veya bazı kullanıyorsanız, TestFlight gibi framework sınayın. Örneği çalıştıran her uygulama bunu yeni DeviceID alınırken ve yeni bir cihaz ve bildirim göndererek işlemek Mobile Engagement platformu neden bizim arka uç gönderme olduğunu İşte olanlar. 

## <a name="getting-support"></a>Destek alma
Bu sorunu çözmek erişemiyorsanız kendiniz bundan sonra ' nı kullanabilirsiniz:

1. Sorununuzu StackOverflow Forumunda varolan parçacıklarında arayın ve [MSDN Forumu](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) ve değil sonra var. bir soru sorun. 
2. Üzerinde sonra ekleme/oy isteği için eksik bir özellik bulursanız bizim [UserVoice Forumu](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Microsoft destek açık bir destek olayını aşağıdaki ayrıntıları sağlayarak varsa: 
   * Azure abonelik kimliği
   * Platform (örn. iOS, Android vb.)
   * Uygulama Kimliği
   * Kampanya kimliği (için anında iletme bildirimi sorunları)
   * Cihaz Kimliği
   * Mobile Engagement SDK'sı sürüm (örneğin Android SDK v2.1.0)
   * Hata ayrıntılarını tam hata iletisi ve senaryosu

