---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b23f9532aa1ca6f7bae914ff8cb9d7566a0fec86
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841473"
---
Event Hubs her bir tüketicinin ileti akışında yalnızca belirli bir alt küme ya da bölümü okuduğu bölünmüş bir tüketici modeli aracılığıyla ileti akışı sağlar. Bu model, olay işleme için yatay ölçek sağlar ve kuyruklar ile konularda kullanılamayan diğer akış odaklı özellikleri sunar.

Bölüm bir olay hub'ında tutulan olayların sıralı dizisidir. Yeni olaylar geldikçe dizinin sonuna eklenir. Bölüm bir "yürütme günlüğü" olarak düşünülebilir.

![Event Hubs](./media/event-hubs-partitions/partition.png)

Olay hub'ları, tüm bölümler, olay hub'ı uygulayan bir yapılandırılmış elde tutma süresi verilerini korur. Olayların süresi saat bazında dolar; bunları açıkça silemezsiniz. Bölümler birbirinden bağımsız olup kendi veri dizisini içerdiğinden genellikle farklı hızlarda büyürler.

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

Bölüm sayısı, oluşturma sırasında belirtilir ve 2 ile 32 arasında olmalıdır. Bölüm sayısı değiştirilemez; bu nedenle, bölüm sayısını ayarlarken uzun vadeli ölçeği dikkate almanız gerekir. Bölümler, tüketen uygulamalarda gerekli aşağı akış paralelliğiyle ilişkili bir veri düzenleme mekanizmasıdır. Bir olay hub'ındaki bölüm sayısı, sahip olmayı beklediğiniz eşzamanlı okuyucu sayısıyla doğrudan ilgilidir. Event Hubs ekibine başvurarak bölüm sayısını 32’nin üzerine çıkarabilirsiniz.

Bölümler tanımlanabilir ve doğrudan gönderilebilir olsa da doğrudan bir bölüme göndermek önerilmez. Bunun yerine, sunulan daha yüksek düzeyli yapıları kullanabilirsiniz [olay yayımcıları](../articles/event-hubs/event-hubs-features.md#event-publishers) bölümü. 

Bölümler, olayı, kullanıcı tanımlı bir özellik paketini ve bölümdeki uzaklığı ve akış dizisindeki sayısı gibi meta verileri gövdesi içerir olay verileri dizisi ile doldurulur.

En iyi ölçeği elde etmek için 1:1 işleme birimleri ve bölümlerini dengelemeniz önerilir. Bir garanti edilen giriş ve çıkış en fazla bir işleme biriminden oluşan tek bir bölüm vardır. Bir bölüme daha yüksek performans sağlamak olabilir, ancak performans garanti edilmez. Bir olay hub'ındaki bölüm sayısı en az üretilen iş birimlerinin sayısı için önerilir nedeni budur.

Toplam aktarım hızı gerektiren üzerinde planlama göz önünde bulundurulduğunda, ihtiyaç duyduğunuz üretilen iş birimlerinin sayısı ve en düşük bölüm sayısı, ancak kaç bölümler gerekir biliyor musunuz? Gelecekteki bir üretilen iş hacmi gereksinimlerinizi yanı sıra ulaşmak istediğiniz aşağı akış paralelliğiyle üzerinde göre bölüm seçin. Sahip olduğunuz bir olay hub'ı bölüm sayısı için ücret alınmaz.

Bölümleri ve kullanılabilirlikleri ile güvenilirlikleri arasındaki dengeleme hakkında daha fazla bilgi için, bkz: [Event Hubs programlama kılavuzu](../articles/event-hubs/event-hubs-programming-guide.md#partition-key) ve [Event Hubs’ta kullanılabilirlik ve tutarlılık](../articles/event-hubs/event-hubs-availability-and-consistency.md) makalesi.
