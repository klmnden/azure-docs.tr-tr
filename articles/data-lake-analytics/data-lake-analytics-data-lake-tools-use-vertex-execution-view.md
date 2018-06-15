---
title: Visual Studio için Data Lake araçları köşe yürütme görünümünde kullanın
description: Bu makalede, köşe yürütme görünümü incelemesi Data Lake Analytics işleri için kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: mumian
ms.author: jgao
manager: kfile
editor: jasonwhowell
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.topic: conceptual
ms.date: 10/13/2016
ms.openlocfilehash: af15bb9fd1131f598dc87f13c4af481b63d023e3
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34735450"
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları köşe yürütme görünümünde kullanın
Köşe yürütme görünümü incelemesi Data Lake Analytics işlerini kullanmayı öğrenin.


## <a name="open-the-vertex-execution-view"></a>Köşe yürütme görünümü Aç
U-SQL işi Visual Studio için Data Lake araçları içinde açın. Tıklatın **köşe yürütme görünümü** sol alt köşedeki içinde. Profilleri ilk yük istenebilir ve ağ bağlantınızı bağlı olarak biraz zaman alabilir.

![Data Lake Analytics köşe yürütme görünümü araçları](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Köşe yürütme görünümü anlama
Köşe yürütme görünümü üç bölümden oluşur:

![Data Lake Analytics köşe yürütme görünümü araçları](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

**Köşe Seçici** üzerinde sol sağlar (10 veri top gibi okuma veya aşamasına göre seçin) özellikleri tarafından köşeleri seçin. En sık kullanılan filtreleri biri görmek için **kritik yol köşelerinin**. **Kritik yol** köşeleri U-SQL işi, en uzun zinciri. Kritik yol anlama işleriniz hangi köşe uzun süren denetleyerek en iyi duruma getirme için yararlıdır.
  
![Data Lake Analytics köşe yürütme görünümü araçları](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

Üst Orta bölmesinde gösterir **tüm köşeleri durumunu çalıştıran**.
  
![Data Lake Analytics köşe yürütme görünümü araçları](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Alt bölmeyi her köşe hakkında bilgileri gösterir:
* İşlem adı: Köşe örneğinin adı. StageName farklı bölümlerinin oluşan | VertexName | VertexRunInstance. Örneğin, ikinci çalışan örneği (.v1, dizini 0'dan başlayarak) SV7_Split [62] .v1 köşe gösterir 62 köşe sayısının aşama SV7_Split.
* Toplam veri okuma/yazılan: Bu köşe tarafından okunur/yazılır veri.
* Durumu/Çıkış durumu: köşe sona erdikten sonra son durumu.
* Çıkış kodu/hatası türü: köşe başarısız olduğunda hata oluştu.
* Oluşturma nedeni: Köşe neden oluşturuldu.
* Kaynak gecikme/işlem gecikmesi/PN sıra gecikme süresi: kaynaklar için beklenecek köşe için verileri işlemek ve sırada kalmak için geçen süre.
* İşlem/Creator GUID: Geçerli çalışan köşe veya oluşturana GUID.
* Sürümü: (birçok nedeni, örneğin yük devretme, işlem için artıklık, vb. Sistem köşe yeni örneklerini zamanlama.) çalışan köşesinin n. örneği
* Sürümü zaman oluşturuldu.
* İşlem oluşturma başlangıç süre/işlem sıraya alınan süre/işlem başlangıç saati/işlem tam zamanı: köşe işlemi oluşturma; başladığında Köşe işlem kuyruğuna başladığında; belirli köşe işlem başladığında; belirli köşe tamamlandığında.

## <a name="next-steps"></a>Sonraki adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* İş ayrıntılarını görüntülemek için bkz: [kullanım iş tarayıcı ve Azure Data lake Analytics işleri için iş görünümünde](data-lake-analytics-data-lake-tools-view-jobs.md)
