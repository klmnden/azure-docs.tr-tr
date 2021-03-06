---
title: Query Store PostgreSQL - tek bir sunucu için Azure veritabanı'ndaki en iyi yöntemler
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanında Query Store için en iyi uygulamalar açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 798a7a3edbf11c8421848871d26ba55b5bada0b6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067247"
---
# <a name="best-practices-for-query-store"></a>Query Store için en iyi uygulamalar

**Şunlara uygulanır:** -Tek bir sunucu 9.6 ve 10 olan PostgreSQL için Azure veritabanı

Bu makalede, PostgreSQL için Azure veritabanı'nda Query Store kullanmak için en iyi uygulamalar özetlenmektedir.

## <a name="set-the-optimal-query-capture-mode"></a>En iyi sorgu yakalama modunu ayarlayın
Let Query Store, önem verdiğiniz verileri yakalayın. 

|**pg_qs.query_capture_mode** | **Senaryo**|
|---|---|
|_Tümü_  |İş yükünüz kapsamlı olarak tüm sorgular ve bunların yürütme sıklığı ve diğer istatistikleri açısından analiz edin. İş yükünüz yeni sorguları belirleyin. Geçici sorgular için kullanıcı veya otomatik Parametreleştirme fırsatlarını belirlemek için kullanılıp kullanılmadığını algılayın. _Tüm_ maliyet bir artan kaynak tüketimi ile birlikte gelir. |
|_Sayfanın Üstü_  |İlgilenmeniz en sık kullanılan sorgular - bu istemciler tarafından gönderilen odaklanır.
|_Yok._ |Bir sorgu kümesi zaten topladık ve araştırmak istediğiniz zaman penceresi ve diğer sorgular yükleyebilecek karışıklıkları ortadan kaldırmak istiyorsanız. _Hiçbiri_ test etmek için uygun ve Kıyaslama olduğu ortamlar. _Hiçbiri_ izlemek ve önemli yeni sorgular en iyi duruma getirme olanağı kaçırabilirsiniz dikkatli kullanılmalıdır. Bu geçmiş zaman pencereleri üzerinde verileri kurtaramazsınız. |

Query Store, bir deposu bekleme istatistikleri de içerir. Bekleme istatistikleri yöneten bir ek bir yakalama modu sorgu: **pgms_wait_sampling.query_capture_mode** ayarlanabilir _hiçbiri_ veya _tüm_. 

> [!NOTE] 
> **pg_qs.query_capture_mode** yerini **pgms_wait_sampling.query_capture_mode**. Pg_qs.query_capture_mode ise _hiçbiri_, pgms_wait_sampling.query_capture_mode ayarın hiçbir etkisi olmaz. 


## <a name="keep-the-data-you-need"></a>İhtiyacınız olan verileri tut
**Pg_qs.retention_period_in_days** parametresi, gün Query Store veri saklama süresini belirtir. Eski veri sorgulamak ve istatistikleri silinir. Varsayılan olarak 7 gün boyunca verileri saklamak için Query Store yapılandırılır. Geçmiş verileri kullanmayı planlamıyorsanız kaçının. Verileri daha uzun süre tutmanız gerekiyorsa değerini artırın.


## <a name="set-the-frequency-of-wait-stats-sampling"></a>Örnekleme bekleme istatistikleri sıklığını ayarlayın 
**Pgms_wait_sampling.history_period** parametresinin belirttiği ne sıklıkta bekleme olayları örneklenen (milisaniye cinsinden). Kısa dönem örnekleme daha sık. Daha fazla bilgi alınır, ancak daha büyük kaynak tüketimi maliyetini ortaya çıktı. Sunucu yükü altında veya ayrıntı düzeyi gerekmez bu süresini artırın


## <a name="get-quick-insights-into-query-store"></a>Query Store hızlı Öngörüler edinin
Kullanabileceğiniz [sorgu performansı İçgörüleri](concepts-query-performance-insight.md) Query Store veriler hakkında hızlı Öngörüler almak için Azure portalında. Görselleştirmeler, uzun çalışan sorguları yüzeyine ve en uzun zaman içinde olayları bekleyin.

## <a name="next-steps"></a>Sonraki adımlar
- Alınacak veya ayarlanacak kullanarak parametreleri hakkında bilgi edinin [Azure portalında](howto-configure-server-parameters-using-portal.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md).