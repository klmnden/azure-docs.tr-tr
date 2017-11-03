---
title: "Otomatik ölçeklendirme ve uygulama hizmeti ortamı v1"
description: "Otomatik ölçeklendirme ve uygulama hizmeti ortamı"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 0feb6e4862f643c35a58c0321181bdda22b032e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a>Otomatik ölçeklendirme ve uygulama hizmeti ortamı v1

> [!NOTE]
> Bu makale hakkında uygulama hizmeti ortamı v1 yazılmıştır.  Uygulama hizmeti ortamı kullanmak daha kolay ve daha güçlü altyapısı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm Başlarken hakkında daha fazla bilgi edinmek için [uygulama hizmeti ortamı giriş](intro.md).
> 

Azure uygulama hizmeti ortamları desteği *otomatik ölçeklendirmeyi*. Ölçümleri veya zamanlamaya göre otomatik ölçeklendirme ayrı ayrı çalışan havuzlarını kullanabilirsiniz.

![Bir çalışan havuzu için otomatik ölçeklendirme seçenekleri.][intro]

Otomatik ölçeklendirmeyi kaynak kullanımını otomatik olarak büyüyen ve bütçenizi sığacak ve veya yük profili için bir uygulama hizmeti ortamı küçültme en iyi duruma getirir.

## <a name="configure-worker-pool-autoscale"></a>Çalışan havuzu otomatik ölçeklendirme yapılandırın
Otomatik ölçeklendirme işlevinden erişebilirsiniz **ayarları** çalışan havuzunda sekmesinde.

![Çalışan havuzunda Ayarlar sekmesinde.][settings-scale]

Buradan, arabirim olmalıdır ne zaman bir uygulama hizmeti planı ölçeklendirmek gördüğünüz aynı bu yana oldukça tanıdık deneyimidir. 

![El ile ölçek ayarları.][scale-manual]

Bir otomatik ölçeklendirme profili de yapılandırabilirsiniz.

![Otomatik ölçeklendirme ayarları.][scale-profile]

Otomatik ölçeklendirme profilleri, Ölçek sınırlarını ayarlamak yararlıdır. Bu şekilde, bir alt sınır ölçek değeri (1) ve tahmin edilebilir harcama cap bir üst sınır (2) ayarlayarak ayarlayarak deneyimi tutarlı bir performans olabilir.

![Ölçek ayarları profilinde.][scale-profile2]

Bir profili tanımladıktan sonra Yukarı veya aşağı profili tarafından tanımlanan sınırları içinde çalışan havuzunda örneklerinin sayısını ölçeklendirmek için otomatik ölçeklendirme kuralı ekleyebilirsiniz. Otomatik ölçeklendirme kurallarını ölçümleri temel alır.

![Ölçek kuralı.][scale-rule]

 Herhangi bir çalışan havuzu veya ön uç ölçümleri, otomatik ölçeklendirme kurallarını tanımlamak için kullanılabilir. Bu ölçümler kaynak dikey grafiklerde izlemek ya da uyarılar için aynı ölçümleridir.

## <a name="autoscale-example"></a>Otomatik ölçeklendirme örneği
Otomatik ölçeklendirme bir uygulama hizmeti ortamında en iyi bir senaryo adım adım ilerlemenizi sağlayarak gösterilebilir.

Otomatik ölçeklendirme ayarladığınızda, bu makalede gerekli tüm konuları açıklanmaktadır. Makale, uygulama hizmeti ortamı'nda barındırılan uygulama hizmeti ortamları otomatik ölçeklendirmeyi içinde faktörü yükleyen oyuna gelen etkileşimler açıklanmaktadır.

### <a name="scenario-introduction"></a>Senaryo giriş
Frank bir kısmı kendisi yönetir iş yükleri için uygulama hizmeti ortamı geçirildiğini bir kuruluş için bir sysadmin ' dir.

Uygulama hizmeti ortamı için el ile ölçek aşağıdaki gibi yapılandırılır:

* **Ön Uçları:** 3
* **Çalışan havuzunda 1**: 10
* **Çalışan havuzunda 2**: 5
* **Çalışan havuzunda 3**: 5

Çalışan havuzunda 2 ve 3 çalışan havuzunda kalite güvence (QA) ve geliştirme iş yükleri için kullanılan sırasında çalışan havuzu 1 üretim iş yükleri için kullanılır.

Uygulama hizmeti planları QA ve geliştirme için el ile ölçeklendirme için yapılandırılır. Uygulama hizmeti planı üretim için otomatik ölçeklendirme yükünü ve trafik Çeşitlemeler uğraşmanız ayarlanır.

Frank uygulama ile bilgili ' dir. Ki, bu çalışanlar ofiste oldukları sırada kullanan bir satır iş kolu (LOB) uygulaması olduğundan yük için yoğun saatler 9: 00'da ve 6:00 arasında olduğunu bilir. Kullanıcılar o gün için bittiğinde kullanım bundan sonra bırakır. Yoğun saatler dışında yoktur hala bazı yük kullanıcıların uygulamayı uzaktan kendi mobil aygıtlar veya ev bilgisayarları kullanarak erişebildiğinden. Uygulama hizmeti planı üretim aşağıdaki kurallar ile CPU kullanımına bağlı olarak otomatik ölçeklendirme için zaten yapılandırıldı:

![LOB uygulaması için özel ayarlar.][asp-scale]

| **Otomatik ölçeklendirme profili – haftanın günlerini – uygulama hizmeti planı** | **Otomatik ölçeklendirme profili – hafta sonları – uygulama hizmeti planı** |
| --- | --- |
| **Ad:** haftanın günü profili |**Ad:** hafta sonu profili |
| **Ölçek tarafından:** zamanlama ve performans kuralları |**Ölçek tarafından:** zamanlama ve performans kuralları |
| **Profil:** haftanın günü |**Profil:** hafta sonu |
| **Tür:** yineleme |**Tür:** yineleme |
| **Hedef aralık:** 5-20 örnekleri |**Hedef aralık:** 3-10 örnekleri |
| **Gün sayısı:** Pazartesi, Salı, Çarşamba, Perşembe, Cuma |**Gün sayısı:** Cumartesi, Pazar |
| **Başlangıç zamanı:** 09:00:00 |**Başlangıç zamanı:** 09:00:00 |
| **Saat dilimi:** UTC-08 |**Saat dilimi:** UTC-08 |
|  | |
| **Otomatik ölçeklendirme kuralı (ölçeklendirme)** |**Otomatik ölçeklendirme kuralı (ölçeklendirme)** |
| **Kaynak:** üretim (uygulama hizmeti ortamı) |**Kaynak:** üretim (uygulama hizmeti ortamı) |
| **Ölçüm:** CPU % |**Ölçüm:** CPU % |
| **İşlem:** % 60 daha büyük |**İşlem:** % 80 daha büyük |
| **Süresi:** 5 dakika |**Süresi:** 10 dakika |
| **Zaman toplama:** ortalama |**Zaman toplama:** ortalama |
| **Eylem:** sayısı 2 ile artırın |**Eylem:** sayısı 1 ile artırın |
| **(Dakika) basılı güzel:** 15 |**(Dakika) basılı güzel:** 20 |
|  | |
| **Otomatik ölçeklendirme kural (Ölçek aşağı)** |**Otomatik ölçeklendirme kural (Ölçek aşağı)** |
| **Kaynak:** üretim (uygulama hizmeti ortamı) |**Kaynak:** üretim (uygulama hizmeti ortamı) |
| **Ölçüm:** CPU % |**Ölçüm:** CPU % |
| **İşlem:** % 30'den az |**İşlem:** % 20'den az |
| **Süresi:** 10 dakika |**Süresi:** 15 dakika |
| **Zaman toplama:** ortalama |**Zaman toplama:** ortalama |
| **Eylem:** sayısı 1 ile azaltma |**Eylem:** sayısı 1 ile azaltma |
| **(Dakika) basılı güzel:** 20 |**(Dakika) basılı güzel:** 10 |

### <a name="app-service-plan-inflation-rate"></a>Uygulama hizmeti plan Enflasyon oranı
Saat başına en yüksek bir hızda yapılandırılmış olan uygulama hizmeti planları otomatik ölçeklendirme için bunu yapın. Bu oran tabanlı otomatik ölçeklendirme kuralı sağlanan değerlere hesaplanabilir.

Anlama ve hesaplama *uygulama hizmeti plan Enflasyon oranı* çalışan havuzunda ölçeği değişiklikler anlık olmadığından uygulama hizmeti ortamı ölçeklendirme için önemlidir.

Uygulama hizmeti plan Enflasyon oranı aşağıdaki gibi hesaplanır:

![Uygulama hizmeti planı Enflasyon oranı hesaplama.][ASP-Inflation]

Otomatik ölçeklendirme – ölçeği Artır kural uygulama hizmeti planı üretim haftanın günü profili için temel:

![Otomatik ölçeklendirme üzerinde – ölçeği Artır kural tabanlı haftanın günü için uygulama hizmeti planı Enflasyon oranı.][Equation1]

Otomatik ölçeklendirme – ölçeği Artır kural uygulama hizmeti planı, üretim hafta sonu profili için söz konusu olduğunda formülü çözümlenmesi:

![Otomatik ölçeklendirme üzerinde – ölçeği Artır kural tabanlı hafta sonları için uygulama hizmeti planı Enflasyon oranı.][Equation2]

Bu değer de ölçek azaltma işlemleri için hesaplanabilir.

Otomatik ölçeklendirme – ölçek aşağı kural uygulama hizmeti planı, üretim haftanın günü profili için temel bunu şu şekilde görünür:

![Otomatik ölçeklendirme üzerinde – ölçek aşağı kural tabanlı haftanın günü için uygulama hizmeti planı Enflasyon oranı.][Equation3]

Otomatik ölçeklendirme – ölçek aşağı kural uygulama hizmeti planı, üretim hafta sonu profili için söz konusu olduğunda formülü çözümlenmesi:  

![Otomatik ölçeklendirme üzerinde – ölçek aşağı kural tabanlı hafta sonları için uygulama hizmeti planı Enflasyon oranı.][Equation4]

Uygulama hizmeti planı üretim sekiz örnekleri/saat hafta sırasında en yüksek hızı ve hafta sonu sırasında dört örnekleri saate büyüyebilir. En fazla dört örnekleri/saat hafta sırasında örnekleri ve hafta sonları sırasında altı örnekleri saate serbest bırakabilirsiniz.

Çalışan havuzunda birden çok uygulama hizmeti planları barındırılan hesaplamak varsa *toplam Enflasyon oranı* yükleniyor tüm uygulama hizmeti planları Enflasyon oranındaki toplamı, bir çalışan havuzunda barındırma.

![Çalışan havuzunda barındırılan birden çok uygulama hizmeti planları için toplam Enflasyon oran hesaplaması.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Uygulama hizmeti plan Enflasyon oranı çalışan havuzu otomatik ölçeklendirme kurallarını tanımlamak için kullanın
Çalışan barındıran yapılandırılmış olan uygulama hizmeti planları otomatik ölçeklendirme kapasitesinin bir arabellek ayrılması gereken havuza alır. Arabellek büyümesine ve gerektiği gibi uygulama hizmeti planı küçültmek için otomatik ölçeklendirme işlemlerine izin verir. En küçük arabellek hesaplanan toplam uygulama hizmeti planı Enflasyon oranı olacaktır.

Uygulama hizmeti ortamı ölçeklendirme işlemleri uygulamak için biraz zaman ayırın ve nedeni herhangi bir değişiklik bir ölçeklendirme işlemi devam ederken ortaya çıkar daha fazla isteğe bağlı değişiklikler için hesap. Bu gecikme karşılamak için her otomatik ölçeklendirme işlemi için en az sayıda eklenen örnekleri hesaplanan toplam uygulama hizmeti planı Enflasyon oranı kullanmanızı öneririz.

Bu bilgileri kullanarak, aşağıdaki otomatik ölçeklendirme profili ve kuralları Frank tanımlayabilirsiniz:

![LOB örnek için otomatik ölçeklendirme profili kuralları.][Worker-Pool-Scale]

| **Otomatik ölçeklendirme profili – haftanın günü** | **Otomatik ölçeklendirme profili – hafta sonları** |
| --- | --- |
| **Ad:** haftanın günü profili |**Ad:** hafta sonu profili |
| **Ölçek tarafından:** zamanlama ve performans kuralları |**Ölçek tarafından:** zamanlama ve performans kuralları |
| **Profil:** haftanın günü |**Profil:** hafta sonu |
| **Tür:** yineleme |**Tür:** yineleme |
| **Hedef aralık:** 13-25 örnekleri |**Hedef aralık:** 6 ila 15 örnekleri |
| **Gün sayısı:** Pazartesi, Salı, Çarşamba, Perşembe, Cuma |**Gün sayısı:** Cumartesi, Pazar |
| **Başlangıç zamanı:** 7: 00'da |**Başlangıç zamanı:** 09:00:00 |
| **Saat dilimi:** UTC-08 |**Saat dilimi:** UTC-08 |
|  | |
| **Otomatik ölçeklendirme kuralı (ölçeklendirme)** |**Otomatik ölçeklendirme kuralı (ölçeklendirme)** |
| **Kaynak:** çalışan havuzu 1 |**Kaynak:** çalışan havuzu 1 |
| **Ölçüm:** WorkersAvailable |**Ölçüm:** WorkersAvailable |
| **İşlem:** değerinden 8 |**İşlem:** 3'ten az |
| **Süresi:** 20 dakika |**Süresi:** 30 dakika |
| **Zaman toplama:** ortalama |**Zaman toplama:** ortalama |
| **Eylem:** sayısı 8 ile artırın |**Eylem:** artırmak sayısı 3 ile |
| **(Dakika) basılı güzel:** 180 |**(Dakika) basılı güzel:** 180 |
|  | |
| **Otomatik ölçeklendirme kural (Ölçek aşağı)** |**Otomatik ölçeklendirme kural (Ölçek aşağı)** |
| **Kaynak:** çalışan havuzu 1 |**Kaynak:** çalışan havuzu 1 |
| **Ölçüm:** WorkersAvailable |**Ölçüm:** WorkersAvailable |
| **İşlem:** 8 büyük |**İşlem:** 3'ten büyük |
| **Süresi:** 20 dakika |**Süresi:** 15 dakika |
| **Zaman toplama:** ortalama |**Zaman toplama:** ortalama |
| **Eylem:** sayısı 2 ile azaltma |**Eylem:** sayısı 3 ile azaltma |
| **(Dakika) basılı güzel:** 120 |**(Dakika) basılı güzel:** 120 |

Profilinde tanımlanmış hedef aralığı uygulama hizmeti planı + arabellek profilinde tanımlanan en düşük örnekler tarafından hesaplanır.

En fazla aralık çalışan havuzunda barındırılan tüm uygulama hizmeti planları için tüm maksimum aralıklarını toplamı olacaktır.

En az 1 X ölçek için uygulama hizmeti planı Enflasyon oranı kuralları ölçek artırma sayısı ayarlanması.

Azaltma sayısı için bir şeyler 1/2 X veya 1'uygulama hizmeti planı Enflasyon oranı ölçek X arasında aşağı ayarlanabilir.

### <a name="autoscale-for-front-end-pool"></a>Ön uç havuzu için otomatik ölçeklendirme
Ön uç otomatik ölçeklendirme kurallarını çalışan havuzlarında daha basittir. Öncelikle yapmanız gerekenler  
süresi ölçümü ve cooldown zamanlayıcılar bir uygulama hizmeti planı üzerinde ölçeklendirme işlemleri anlık olmayan dikkate aldığınızdan emin olun.

Bu senaryo için hata oranı % 80 CPU kullanımı ön uçlar eriştikten sonra artırır ve örnekler gibi artırmak için otomatik ölçeklendirme kural kümesi Frank bilir:

![Ön uç havuzu için otomatik ölçeklendirme ayarları.][Front-End-Scale]

| **Otomatik ölçeklendirme profili – ön Uçlar** |
| --- |
| **Ad:** otomatik ölçeklendirme – ön Uçlar |
| **Ölçek tarafından:** zamanlama ve performans kuralları |
| **Profil:** her gün |
| **Tür:** yineleme |
| **Hedef aralık:** 3-10 örnekleri |
| **Gün sayısı:** her gün |
| **Başlangıç zamanı:** 09:00:00 |
| **Saat dilimi:** UTC-08 |
|  |
| **Otomatik ölçeklendirme kuralı (ölçeklendirme)** |
| **Kaynak:** ön uç havuzu |
| **Ölçüm:** CPU % |
| **İşlem:** % 60 daha büyük |
| **Süresi:** 20 dakika |
| **Zaman toplama:** ortalama |
| **Eylem:** artırmak sayısı 3 ile |
| **(Dakika) basılı güzel:** 120 |
|  |
| **Otomatik ölçeklendirme kural (Ölçek aşağı)** |
| **Kaynak:** çalışan havuzu 1 |
| **Ölçüm:** CPU % |
| **İşlem:** % 30'den az |
| **Süresi:** 20 dakika |
| **Zaman toplama:** ortalama |
| **Eylem:** sayısı 3 ile azaltma |
| **(Dakika) basılı güzel:** 120 |

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
