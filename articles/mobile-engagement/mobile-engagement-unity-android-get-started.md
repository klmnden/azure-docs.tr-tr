---
title: Unity Android dağıtımı için Azure Mobile Engagement kullanmaya başlama
description: Unity uygulamalarını iOS cihazlarına dağıtmak için Analizler ve Anında İletme Bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin.
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: ''
editor: ''

ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo

---
# Unity Android dağıtımı için Azure Mobile Engagement kullanmaya başlama
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konu size uygulama kullanımınızı anlamak için Azure Mobile Engagement kullanmayı ve Android cihazına dağıtırken bir Unity uygulamasının kesimli kullanıcılarına anında iletme bildirimleri göndermeyi gösterir.
Bu öğretici, başlangıç noktası olarak bir Küre öğretici olarak klasik Unity Roll kullanır. Aşağıdaki öğreticide gösterdiğimiz Mobile Engagement tümleştirmesine geçmeden önce bu [öğreticideki](mobile-engagement-unity-roll-a-ball.md) adımları izlemelisiniz. 

Bu öğretici için aşağıdakiler gereklidir:

* [Unity Düzenleyicisi](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* Google Android SDK

> [!NOTE]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).
> 
> 

## <a id="setup-azme"></a>Android uygulamanız için Mobile Engagement kurma
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama
### Unity paketini içeri aktarma
1. [Mobile Engagement Unity paketini](https://aka.ms/azmeunitysdk) indirin ve yerel makinenize kaydedin. 
2. **Varlıklar -> Paketi İçeri Aktar -> Özel Paket**’e gidin ve yukarıdaki adımda indirdiğiniz paketi seçin. 
   
    ![][70] 
3. Tüm dosyaların seçildiğinden emin olun ve **İçeri Aktar** düğmesine tıklayın. 
   
    ![][71] 
4. İçeri aktarma başarılı olduktan sonra, içeri aktarılan SDK dosyalarını projenizde görürsünüz.  
   
    ![][72] 

### EngagementConfiguration’ı güncelleştirme
1. SDK klasöründe **EngagementConfiguration** betik dosyasını açın ve daha önce Azure portaldan aldığınız bağlantı dizesiyle **ANDROID\_CONNECTION\_STRING**’i güncelleştirin.  
   
    ![][73]
2. Dosyayı kaydetme 
3. **Dosya -> Engagement -> Android Derleme Bildirimi Oluştur**’yürütün. Bu, Mobile Engagement SDK tarafından eklenen eklentidir ve buna tıklamak proje ayarlarınızı otomatik olarak güncelleştirir. 
   
    ![][74]

> [!IMPORTANT]
> **EngagementConfiguration** dosyasını güncelleştirdiğiniz her sefer bunu yürüttüğünüzden emin olun, aksi takdirde değişiklikleriniz uygulamada yansıtılmaz. 
> 
> 

### Uygulamayı temel izleme için yapılandırma
1. Düzenlemek için Player nesnesine ekli olan **PlayerController** betiğini açın. 
2. Şu deyimi kullanarak aşağıdakileri ekleyin:
   
        using Microsoft.Azure.Engagement.Unity;
3. Aşağıdakileri `Start()` yöntemine ekleyin.
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### Uygulamayı dağıtma ve çalıştırma
Bu Unity uygulamasını cihazınıza dağıtmaya çalışmadan önce makinenizde Android SDK yüklü olduğundan emin olun. 

1. Bir Android cihazı makinenize bağlayın. 
2. **Dosya -> Yapı Ayarları**’nı açın 
   
    ![][40]
3. **Android**’i seçin ve ardından **Anahtar Platformu**’na tıklayın
   
    ![][51]
   
    ![][52]
4. **Player ayarları**’na tıklayın ve geçerli bir Paket Tanımlayıcısı girin. 
   
    ![][53]
5. Son olarak, **Derle ve Çalıştır**’a tıklayın.
   
    ![][54]
6. Android paketini depolamak için bir klasör adı sağlamanız istenebilir. 
7. Her şey yolunda giderse, paket bağlı cihazınıza dağıtılır ve telefonunuzda Unity oyununuzu görmelisiniz. 

## <a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### EngagementConfiguration’ı güncelleştirme
1. SDK klasöründe **EngagementConfiguration** betik dosyasını açın ve daha önce Google Cloud Developer portaldan aldığınız **Google Project Number** ile **ANDROID\_GOOGLE\_NUMBER**’i güncelleştirin. Bu bir dize değeridir, bu nedenle çift tırnak içine aldığınızdan emin olun. 
   
    ![][75]
2. Dosyayı kaydedin. 
3. **Dosya -> Engagement -> Android Derleme Bildirimi Oluştur**’yürütün. Bu, Mobile Engagement SDK tarafından eklenen eklentidir ve buna tıklamak proje ayarlarınızı otomatik olarak güncelleştirir. 
   
    ![][74]

### Bildirimleri almak üzere uygulamayı yapılandırma
1. Düzenlemek için Player nesnesine ekli olan **PlayerController** betiğini açın. 
2. Aşağıdakileri `Start()` yöntemine ekleyin.
   
        EngagementReachAgent.Initialize();
3. Artık uygulama güncelleştirildiğine göre, uygulamayı aşağıda verilen yönergelere göre bir cihaza dağıtın ve çalıştırın. 

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png



<!--HONumber=Oct16_HO3-->


