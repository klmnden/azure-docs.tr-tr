---
title: Azure Data Factory fiyatlandırma örnekleri anlama | Microsoft Docs
description: Bu makalede açıklanır ve Azure Data Factory fiyatlandırma modeli ile ilgili ayrıntılı örnekler gösterilmektedir.
documentationcenter: ''
author: shlo
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: shlo
ms.openlocfilehash: a825982532047f6e311c5508394df243310f02ab
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233936"
---
# <a name="understanding-data-factory-pricing-through-examples"></a>Data Factory fiyatlandırma örnekleri anlama

Bu makalede açıklanır ve Azure Data Factory fiyatlandırma modeli ile ilgili ayrıntılı örnekler gösterilmektedir.

> [!NOTE]
> Bu örneklerde kullanılan fiyatların kuramsal ve gerçek fiyatlandırma yaptığından amaçlanmamıştır.

## <a name="copy-data-from-aws-s3-to-azure-blob-storage-hourly"></a>Azure Blob depolama alanına AWS S3'ten veri saatlik kopyalama

Bu senaryoda, saatlik bir zamanlamaya göre Azure Blob Depolama, AWS S3'ten verileri kopyalamak istersiniz.

Senaryoyu gerçekleştirmek için aşağıdaki öğeleri içeren bir işlem hattı oluşturmak gerekir:

1. Kopyalama etkinliği ile bir giriş veri kümesi için AWS S3'ten kopyalanacak verileri.

2. Bir çıkış veri kümesi için Azure depolama üzerinde verileri.

3. İşlem hattı her saat için bir zamanlama tetikleyicisi.

   ![Scenario1](media/pricing-concepts/scenario1.png)

| **İşlemler** | **Türler ve birimler** |
| --- | --- |
| Bağlı hizmet oluşturma | 2 okuma/yazma varlık  |
| Veri kümeleri oluşturma | 4 okuma/yazma varlıkları (veri kümesi oluşturmak için 2, bağlı hizmet başvuruları için 2) |
| Ardışık Düzen Oluştur | 3 okuma/yazma varlıkları (işlem hattı oluşturmak için 1, 2 veri kümesi başvurular için) |
| İşlem hattına sahip | 1 okuma/yazma varlık |
| İşlem hattı çalıştırma | 2 etkinlik çalıştırması (tetikleyici çalıştırması, etkinlik çalıştırması için 1 için 1) |
| 10 dakikalık kopyalama veri varsayım: yürütme süresi = | 10 \* 4 azure Integration Runtime (varsayılan DIU ayarı = 4) veri tümleştirme birimleri ve kopyalama performansı iyileştirme hakkında daha fazla bilgi için bkz. [bu makalede](copy-activity-performance.md) |
| İşlem hattı varsayım İzleyici: Yalnızca 1 oluştu çalıştırın | 2 Çalıştır izleme kayıtları (1 işlem hattı çalıştırmasında, etkinlik çalıştırması için 1) yeniden denenir. |

**Toplam senaryo fiyatlandırma: $0.16811**

- Data Factory işlem = **0,0001 $**
  - Okuma/yazma = 10\*00001 0,0001 $ = [1 R/W $ 0,50/50000 = 0,00001 =]
  - İzleme 2 =\*000005 $0,00001 = [1 izleme $ 0,25/50000 = 0.000005 =]
- İşlem hattı düzenlemesi &amp; yürütme = **$0.168**
  - Etkinlik çalıştırmalarını = 001\*2 = 0,002 [çalışma 1 = $1/1000 0,001 =]
  - Veri taşıma etkinlikleri = $0.166 (Prorated yürütme süresi 10 dakika. Azure tümleştirme çalışma zamanı üzerinde 0,25 dolar/saat)

## <a name="copy-data-and-transform-with-azure-databricks-hourly"></a>Veri kopyalama ve Azure Databricks ile saatlik dönüştürün

Bu senaryoda, AWS S3'ten Azure Blob depolama alanına veri kopyalama ve saatlik bir zamanlamaya göre Azure Databricks ile verileri dönüştürmek istersiniz.

Senaryoyu gerçekleştirmek için aşağıdaki öğeleri içeren bir işlem hattı oluşturmak gerekir:

1. AWS S3'ten kopyalanacak veriler için girdi veri kümesi ve Azure depolama üzerinde verileri için bir çıktı veri kümesi bir kopyalama etkinliği.
2. Veri dönüştürme için bir Azure Databricks etkinlik.
3. İşlem hattı her saat için bir zamanlama tetikleyicisi.

![Scenario2](media/pricing-concepts/scenario2.png)

| **İşlemler** | **Türler ve birimler** |
| --- | --- |
| Bağlı hizmet oluşturma | 3 okuma/yazma varlık  |
| Veri kümeleri oluşturma | 4 okuma/yazma varlıkları (veri kümesi oluşturmak için 2, bağlı hizmet başvuruları için 2) |
| Ardışık Düzen Oluştur | 3 okuma/yazma varlıkları (işlem hattı oluşturmak için 1, 2 veri kümesi başvurular için) |
| İşlem hattına sahip | 1 okuma/yazma varlık |
| İşlem hattı çalıştırma | 3 etkinlik çalıştırması (tetikleyici çalıştırması, etkinlik çalışması için 2 için 1) |
| 10 dakikalık kopyalama veri varsayım: yürütme süresi = | 10 \* 4 azure Integration Runtime (varsayılan DIU ayarı = 4) veri tümleştirme birimleri ve kopyalama performansı iyileştirme hakkında daha fazla bilgi için bkz. [bu makalede](copy-activity-performance.md) |
| İşlem hattı varsayım İzleyici: Yalnızca 1 oluştu çalıştırın | 3 Çalıştır izleme kayıtları (1 işlem hattı çalıştırmasında, etkinlik çalıştırması için 2) ile yeniden denenir. |
| Databricks etkinlik varsayım yürütün: yürütme süresi 10 dakika = | 10 dakikalık dış işlem hattı etkinliği çalıştırma |

**Toplam senaryo fiyatlandırma: $0.16916**

- Data Factory işlem = **$0.00012**
  - Okuma/yazma 11 =\*00001 $0.00011 = [1 R/W $ 0,50/50000 = 0,00001 =]
  - İzleme 3 =\*000005 $0,00001 = [1 izleme $ 0,25/50000 = 0.000005 =]
- İşlem hattı düzenlemesi &amp; yürütme = **$0.16904**
  - Etkinlik çalıştırmalarını = 001\*3 = 0,003 [çalışma 1 = $1/1000 0,001 =]
  - Veri taşıma etkinlikleri = $0.166 (Prorated yürütme süresi 10 dakika. Azure tümleştirme çalışma zamanı üzerinde 0,25 dolar/saat)
  - Dış işlem hattı etkinliği = $0.000041 (Prorated yürütme süresi 10 dakika. Azure tümleştirme çalışma zamanı üzerinde 0.00025$ / saat)

## <a name="copy-data-and-transform-with-dynamic-parameters-hourly"></a>Veri kopyalama ve dinamik parametrelerle saatlik dönüştürün

Bu senaryoda, Azure Blob Depolama ve Azure Databricks ile dönüştürme (ile betik dinamik parametreleri), AWS S3'ten veri kopyalamak saatlik bir zamanlamaya göre istediğiniz.

Senaryoyu gerçekleştirmek için aşağıdaki öğeleri içeren bir işlem hattı oluşturmak gerekir:

1. AWS S3, verileri Azure depolama için bir çıktı veri kümesi kopyalanacak veriler için girdi veri kümesi bir kopyalama etkinlikli.
2. Parametreleri dinamik olarak dönüştürme betiğe geçirmek için bir arama etkinliği.
3. Veri dönüştürme için bir Azure Databricks etkinlik.
4. İşlem hattı her saat için bir zamanlama tetikleyicisi.

![Scenario3](media/pricing-concepts/scenario3.png)

| **İşlemler** | **Türler ve birimler** |
| --- | --- |
| Bağlı hizmet oluşturma | 3 okuma/yazma varlık  |
| Veri kümeleri oluşturma | 4 okuma/yazma varlıkları (veri kümesi oluşturmak için 2, bağlı hizmet başvuruları için 2) |
| Ardışık Düzen Oluştur | 3 okuma/yazma varlıkları (işlem hattı oluşturmak için 1, 2 veri kümesi başvurular için) |
| İşlem hattına sahip | 1 okuma/yazma varlık |
| İşlem hattı çalıştırma | 4 etkinlik çalıştırması (tetikleyici çalıştırması, etkinlik çalışması için 3 için 1) |
| 10 dakikalık kopyalama veri varsayım: yürütme süresi = | 10 \* 4 azure Integration Runtime (varsayılan DIU ayarı = 4) veri tümleştirme birimleri ve kopyalama performansı iyileştirme hakkında daha fazla bilgi için bkz. [bu makalede](copy-activity-performance.md) |
| İşlem hattı varsayım İzleyici: Yalnızca 1 oluştu çalıştırın | 4 Çalıştır izleme kayıtları (1 işlem hattı çalıştırmasında, etkinlik çalıştırması için 3) ile yeniden denenir. |
| Arama etkinliği varsayım yürütün: yürütme süresi 1 dakika = | 1 dakika işlem hattı Etkinlik yürütme |
| Databricks etkinlik varsayım yürütün: yürütme süresi 10 dakika = | 10 dakikalık dış işlem hattı Etkinlik yürütme |

**Toplam senaryo fiyatlandırma: $0.17020**

- Data Factory işlem = **$0.00013**
  - Okuma/yazma 11 =\*00001 $0.00011 = [1 R/W $ 0,50/50000 = 0,00001 =]
  - İzleme = 4\*000005 $0.00002 = [1 izleme $ 0,25/50000 = 0.000005 =]
- İşlem hattı düzenlemesi &amp; yürütme = **$0.17007**
  - Etkinlik çalıştırmalarını = 001\*4 = 0.004 [çalışma 1 = $1/1000 0,001 =]
  - Veri taşıma etkinlikleri = $0.166 (Prorated yürütme süresi 10 dakika. Azure tümleştirme çalışma zamanı üzerinde 0,25 dolar/saat)
  - İşlem hattı, etkinlik = $0.00003 (Prorated yürütme süresi 1 dakika için. Azure tümleştirme çalışma zamanı üzerinde $ 0,002/saat)
  - Dış işlem hattı etkinliği = $0.000041 (Prorated yürütme süresi 10 dakika. Azure tümleştirme çalışma zamanı üzerinde 0.00025$ / saat)

## <a name="using-mapping-data-flow-debug-for-a-normal-workday-preview-pricing"></a>Eşleme veri akışı hata ayıklama normal iş günü için (Önizleme fiyatı) kullanma

Bir veri mühendisi tasarlama, oluşturma ve her gün eşleme veri akışları test etme için sorumlu olursunuz. Sabah saatlerinde ADF kullanıcı Arabiriminde oturum açıp veri akışları için hata ayıklama modunu etkinleştirin. Hata ayıklama oturumları için varsayılan TTL değeri 60 dakikadır. Hata ayıklama oturumunuzu her zaman geçerli olsun için 10 saat, gün boyunca çalışır. Bu nedenle, gün için ücret şöyle olacaktır:

**10 (saat) x 8 (çekirdek) x 0.112 $ $8.96 =**

## <a name="transform-data-in-blob-store-with-mapping-data-flows-preview-pricing"></a>Veri akışı (Önizleme fiyatı) eşleme ile blob deposu verileri dönüştürün

Bu senaryoda, Blob Store ADF eşleme veri akışları saatlik bir zamanlamaya göre görsel olarak verileri dönüştürmek istersiniz.

Senaryoyu gerçekleştirmek için aşağıdaki öğeleri içeren bir işlem hattı oluşturmak gerekir:

1. Bir veri akışı etkinliği dönüştürme mantığını ile.

2. Azure depolama üzerinde verileri için girdi veri kümesi.

3. Bir çıkış veri kümesi için Azure depolama üzerinde verileri.

4. İşlem hattı her saat için bir zamanlama tetikleyicisi.

| **İşlemler** | **Türler ve birimler** |
| --- | --- |
| Bağlı hizmet oluşturma | 2 okuma/yazma varlık  |
| Veri kümeleri oluşturma | 4 okuma/yazma varlıkları (veri kümesi oluşturmak için 2, bağlı hizmet başvuruları için 2) |
| Ardışık Düzen Oluştur | 3 okuma/yazma varlıkları (işlem hattı oluşturmak için 1, 2 veri kümesi başvurular için) |
| İşlem hattına sahip | 1 okuma/yazma varlık |
| İşlem hattı çalıştırma | 2 etkinlik çalıştırması (tetikleyici çalıştırması, etkinlik çalıştırması için 1 için 1) |
| Veri akışı varsayımlar: yürütme süresi 10 dakika + 10 dakikalık TTL = | 10 \* TTL 10 ile genel işlem, 8 çekirdek |
| İşlem hattı varsayım İzleyici: Yalnızca 1 oluştu çalıştırın | 2 Çalıştır izleme kayıtları (1 işlem hattı çalıştırmasında, etkinlik çalıştırması için 1) yeniden denenir. |

**Toplam senaryo fiyatlandırma: $0.3011**

- Data Factory işlem = **0,0001 $**
  - Okuma/yazma = 10\*00001 0,0001 $ = [1 R/W $ 0,50/50000 = 0,00001 =]
  - İzleme 2 =\*000005 $0,00001 = [1 izleme $ 0,25/50000 = 0.000005 =]
- İşlem hattı düzenlemesi &amp; yürütme = **$0.301**
  - Etkinlik çalıştırmalarını = 001\*2 = 0,002 [çalışma 1 = $1/1000 0,001 =]
  - Veri akış etkinlikleri $0.299 Prorated = 20 dakika için (yürütme süresi 10 dakika + 10 dakika TTL). 8 çekirdek genel 0.112 $/ saat Azure tümleştirme çalışma zamanı üzerinde işlem

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory için fiyatlandırma anlamak, başlayabilirsiniz!

- [Azure Data Factory UI kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md)

- [Azure Data Factory'ye giriş](introduction.md)

- [Azure Data Factory'de görsel yazma](author-visually.md)
