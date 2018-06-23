---
title: Azure Application Insights ile Azure API Management tümleştirme | Microsoft Docs
description: Günlüğe kaydetme ve Azure Application Insights'ta Azure API Management olaylarını görüntüleme öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 7740da505f7635944536252d60ec2c2039295975
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320586"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Azure API Management Azure Application Insights ile tümleştirme

Azure API Management Azure Application Insights - genişletilebilir bir hizmet oluşturma ve birden çok platformdaki uygulamaları yönetme web geliştiricileri için ile kolay tümleştirme sağlar. Bu kılavuz, böyle bir tümleştirme her adım anlatılmaktadır ve API Management hizmet örneği üzerindeki performans etkisini azaltmak için stratejilerini açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuz izlemek için Azure API Management örneği olması gerekir. Yoksa, tamamlamak [öğretici](get-started-create-service-instance.md) ilk.

## <a name="create-an-azure-application-insights-instance"></a>Azure Application Insights örneğini oluşturma

Azure Application Insights kullanabilmek için öncelikle Hizmeti'nin bir örneğini oluşturmanız gerekir.

1. Açık **Azure portal** gidin **Application Insights**.  
    ![Uygulama Öngörüler oluşturun](media/api-management-howto-app-insights/apim-app-insights-instance-1.png)  
2. **+ Ekle**'ye tıklayın.  
    ![Uygulama Öngörüler oluşturun](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. Formu doldurun. Seçin **genel** olarak **uygulama türü**.
4. **Oluştur**’a tıklayın.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Azure Application Insights ile Azure API Management hizmet örneği arasında bir bağlantı oluştur

1. Gidin, **Azure API Management hizmet örneği** içinde **Azure portal**.
2. Seçin **Application Insights** sol taraftaki menüden.
3. **+ Ekle**'ye tıklayın.  
    ![App Insights Günlükçü](media/api-management-howto-app-insights/apim-app-insights-logger-1.png)  
4. Önceden oluşturulmuş seçin **Application Insights** örneği ve kısa bir açıklama sağlayın.
5. **Oluştur**’a tıklayın.
6. Azure Application Insights Günlükçü izleme anahtarı ile oluşturduğunuz. Şimdi listesinde görünmesi gerekir.  
    ![App Insights Günlükçü](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

## <a name="enable-application-insights-logging-for-your-api"></a>API'nizi için Application Insights günlük kaydını etkinleştir

1. Gidin, **Azure API Management hizmet örneği** içinde **Azure portal**.
2. Seçin **API'leri** sol taraftaki menüden.
3. API'nizi, bu durumda tıklatın **Demo konferans API**.
4. Git **ayarları** üst çubuğu sekmesinden.
5. Ekranı aşağı kaydırarak **tanılama günlükleri** bölümü.  
    ![App Insights Günlükçü](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. Denetleme **etkinleştirmek** kutusu.
7. Ekli Günlükçü seçin **hedef** açılır.
8. Giriş **100** olarak **örnekleme (%)** ve değer **her zaman hataları günlüğe** onay kutusu.
9. Giriş **1024** içinde **gövde ilk baytını** alan.
10. **Kaydet**’e tıklayın.

| Ayar adı                        | Değer türü                        | Açıklama                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Etkinleştirme                              | boole                           | Bu API günlüğe etkinleştirilip etkinleştirilmeyeceğini belirtir.                                                                                                                                                                                                                                                                                                |
| Hedef                         | Azure Application Insights Günlükçü | Azure Application Insights Günlükçü kullanılacak belirtir                                                                                                                                                                                                                                                                                           |
| Örnekleme (%)                        | Ondalık                           | 100 (yüzde) için değerleri 0. <br/> Azure Application Insights'a isteklerinin yüzdesini kaydedilir belirtir. %0 örnekleme % 100 örnekleme tüm istekleri günlüğe anlamına gelmekle birlikte, oturum açmış sıfır istekleri anlamına gelir. <br/> Bu ayar, Azure Application Insights günlük isteklerini performans etkilerini azaltmak için kullanılır (aşağıdaki bölüme bakın). |
| Her zaman günlük hataları                   | boole                           | Bu ayar seçili ise, tüm hataları Azure Application Insights'a bakılmaksızın kaydedilir **örnekleme** ayarı.                                                                                                                                                                                                                  |
| Temel Seçenekler: üstbilgileri              | liste                              | Azure Application Insights'a isteklerini ve yanıtlarını günlüğe üstbilgileri belirtir.  Varsayılan: hiçbir üstbilgi günlüğe kaydedilir.                                                                                                                                                                                                             |
| Temel seçenekleri: Gövde ilk baytını  | integer                           | Gövde kaç ilk baytını Azure Application Insights'a isteklerini ve yanıtlarını günlüğe belirtir.  Varsayılan: gövde günlüğe kaydedilmez.                                                                                                                                                                                              |
| Gelişmiş Seçenekleri: Ön uç isteği  |                                   | Olup olmadığını ve nasıl belirtir *ön uç istekleri* Azure Application Insights'a günlüğe kaydedilir. *Ön uç isteği* bir Azure API Management hizmeti gelen isteğidir.                                                                                                                                                                        |
| Gelişmiş Seçenekleri: Ön uç yanıt |                                   | Olup olmadığını ve nasıl belirtir *ön uç yanıtlarını* Azure Application Insights'a günlüğe kaydedilir. *Ön uç yanıt* olan bir yanıt giden Azure API Management hizmetinden.                                                                                                                                                                   |
| Gelişmiş Seçenekleri: Arka uç isteği   |                                   | Olup olmadığını ve nasıl belirtir *arka uç istekleri* Azure Application Insights'a günlüğe kaydedilir. *Arka uç isteği* olan bir isteği giden Azure API Management hizmetinden.                                                                                                                                                                        |
| Gelişmiş Seçenekleri: Arka uç yanıt  |                                   | Olup olmadığını ve nasıl belirtir *arka uç yanıtlarını* Azure Application Insights'a günlüğe kaydedilir. *Arka uç yanıt* yanıt Azure API Management hizmeti gelen olan.                                                                                                                                                                       |

> [!NOTE]
> Farklı düzeylerde - tek API Günlükçü veya Günlükçü tüm API'leri için günlükçüleri belirtebilirsiniz.
>  
> Her ikisini de belirtirseniz:
> + bunların her ikisi de (günlükleri çoğullama) kullanılır farklı günlükçüleri olmaları durumunda
> + Böyle bir durumda aynı günlükçüleri ancak farklı ayarlara sahip sonra tek API (daha ayrıntılı düzey) için tüm API'leri için geçersiz kılar.

## <a name="what-data-is-added-to-azure-application-insights"></a>Hangi verilerin Azure Application Insights'a eklenir

Azure Application Insights alır:

+ *İstek* her gelen istek için telemetri öğesi (*ön uç isteği*, *ön uç yanıt*),
+ *Bağımlılık* bir arka uç hizmetine iletilen her istek için telemetri öğesi (*arka uç isteği*, *arka uç yanıt*),
+ *Özel durum* başarısız her istek için telemetri öğesi.

Bir isteği başarısız bir istek olup hangi:

+ Kapalı istemci bağlantısı nedeniyle başarısız oldu veya
+ tetiklenen bir *hata* API ilkeleri bölümünde veya
+ bir yanıt HTTP durum kodu eşleşen 4xx veya 5xx sahiptir.

## <a name="performance-implications-and-log-sampling"></a>Performans etkileri ve günlük örnekleme

> [!WARNING]
> Tüm olayları günlüğe kaydetmeyi gelen istek oranı bağlı olarak önemli performans etkileri olabilir.

İstek hızı saniye başına 1.000 isteği aşıldığında iç yük testleri temel alan, bu özelliği etkinleştirmek % 40-%50 azaltma performansı neden oldu. Azure Application Insights istatistiksel çözümleme uygulama performanslarını değerlendirmek için kullanmak üzere tasarlanmıştır. Bir denetim sistemi olması amaçlanmamıştır ve tek tek her istek için yüksek hacimli API'leri günlüğü için uygun değildir.

Ayarlayarak günlüğe kaydedilmesini istek sayısı işleyebileceğiniz **örnekleme** (yukarıdaki adımları bakın) ayarlama. Değer % 0 hiçbir hiç günlük yansıtır sırada tüm istekleri günlüğe kaydedilen % 100 anlamına gelir. **Örnekleme** etkili bir şekilde hala günlük yararları taşıma sırasında önemli performans azalmasına önleme telemetri hacmi azaltılmasına yardımcı olur.

Üstbilgiler ve gövde isteklerin ve yanıtların günlük kaydını atlanıyor de performans sorunlarını ortadan kaldırılmasına üzerinde pozitif etkisi olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

+ Daha fazla bilgi edinmek [Azure Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/).
+ Göz önünde bulundurun [Azure Event Hubs ile oturum](api-management-howto-log-event-hubs.md).
