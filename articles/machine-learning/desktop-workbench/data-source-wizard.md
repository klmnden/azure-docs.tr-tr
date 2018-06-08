---
title: Azure veri kaynağı Sihirbazı'nı Azure Machine Learning için | Microsoft Docs
description: Veri kaynağı Sihirbazı AML ekranının açıklar
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: dcf064817f2ad9855cfeef8fec6775d32ee992ec
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831148"
---
# <a name="data-source-wizard"></a>Veri kaynağı Sihirbazı #

Veri kaynağı Sihirbazı'nı, bir veri kümesi kodu olmadan Azure ML çalışma ekranına aktardığınız getirmek için hızlı ve kolay bir yoludur. Bu, aynı zamanda bir örnek stratejisi her sütun için veri kümesi ve veri türleri için seçebileceğiniz olur. 

## <a name="step-1-trigger-the-data-source-wizard"></a>1. adım: veri kaynağı Sihirbazı'nı tetikler ## 

Veri kaynağı Sihirbazı'nı kullanarak bir projeye verileri getirmek için. Seçin **+** düğmesini Veri Görünümü'ndeki arama kutusunun yanında ve veri kaynağı Ekle'i seçin. 

![Veri kaynağı ekleme](media/data-source-wizard/add-data-source.png)

## <a name="step-2-select-where-data-is-stored"></a>2. adım: verilerinin nerede depolanacağını belirleyin ##
İlk olarak, verilerinizin nasıl halen belirtin. Düz dosya veya dizin, bir parquet dosyası, bir Excel dosyasını veya bir veritabanı depolanabilir. Daha fazla bilgi için bkz: [desteklenen veri kaynakları](data-prep-appendix2-supported-data-sources.md).

![1. adım](media/data-source-wizard/step1.png)

## <a name="step-3-select-data-file"></a>3. adım: veri dosyası seçin ##
Bir dosya/dizin için dosya yolunu belirtin. Aşağı açılır listeden verilerin konumu seçin – bir yerel dosya yolu veya Azure Blob Storage olabilir. 

İçine yazarak veya tıklayarak yolunu belirtin **Gözat...** Bunun için gözatmak için düğmeyi. Bir dizin veya bir veya daha fazla göz atabilirsiniz.

Tıklatın **son** adımları geri kalanı için Varsayılanları kabul etmek için veya sonraki bir sonraki adıma devam etmek için.


![4. adım](media/data-source-wizard/step2.png)

## <a name="step-4-choose-file-parameters"></a>Adım 4: dosya parametreleri seçin ##

Veri kaynağı Sihirbazı'nı otomatik olarak dosyayı algılayabilir türü, ayırıcılar ve kodlama. Ayrıca verileri nasıl görüneceğini örneği göster. Ayrıca bu parametrelerden birini el ile değiştirebilirsiniz. 

![5. adım](media/data-source-wizard/step3.png)

## <a name="step-5-set-data-types-for-columns"></a>5. adım: sütunların veri türleri ayarlama ##

Veri kaynağı Sihirbazı'nı veri kümesi'nin sütunların veri türlerini otomatik olarak algılar. Bir İsabetsiz Okuma ya da bir veri türü zorlamak istiyorsanız, veri türü el ile değiştirebilirsiniz. **Örnek çıktı verilerini** sütun örnekleri veri nasıl göründüğünü gösterir.

Tarihleri içeren için veri hazırlığı oluşturur sütunlar için tarih biçiminde ay ve gün sırasını seçmek için istenebilir. Örneğin, 1/2/2013 Ocak 2 temsil edebilir (Bunun için seçin *gün ay*) veya Feburary 1 (seçin *ay gün*).

![6. adım](media/data-source-wizard/step4.png)

## <a name="step-6-choose-sampling-strategy-for-data"></a>6. adım: veri örnekleme stratejisi seçme ##

Veri kümesi için bir veya daha fazla örnekleme stratejileri belirtin ve etkin stratejisi olarak seçin. Üst 10000 satırları yüklemek için varsayılandır. Bu örnek tıklayarak düzenleyebilirsiniz **Düzenle** araç çubuğu düğmesini veya yeni bir strateji tıklayarak + yeni tarafından ekleyin. Şu anda destek stratejiler verilmiştir

-     İlk satır sayısı
-     Rastgele satır sayısı
-     Satır rastgele yüzdesi
-     Tam dosya

İstediğiniz, ancak yalnızca bir tane verilerinizi hazırlanırken etkin ayarlanabilecek sayıda örnekleme stratejileri belirtebilirsiniz. Stratejisi seçerek etkin olması için tüm stratejisini ayarlamak ve araç çubuğundaki etkin olarak Ayarla'yı tıklatın.

Burada verilerin geldiği bağlı olarak, bazı örnek stratejileri desteklenmiyor olabilir. Örnekleme hakkında daha fazla bilgi için örnekleme bölümünde bakın [bu belgede](data-prep-user-guide.md) 

![6. adım](media/data-source-wizard/step5.png)

## <a name="step-7-path-column-handling"></a>7. adım: Yolu sütun işleme ##

Dosya yolu önemli veri içeriyorsa, ilk sütun kümesindeki olarak dahil etmeyi seçebilirsiniz. Bu seçeneği birden çok dosyada getiren varsa yararlı olacaktır. Aksi takdirde, içermeyecek şekilde seçebilirsiniz.

![7. adım](media/data-source-wizard/step6.png)

Son'a tıkladıktan sonra yeni bir veri kaynağı projeye eklenir. Bu veri kaynaklarını grubu altında veri görünümü'ndeki veya dsource dosyası olarak bulabileceğiniz **dosya görünümü**.
