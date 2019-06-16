---
title: Azure Application Insights ile Azure API Management tümleştirme | Microsoft Docs
description: Günlüğe kaydetme ve Azure Application ınsights'ı Azure API Yönetimi'nden olayları görüntüleme hakkında bilgi edinin.
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
ms.openlocfilehash: 3bbab82831fba389cd4bf172e7ea762d5971579b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241839"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Azure API Management, Azure Application Insights ile tümleştirme

Azure API yönetimi, Azure Application Insights - birden çok platformda uygulamalar oluşturmaya ve yönetmeye web geliştiricilerine yönelik genişletilebilir bir hizmet ile kolay tümleştirme sağlar. Bu kılavuzda Bu tümleştirme her adımında ve API Management hizmet örneğinizin performans etkisini azaltmak için stratejilerini açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu takip etmek için Azure API Management örneği olması gerekir. Yoksa, tamamlama [öğretici](get-started-create-service-instance.md) ilk.

## <a name="create-an-azure-application-insights-instance"></a>Azure Application Insights örneği oluşturma

Azure Application ınsights'ı kullanabilmeniz için önce Hizmeti'nin bir örneğini oluşturmanız gerekir.

1. Açık **Azure portalında** gidin **Application Insights**.  
    ![App Insights'ı oluşturma](media/api-management-howto-app-insights/apim-app-insights-instance-1.png)  
2. **+ Ekle**'ye tıklayın.  
    ![App Insights'ı oluşturma](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. Formu doldurun. Seçin **genel** olarak **uygulama türü**.
4. **Oluştur**’a tıklayın.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Azure Application Insights ile Azure API Management hizmet örneği arasında bir bağlantı oluşturun

1. Gidin, **Azure API Management hizmet örneği** içinde **Azure portalında**.
2. Seçin **Application Insights** sol taraftaki menüden.
3. **+ Ekle**'ye tıklayın.  
    ![App Insights Günlükçü](media/api-management-howto-app-insights/apim-app-insights-logger-1.png)  
4. Önceden oluşturulmuş seçin **Application Insights** örneği ve kısa bir açıklama sağlayın.
5. **Oluştur**’a tıklayın.
6. Yalnızca, bir izleme anahtarı ile bir Azure Application Insights Günlükçü oluşturdunuz. Artık listede görünmesi gerekir.  
    ![App Insights Günlükçü](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

> [!NOTE]
> Sahne arkasında bir [Günlükçü](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger/createorupdate) varlık örneğinin Application Insights izleme anahtarını içeren API Management örneğinizin içinde oluşturulur.

## <a name="enable-application-insights-logging-for-your-api"></a>API'niz için Application Insights günlük kaydını etkinleştirme

1. Gidin, **Azure API Management hizmet örneği** içinde **Azure portalında**.
2. Soldaki menüden **API'ler** seçeneğini belirleyin.
3. Bu durumda, API'nizi tıklayın **tanıtım konferansı API'si**.
4. Git **ayarları** üst çubuktaki sekmesinden.
5. Ekranı aşağı kaydırarak **tanılama günlükleri** bölümü.  
    ![App Insights Günlükçü](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. Denetleme **etkinleştirme** kutusu.
7. İçinde ekli Günlükçü seçin **hedef** açılır.
8. Giriş **100** olarak **örnekleme (%)** ve işaretleyerek **hataları günlüğe her zaman** onay kutusu.
9. **Kaydet**’e tıklayın.

> [!WARNING]
> Varsayılan değer geçersiz kılma **0** içinde **gövdesi ilk baytı** alan Apı'lerinizi performansını önemli ölçüde küçültebilir.

> [!NOTE]
> Sahne arkasında bir [tanılama](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/diagnostic/createorupdate) 'applicationınsights' adlı varlık API düzeyinde oluşturulur.

| Ayar adı                        | Değer türü                        | Açıklama                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Etkinleştirme                              | boole                           | Bu API'nin günlük kaydı etkin olup olmadığını belirtir.                                                                                                                                                                                                                                                                                                |
| Hedef                         | Azure Application Insights Günlükçü | Azure Application Insights Günlükçü kullanılacak belirtir                                                                                                                                                                                                                                                                                           |
| Örnekleme (%)                        | decimal                           | -100 (yüzde) değerlerini 0. <br/> Azure Application ınsights'ı isteklerinin yüzdesini kaydedilir belirtir. %0 örnekleme sıfır istekleri günlüğe kaydedilen tüm istekleri % 100 örnekleme anlamına anlamına gelir. <br/> Bu ayar, Azure Application Insights günlük isteklerini performans etkilerini azaltmak için kullanılır (bölümüne bakın). |
| Her zaman günlüğü hataları                   | boole                           | Bu ayar seçili ise, tüm hataları Azure Application Insights'a açmamasından kaydedilir **örnekleme** ayarı.                                                                                                                                                                                                                  |
| Temel Seçenekler: Üst bilgiler              | list                              | Azure Application ınsights'ı, istekler ve yanıtlar için günlüğe kaydedilir üst bilgileri belirtir.  Varsayılan: üst bilgi olarak kaydedilir.                                                                                                                                                                                                             |
| Temel Seçenekler: İlk baytı gövdesi  | integer                           | Gövde kaç ilk bayt için Azure Application Insights istekler ve yanıtlar için günlüğe belirtir.  Varsayılan: gövdesi günlüğe kaydedilmez.                                                                                                                                                                                              |
| Gelişmiş Seçenekleri: Ön uç isteği  |                                   | Belirtir olup olmadığını ve nasıl *ön uç isteklerini* Azure Application ınsights'ı günlüğe kaydedilir. *Ön uç isteği* Azure API Management hizmeti için gelen isteğidir.                                                                                                                                                                        |
| Gelişmiş Seçenekleri: Ön uç yanıtı |                                   | Belirtir olup olmadığını ve nasıl *ön uç yanıtlarını* Azure Application ınsights'ı günlüğe kaydedilir. *Ön uç yanıt* yanıt giden olan Azure API Management hizmetinden.                                                                                                                                                                   |
| Gelişmiş Seçenekleri: Arka uç isteği   |                                   | Ve bunun nasıl yapılacağını belirtir *arka uç istekleri* Azure Application ınsights'ı günlüğe kaydedilir. *Arka uç isteği* istek giden olan Azure API Management hizmetinden.                                                                                                                                                                        |
| Gelişmiş Seçenekleri: Arka uç yanıtı  |                                   | Belirtir olup olmadığını ve nasıl *arka uç yanıtlarını* Azure Application ınsights'ı günlüğe kaydedilir. *Arka uç yanıtı* bir Azure API Management hizmeti için gelen yanıttır.                                                                                                                                                                       |

> [!NOTE]
> Farklı düzeylerde - tek API Günlükçü ya da tüm API'leri için bir Günlükçü günlükçüleri belirtebilirsiniz.
>  
> Her ikisini de belirtirseniz:
> + her ikisi de (günlükleri çoğullama) kullanılır farklı günlükçüleri olmaları durumunda
> + olmaları durumunda aynı günlükçüler ancak farklı ayarlar ve ardından tüm API'leri için tek API (daha ayrıntılı düzeyde) için geçersiz kılar.

## <a name="what-data-is-added-to-azure-application-insights"></a>Azure Application Insights için hangi verilerin eklenir

Azure Application ınsights'ı alır:

+ *İstek* her gelen istek için telemetri öğesinin (*ön uç isteği*, *ön uç yanıt*),
+ *Bağımlılık* bir arka uç hizmetine iletilmesini her istek için telemetri öğesinin (*arka uç isteği*, *arka uç yanıtı*),
+ *Özel durum* başarısız her istek için telemetri öğesi.

Bir istek, başarısız bir istek olduğunu da:

+ bir kapalı istemci bağlantısı nedeniyle başarısız oldu veya
+ tetiklenen bir *hata* API ilkeler bölümünü veya
+ bir yanıt HTTP durum kodu eşleşen 4xx veya 5xx sahiptir.

## <a name="performance-implications-and-log-sampling"></a>Performans etkileri ve günlük örnekleme

> [!WARNING]
> Tüm olayları günlüğe kaydetme, gelen istek oranı bağlı olarak performans ciddi etkileri olabilir.

İstek hızı, saniye başına 1000 istek aşıldığında iç yük testleri bağlı olarak, bu özelliğin etkinleştirilmesi % 40-%50 azaltma verim neden oldu. Azure Application Insights, uygulama performanslarını değerlendirmek için istatistiksel analiz kullanması için tasarlanmıştır. Bir denetim sistemi olacak şekilde tasarlanmamıştır ve yüksek hacimli API'leri için tek tek her isteği günlüğe kaydetme için uygun değildir.

Ayarlayarak günlüğe kaydedilmesini isteklerinin sayısı işleyebileceğiniz **örnekleme** ayarlama (yukarıdaki adımlara bakın). % 100 anlamına gelir hiçbir hiç oturum %0 yansıtır sırada tüm istekleri günlüğe kaydedilen değeri. **Örnekleme** hala günlük avantajlarını taşıma sırasında önemli bir performans düşüşüne, etkili bir şekilde engelleyen, telemetri hacmini azaltmak için yardımcı olur.

Üstbilgi ve gövde isteklerin ve yanıtların günlüğe atlanıyor performans sorunlarını ortadan kaldırılmasına üzerinde olumlu bir etkisi olacaktır.

## <a name="video"></a>Video

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2pkXv]
>
>

## <a name="next-steps"></a>Sonraki adımlar

+ Daha fazla bilgi edinin [Azure Application Insights](https://docs.microsoft.com/azure/application-insights/).
+ Göz önünde bulundurun [Azure Event Hubs ile günlüğe kaydetme](api-management-howto-log-event-hubs.md).
