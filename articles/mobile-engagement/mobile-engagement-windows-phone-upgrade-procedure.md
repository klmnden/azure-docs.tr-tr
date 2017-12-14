---
title: "Windows Phone Silverlight SDK yükseltme yordamları"
description: "Windows Phone Silverlight SDK yükseltme yordamları için Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 87130026-9759-4659-9184-788a3627a165
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 633bf79a3bcaa9c97a5c70e3b362fd928178dcce
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK yükseltme yordamları
Uygulamanıza bizim SDK daha eski bir sürümü zaten bütünleştirdiyseniz, SDK'yı yükseltirken aşağıdaki noktaları dikkate almanız gerekir.

SDK çeşitli sürümleri eksik, birçok yordamı uygulamanız gerekebilir. Örneğin, 0.10.1 önce "Kimden 0.9.0'dan 0.10.1 için" yordamını takip etmek için sahip 0.11.0 sonra "Kimden 0.10.1 0.11.0 için" yordamı geçiş ise.

## <a name="from-200-to-330"></a>2.0.0 3.3.0 için
### <a name="test-logs"></a>Test günlükleri
SDK'sı tarafından üretilen konsol günlükleri artık etkin/devre dışı bırakılmış/filtrelenmiş olabilir. Bu özelleştirmek için özellik güncelleştirme `EngagementAgent.Instance.TestLogEnabled` bulunan değer birine `EngagementTestLogLevel` numaralandırma, örneğin:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-to-200"></a>1.1.1 2.0.0 için
Aşağıdaki nasıl Azure Mobile Engagement tarafından desteklenen bir uygulamaya Capptain SAS tarafından sunulan Capptain hizmetinden bir SDK tümleştirmesi geçirileceğini açıklar. 

> [!IMPORTANT]
> Capptain ve Mobile Engagement aynı Hizmetleri değildir ve aşağıda verilen yordamı yalnızca istemci uygulaması geçirmek nasıl vurgular. Uygulama SDK'yı geçirme verilerinizi Capptain sunucularından Mobile Engagement sunucuya geçişi YAPILMAZ
> 
> 

Önceki bir sürümünden geçiş yapıyorsanız, Lütfen 1.1.1 için önce geçirmenizi sonra aşağıdaki yordamı uygulamak için Capptain web sitesine bakın

### <a name="nuget-package"></a>Nuget paketi
Değiştir **Capptain.WindowsPhone** tarafından **MicrosoftAzure.MobileEngagement** Nuget paketi.

### <a name="applying-mobile-engagement"></a>Mobil katılım uygulama
Terimi SDK kullanan `Engagement`. Projenizi bu değişikliği eşleşecek şekilde güncelleştirmeniz gerekir.

Geçerli Capptain nuget paketini kaldırmanız gerekir. Tüm değişikliklerinizi Capptain Kaynaklar klasörünü kaldırılacak göz önünde bulundurun. Bu dosyaları saklamak isterseniz bir kopyasını oluşturun.

Bundan sonra projenizde yeni Microsoft Azure katılım nuget paketini yükleyin. Doğrudan üzerinde bulabilirsiniz [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Bu eylem, katılım tarafından kullanılan tüm kaynakları dosyalarının yerini alır ve yeni katılım DLL, proje başvuruları ekler.

Proje başvuruları Capptain DLL başvurularını silerek temizlemeniz gerekir. Bunu yapmazsanız Capptain sürümü çakışır ve hataları gerçekleşir.

Capptain kaynakları özelleştirdiyseniz, eski dosyaları içeriğinizi kopyalayın ve bunları yeni katılım dosyalar yapıştırın. Xaml ve cs dosyaları güncelleştirilmesi gerektiğini unutmayın.

Bu adımlar tamamlandığında yeni katılım başvurular tarafından eski Capptain başvuruları değiştirin yeterlidir.

1. Tüm Capptain ad alanlarını güncelleştirilmesi gerekiyor.
   
    Geçişten önce:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    Geçişten sonra:
   
        using Microsoft.Azure.Engagement;
2. "Capptain" içeren tüm Capptain sınıfları "Katılım" içermelidir.
   
    Geçişten önce:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    Geçişten sonra:
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. XAML dosyaları için de Capptain ad alanı ve özniteliklerini değiştirir.
   
    Geçişten önce:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    Geçişten sonra:
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. İçin diğer kaynaklar Capptain resimler gibi bunlar ayrıca "Katılım" kullanacak şekilde yeniden adlandırıldığı gerektiğini lütfen unutmayın.

### <a name="application-id--sdk-key"></a>Uygulama Kimliği / SDK anahtarı
Katılım bağlantı dizesini kullanır. Mobile Engagement ile bir uygulama kimliği ve bir SDK anahtarı belirtmeniz gerekmez, yalnızca bir bağlantı dizesi belirtmeniz gerekir. Bunu EngagementConfiguration dosyanızı ayarlayabilirsiniz.

Katılım yapılandırma ayarlanabilir, `Resources\EngagementConfiguration.xml` projenizin dosya.

Belirtmek için bu dosyayı düzenleyin:

* Uygulama bağlantı dizenizi etiketleri arasına `<connectionString>` ve `<\connectionString>`.

Bunun yerine çalışma zamanında belirtmek istiyorsanız, katılım aracı başlatmadan önce aşağıdaki yöntemini çağırabilirsiniz:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Bağlantı dizesi, uygulamanız için Azure Portalı'nda görüntülenir.

### <a name="items-name-change"></a>Öğe adı değişikliği
Tüm öğeleri adlı *capptain* adlandırılmış *engagement*. Benzer şekilde *Capptain* için *Engagement*.

Yaygın olarak kullanılan Capptain öğeleri örnekleri:

* Şimdi EngagementConfiguration adlı CapptainConfiguration
* Şimdi EngagementAgent adlı CapptainAgent
* Şimdi EngagementReach adlı CapptainReach
* Şimdi EngagementHttpConfig adlı CapptainHttpConfig
* Şimdi GetEngagementPageName adlı GetCapptainPageName

Ayrıca etkiler yeniden adlandırmak Not yöntemleri geçersiz kılındı.

