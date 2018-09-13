---
title: Azure veri kaynağı Sihirbazı'nı Azure Machine Learning | Microsoft Docs
description: Veri kaynağı Sihirbazı'nın AML workbench açıklar
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: c74229504a43179673cc99ccff321b65e3f6ed4f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651362"
---
# <a name="data-source-wizard"></a>Veri kaynağı Sihirbazı #

Veri kaynağı Sihirbazı, bir veri kümesi, kodu olmadan Azure ML Workbench getirmek için hızlı ve kolay bir yoludur. Bu, ayrıca bir örnek stratejisi her sütun için veri kümesi ve veri türleri için seçebileceğiniz olur. 

## <a name="step-1-trigger-the-data-source-wizard"></a>1. adım: veri kaynağı Sihirbazı tetikleyin ## 

Veri kaynağı Sihirbazı'nı kullanarak bir projeye verileri getirmek için. Seçin **+** düğmesini veri görünümünde arama kutusunun yanındaki ve veri kaynağı Ekle öğesini seçin. 

![Veri kaynağı ekleme](media/data-source-wizard/add-data-source.png)

## <a name="step-2-select-where-data-is-stored"></a>2. adım: verilerinin nerede depolanacağını belirleyin. ##
İlk olarak verilerinizi nasıl şu anda belirtin. Düz dosya veya dizin, bir parquet dosyasına, bir Excel dosyasına veya bir veritabanı depolanabilir. Daha fazla bilgi için [desteklenen veri kaynakları](data-prep-appendix2-supported-data-sources.md).

![1. adım](media/data-source-wizard/step1.png)

## <a name="step-3-select-data-file"></a>3. adım: verileri dosyası seçin ##
Bir dosya/dizin için dosya yolunu belirtin. Açılan listeden veri konumunu seçin: bir yerel dosya yolu veya Azure Blob Depolama olabilir. 

Yazmak veya tıklayarak yolunu belirtin **Gözat...** Bunun için Gözat düğmesini. Bir dizin veya bir veya daha fazla dosya için göz atabilirsiniz.

Tıklayın **son** adımları geri kalanı için Varsayılanları kabul edin veya sonraki sonraki adıma geçin.


![4. adım](media/data-source-wizard/step2.png)

## <a name="step-4-choose-file-parameters"></a>4. adım: dosya parametreleri seçin ##

Veri kaynağı Sihirbazı dosyayı otomatik olarak algılayabilir türü, ayırıcılar ve kodlama. Ayrıca, verileri nasıl görüneceğini gösteren bir örnek de gösterebilir. Ayrıca bu parametrelerden herhangi birini el ile değiştirebilirsiniz. 

![5. adım](media/data-source-wizard/step3.png)

## <a name="step-5-set-data-types-for-columns"></a>5. adım: sütunlar için veri türleri ayarlama ##

Veri kaynağı Sihirbazı veri kümesinin sütunların veri türlerini otomatik olarak algılar. Bir kaçan ya da bir veri türü uygulamak istediğiniz veri türünü el ile değiştirebilirsiniz. **Örnek çıktı verilerini** sütun verilerin görünmesi örnekleri gösterir.

Tarihleri içeren Data Prep çıkarsar sütunlar için tarih biçiminde ay ve gün sırasını seçmek için istenebilir. Örneğin, 1/2/2013 2 Ocak gösterebilecek (Bunun için seçin *gün aylık*) veya Feburary 1 (seçin *ayın günü*).

![6. adım](media/data-source-wizard/step4.png)

## <a name="step-6-choose-sampling-strategy-for-data"></a>6. adım: veri örnekleme stratejisi seçin ##

Veri kümesi için bir veya daha fazla örnekleme stratejileri belirtin ve etkin strateji olarak seçin. İlk 10000 satır Yükleme varsayılandır. Bu örnek tıklayarak düzenleyebilirsiniz **Düzenle** düğme araç çubuğunda veya yeni bir strateji tıklayarak + yeni Ekle. Şu anda destek stratejiler şunlardır

-     Üst satır sayısı
-     Rastgele satır sayısı
-     Satırlar rastgele yüzdesi
-     Tam dosya

İstediğiniz, ancak yalnızca bir verilerinizi hazırlanırken etkin ayarlanabilen çok örnekleme stratejileri belirtebilirsiniz. Tüm strateji etkin stratejisine seçerek olacak şekilde ayarlayın ve araç çubuğundaki etkin olarak Ayarla'yı tıklatın.

Burada verilerin geldiği bağlı olarak, bazı örnek stratejileri desteklenmeyebilir. Örnekleme hakkında daha fazla bilgi için örnekleme bölümünde bakın [bu belge](data-prep-user-guide.md) 

![6. adım](media/data-source-wizard/step5.png)

## <a name="step-7-path-column-handling"></a>7. adım: Yol sütunu işleme ##

Önemli verilerin dosya yolu içeriyorsa, ilk sütunda veri kümesi olarak eklemek seçebilirsiniz. Bu seçenek birden çok dosyalarında olan çerçevesinde yararlı olabilir. Aksi takdirde, içermeyecek şekilde seçebilirsiniz.

![7. adım](media/data-source-wizard/step6.png)

Son'a tıkladıktan sonra yeni bir veri kaynağı projeye eklenir. Bu veri kaynakları grubu altında veri görünümünde veya dsource dosyasında bulabilirsiniz **dosya görünümü**.
