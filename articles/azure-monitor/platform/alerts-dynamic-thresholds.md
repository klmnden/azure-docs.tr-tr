---
title: Azure İzleyici'de dinamik eşikler ile uyarılar oluşturma
description: Makine öğrenimi tabanlı dinamik eşikler ile uyarılar oluşturun
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: yalavi
ms.reviewer: mbullwin
ms.openlocfilehash: 3773a3e121c3b0162b83ea075601b7386228e4d5
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64876206"
---
# <a name="metric-alerts-with-dynamic-thresholds-in-azure-monitor-public-preview"></a>Azure İzleyici (genel Önizleme) içinde dinamik eşikler ile ölçüm uyarıları

Dinamik eşikler algılama ile ölçüm Uyarısı, Gelişmiş makine öğrenimi ölçümleri geçmiş davranışı hakkında bilgi edinmek için desenlerini ve olası hizmet sorunları gösteren anormallikleri belirlemek için (ML) yararlanır. Uyarı kuralları Azure Resource Manager API'si aracılığıyla tam otomatik bir şekilde yapılandırmak, kullanıcıların basit bir kullanıcı Arabirimi ve uygun ölçekte işlemleri destek sağlar.

Bir uyarı kuralı oluşturulduktan sonra yalnızca zaman izlenen ölçüm beklendiği gibi davranmaya değil, özel olarak uyarlanmış eşiklere dayanarak etkinleşecektir.

İsteriz, yakında kalmasını sağlamak Geri bildirimlerinizi <azurealertsfeedback@microsoft.com>.

## <a name="why-and-when-is-using-dynamic-condition-type-recommended"></a>Neden ve ne zaman önerilen dinamik koşul türü kullanıyor?

1. **Ölçeklenebilir uyarı verme** – dinamik eşikler uyarı kuralları oluşturabilir, aynı anda ölçüm serisi yüzlerce eşikleri uyarlanmış. Henüz bir uyarı kuralı tanımlayan tek bir ölçüme göre aynı kolaylığı sağlama. Kullanıcı Arabirimi veya Azure Resource Manager API'si sonuçlarını daha az uyarı kuralları yönetmek için kullanma. Ölçeklenebilir yaklaşım, ölçüm boyutlarla ilgilenme veya tüm abonelik kaynaklarını gibi birden fazla kaynağı uygularken özellikle yararlı olur. Yönetim ve Uyarılar kurallar oluşturma kaydetme uzunca bir zaman çevirir. [Şablonları kullanarak dinamik eşikler ile ölçüm uyarılarını yapılandırma hakkında daha fazla bilgi](alerts-metric-create-templates.md).

1. **Akıllı ölçüm desen tanıma** – benzersiz ML teknolojimizin kullanarak, ölçüm desenleri otomatik olarak algılayıp ölçüm değişiklikleri uyum mevsimsellik (saatlik / günlük / haftalık) genellikle içerebilecek zaman içinde sunabiliyoruz. Ölçümleri davranışı için zaman ve sapmalar desenine göre uyarı üzerinden uyarlama "doğru" her ölçüm eşiği bilerek yükü hafifletir. Dinamik eşikler içinde kullanılan ML algoritmasını gürültülü (düşük duyarlılık) veya geniş (düşük geri çağırma) eşikleri, beklenen desene sahip değilseniz önlemek için tasarlanmıştır.

1. **Sezgisel yapılandırma** – dinamik eşikler kullanarak üst düzey kavramlarını, ölçüm uyarıları ayarlama ölçüm hakkında kapsamlı etki alanı bilgisini gerek ortadan kaldırılmasına izin verin.

## <a name="how-to-configure-alerts-rules-with-dynamic-thresholds"></a>Dinamik eşikler ile uyarılar kurallarını yapılandırmak nasıl?

Dinamik eşikler ile uyarılar, Azure İzleyici ölçüm uyarıları aracılığıyla yapılandırılabilir. [Ölçüm uyarılarının nasıl yapılandırılacağı hakkında daha fazla bilgi](alerts-metric.md).

## <a name="how-are-the-thresholds-calculated"></a>Eşikleri nasıl hesaplanır?

Dinamik Eşik sürekli olarak ölçüm serisi verilerinin öğrenir ve bir dizi algoritmaları ve yöntemleri kullanarak model çalışır. Mevsimsellik (saatlik / günlük / haftalık) gibi verilerdeki desenleri algılar ve düşük dağılım (örneğin, kullanılabilirlik ve hata oranı) Ölçümleriyle yanı sıra gürültülü ölçümleri (örneğin, makine CPU veya bellek) işleyebilir.

Eşikleri, bu eşikleri bir sapma bir anomali ölçüm davranış gösterir şekilde seçilir.

> [!NOTE]
> Dönemsel deseni algılama, saat, gün veya hafta aralığını ayarlanır. Diğer desenleri bihourly desen gibi başka bir deyişle veya semiweekly algılanmadı.

## <a name="what-does-sensitivity-setting-in-dynamic-thresholds-mean"></a>Dinamik eşikler ortalama ayarında 'Duyarlılık' nedir?

Uyarı eşiği duyarlılık denetleyen bir uyarı tetiklenmesi için gerekli ölçüm davranışını sapma miktarını üst düzey bir kavramdır.
Bu seçenek, ölçüm statik eşik gibi etki alanı bilgileri gerektirmez. Şu seçenekleri kullanabilirsiniz:

- Yüksek – eşikleri sıkı ve ölçüm serisi desenini yakın olacaktır. Daha fazla elde edilen en küçük sapma üzerinde bir uyarı kuralı tetiklenir.
- Orta – daha sıkı ve daha dengeli eşik değerinden daha az uyarı ile Yüksek duyarlılık (varsayılan).
- Düşük – eşikleri daha fazla mesafe ölçüm serisi desen ile gevşek olacaktır. Bir uyarı kuralı, yalnızca şirket içinde daha az uyarı sonuçlanan büyük sapmalar tetikler.

## <a name="what-are-the-operator-setting-options-in-dynamic-thresholds"></a>Dinamik Eşik 'Operator' ayarı seçenekleri nelerdir?

Uyarı kuralı oluşturabilmeniz dinamik eşikler eşikleri ölçüm davranışı için her iki üst ve alt sınırı aynı uyarı kuralı kullanarak göre uyarlanmış.
Aşağıdaki üç koşulun birinde tetiklenmesi için uyarıyı seçebilirsiniz:

- Üst eşik değerinden büyük veya eşiğin daha düşük (varsayılan)
- Üst eşik değerinden büyük
- Alt eşik değerinden düşük.

## <a name="what-do-the-advanced-settings-in-dynamic-thresholds-mean"></a>Dinamik eşikler ortalama Gelişmiş ayarlarda neler?

**Nokta başarısız** -dinamik eşikler ayrıca "bir uyarı tetiklenmesi için ihlal sayısı", yapılandırmanıza olanak tanır sapma içinde belirli bir zaman penceresi (zaman penceresi varsayılan dörttür uyarı sistemi için gerekli en düşük sayısı sapmalar 20 dakika cinsinden). Kullanıcı, başarısız olan dönemler yapılandırma ve ne zaman penceresi ve başarısız olan dönemler değiştirerek uyarı seçin. Bu özelliği geçici artışlara tarafından oluşturulan uyarı gürültüsünü azaltır. Örneğin:

20 dakika boyunca sürekli sorun olduğunda uyarı tetiklemek için 5 dakika, belirli bir dönem gruplandırmaki 4 art arda kaç kez aşağıdaki ayarları kullanın:

![Sürekli sorun dönemleri ayarlarını 20 dakika, 5 dakikalık belirli bir dönem gruplandırmaki 4 art arda kaç kez başarısız olan](media/alerts-dynamic-thresholds/0008.png)

Gelen bir Dinamik Eşik ihlalinin 20 dakika cinsinden süre 5 dakika ile son 30 dakika dışında olduğunda bir uyarı tetiklemek için aşağıdaki ayarları kullanın:

![Son 30 dakika, 5 dakikalık dönem gruplandırma ile dışında 20 dakika için sorun dönemleri ayarlarını başarısız](media/alerts-dynamic-thresholds/0009.png)

**Önceki verileri Yoksay** -kullanıcılar ayrıca isteğe bağlı olarak, system başlaması gerektiği gelen eşikleri hesaplamak başlangıç tarihi tanımlayabilir. Tipik bir kullanım örneği kaynak sınama modunda çalışan bir edildi ve artık üretim iş yükü sunmak için yükseltilir ve sınama aşaması sırasında herhangi bir ölçümü davranışını gözardı bu nedenle meydana gelebilir.

## <a name="how-do-you-find-out-why-a-dynamic-thresholds-alert-was-triggered"></a>Dinamik eşikler uyarı neden tetiklendi nasıl bulurum?

E-posta veya SMS mesajı ya da tarayıcı Azure portalında görüntüleyin uyarıları görmek için bağlantıya tıklayarak ya da uyarılar görünümünde tetiklenen uyarı örneklerinin keşfedebilirsiniz. [Uyarılar görünümü hakkında daha fazla bilgi](alerts-overview.md#alerts-experience).

Uyarı görünümü görüntüler:

- Ölçüm ayrıntıları anda dinamik eşikler uyarı tetiklenir.
- Bir grafiği içeren dinamik zamandaki o noktada kullanılan eşikleri, uyarı tetiklendi süresi.
- Geri bildirim uyarı dinamik eşikler ve Uyarıları gelecekteki algılamalar geliştirebileceğimiz görüntüleme deneyimi sağlayabilme kabiliyeti.

## <a name="will-slow-behavior-change-in-the-metric-trigger-an-alert"></a>Yavaş davranışı, bir uyarı ölçüm tetikleyicisi değişecek mi?

Olmayabilir. Dinamik eşikler önemli sapmaları algılama yerine yavaş sorunları gelişen iyidir.

## <a name="how-much-data-is-used-to-preview-and-then-calculate-thresholds"></a>Ne kadar veri Önizleme ve ardından eşikleri hesaplamak için kullanılır?

Grafikte, bir uyarı kuralı bir ölçüme göre oluşturulmadan önce görüntülenen eşikleri saatlik veya günlük dönemsel desenleri (10 gün olarak) hesaplamak için yeterli geçmiş veri hesaplanır. Bir uyarı kuralı oluşturulduktan sonra dinamik eşikler mevcuttur ve sürekli olarak öğrenin ve eşikleri daha doğru hale getirmek için yeni verilere dayalı olarak uyum tüm gerekli geçmiş verilerinizi kullanır. Bu, sonra bu hesaplama, grafik de haftalık desenleri görüntüleyeceği anlamına gelir.

## <a name="how-much-data-is-needed-to-trigger-an-alert"></a>Uyarı tetiklemek için ne kadar veri gerekiyor?

Yeni bir kaynak veya eksik ölçüm verileri varsa, dinamik eşikler üç günlük veri doğru eşikleri emin olmak kullanılabilir olmadan önce uyarı tetiklemez.

## <a name="dynamic-thresholds-best-practices"></a>Dinamik eşikler en iyi uygulamalar

Dinamik eşikler herhangi bir platform veya özel ölçüm Azure İzleyici'de uygulanabilir ve ayrıca ortak uygulama ve altyapı ölçümler için ayarlanmış.
Bazı dinamik eşikler kullanarak bu ölçümler üzerinde uyarılar yapılandırma en iyi öğelerdir.

### <a name="dynamic-thresholds-on-virtual-machine-cpu-percentage-metrics"></a>Sanal makine CPU yüzdesi ölçümlere ilişkin dinamik eşikler

1. İçinde [Azure portalında](https://portal.azure.com), tıklayarak **İzleyici**. İzleme görünümü, tüm izleme ayarlarınızı ve tek bir görünümde verileri birleştirir.

2. Tıklayın **uyarılar** ardından **+ yeni uyarı kuralı**.

    > [!TIP]
    > Çoğu kaynak dikey pencerelerinin de **uyarılar** altında kendi kaynak menüsünde **izleme**, oradan da uyarılar oluşturabilirsiniz.

3. Tıklayın **hedefi seçme**, yükler içerik bölmesinde, uyarı istediğiniz hedef kaynak seçin. Kullanım **abonelik** ve **'Sanal makineleri' kaynak türü** izlemek istediğiniz kaynak bulmak için açılan listeler. Kaynak bulmak için arama çubuğunu da kullanabilirsiniz.

4. Hedef kaynak seçtikten sonra tıklayarak **koşul Ekle**.

5. Seçin **'CPU yüzdesi'**.

6. İsteğe bağlı olarak ayarlayarak ölçüm İyileştir **süresi** ve **toplama**. Daha az temsilcisi davranış olduğu gibi bu ölçüm türü için 'Maximum' toplama türü kullanmak için önerilmez. 'En fazla' toplama türü statik eşiği için belki de daha uygun.

7. Son 6 saat boyunca ölçüm için bir grafik görürsünüz. Uyarı parametreleri tanımlayın:
    1. **Koşul türü** -'Dynamic' seçeneğini belirleyin.
    1. **Duyarlılık** -uyarı gürültüsünü azaltmak için duyarlılık Orta/düşük seçin.
    1. **İşleç** -davranışını uygulama kullanımını gösteren sürece 'Büyüktür' seçin.
    1. **Sıklık** -tabanlı uyarı iş etkisini azaltmayı göz önünde bulundurun.
    1. **Nokta başarısız** (Gelişmiş seçenek) - görünüm arka pencere en az 15 dakika olmalıdır. Dönem beş dakikaya ayarlarsanız, örneğin, ardından nokta başarısız olan en az üç veya daha fazla olmalıdır.

8. Son verileri temel alan hesaplanan eşikleri ölçüm grafiği görüntüler.

9. **Bitti**’ye tıklayın.

10. Doldurun **uyarı ayrıntıları** gibi **uyarı kuralı adı**, **açıklama**, ve **önem derecesi**.

11. Yeni bir eylem grubu oluşturma veya mevcut bir eylem grubu seçerek uyarıyı bir eylem grubu ekleyin.

12. Tıklayın **Bitti** ölçüm uyarı kuralını kaydetmek için.

> [!NOTE]
> Ölçüm uyarı kuralları portalı üzerinden oluşturuldu, hedef kaynağın aynı kaynak grubunda oluşturulur.

### <a name="dynamic-thresholds-on-application-insights-http-request-execution-time"></a>Dinamik eşikler Application Insights HTTP isteği yürütme süresi

1. İçinde [Azure portalında](https://portal.azure.com), tıklayarak **İzleyici**. İzleme görünümü, tüm izleme ayarlarınızı ve tek bir görünümde verileri birleştirir.

2. Tıklayın **uyarılar** ardından **+ yeni uyarı kuralı**.

    > [!TIP]
    > Çoğu kaynak dikey pencerelerinin de **uyarılar** altında kendi kaynak menüsünde **izleme**, oradan da uyarılar oluşturabilirsiniz.

3. Tıklayın **hedefi seçme**, yükler içerik bölmesinde, uyarı istediğiniz hedef kaynak seçin. Kullanım **abonelik** ve **'Application Insights' kaynak türü** izlemek istediğiniz kaynak bulmak için açılan listeler. Kaynak bulmak için arama çubuğunu da kullanabilirsiniz.

4. Hedef kaynak seçtikten sonra tıklayarak **koşul Ekle**.

5. Seçin **'HTTP isteği yürütme süresi'**.

6. İsteğe bağlı olarak ayarlayarak ölçüm İyileştir **süresi** ve **toplama**. Daha az temsilcisi davranış olduğu gibi bu ölçüm türü için 'Maximum' toplama türü kullanmak için önerilmez. 'En fazla' toplama türü statik eşiği için belki de daha uygun.

7. Son 6 saat boyunca ölçüm için bir grafik görürsünüz. Uyarı parametreleri tanımlayın:
    1. **Koşul türü** -'Dynamic' seçeneğini belirleyin.
    1. **İşleç** -'Üzerinde geliştirme süresi tetiklenen uyarılar azaltmak için büyük' ı seçin.
    1. **Sıklık** -tabanlı uyarı iş etkisini azaltmayı göz önünde bulundurun.

8. Son verileri temel alan hesaplanan eşikleri ölçüm grafiği görüntüler.

9. **Bitti**’ye tıklayın.

10. Doldurun **uyarı ayrıntıları** gibi **uyarı kuralı adı**, **açıklama**, ve **önem derecesi**.

11. Yeni bir eylem grubu oluşturma veya mevcut bir eylem grubu seçerek uyarıyı bir eylem grubu ekleyin.

12. Tıklayın **Bitti** ölçüm uyarı kuralını kaydetmek için.

> [!NOTE]
> Ölçüm uyarı kuralları portalı üzerinden oluşturuldu, hedef kaynağın aynı kaynak grubunda oluşturulur.
