---
title: Azure İzleyici'de özel ölçümler
description: Azure İzleyici ve nasıl modellenir özel ölçümler hakkında bilgi edinin.
author: ancav
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 8602027431fdf2c1378834419977606bab5c6921
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60254072"
---
# <a name="custom-metrics-in-azure-monitor"></a>Azure İzleyici'de özel ölçümler

Kaynakları ve uygulamaları azure'da dağıtmak gibi performans ve sistem durumu hakkında Öngörüler elde etmek için telemetri toplamaya başlamak isteyebilirsiniz. Azure bazı ölçümler kullanılabilir, kullanıma hazır hale getirir. Bu ölçümler, standart veya platform adı verilir. Ancak, doğası gereği sınırlama getirilir. Bazı özel performans göstergelerinizi ya da daha ayrıntılı Öngörüler sağlamak için işe özgü ölçümleri toplamak isteyebilirsiniz.
Bunlar **özel** ölçümleri uygulama telemetrinizi, Azure kaynaklarınızı veya hatta bir dışarıdan içeriye izleme sistemi çalıştırır ve doğrudan Azure İzleyici için gönderilen bir aracı yoluyla toplanacak. Azure İzleyici yayımlanan sonra sorgu göz atabilir ve Azure kaynakları ve Azure tarafından yayılan standart ölçümler ile yan yana uygulamalar için özel ölçümler üzerinde uyarı.

## <a name="send-custom-metrics"></a>Özel ölçümler Gönder
Özel ölçümler, Azure İzleyici için çeşitli yöntemler gönderilebilir:
- Uygulamanızı Azure Application Insights SDK'sını kullanarak izleyin ve Azure İzleyici özel telemetri gönderin. 
- Windows Azure tanılama (WAD) uzantıyı yükleyin, [Azure VM](collect-custom-metrics-guestos-resource-manager-vm.md), [sanal makine ölçek kümesi](collect-custom-metrics-guestos-resource-manager-vmss.md), [Klasik VM](collect-custom-metrics-guestos-vm-classic.md), veya [KlasikbulutHizmetleri](collect-custom-metrics-guestos-vm-cloud-service-classic.md) ve performans sayaçları için Azure İzleyici gönderin. 
- Yükleme [InfluxData Telegraf aracı](collect-custom-metrics-linux-telegraf.md) gönderme ölçümleri kullanarak Azure İzleyici ve Azure Linux VM üzerinde eklenti çıktı.
- Özel ölçümler Gönder [doğrudan Azure İzleyici REST API'si için](../../azure-monitor/platform/metrics-store-custom-rest-api.md), `https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics`.

Azure İzleyici, her veri noktasının veya değeri bildirilen özel ölçümler gönderdiğinizde, aşağıdaki bilgileri içermelidir.

### <a name="authentication"></a>Kimlik Doğrulaması
Azure İzleyici için özel ölçümleri göndermek için geçerli bir Azure Active Directory (Azure AD) belirtecinde ölçüm gönderen varlık gerekiyor **taşıyıcı** isteği üstbilgisi. Geçerli bir taşıyıcı belirteç almak için desteklenen birkaç yolu vardır:
1. [Kimlikler Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Bir Azure kaynağının, bir VM gibi bir kimlik sağlar. Yönetilen hizmet kimliği (MSI), kaynakları belirli işlemleri gerçekleştirmek için izin vermek için tasarlanmıştır. Örneği, kendisi hakkında ölçümler yaymak bir kaynak izin verir. Bir kaynak veya kendi MSI verilebilir **izleme ölçümleri yayımcı** izinlerini başka bir kaynak. Bu izne sahip ölçümleri de diğer kaynaklar için MSI gönderebilir.
2. [Azure AD hizmet sorumlusu](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals). Bu senaryo, bir Azure AD uygulaması veya hizmeti, bir Azure kaynağı ile ilgili ölçümleri yaymak için izinler atanabilir.
İstek kimliğini doğrulamak için Azure AD genel anahtarlar kullanılarak Azure İzleyici uygulama belirtecini doğrular. Varolan **izleme ölçümleri yayımcı** rolü zaten bu izne sahiptir. Azure portalında kullanılabilir. Hizmet sorumlusu, özel ölçümler, kendisini çıkaran derlemeninkinden hangi kaynaklara bağlı olarak verilen **izleme ölçümleri yayımcı** gerekli kapsamında rol. Abonelik, kaynak grubu ya da belirli bir kaynağa verilebilir.

> [!NOTE]  
> Özel ölçümler yaymak için bir Azure AD belirteç isterken kitle veya kaynak için istenen belirteç olmasına https://monitoring.azure.com/. Sondaki eklediğinizden emin olun '/'.

### <a name="subject"></a>Subject
Bu özellik, özel bir ölçü için bildirilen hangi Azure kaynak Kimliğini yakalar. Bu bilgiler yapılan API çağrısı URL'de kodlamalı. Her API, yalnızca tek bir Azure kaynak için ölçüm değerleri gönderebilirsiniz.

> [!NOTE]  
> Bir kaynak grubuna veya aboneliğe kaynak Kimliğine karşı özel ölçümler yayılamıyor.
>
>

### <a name="region"></a>Bölge
Bu özellik, hangi Azure bölgesi için ölçümleri yayma kaynağın dağıtıldığı yakalar. Aynı Azure İzleyici'nin bölgesel uç nokta kaynak bölge olarak dağıtıldığı için ölçümleri yayılan gerekir. Örneğin, Batı ABD bölgesinde dağıtılan bir sanal makine için özel ölçümleri WestUS bölgesel Azure İzleyici uç noktasına gönderilmesi gerekir. Bölge bilgileri de API çağrısı URL'de kodlanır.

> [!NOTE]  
> Genel Önizleme sırasında özel ölçümler yalnızca Azure bölgelerin alt kümesinde kullanılabilir. Desteklenen bölgelerin listesi bu makalenin sonraki bir bölümde belgelenmiştir.
>
>

### <a name="timestamp"></a>Zaman damgası
Azure İzleyici gönderilen her veri noktası, bir zaman damgası ile işaretlenmelidir. Bu zaman damgası, ölçüm değeri ölçülen veya toplanan DateTime yakalar. Azure İzleyici ölçüm verileri Geçmiş ve gelecekteki 5 dakikada 20 dakika sunulan ürünün kendinde zaman damgalı kabul eder. Zaman damgası ISO 8601 biçiminde olması gerekir.

### <a name="namespace"></a>Ad alanı
Ad alanları, kategorilere veya benzer ölçümler gruplamak için bir yoludur. Ad alanlarını kullanarak farklı öngörüleri ya da performans göstergeleri toplayabilir ve ölçüm grupları arasında yalıtım elde edebilirsiniz. Örneğin, adlı bir ad alanına sahip olabileceğiniz **ContosoMemoryMetrics** bellek kullanım ölçümleri, bir uygulama profili izler. Adlı başka bir ad alanı **ContosoAppTransaction** uygulamanızdaki kullanıcı işlemleri ile ilgili tüm ölçümleri izleyebilir.

### <a name="name"></a>Ad
**Adı** bildirildiğinden ölçümü adıdır. Genellikle, ölçülen belirlemenize yardımcı olması için tanımlayıcı adıdır. Belirli bir sanal Makinenin üzerinde kullanılan bellek bayt sayısını ölçer bir ölçüm buna bir örnektir. Bir ölçüm adı gibi olabilir **bellek bayt kullanımda**.

### <a name="dimension-keys"></a>Boyut anahtarlarını
Bir boyut, toplanmakta olan ölçüm hakkında ek özelliklerini açıklayan yardımcı olan bir anahtar veya değer çiftidir. Ek özelliklerini kullanarak daha fazla bilgi için daha kapsamlı içgörüler sağlayan ölçüm toplayabilirsiniz. Örneğin, **bellek bayt kullanımda** ölçüm adlı bir boyut anahtar olabilir **işlem** her işlem bir VM'de kaç bayt bellek tüketir yakalar. Bu anahtarı kullanarak belirli işlemlerin ne kadar bellek görmek veya bellek kullanımı üst beş işlemleri tanımlamak için ölçüm filtreleyebilirsiniz.
Boyutlar isteğe bağlı olarak, tüm ölçüleri, boyutları olabilir. Özel ölçüm en fazla 10 boyuta sahip olabilir.

### <a name="dimension-values"></a>Boyut değerleri
Her boyut anahtarı raporlanan, ölçüm için bir ölçüm veri noktası bildirirken karşılık gelen bir boyut değeri yoktur. Örneğin, vm'nizdeki ContosoApp tarafından kullanılan bellek rapor isteyebilirsiniz:

* Ölçüm adı **bellek bayt**.
* Boyut anahtar olacaktır **işlem**.
* Boyut değer **ContosoApp.exe**.

Ölçüm değeri yayımlama sırasında boyut anahtarı başına tek bir boyut değeri yalnızca belirtebilirsiniz. VM'de birden çok işlem için aynı bellek kullanımını toplamak, zaman damgası için birden çok ölçüm değerleri raporlayabilirsiniz. Her bir ölçüm değeri farklı boyut değeri belirtebilirdiniz **işlem** boyutu anahtarı.
Boyutlar isteğe bağlı olarak, tüm ölçüleri, boyutları olabilir. Bir ölçüm gönderi boyut anahtarlarını tanımlıyorsa, ilgili boyut değerleri zorunludur.

### <a name="metric-values"></a>Ölçüm değerleri
Azure İzleyici, bir dakikalık ayrıntı aralıklarla tüm ölçümler depolar. Belirli bir dakika birkaç kez örneğinin alınıp bir ölçüm gerekebileceğini biliyoruz. CPU kullanımı bir örnektir. Veya, birçok ayrı olayları ölçülmesi gerekebilir. Oturum açma işlemi gecikmeleri buna bir örnektir. Azure İzleyici'de buna göre ödeme yapmanız yayma sahip ham değerler sayısını sınırlamak için yerel olarak önceden toplama ve değerleri yayma:

* **Min**: En düşük değerini tüm örnekleri ve ölçümleri dakikasındaki gözlemledik.
* **En fazla**: En büyük değerini tüm örnekleri ve ölçümleri dakikasındaki gözlemledik.
* **Sum**: Tüm örnekleri ve ölçümleri gözlemlenen tüm değerler toplamı dakikada.
* **Sayısı**: Örnekler ve dakikasındaki alınan ölçümlere sayısı.

Örneğin, 4 işlem sırasında uygulamanız için oturum açma oluşturduysanız bir dakikalık göz önünde bulundurulduğunda, ortaya çıkan ölçülen gecikme süreleriyle her aşağıdaki gibi olabilir:

|İşlem 1|İşlem 2|İşlem 3|Hareket 4|
|---|---|---|---|
|7 ms|4 ms|13 ms|16 ms|
|

Ardından Azure İzleyici ölçüm elde edilen yayına şu şekilde olacaktır:
* En küçük: 4
* En fazla: 16
* Toplama: 40
* Sayısı: 4

Uygulamanızı yerel olarak önceden toplama belirleyemez ve her bir ayrık örnek veya olay toplama üzerine hemen yayması gerekir, ham ölçüm değerlerini gönderebilir. Örneğin, her bir oturum açma işlemi oluştuğunda uygulamanız üzerinde yalnızca tek bir ölçümü ile Azure İzleyici için bir ölçüm yayımlayın. Dolayısıyla, 12 ms sürdü bir oturum açma işlemi için ölçüm yayının şu şekilde olacaktır:
* En küçük: 12
* En fazla: 12
* Toplama: 12
* Sayısı: 1

Bu işlem ile aynı Ölçüm yanı sıra belirli bir dakika boyunca boyut birleşimi için birden çok değer gönderebilir. Azure İzleyici belirli bir dakika yayılan ham değerlerini alır ve bunları bir araya toplar.

### <a name="sample-custom-metric-publication"></a>Örnek özel ölçüm yayını
Aşağıdaki örnekte adlı özel bir ölçü oluşturma **bellek bayt** ölçüm ad alanı altında **bellek profili** bir sanal makine için. Ölçüm adı verilen tek boyutlu sahip **işlem**. Belirtilen zaman damgasından için biz iki farklı işlemler için ölçüm değerlerini göstermiyor:

```json
{
    "time": "2018-08-20T11:25:20-7:00",
    "data": {

      "baseData": {

        "metric": "Memory Bytes in Use",
        "namespace": "Memory Profile",
        "dimNames": [
          "Process"        ],
        "series": [
          {
            "dimValues": [
              "ContosoApp.exe"
            ],
            "min": 10,
            "max": 89,
            "sum": 190,
            "count": 4
          },
          {
            "dimValues": [
              "SalesApp.exe"
            ],
            "min": 10,
            "max": 23,
            "sum": 86,
            "count": 4
          }
        ]
      }
    }
  }
```
> [!NOTE]  
> Application Insights, tanılama uzantısını ve InfluxData Telegraf Aracısı zaten ölçüm değerleri karşı doğru bölgesel uç noktaya yayan ve yukarıdaki özelliklerin tümü her Emisyonu yürütmek için yapılandırılır.
>
>

## <a name="custom-metric-definitions"></a>Özel ölçüm tanımları
Özel ölçüm Azure İzleyici'de, yayılan önce ön tanımlamasında gerek yoktur. Yayımlanan her bir ölçüm veri noktası ad alanı adı ve boyut bilgileri içerir. Azure İzleyici yayılan ilk kez özel ölçüm için bir ölçüm tanımını otomatik olarak oluşturulur. Bu ölçüm tanımı ardından ölçüm karşı ölçüm tanımlarını yayıldığını herhangi bir kaynak üzerinde bulunabilir.

> [!NOTE]  
> Azure İzleyici, tanımlama henüz desteklemiyor **birimleri** özel ölçüm için.

## <a name="using-custom-metrics"></a>Özel ölçümler kullanma
Özel ölçümler, Azure İzleyici gönderildikten sonra bunları Azure portalı üzerinden göz atabilir ve bunları Azure İzleyici REST API'leri sorgulayabilirsiniz. Ayrıca bunlar üzerinde belirli koşullar karşılandığında size bildirilmesini uyarı oluşturabilir.
### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Özel ölçümleriniz Azure portal aracılığıyla göz atın
1.  [Azure Portal](https://portal.azure.com) gidin.
2.  Seçin **İzleyici** bölmesi.
3.  **Ölçümler**’i seçin.
4.  Özel ölçümler karşı yayılan bir kaynak seçin.
5.  Ölçümleri için ad alanı, özel bir ölçüm seçin.
6.  Özel ölçüm seçin.

## <a name="supported-regions"></a>Desteklenen bölgeler
Genel Önizleme sırasında özel ölçümler yayımlama olanağı yalnızca bir alt kümesini Azure bölgeleri içinde kullanılabilir. Bu kısıtlama, desteklenen bölgelerden birine kaynaklar için yalnızca ölçümleri yayımlanabilir anlamına gelir. Aşağıdaki tabloda, özel ölçümler için desteklenen Azure bölgeleri kümesini listeler. Ayrıca, bu bölgelerdeki kaynaklara yönelik ölçümleri için yayımlanmasına karşılık gelen uç noktaları listeler:

|Azure bölgesi|Bölgesel uç noktası ön eki|
|---|---|
|Doğu ABD| https:\//eastus.monitoring.azure.com/ |
|Orta Güney ABD| https:\//southcentralus.monitoring.azure.com/ |
|Batı Orta ABD| https:\//westcentralus.monitoring.azure.com/ |
|Batı ABD 2| https:\//westus2.monitoring.azure.com/ |
|Güneydoğu Asya| https:\//southeastasia.monitoring.azure.com/ |
|Kuzey Avrupa| https:\//northeurope.monitoring.azure.com/ |
|Batı Avrupa| https:\//westeurope.monitoring.azure.com/ |

## <a name="quotas-and-limits"></a>Kotalar ve sınırlar
Azure İzleyici, özel ölçümler üzerinde aşağıdaki kullanım sınırlarını getirir:

|Kategori|Sınır|
|---|---|
|Etkin zaman serisi/abonelikler/bölge|50,000|
|Boyut anahtarlarını ölçüm başına|10|
|Ölçüm ad alanları, ölçüm adları, boyut anahtarlarını ve boyut değerlerinin dize uzunluğu|256 karakter|

Etkin zaman serisi, ölçüm, boyut anahtar veya son 12 saat içinde yayımlanan ölçüm değerleri olan boyut değeri benzersiz herhangi bir birleşimini olarak tanımlanır.

## <a name="next-steps"></a>Sonraki adımlar
Farklı Hizmetleri özel ölçümleri kullanın: 
 - [Sanal Makineler](collect-custom-metrics-guestos-resource-manager-vm.md)
 - [Sanal makine ölçek kümesi](collect-custom-metrics-guestos-resource-manager-vmss.md)
 - [Azure sanal makineler (Klasik)](collect-custom-metrics-guestos-vm-classic.md)
 - [Telegraf Aracısı'nı kullanarak Linux sanal makinesi](collect-custom-metrics-linux-telegraf.md)
 - [REST API](../../azure-monitor/platform/metrics-store-custom-rest-api.md)
 - [Klasik bulut Hizmetleri](collect-custom-metrics-guestos-vm-cloud-service-classic.md)
 
