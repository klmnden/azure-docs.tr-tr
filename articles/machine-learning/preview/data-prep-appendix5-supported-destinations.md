---
title: "Desteklenen veri hedefleri ve Azure Machine Learning ile veri hazırlama çıktıların | Microsoft Docs"
description: "Bu belge desteklenen hedefleri tam bir listesini sağlar ve Azure Machine Learning veri hazırlığı kullanılabilir çıkarır"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 50d2d481b91199630bbfbf3cfdd21a1bf3062ff0
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="supported-data-exports-for-this-preview"></a>Bu önizleme için verileri dışarı aktarma desteklenen 
Birkaç farklı biçimlerde dışa aktarmak mümkündür. Machine Learning iş akışının geri kalanı sonuçları tümleştirmeden önce veri hazırlığı Ara sonuçlarını korumak için bu biçimlerini kullanabilirsiniz.

## <a name="types"></a>Türler 
### <a name="csv-file"></a>CSV dosyası 
Bir virgülle ayrılmış değer dosyası depolama alanına yazın.

#### <a name="options"></a>Seçenekler
- Satır sonları
- Null değerler ile değiştir
- Hatalarla değiştirin 
- ayırıcı


### <a name="parquet"></a>Parquet 
Bir veri kümesi Parquet depolama alanına yazın.

Parquet gibi bir biçim çeşitli depolama biçimlerde olabilir. Daha küçük veri kümeleri için tek .parquet dosyası bazen kullanılır. Çeşitli Python kitaplıkları tek .parquet dosyaları okuma ve yazma destekler. 

Şu anda, Azure Machine Learning çalışma sırasında yerel etkileşimli kullanım Parquet yazma PyArrow Python kitaplığı kullanır. Bu, tek dosyalı Parquet şu anda yerel etkileşimli kullanım sırasında desteklenen tek Parquet çıktı biçimi olduğu anlamına gelir.

Genişleme çalışır sırasında (Spark), Azure Machine Learning çalışma ekranı üzerinde okuma ve yazma yeteneklerini Spark'ın Parquet kullanır. Bir Hive veri kümesi yapısında Parquet (şu anda desteklenen tek bir) için Spark'ın varsayılan çıkış biçimi benzer. Başka bir deyişle, bir klasör daha büyük bir veri kümesi daha küçük bir bölümünü her birçok .parquet dosyalarını içerir. 

#### <a name="caveats"></a>Uyarılar 
Parquet bir biçim görece küçük yaştaki ve bazı uygulama tutarsızlıklar arasında farklı kitaplık vardır. Örneğin, Spark karakterler için Parquet yazılırken sütun adları geçerli olduğu kısıtlamaları getirir. PyArrow bunu yapabilirsiniz. Sütun adında şu karakterler olamaz: 
- ,
- ;
- {}
- ()
- \\n
- \\t
- =

>[!NOTE]
>- Spark uyum sağlamak için Parquet için veri yazma dilediğiniz zaman bu karakterler sütun adları tekrarı ile değiştirilir ve alt çizgi (_).
>- Yerel ve genişleme çalıştığında, uygulama, Python veya Spark Parquet için yazılan tüm veri arasında tutarlılık sağlamak için sütun adlarının Spark uyumluluğundan emin olmak için ayıklanır. Spark Parquet karakter yazılırken beklenen sütun adları emin olmak için bunları yazmadan önce sütunlarından ayarlamak geçersiz kaldırın.



## <a name="locations"></a>Konumlar 
### <a name="local"></a>Yerel 
Yerel sabit diske veya eşlenen ağ depolama konumu.

### <a name="azure-blob-storage"></a>Azure Blob depolama
Azure Blob Depolama bir Azure aboneliği gerektirir.

