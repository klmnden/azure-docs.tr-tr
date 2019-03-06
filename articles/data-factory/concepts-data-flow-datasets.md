---
title: Azure veri fabrikası veri akış veri kümeleri eşlemesi
description: Azure Data Factory eşleme veri akışı sepecific veri kümesi uyumluluk vardır
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: c704fdba22138eb8e4610a7bc34bd4c2cc98f4ca
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57432099"
---
# <a name="mapping-data-flow-datasets"></a>Veri akış veri kümeleri eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Veri kümeleri, işlem hattınızda çalıştığınız veri şeklini tanımlayan bir Data Factory yapısı var. Veri akışı satır ve sütun düzeyi verileri ince ayrıntılı veri kümesi tanımı gerektirir. Denetim akışı işlem hatlarında kullanılan veri kümelerindeki verileri anlama aynı derinliğini gerektirmez.

Veri akışı veri kümeleri, kaynak ve havuz dönüşümlerini kullanılır. Bunlar, temel veri şemaları tanımlamak için kullanılır. Şeması verinizdeki yoksa, şema değişikliklerini kaynak ve havuz için ayarlayabilirsiniz. Veri kümesinden tanımlanan şemasıyla ilgili veri türleri, veri biçimleri, dosya konumu ve ilişkili bağlı hizmeti bağlantı bilgileri olacaktır. Meta veriler veri kümelerinden, kaynak dönüştürme "Projeksiyon" kaynağı olarak görünür.

## <a name="dataset-types"></a>Veri kümesi türleri

Şu anda veri akışı, dört veri kümesi türleri bulacaksınız:

* Azure SQL DB
* Azure SQL DW
* Parquet (Başlangıç, ADLS & Blob)
* Sınırlandırılmış metin (ADLB & Blob)

Veri akış veri kümeleri ayrı *kaynak türünü* gelen *bağlı hizmet bağlantı türü*. Data Factory, genellikle de (Blob, ADLS, vb.) bağlantı türünü seçin ve veri kümesinde dosya türü tanımlayabilirsiniz. Veri akışı içinde farklı bağlantılı hizmet bağlantı türleri ile ilişkili kaynak türlerini seçer.

![Dönüştürme seçenekleri kaynak](media/data-flow/dataset1.png "kaynakları")

## <a name="data-flow-compatible-datasets"></a>Veri akışı uyumlu veri kümeleri

Yeni bir veri kümesi oluştururken, "Veri akışı uyumlu" sağ üst kısımdaki panel etiketli onay kutusu yok. Bu düğmeye tıklayarak veri akışları ile kullanılabilecek veri kümelerini filtreleyin. 

## <a name="import-schemas"></a>Şemaları İçeri Aktar

Veri akışı veri kümesi şemasını içeri aktarılırken bir şemayı İçeri Aktar düğmesi görürsünüz. Bu düğmeye tıklayarak, iki seçenek sunar: Kaynak sunucudan içeri aktarmak veya yerel bir dosyadan içeri aktarın. Çoğu durumda, şema doğrudan kaynaktan alınır. Var olan bir şema dosyası (Parquet dosya veya üst bilgi içeren CSV) varsa, ancak yerel dosya ve Data Factory, şema dosyasını temel alan bir şema tanımlayın gösterebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Başlayın [yeni bir veri akışı oluşturma](data-flow-create.md) ve bir kaynak dönüştürme ekleyin. Daha sonra veri kaynağınız için yapılandırın.

