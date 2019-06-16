---
title: Otomatik ölçeklendirme ve App Service ortamı v1 - Azure
description: Otomatik ölçeklendirme ve App Service ortamı
services: app-service
documentationcenter: ''
author: btardif
manager: erikre
editor: ''
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 6660aa4e21aa36dc94c4ed9201fecb5637dddb3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65955972"
---
# <a name="autoscaling-and-app-service-environment-v1"></a>Otomatik ölçeklendirme ve App Service ortamı v1

> [!NOTE]
> Bu makale, App Service ortamı v1 hakkında yöneliktir.  App Service ortamı, kullanımı daha kolay ve daha güçlü bir altyapı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm başlama hakkında daha fazla bilgi edinmek için [App Service ortamı giriş](intro.md).
> 

Azure App Service ortamları Destek *otomatik ölçeklendirme*. Ölçümleri veya zamanlamaya göre otomatik ölçeklendirme çalışan havuzlarını ayrı ayrı geçirebilirsiniz.

![Bir çalışan havuzu için otomatik ölçeklendirme seçenekleri.][intro]

Otomatik ölçeklendirme, kaynak kullanımını otomatik olarak büyüyen ve küçültme bütçenize uygun ve profili yüklemek için bir App Service ortamı iyileştirir.

## <a name="configure-worker-pool-autoscale"></a>Çalışan havuzu otomatik ölçeklendirmeyi yapılandırma
Otomatik ölçeklendirme işlevinden erişebileceğiniz **ayarları** çalışan havuzunu sekmesi.

![Çalışan havuzu ayarları sekmesi.][settings-scale]

Buradan arabirimi olmalıdır, bir App Service planı ölçek gördüğünüz aynı olduğundan oldukça tanıdık deneyimidir. 

![El ile ölçek ayarları.][scale-manual]

Otomatik ölçeklendirme profili de yapılandırabilirsiniz.

![Otomatik ölçeklendirme ayarları.][scale-profile]

Otomatik ölçeklendirme profilleri ölçek sınırlarını ayarlamak yararlıdır. Bu şekilde, bir alt sınırı ölçek değeri (1) ve tahmin edilebilir bir harcama sınırına bir üst sınır (2) ayarlayarak ayarlayarak deneyimi tutarlı bir performans olabilir.

![Ölçek ayarları profilinde.][scale-profile2]

Bir profili tanımladıktan sonra Yukarı veya aşağı profiliyle tanımlanan sınırları içinde çalışan havuzunda örnek sayısını ölçeklendirmek için otomatik ölçeklendirme kuralları ekleyebilirsiniz. Otomatik ölçeklendirme kuralları, ölçümlere ilişkin temel alır.

![Ölçek kuralı.][scale-rule]

 Herhangi bir çalışan havuzu veya ön uç ölçümleri otomatik ölçeklendirme kurallarını tanımlamak için kullanılabilir. Bu ölçümler kaynak dikey penceresinde grafiklerde izlemek ya da uyarılar için aynı ölçümleridir.

## <a name="autoscale-example"></a>Otomatik ölçeklendirme örnek
Bir App Service ortamında otomatik ölçeklendirme, en iyi bir senaryoyu adım anlatarak tarafından gösterilebilir.

Otomatik ölçeklendirme ' ayarladığınızda bu makalede, gerekli tüm konuları açıklanmaktadır. Makale App Service Ortamı'nda barındırılan App Service ortamları otomatik ölçeklendirmeyi faktörü yükleyen oyuna gelen etkileşimler size kılavuzluk eder.

### <a name="scenario-introduction"></a>Senaryo giriş
Frank bir kısmını yönettikleri iş yükleri için App Service ortamı geçirildikten bir kuruluş için bir sysadmin ' dir.

App Service ortamı için el ile ölçeklendirmenin aşağıdaki gibi yapılandırılır:

* **Ön uçlar:** 3
* **Çalışan havuzu 1**: 10
* **Çalışan havuzu 2**: 5
* **Çalışan havuzu 3**: 5

Çalışan havuzu 1, 2 çalışan havuzu ve çalışan havuzunu 3 kalite güvencesi (kapsayan QA) ve geliştirme iş yükleri için kullanılabilir ancak üretim iş yükleri için kullanılır.

App Service planları QA ve geliştirme için el ile ölçeklendirme için yapılandırılır. App Service planı üretim yükünü ve trafik farklılığı uğraşmanız otomatik ölçeklendirme için ayarlanır.

Frank uygulama ile tanıdık. Bunlar, bu çalışanlar ofiste oldukları sırada kullanan bir satır iş kolu (LOB) uygulaması olduğundan yoğun yük 9: 00'da ve 18:00:00 arasında olduğundan emin olabilirsiniz. Kullanıcı o gün için tamamladıktan sonra kullanım bırakır. Yoğun saatler dışında yoktur hala bazı yük çünkü kullanıcılar uygulamayı uzaktan kendi mobil cihazlarını veya ev bilgisayarları kullanarak erişebilirsiniz. App Service planı üretim, aşağıdaki kurallar ile CPU kullanımı temel alan otomatik ölçeklendirme için zaten yapılandırıldı:

![LOB uygulaması için özel ayarlar.][asp-scale]

| **Otomatik ölçeklendirme profili – haftanın günlerine – App Service planı** | **Otomatik ölçeklendirme profili – hafta sonları – App Service planı** |
| --- | --- |
| **Adı:** Haftanın günü profili |**Adı:** Hafta sonu profili |
| **Tarafından ölçeğini genişletin:** Zamanlama ve performans kuralları |**Tarafından ölçeğini genişletin:** Zamanlama ve performans kuralları |
| **Profil:** Haftanın günü |**Profil:** Hafta sonu |
| **Türü:** Yinelenme |**Türü:** Yinelenme |
| **Hedef aralığı:** 5-20 örnekleri |**Hedef aralığı:** 3 ila 10 örnekleri |
| **Gün sayısı:** Monday, Tuesday, Wednesday, Thursday, Friday |**Gün sayısı:** Pazar Cumartesi |
| **Başlangıç zamanı:** 9: 00'DA |**Başlangıç zamanı:** 9: 00'DA |
| **Saat dilimi:** UTC-08 |**Saat dilimi:** UTC-08 |
|  | |
| **Otomatik ölçek kuralını (ölçeği Artır)** |**Otomatik ölçek kuralını (ölçeği Artır)** |
| **Kaynak:** Üretim (App Service ortamı) |**Kaynak:** Üretim (App Service ortamı) |
| **Ölçüm:** CPU % |**Ölçüm:** CPU % |
| **İşlem:** % 60 daha büyük |**İşlem:** % 80 daha büyük |
| **Süresi:** 5 Dakika |**Süresi:** 10 dakika |
| **Zaman toplama:** Ortalama |**Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı şu kadar 2 Artır |**Eylem:** 1 ile sayıyı Artır: |
| **Seyrek erişimli (dakika):** 15 |**Seyrek erişimli (dakika):** 20 |
|  | |
| **Otomatik ölçek kuralını (ölçeği aşağı)** |**Otomatik ölçek kuralını (ölçeği aşağı)** |
| **Kaynak:** Üretim (App Service ortamı) |**Kaynak:** Üretim (App Service ortamı) |
| **Ölçüm:** CPU % |**Ölçüm:** CPU % |
| **İşlem:** Değerinden % 30'luk |**İşlem:** % 20'den az |
| **Süresi:** 10 dakika |**Süresi:** 15 dakika |
| **Zaman toplama:** Ortalama |**Zaman toplama:** Ortalama |
| **Eylem:** 1 Azalt |**Eylem:** 1 Azalt |
| **Seyrek erişimli (dakika):** 20 |**Seyrek erişimli (dakika):** 10 |

### <a name="app-service-plan-inflation-rate"></a>App Service planı Enflasyon oranı
Yapılandırılan app Service planları otomatik ölçeklendirme için en fazla saatte bunu yapın. Bu oran göre otomatik ölçeklendirme kuralının sağlanan değerlere hesaplanabilir.

Anlama ve hesaplama *App Service planı Enflasyon oranı* çalışan havuzunda ölçeği değişiklikler anlık olmadığı için App Service ortamında otomatik ölçeklendirme için önemlidir.

App Service planı Enflasyon oranı şu şekilde hesaplanır:

![App Service planı Enflasyon oran hesaplaması.][ASP-Inflation]

Otomatik ölçeklendirme – ölçeği Artır kural App Service planı üretim iş günü profili için temel:

![App Service planı Enflasyon oranı – otomatik ölçek ölçeği Artır kural tabanlı haftanın günü.][Equation1]

Otomatik ölçeklendirme – ölçeği Artır kuralı için App Service planı, üretim hafta profilini söz konusu olduğunda formülü çözümlenmesi:

![App Service planı Enflasyon oranı – otomatik ölçek ölçeği Artır kural tabanlı hafta sonu.][Equation2]

Bu değer de ölçek azaltma işlemleri için hesaplanabilir.

Otomatik ölçeklendirme – App Service planı, üretim iş günü profilini için ölçeği aşağı kuralı göre bunu şu şekilde görünür:

![App Service planı Enflasyon oranı – otomatik ölçek ölçeği aşağı kural tabanlı haftanın günü.][Equation3]

Otomatik ölçeklendirme – ölçeği aşağı kuralı için App Service planı, üretim hafta profilini söz konusu olduğunda formülü çözümlenmesi:  

![App Service planı Enflasyon oranı – otomatik ölçek ölçeği aşağı kural tabanlı hafta sonu.][Equation4]

App Service planı üretim sekiz örnekleri/saat hafta boyunca en fazla fiyatı ve dört örnek/saat hafta boyunca büyüyebilir. En fazla dört örnek/saat hafta sırasında örnekleri ve hafta sonları gibi altı örnekleri saatlik serbest bırakabilirsiniz.

Çalışan havuzunda birden fazla App Service planları barındırılan, hesaplamak zorunda *toplam Enflasyon oranı* gönderildiğini tüm App Service planları için Enflasyon hızı toplamı olarak, çalışan havuzunda barındırma.

![Çalışan havuzunda barındırılan birden fazla App Service planları için toplam Enflasyon oran hesaplaması.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Çalışan havuzu otomatik ölçeklendirme kurallarını tanımlamak için App Service planı Enflasyon oranı kullanın
Otomatik ölçeklendirme için yapılandırılmış olan App Service planları kapasitesinin bir arabellek ayrılması gereken barındıran çalışan havuzları. Büyümek ve App Service planı gerektiği şekilde küçültmek otomatik ölçeklendirme işlemleri için arabellek sağlar. En küçük arabellek hesaplanan toplam uygulama hizmeti planı Enflasyon oranı olacaktır.

App Service ortamı ölçeklendirme işlemleri uygulamak için biraz zaman tuttuğundan, herhangi bir değişiklik başka bir ölçeklendirme işlemi devam ederken, oluşabilir talebi değiştikçe hesabı. Bu gecikme süresi uyum sağlamak için her otomatik ölçeklendirme işlemi için en az eklenen örnek sayısı hesaplanan toplam uygulama hizmeti planı Enflasyon oranı kullanmanızı öneririz.

Bu bilgileri kullanarak, aşağıdaki otomatik ölçeklendirme profilini ve kurallarını Frank tanımlayabilirsiniz:

![LOB örnek için otomatik ölçeklendirme profili kuralları.][Worker-Pool-Scale]

| **Otomatik ölçeklendirme profili – haftanın günü** | **Otomatik ölçeklendirme profili – hafta sonları** |
| --- | --- |
| **Adı:** Haftanın günü profili |**Adı:** Hafta sonu profili |
| **Tarafından ölçeğini genişletin:** Zamanlama ve performans kuralları |**Tarafından ölçeğini genişletin:** Zamanlama ve performans kuralları |
| **Profil:** Haftanın günü |**Profil:** Hafta sonu |
| **Türü:** Yinelenme |**Türü:** Yinelenme |
| **Hedef aralığı:** 13-25 örnekleri |**Hedef aralığı:** 6-15 örnekleri |
| **Gün sayısı:** Monday, Tuesday, Wednesday, Thursday, Friday |**Gün sayısı:** Pazar Cumartesi |
| **Başlangıç zamanı:** 7: 00'DA |**Başlangıç zamanı:** 9: 00'DA |
| **Saat dilimi:** UTC-08 |**Saat dilimi:** UTC-08 |
|  | |
| **Otomatik ölçek kuralını (ölçeği Artır)** |**Otomatik ölçek kuralını (ölçeği Artır)** |
| **Kaynak:** Çalışan havuzu 1 |**Kaynak:** Çalışan havuzu 1 |
| **Ölçüm:** WorkersAvailable |**Ölçüm:** WorkersAvailable |
| **İşlem:** 8'den küçük |**İşlem:** 3'ten az |
| **Süresi:** 20 dakika |**Süresi:** 30 dakika |
| **Zaman toplama:** Ortalama |**Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı şu kadar 8 Artır |**Eylem:** Sayıyı şu kadar 3 Artır |
| **Seyrek erişimli (dakika):** 180 |**Seyrek erişimli (dakika):** 180 |
|  | |
| **Otomatik ölçek kuralını (ölçeği aşağı)** |**Otomatik ölçek kuralını (ölçeği aşağı)** |
| **Kaynak:** Çalışan havuzu 1 |**Kaynak:** Çalışan havuzu 1 |
| **Ölçüm:** WorkersAvailable |**Ölçüm:** WorkersAvailable |
| **İşlem:** 8 büyüktür |**İşlem:** 3'ten büyük |
| **Süresi:** 20 dakika |**Süresi:** 15 dakika |
| **Zaman toplama:** Ortalama |**Zaman toplama:** Ortalama |
| **Eylem:** 2 Azalt |**Eylem:** 3 Azalt |
| **Seyrek erişimli (dakika):** 120 |**Seyrek erişimli (dakika):** 120 |

Hedef aralığın profilinde tanımlanan, App Service planı + arabellek profilinde tanımlanan en düşük örnekleri tarafından hesaplanır.

En büyük aralık çalışan havuzunda barındırılan tüm App Service planları için en yüksek olan tüm aralıkları toplamını olacaktır.

Sayıyı Artır kurallarını ölçek için en az 1 X ölçek için App Service planı Enflasyon oranı ayarlanmalıdır.

Azaltma sayısı için 1/2 X veya ölçek için uygulama hizmeti planı Enflasyon oranı X 1 arasında aşağı ayarlanabilir.

### <a name="autoscale-for-front-end-pool"></a>Ön uç havuzu için otomatik ölçeklendirme
Ön uç otomatik ölçeklendirme kuralları için çalışan havuzları daha basittir. Öncelikle, yapmanız gerekenler  
Bu süre ölçüm emin olun ve dakikaysa zamanlayıcılar bir App Service planı ölçeklendirme işlemleri anlık olmadığını göz önünde bulundurun.

Bu senaryo için hata oranı % 80 CPU kullanımı ön uçlar ulaştıktan sonra artırır ve örnekleri aşağıdaki şekilde artırmak için otomatik ölçeklendirme kuralının ayarlar Frank bilir:

![Ön uç havuzu için otomatik ölçeklendirme ayarları.][Front-End-Scale]

| **Otomatik ölçeklendirme profili – ön Uçlar** |
| --- |
| **Adı:** Otomatik ölçeklendirmeyi ön Uçlar |
| **Tarafından ölçeğini genişletin:** Zamanlama ve performans kuralları |
| **Profil:** her gün |
| **Türü:** Yinelenme |
| **Hedef aralığı:** 3 ila 10 örnekleri |
| **Gün sayısı:** her gün |
| **Başlangıç zamanı:** 9: 00'DA |
| **Saat dilimi:** UTC-08 |
|  |
| **Otomatik ölçek kuralını (ölçeği Artır)** |
| **Kaynak:** Ön uç havuzu |
| **Ölçüm:** CPU % |
| **İşlem:** % 60 daha büyük |
| **Süresi:** 20 dakika |
| **Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı şu kadar 3 Artır |
| **Seyrek erişimli (dakika):** 120 |
|  |
| **Otomatik ölçek kuralını (ölçeği aşağı)** |
| **Kaynak:** Çalışan havuzu 1 |
| **Ölçüm:** CPU % |
| **İşlem:** Değerinden % 30'luk |
| **Süresi:** 20 dakika |
| **Zaman toplama:** Ortalama |
| **Eylem:** 3 Azalt |
| **Seyrek erişimli (dakika):** 120 |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
