---
title: Azure İzleyicisi'nde özel ölçümler
description: Azure İzleyici ve nasıl modellenir özel ölçümler hakkında bilgi edinin.
author: ancav
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 1bdf1e1f5e58ecb0939d5876e0cef349e32de517
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344768"
---
# <a name="custom-metrics-in-azure-monitor"></a>Azure İzleyicisi'nde özel ölçümler

Kaynakları ve uygulamaları azure'da dağıtmak gibi performans ve sistem durumu hakkında Öngörüler elde etmek için telemetri toplamaya başlamak isteyebilirsiniz. Kaynakları dağıtma gibi azure bazı ölçüm kullanılabilir, kullanıma hazır hale getirir. Bu, standart veya platform ölçüm adı verilir. Ancak, bu ölçümleri doğası gereği sınırlıdır. Bazı özel performans göstergelerinizi ya da daha ayrıntılı Öngörüler sağlamak için işe özgü ölçümleri toplamak isteyebilirsiniz.
"Özel" Bu ölçümleri, uygulama telemetrinizi, Azure kaynaklarını veya hatta dışarıdan içeriye izleme sistemi üzerinde çalışan bir aracının aracılığıyla toplanan olabilir ve doğrudan Azure İzleyici için gönderildi. Azure İzleyici yayımlandıktan sonra göz atın, sorgu ve Azure tarafından yayılan standart ölçümler, Azure kaynaklarını ve yan yana uygulamalar için özel ölçümleri uyar.

## <a name="send-custom-metrics"></a>Özel ölçümler Gönder
Özel ölçümler, Azure İzleyici birçok farklı yöntemle aracılığıyla gönderilebilir.
- Application Insights SDK'sını kullanarak uygulamanızı izleme ve özel telemetri göndermek için Azure İzleyici 
- Windows Tanılama uzantısını yükleyin, [Azure VM](metrics-store-custom-guestos-resource-manager-vm.md), [sanal makine ölçek kümesi](metrics-store-custom-guestos-resource-manager-vmss.md), [Klasik VM](metrics-store-custom-guestos-classic-vm.md), veya [Klasik bulut hizmeti](metrics-store-custom-guestos-classic-cloud-service.md)ve performans sayaçları göndermek için Azure İzleyici 
- Yükleme [InfluxDB Telegraf aracı](metrics-store-custom-linux-telegraf.md) Azure Linux VM ve Azure İzleyici çıkış eklentisini kullanarak gönderme ölçümleri
- Özel ölçümler Gönder [doğrudan Azure İzleyici REST API'si için](metrics-store-custom-rest-api.md) https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics

Özel ölçümleriniz Azure İzleyici gönderdiğinizde, her veri noktası (veya değer) bildirilen aşağıdaki bilgileri içermelidir:

### <a name="authentication"></a>Kimlik Doğrulaması
Azure İzleyici için özel ölçümleri göndermek için ölçüm gönderme varlık geçerli bir Azure Active Directory belirteci "Bearer" istek üst bilgisinde olmalıdır. Geçerli bir taşıyıcı belirteç almak için desteklenen birkaç yolu vardır:
1. [Kimlikler Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) -bir Azure kaynağının (örneğin, bir VM) bir kimlik sağlar. MSI kaynakları belirli işlemleri – izin vermek için örneğin, kendisi hakkında ölçümler yaymak bir kaynak sağlayan tasarlanmıştır. Bir kaynak (veya kendi MSI) "İzleme ölçümleri Publisher" izinlerini başka bir kaynak, böylelikle de diğer kaynakların ölçümlerini yaymak MSI etkinleştirme verilebilir.
2. [AAD hizmet sorumlusu](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) -burada bir AAD uygulaması (hizmet) bir Azure kaynağı ile ilgili ölçümleri yaymak için izinler atanabilir bir senaryodur.
İsteğin kimliğini doğrulamak için Azure İzleyici AAD ortak anahtarları kullanarak uygulama belirtecini doğrular. Azure portalında kullanılabilir olan bu izin, mevcut "İzleme ölçümleri yayımcı" rolü zaten var. Hizmet sorumlusu, özel ölçümler yayma hangi kaynaklara bağlı olarak gereken kapsam (abonelik, kaynak grubuna ya da belirli kaynak) "İzleme ölçümleri Publisher" rolü verilebilir.

> [!NOTE]
> Ne zaman isteyen özel ölçümler yaymak için bir AAD belirteci sağlamak belirteç istenen kaynak için İzleyici/kaynak https://monitoring.azure.com/ (sondaki eklediğinizden emin olun '/')

### <a name="subject"></a>Özne
Bu özellik, özel bir ölçü için bildirilen hangi Azure kaynak Kimliğini yakalar. Bu bilgiler yapılan API çağrısı URL'de kodlamalı. Her API, yalnızca tek bir Azure kaynak için ölçüm değerleri gönderebilirsiniz.

> [!NOTE]
> Bir kaynak grubuna veya aboneliğe kaynak Kimliğine karşı özel ölçümler yayılamıyor.
>
>

### <a name="region"></a>Bölge
Bu özellik, hangi Azure bölgesi için ölçümleri yayma kaynağın dağıtıldığı yakalar. Aynı Azure İzleyici'nin bölgesel uç nokta kaynak bölge olarak dağıtıldığı için ölçümleri yayılan gerekir. Örneğin, Batı ABD bölgesinde bir VM dağıtılır için özel ölçümleri WestUS bölgesel Azure İzleyici uç noktasına gönderilmesi gerekir. Bölge bilgileri de API çağrısı URL'de kodlanır.

> [!NOTE]
> Genel Önizleme sırasında özel ölçümleri yalnızca Azure bölgelerin alt kümesinde kullanılabilir. Desteklenen bölgelerin listesi bu makalenin sonraki bir bölümde belgelenmiştir.
>
>

### <a name="timestamp"></a>Zaman damgası
Azure İzleyici gönderilen her veri noktası, bir zaman damgası ile işaretlenmelidir. Bu zaman damgasından ölçülür ve toplanan ölçüm değeri, datetime yakalar. Azure İzleyici, durum 20 dakika önce ve sonra 5 dakika sunulan ürünün kendinde zaman damgalı ölçüm verileri kabul eder.

### <a name="namespace"></a>Ad Alanı
Ad alanları, kategorilere veya benzer ölçümler gruplamak için bir yoludur. Ad alanları farklı öngörüleri ya da performans göstergeleri toplama, ölçüm grupları arasında yalıtım elde etmenizi sağlar. Örneğin, ad alanı olabilir *ContosoMemoryMetrics* uygulamanız ve başka bir ad alanı adlı profil diğer bir deyişle kullanılan izleme bellek kullanım ölçümleri *ContosoAppTransaction* tüm izler uygulamanızdaki kullanıcı işlemleri ile ilgili ölçümleri.

### <a name="name"></a>Ad
Bildirildiğinden ölçüm adı. Genellikle hangi ölçülen belirlemenize yardımcı olması için tanımlayıcı adıdır. Örneğin, belirli bir sanal Makinenin üzerinde kullanılan belleğin bayt sayısını ölçme bir ölçüm "Bellek bayt kullanımda" gibi bir ölçüm adı olabilir.

### <a name="dimension-keys"></a>Boyut anahtarlarını
Bir boyut, toplanmakta olan ölçüm hakkında ek özelliklerini açıklayan yardımcı olan bir anahtar/değer çifti var. Ek özellikleri için daha kapsamlı içgörüler sağlayan ölçümü hakkında daha fazla bilgi toplamayı etkinleştirin. Örneğin, "Bellek bayt kullanımda" ölçüm "her işlem bir VM'de kaç bayt bellek tüketiyor yakalar işlem" adlı bir boyut anahtar olabilir. Bu ölçüm, ne kadar bellek özgü işlemler kullandığını görün veya bellek kullanımı ilk 5 işlemleri tanımlamak için filtrelemenize olanak sağlar.
Her özel ölçüm en fazla 10 boyuta sahip olabilir.

### <a name="dimension-values"></a>Boyut Değerleri
Her boyut anahtar üzerinde bildirilen, ölçüm için bir ölçüm datapoint bildirirken karşılık gelen bir boyut değeri yoktur. Örneğin, vm'nizdeki ContosoApp tarafından kullanılan bellek bildirmek istiyorsanız:

* Ölçüm adı *bellek bayt yüzdesi*
* Boyut anahtar olacaktır *işlemi*
* Boyut değer *ContosoApp.exe*

Ölçüm değeri yayımlama sırasında boyut anahtarı başına tek bir boyut değeri yalnızca belirtebilirsiniz. VM'de birden çok işlem için aynı bellek kullanımını toplamak, zaman damgası için birden çok ölçüm değerleri raporlayabilirsiniz. Her bir ölçüm değeri, işlem boyut anahtar için farklı boyut değerini belirtmeniz gerekir.

### <a name="metric-values"></a>Ölçüm değerleri
Azure İzleyici, bir dakikalık ayrıntı aralıklarla tüm ölçümler depolar. Bir ölçüm birden çok kez (örn. örneklenen gerekebilir anlama CPU kullanımı) veya ölçülen birçok ayrı olayları (ör. İşlem gecikme süreleri oturum) belirli bir dakika boyunca. Azure İzleyici'de buna göre ödeme yapmanız yayma sahip ham değerler sayısını sınırlamak için yerel olarak önceden toplama ve değerleri yayma:

* En az: En düşük tüm örnekleri/ölçümler değerinden dakikasındaki gözlemlenen
* En fazla: Gözlemlenen en yüksek tüm örnekleri/ölçümler değerinden dakikada
* Toplamı: Dakikada tüm örnek/ölçümler gözlemlenen tüm değerleri özetleme
* Sayısı: Dakikada alınan samples/ölçümlere sayısını

Örneğin, 4 işlem sırasında uygulamanız için oturum açma oluşturduysanız bir dakika ve ortaya çıkan ölçülen gecikme süreleriyle her verilen:

|İşlem 1|İşlem 2|İşlem 3|Hareket 4|
|---|---|---|---|
|7 ms|4 ms|13 ms|16 ms|
|

Azure İzleyici ölçüm elde edilen yayına olacaktır:
* En düşük: 4
* En fazla: 16
* Toplam: 40
* Sayısı: 4

Uygulamanızı yerel olarak önceden toplama belirleyemez ve her bir ayrık örnek veya olay toplama üzerine hemen yayması gerekir, ham ölçüm değerlerini gönderebilir.
Örneğin, her zaman Azure İzleyici için bir ölçüm yalnızca tek bir ölçümü ile yayımlamak istediğiniz bir oturum açma işlemi uygulamanızı oluştu. Bu nedenle, 12 ms sonra ölçüm geçen bir oturum açma işlemi için yayın olacaktır:
* En düşük: 12
* En fazla: 12
* Toplam: 12
* Sayısı: 1

Bu işlem, birden çok değer için aynı Ölçüm + boyut birleşimi belirli bir dakika boyunca yaymak sağlar. Azure İzleyici, ardından belirli bir dakika yayılan ham değerler alır ve bunları bir araya toplama.

### <a name="sample-custom-metric-publication"></a>Örnek özel ölçüm yayını
Aşağıdaki örnekte, "Bellek bayt kullanımda", "Bellek profili" bir sanal makine ölçüm ad alanı altında adlı özel bir ölçü oluşturun. "İşlem" olarak adlandırılan tek boyutlu ölçüm vardır. Belirtilen zaman damgasından için biz ölçüm değerleri iki farklı işlemler için yayma:

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
> Application Insights, Windows Azure tanılama uzantısı ve InfluxData Telegraf Aracısı zaten ölçüm değerleri karşı doğru bölgesel uç noktaya yayan ve yukarıdaki özelliklerin tümü her Emisyonu yürütmek için yapılandırılır.
>
>

## <a name="custom-metric-definitions"></a>Özel ölçüm tanımları
Özel ölçüm Azure İzleyici'de, yayılan önce önceden tanımlamak için gerek yoktur. Özel ölçüm için Azure İzleyici yayıldığını ilk kez bir ölçüm tanımı, yayımlanan her bir ölçüm veri noktası ad alanı adı ve boyut bilgileri içerdiğinden, otomatik olarak oluşturulur. Bu ölçüm tanımı ardından ölçüm karşı ölçüm tanımlarını gösteriliyordu herhangi bir kaynak üzerinde bulunabilir.

> [!NOTE]
> Azure İzleyici, "Units" için özel bir ölçü tanımlama henüz desteklemiyor.

## <a name="using-custom-metrics"></a>Özel ölçümler kullanma
Özel ölçümleriniz Azure İzleyici gönderildikten sonra bunları Azure Portalı aracılığıyla göz atın, bunları Azure İzleyici REST API'leri sorgu veya bunlar üzerinde belirli koşullar karşılandığında bilgilendirilebilirsiniz için uyarılar oluşturun.
### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Özel ölçümleriniz Azure portal aracılığıyla göz atın
1.  [Azure portal](https://portal.azure.com)'a gidin
2.  İzleyici dikey penceresini seçin
3.  Ölçümler tıklayın
4.  Özel ölçümler karşı yayılan bir kaynak seçin
5.  Ölçümleri için ad alanı, özel bir ölçüm seçin
6.  Özel ölçüm seçin

## <a name="supported-regions"></a>Desteklenen bölgeler
Genel Önizleme sırasında özel ölçümler yayımlama olanağı yalnızca Azure bölgelerin alt kümesinde kullanılabilir. Bu ölçüm, desteklenen bölgelerden birine kaynaklar için yalnızca yayımlanabilir anlamına gelir. Özel ölçümler için desteklenen Azure bölgeleri kümesini ve bu bölgelerdeki kaynaklara yönelik ölçümleri için yayımlanmasına karşılık gelen uç noktası aşağıdaki tabloda listelenmiştir.

|Azure Bölgesi|Bölgesel uç noktası ön eki|
|---|---|
|Doğu ABD|https://eastus.monitoring.azure.com/|
|Orta Güney ABD|https://southcentralus.monitoring.azure.com/|
|Batı Orta ABD|https://westcentralus.monitoring.azure.com/|
|Batı ABD 2|https://westus2.monitoring.azure.com/|
|Güneydoğu Asya|https://southeastasia.monitoring.azure.com/|
|Kuzey Avrupa|https://northeurope.monitoring.azure.com/|
|Batı Avrupa|https://westeurope.monitoring.azure.com/|

## <a name="quotas-and-limits"></a>Kotalar ve sınırlar
Azure İzleyici, özel ölçümler üzerinde aşağıdaki kullanım sınırlarını uygular.

|Kategori|Sınır|
|---|---|
|Etkin zaman serisi/abonelikler/bölge|50,000|
|Boyut anahtarlarını ölçüm başına|10|
|Ölçüm ad alanları, ölçüm adları, boyut anahtarlarını ve boyut değerlerinin dize uzunluğu|256 karakter|
Etkin zaman serisi, ölçüm, boyut anahtarı, son 12 saat içinde yayımlanan ölçüm değerleri olan boyut değeri benzersiz herhangi bir birleşimi olarak tanımlanır.

## <a name="next-steps"></a>Sonraki adımlar
Farklı Hizmetleri özel ölçümleri kullanma 
 - [Sanal Makine](metrics-store-custom-guestos-resource-manager-vm.md)
 - [Sanal makine ölçek kümesi](metrics-store-custom-guestos-resource-manager-vmss.md)
 - [Sanal makine (Klasik)](metrics-store-custom-guestos-classic-vm.md)
 - [Telegraf aracı kullanarak Linux sanal makinesi](metrics-store-custom-linux-telegraf.md)
 - [REST API](metrics-store-custom-rest-api.md)
 - [Bulut hizmeti (Klasik)](metrics-store-custom-guestos-classic-cloud-service.md)
 