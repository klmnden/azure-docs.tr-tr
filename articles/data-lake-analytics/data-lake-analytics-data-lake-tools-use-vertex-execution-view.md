---
title: Visual Studio için Data Lake araçları, köşe yürütme görünümünü kullanma
description: Bu makalede, sınavı Data Lake Analytics işleri için köşe yürütme görünümünü kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: mumian
ms.author: jgao
ms.reviewer: jasonwhowell
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.topic: conceptual
ms.date: 10/13/2016
ms.openlocfilehash: 73314c5864e3036d102deee2792021345b80bf2e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60687841"
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları, köşe yürütme görünümünü kullanma
Köşe yürütme görünümünü sınavı Data Lake Analytics işleri için kullanmayı öğrenin.


## <a name="open-the-vertex-execution-view"></a>Köşe yürütme görünümünü Aç
U-SQL işi, Visual Studio için Data Lake Araçları ' açın. Tıklayın **köşe yürütme görünümünü** sol alt köşedeki. İlk yük profillerine istenebilir ve ağ bağlantınızı bağlı olarak biraz zaman alabilir.

![Köşe yürütme görünümünü Data Lake Analytics araçları](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Köşe yürütme görünümünü anlama
Köşe yürütme görünümü, üç bölümden oluşur:

![Köşe yürütme görünümünü Data Lake Analytics araçları](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

**Köşe Seçici** üzerinde sol sağlar (ilk 10 veri okuma, veya gibi aşamasına göre seçin) özellikleri tarafından köşe seçin. En sık kullanılan filtrelerden biri olduğunu görmek için **kritik yol üzerindeki köşelerin**. **Kritik yol** köşelerin bir U-SQL işinin en uzun zinciri. Kritik yol anlama, köşe uzun sürüyor denetleyerek işlerinizi iyileştirmek için kullanışlıdır.
  
![Köşe yürütme görünümünü Data Lake Analytics araçları](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

Üst Orta bölmesinde gösterildiği **tüm köşelerin durumunu çalıştıran**.
  
![Köşe yürütme görünümünü Data Lake Analytics araçları](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Alt Orta bölmede, her köşe hakkında bilgileri gösterir:
* İşlem adı: Köşe örneğinin adı. Farklı parçaların StageName içinde oluşan | VertexName | VertexRunInstance. Örneğin, ikinci çalışan örneği (.v1, dizini 0'dan itibaren) SV7_Split [62] .v1 köşe gösterir 62 köşe sayısının aşama SV7_Split.
* Toplam veri okunan/yazılan: Bu köşenin tarafından okunan/yazılan veri.
* Durumu/durum çıkışı: Köşe sonlandırıldığında son durumu.
* Kod/hata türü çıkış: Köşe başarısız olduğunda hata oluştu.
* Oluşturma nedeni: Neden köşe oluşturuldu.
* Kaynak gecikme süresi/işlem gecikme süresi/PN kuyruk gecikme süresi: kaynaklar için beklenecek köşe verilerini işlemek ve kuyrukta kalmak için geçen süre.
* İşlem/Oluşturucu GUID'si: Geçerli çalışan köşe veya oluşturana GUİD'i.
* Sürüm: n. örneğini çalıştıran köşe (yedeklilik, vb. birçok nedeni, örneğin yük devretme, işlem için sistem yeni bir köşe örneklerini zamanlayabilirsiniz.)
* Sürüm oluşturma saati.
* İşlem oluşturma başlangıç zaman/işlemi sıraya alınan zaman/işlem başlangıç saati/işlem tam zamanı: oluşturma; köşe işlem başladığında Köşe işlem kuyruğuna başladığında; belirli köşe işlem başladığında; belirli bir köşe tamamlandığında.

## <a name="next-steps"></a>Sonraki adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* İş ayrıntılarını görüntülemek için bkz: [kullanımı iş tarayıcı ve Azure Data lake Analytics işleri için iş görünümünü](data-lake-analytics-data-lake-tools-view-jobs.md)
