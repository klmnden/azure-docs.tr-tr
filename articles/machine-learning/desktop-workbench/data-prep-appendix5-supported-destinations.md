---
title: Desteklenen veri hedefleri ve Azure Machine Learning veri hazırlama ile çıktıların | Microsoft Docs
description: Bu belge, desteklenen hedefleri tam bir listesini sağlar ve Azure Machine Learning veri hazırlama için kullanılabilir çıkarır
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 123328010758eea6e7eadce29440e204f91dcef6
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647125"
---
# <a name="supported-data-exports-for-this-preview"></a>Bu önizleme için verileri dışarı aktarmaları desteklenir 
Bu, birkaç farklı biçimlerde dışa aktarmak mümkündür. Bu biçimler, Machine Learning iş akışının geri kalanı sonuçları tümleştirmeden önce veri hazırlama Ara sonuçlarını tutmak için kullanabilirsiniz.

## <a name="types"></a>Türler 
### <a name="csv-file"></a>CSV dosyası 
Bir virgülle ayrılmış değer dosyası depolamaya yazma.

#### <a name="options"></a>Seçenekler
- Satır sonları
- Null ile değiştirin
- Hataları Değiştir 
- Ayırıcı


### <a name="parquet"></a>Parquet 
Bir veri kümesi Parquet depolamaya yazma.

Bir biçim çeşitli depolama alanında sürebilir, parquet. Daha küçük veri kümeleri için bazen bir tek .parquet dosyası kullanılır. Çeşitli Python kitaplıkları için tek .parquet dosyaları okuma ve yazma desteği. 

Şu anda Azure Machine Learning Workbench'te yerel etkileşimli kullanım sırasında Parquet yazma PyArrow Python kitaplığı kullanır. Bu, tek dosyalı Parquet şu anda yerel etkileşimli kullanım sırasında desteklenen tek Parquet çıkış biçimi olduğu anlamına gelir.

Azure Machine Learning Workbench, (Spark üzerinde) ölçeklendirme işlemleri sırasında okuma ve yazma özellikleri Spark'ın Parquet kullanır. Parquet (şu anda desteklenen tek bir) için Spark'ın varsayılan çıkış biçimini bir Hive veri kümesine yapısına benzer. Bu, bir klasörün küçük bir bölüm daha büyük bir veri kümesinin her birçok .parquet dosyaları içeren anlamına gelir. 

#### <a name="caveats"></a>Uyarılar 
Parquet formatına görece küçük ve farklı kitaplıklar arasında bazı uygulama tutarsızlıklar içeriyor. Örneğin, Spark üzerinde Parquet yazarken sütun adları geçerli karakterlerdir kısıtlamaları getirir. PyArrow bunu yapın. Sütun adında şu karakterler olamaz: 
- , 
- ;
- {}
- ()
- \\N
- \\T
- =

>[!NOTE]
>- Spark ile uyumluluğu sağlamak için verileri Parquet, yazmak istediğiniz zaman bu karakterler sütun adları, oluşumunu ile değiştirilir ve alt çizgi (_).
>- Yerel ve ölçek genişletme çalıştığında, Parquet, uygulama, Python veya Spark ile yazılan tüm veriler arasında tutarlılık sağlamak için sütun adlarını Spark uyumluluğu sağlamak için sterilize. Spark Parquet karakter yazılırken beklenen sütun adları emin olmak için bunları yazmadan önce sütunlarından ayarlama geçersiz kaldırın.



## <a name="locations"></a>Konumlar 
### <a name="local"></a>Yerel 
Yerel sabit diske veya eşlenmiş ağ depolama konumu.

### <a name="azure-blob-storage"></a>Azure Blob depolama
Azure Blob Depolama, bir Azure aboneliği gerektirir.

