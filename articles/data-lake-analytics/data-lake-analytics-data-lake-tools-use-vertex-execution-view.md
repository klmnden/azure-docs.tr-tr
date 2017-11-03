---
title: "Visual Studio için Data Lake araçları köşe yürütme görünümünde kullanın | Microsoft Docs"
description: "Köşe yürütme görünümü incelemesi Data Lake Analytics işlerini kullanmayı öğrenin."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
ms.openlocfilehash: b788e7bc8ded86ebd49cc0be73e5b4e1bcbeaba3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları köşe yürütme görünümünde kullanın
Köşe yürütme görünümü incelemesi Data Lake Analytics işlerini kullanmayı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

U-SQL betiği geliştirmek için Visual Studio için Data Lake araçları kullanarak temel bilgi gerekir.  Bkz: [öğretici: Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).

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
