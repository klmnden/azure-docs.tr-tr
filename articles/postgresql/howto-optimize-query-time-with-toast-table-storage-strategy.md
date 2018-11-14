---
title: Bildirim tablo depolama stratejisi kullanarak PostgreSQL için Azure veritabanı'nda sorgu süresini iyileştirme
description: Bu makalede, PostgreSQL için Azure veritabanı'nda bildirim tablo depolama stratejisi ile sorgu süresini iyileştirmek açıklar.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: dee8aaaef4b1998a7234a88d07ad5efbc79d050b
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629300"
---
# <a name="optimizing-query-time-with-toast-table-storage-strategy"></a>BİLDİRİM tablo depolama stratejisi ile sorgu süresini en iyi duruma getirme 
Bu makalede bildirim tablo depolama stratejisi ile sorgu süreleri en iyi duruma getirme.

## <a name="toast-table-storage-strategies"></a>BİLDİRİM tablo depolama stratejileri
Sıkıştırma ve satır dışı depolama arasında çeşitli birleşimlerini temsil eden diskte bildirim mümkün sütunları depolamak için dört farklı strateji vardır. Strateji, veri türünün düzeyinde ve sütun düzeyinde ayarlanabilir.
- **Düz** sıkıştırma veya satır sonu depolama; engeller. Ayrıca, bunu tek baytlık üst bilgilerin kullanım varlena türleri için devre dışı bırakır. **Düz** bildirim mümkün olmayan veri türlerinin sütunlar için olası yalnızca stratejisidir.
- **Genişletilmiş** sıkıştırma hem satır dışı depolama sağlar. **Genişletilmiş** çoğu bildirim mümkün veri türleri için varsayılan değerdir. Satırının hala çok büyük ise sıkıştırma denenen ilk sonra satır dışı depolama olacaktır.
- **Dış** satır dışı depolama ancak değil sıkıştırma sağlar. Kullanım **dış** yalnızca gerekli getirmek için bu işlem için iyileştirilmiş olduğundan daha yüksek depolama alanı içinde hiçbir ceza geniş metin ve bytea sütunları daha hızlı, alt işlemlerin yapacak çıkış satır dışı bölümlerini değeri, sıkıştırılmış.
- **Ana** sıkıştırma ancak değil çıkış satır dışı depolama sağlar. Satır bir sayfaya sığmayacak küçük yapmak için başka bir yol olduğunda satır dışı depolama yine de bu tür sütunlar için ancak yalnızca son çare olarak gerçekleştirilir.

## <a name="using-toast-table-storage-strategies"></a>BİLDİRİM tablo depolama stratejilerle
Sorgularınızın bildirim mümkün veri türleri kullanılarak erişiyorsanız, **ana** varsayılan yerine **Genişletilmiş** sorgu sürelerini kısaltmak için seçeneği. **Ana** satır dışı depolama kullanımını değil. Bildirim mümkün veri türleri, sorgularınızı erişim değil, diğer taraftan, tutmak yararlı olabilir **Genişletilmiş** seçeneği. Bu nedenle ana tablo satırlarını daha büyük bir kısmı, performans yardımcı paylaşılan arabellek önbellekte sığdırır.

Geniş tabloların ve yüksek karakter sayısı ile bir şemayı kullanarak bir iş yükü varsa, PostgreSQL bildirim tabloları kullanarak göz önünde bulundurun. Bir örnek müşteri tablosu 255 karakteri kapsayarak çeşitli sütunları 350 sütunlarla büyüktür vardı. Kendi Kıyaslama sorgu süresini 467 saniye, bildirim stratejisi dönüştürmeden sonra bir 89 yüzde geliştirme 4203 saniyeden azaltılmış **ana**.

## <a name="next-steps"></a>Sonraki adımlar
İş yükünüz için yukarıdaki özelliklerini gözden geçirin. 

Aşağıdaki PostgreSQL belgeleri gözden geçirin: [bölüm 68, veritabanı fiziksel depolama alanı](https://www.postgresql.org/docs/current/storage-toast.html) 