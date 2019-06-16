---
title: -Tek bir sunucu PostgreSQL için Azure veritabanı'nda bildirim tablo depolama stratejisi kullanarak sorgu süresini iyileştirme
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı üzerinde bildirim tablo depolama stratejisi ile sorgu süresini iyileştirmek açıklar.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: ac1dc43a2b89bc1cc748947ec08e6ada87edbfcb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65066992"
---
# <a name="optimize-query-time-with-the-toast-table-storage-strategy"></a>BİLDİRİM tablo depolama stratejisi ile sorgu süresini iyileştirme 
Bu makalede, büyük boyutlu özniteliği depolama teknik (bildirim) tablo depolama stratejisi ile sorgu süreleri en iyi duruma getirme açıklanır.

## <a name="toast-table-storage-strategies"></a>BİLDİRİM tablo depolama stratejileri
Dört farklı stratejiler sütunları bildirim kullanabileceğiniz diske depolamak için kullanılır. Sıkıştırma ve satır dışı depolama arasında çeşitli birleşimlerini temsil ederler. Strateji, veri türünün düzeyinde ve sütun düzeyinde ayarlanabilir.
- **Düz** sıkıştırma veya satır sonu depolama engeller. Varlena türleri için tek baytlık üst bilgileri kullanımını devre dışı bırakır. Düz, sütunların veri türlerinin bildirim kullanamazsınız yalnızca olası stratejisidir.
- **Genişletilmiş** sıkıştırma hem satır dışı depolama sağlar. Genişletilmiş bildirim kullanabileceğiniz birçok veri türleri için varsayılandır. Sıkıştırma, ilk olarak denenir. Satırının hala çok büyükse satır dışı depolama denenir.
- **Dış** satır dışı depolama ancak değil sıkıştırma sağlar. Dış kullanımını alt dize işlemleri metin ve bytea genişliğinde daha hızlı hale getirir. Bu hız, daha yüksek depolama alanı içinde hiçbir ceza ile birlikte gelir. Bu işlemleri değil sıkıştırılmış satır dışı değer, yalnızca gerekli bölümlerini getirilecek getirilir.
- **Ana** sıkıştırma ancak değil çıkış satır dışı depolama sağlar. Satır dışı depolama, yine de bu tür sütunlar için ancak yalnızca son çare olarak gerçekleştirilir. Satır bir sayfaya sığmayacak küçük yapmak için başka bir yol olduğunda gerçekleşir.

## <a name="use-toast-table-storage-strategies"></a>BİLDİRİM tablo depolama stratejileri kullanın
Sorgularınızın bildirim kullanabileceğiniz veri türleri erişirseniz, ana stratejisi sorgu sürelerini kısaltmak için yerine varsayılan Extended seçeneğini kullanarak göz önünde bulundurun. Ana satır dışı depolama kural yoktur. BİLDİRİM kullanabileceğiniz veri türleri, sorgularınızı erişmediğiniz, Genişletilmiş seçeneği tutmak yararlı olabilir. Ana tablo satırlarını daha büyük bir kısmını performans yardımcı olan paylaşılan arabellek önbelleğinde sığdırır.

Geniş tabloların ve yüksek karakter sayısı ile bir şema kullanan bir iş yükü varsa, PostgreSQL bildirim tabloları kullanarak göz önünde bulundurun. 350 sütunları 255 karakter yayılmış birden çok sütun içeren daha büyük bir örnek müşteri tablosu var. Ana stratejisi bildirim tabloya dönüştürüldü sonra kendi Kıyaslama sorgu süresini 467 saniyeye 4203 saniyeden azaltıldı. 89 yüzde geliştirme olmasıdır.

## <a name="next-steps"></a>Sonraki adımlar
İş yükünüz önceki özelliklerini gözden geçirin. 

Aşağıdaki PostgreSQL belgeleri inceleyin: 
- [Bölüm 68, veritabanı fiziksel depolama alanı](https://www.postgresql.org/docs/current/storage-toast.html) 