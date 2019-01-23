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
ms.openlocfilehash: 4024ecddde4b0d020e2c657214a4a258ea0b2ea5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449019"
---
# <a name="metric-alerts-with-dynamic-thresholds-in-azure-monitor-public-preview"></a>Azure İzleyici (genel Önizleme) içinde dinamik eşikler ile ölçüm uyarıları

Dinamik eşikler algılama ile ölçüm Uyarısı, Gelişmiş makine öğrenimi ölçümleri geçmiş davranışı hakkında bilgi edinmek için desenlerini ve olası hizmet sorunları gösteren anormallikleri belirlemek için (ML) yararlanır. Uyarı kuralları Azure Resource Manager API'si aracılığıyla tam otomatik bir şekilde yapılandırmak, kullanıcıların basit bir kullanıcı Arabirimi ve uygun ölçekte işlemleri destek sağlar.

Bir uyarı kuralı oluşturulduktan sonra yalnızca zaman izlenen ölçüm beklendiği gibi davranmaya değil, özel olarak uyarlanmış eşiklere dayanarak etkinleşecektir.

İsteriz, yakında kalmasını sağlamak Geri bildirimlerinizi azurealertsfeedback@microsoft.com.

## <a name="why-and-when-is-using-dynamic-condition-type-recommended"></a>Neden ve ne zaman önerilen dinamik koşul türü kullanıyor?

1. **Ölçeklenebilir uyarı verme** – dinamik eşikler uyarı kuralları oluşturabilir, aynı anda ölçüm serisi yüzlerce eşikleri uyarlanmış. Henüz bir uyarı kuralı tanımlayan tek bir ölçüme göre aynı kolaylığı sağlama. Kullanıcı Arabirimi veya Azure Resource Manager API'si sonuçlarını daha az uyarı kuralları yönetmek için kullanma. Ölçeklenebilir yaklaşım, ölçüm boyutlarla ilgilenme veya tüm abonelik kaynaklarını gibi birden fazla kaynağı uygularken özellikle yararlı olur. Yönetim ve Uyarılar kurallar oluşturma kaydetme uzunca bir zaman çevirir. [Şablonları kullanarak dinamik eşikler ile ölçüm uyarılarını yapılandırma hakkında daha fazla bilgi](alerts-metric-create-templates.md).

1. **Akıllı ölçüm desen tanıma** – benzersiz ML teknolojimizin kullanarak, ölçüm desenleri otomatik olarak algılayıp ölçüm değişiklikleri uyum mevsimsellik (saatlik / günlük / haftalık) genellikle içerebilecek zaman içinde sunabiliyoruz. Ölçümleri davranışı için zaman ve sapmalar desenine göre uyarı üzerinden uyarlama "doğru" her ölçüm eşiği bilerek yükü hafifletir. Dinamik eşikler içinde kullanılan ML algoritmasını gürültülü (düşük duyarlılık) veya geniş (düşük geri çağırma) eşikleri, beklenen desene sahip değilseniz önlemek için tasarlanmıştır.

1. **Sezgisel yapılandırma** – dinamik eşikler kullanarak üst düzey kavramlarını, ölçüm uyarıları ayarlama ölçüm hakkında kapsamlı etki alanı bilgisini gerek ortadan kaldırılmasına izin verin.

## <a name="how-to-configure-alerts-rules-with-dynamic-thresholds"></a>Dinamik eşikler ile uyarılar kurallarını yapılandırmak nasıl?

Dinamik eşikler ile uyarılar, Azure İzleyici ölçüm uyarıları aracılığıyla yapılandırılabilir. [Ölçüm uyarılarının nasıl yapılandırılacağı hakkında daha fazla bilgi](alerts-metric.md).

## <a name="how-are-the-thresholds-calculated"></a>Eşikleri nasıl hesaplanır?

Dinamik Eşik sürekli olarak ölçüm serisi verilerinin öğrenir ve bir dizi algoritmaları ve yöntemleri. kullanarak model dener ve algoritmaları ve yöntem kümesi kullanarak model dener. Mevsimsellik (saatlik / günlük / haftalık) gibi verilerdeki desenleri algılar ve düşük dağılım (örneğin, kullanılabilirlik ve hata oranı) Ölçümleriyle yanı sıra gürültülü ölçümleri (örneğin, makine CPU veya bellek) işleyebilir.

Eşikleri, bu eşikleri bir sapma bir anomali ölçüm davranış gösterir şekilde seçilir.

## <a name="what-does-sensitivity-setting-in-dynamic-thresholds-mean"></a>Dinamik eşikler ortalama ayarında 'Duyarlılık' nedir?

Uyarı eşiği duyarlılık denetleyen bir uyarı tetiklenmesi için gerekli ölçüm davranışını sapma miktarını üst düzey bir kavramdır.
Bu seçenek, ölçüm statik eşik gibi etki alanı bilgileri gerektirmez. Şu seçenekleri kullanabilirsiniz:

- Yüksek – eşikleri sıkı ve ölçüm serisi desenini yakın olacaktır. Uyarı kuralı üzerinde daha fazla elde edilen en küçük sapma tetiklenir.
- Orta – daha sıkı ve daha dengeli eşik değerinden daha az uyarı ile Yüksek duyarlılık (varsayılan).
- Düşük – eşikleri daha fazla mesafe ölçüm serisi desen ile gevşek olacaktır. Uyarı kuralı, yalnızca üzerinde daha az uyarıları sonuçlanan büyük sapma tetikler.

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

## <a name="will-slow-behavior-change-in-the-metric-trigger-an-alert"></a>Yavaş davranışı, bir uyarı ölçüm tetikleyicisi değişecek mi?

Olmayabilir. Dinamik eşikler önemli sapmaları algılama yerine yavaş sorunları gelişen iyidir.

## <a name="how-much-data-is-used-to-preview-and-then-calculate-thresholds"></a>Ne kadar veri Önizleme ve ardından eşikleri hesaplamak için kullanılır?

Grafikte, bir uyarı kuralı bir ölçüme göre oluşturulmadan önce görüntülenen eşiklere göre hesaplanır. geçmiş verilerin son 10 gün üzerinde bir uyarı kuralı oluşturulduktan sonra dinamik eşikler kullanılabilir ek geçmiş verileri almak ve olur sürekli olarak eşikleri daha doğru hale getirmek için yeni veriler temel alınarak öğrenin.
