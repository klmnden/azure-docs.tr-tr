---
title: Ölçeklenebilirlik ve performans - Personalizer
titleSuffix: Azure Cognitive Services
description: 'Yüksek performanslı ve trafiği yüksek Web siteleri ve uygulamalar ile Personalizer ölçeklenebilirlik ve performans için dikkate alınması gereken iki ana etmene vardır: gecikme süresi ve eğitim işleme.'
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: edjez
ms.openlocfilehash: 06c2e65c723e18acc515dd7effc61aae0564f411
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722427"
---
# <a name="scalability-and-performance"></a>Ölçeklenebilirlik ve performans

Yüksek performanslı ve trafiği yüksek Web siteleri ve uygulamalar ile Personalizer ölçeklenebilirlik ve performans için dikkate alınması gereken iki ana Etkenler vardır:

* Düşük gecikme süresi derece API çağrısı yaparken tutma
* Eğitim aktarım hızı ile olay girişi sürdürdüğünden emin olma

Kişiselleştirme derecede çok hızlı bir şekilde ile REST API aracılığıyla iletişim ayrılmış çağrı süresi çoğunu döndürebilir. Azure otomatik ölçeklendirme isteklerine hızlı yanıt olanağı sağlar.

##  <a name="low-latency-scenarios"></a>Düşük gecikme süresi senaryoları

Bazı uygulamalar, düşük gecikme süreleriyle derecede döndürülürken gerektirir. Bu gerekli değildir:

* Kullanıcı göstermeden önce belirgin bir süre beklemesini tutmak için içerik sıralanmış.
* Aşırı trafik yaşayan bir sunucu yardımcı olacak nadir işlem saati ve ağ bağlantıları bağlamadan kaçının.

<!--

If your web site is scaled on your infrastructure, you can avoid making HTTP calls by hosting the Personalizer API in your own servers running a Docker container.

This change would be transparent to your application, other than using an endpoint URL referring to the running docker instances as opposed to an online service in the cloud.



### Extreme Low Latency Scenarios

If you require latencies under a millisecond, and have already tested using Personalizer via containers, please contact our support team so we can assess your scenario and provide guidance suited to your needs.

-->

## <a name="scalability-and-training-throughput"></a>Ölçeklenebilirlik ve eğitim aktarım hızı

Boyut sayısı ve ödül API'leri sonra Personalizer tarafından zaman uyumsuz olarak gönderilen iletileri retrained bir model güncelleştirerek personalizer works temel. Bu iletiler, uygulama için bir Azure Event Hubs'a kullanarak gönderilir.

 Çoğu uygulama, en fazla birleştirme ve eğitim Personalizer verimini ulaşacak düşüktür. Bu üst sınıra ulaşana uygulamayı yavaşlatır değil, ancak olay hub'ı kuyrukları, temizlenmesi dahili olarak hızlı doldurulur kapsıyor.

## <a name="how-to-estimate-your-throughput-requirements"></a>Aktarım hızı gereksinimlerinizi tahmin etme

* Bağlamını ve eylem JSON belgelerini uzunluklarının ekleme sıralaması olay başına bayt sayısını tahmin edin.
* 20 MB/sn bu tahmini ortalama bayt sayısı ile böler.

Örneğin, ortalama yükünüzü 500 özelliklere sahiptir ve her bir tahmini 20 karakter ise, her olay yaklaşık 10 kb olduğundan. Bu tahminler, u 20.000.000 / 10.000 2.000 olay/sn, yaklaşık 173 milyon olay/gün olduğu =. 

Limitler ulaşıyor, aynı zamanda mimarisi öneriler için lütfen destek ekibimize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

[Oluşturma ve yapılandırma Personalizer](how-to-settings.md).