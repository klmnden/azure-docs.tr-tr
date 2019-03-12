---
title: Azure veri fabrikası veri akış veri kümeleri eşlemesi
description: Azure Data Factory eşleme veri akışı sepecific veri kümesi uyumluluk vardır
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: ad6cfdad519ab3901c58979970ea07439b3106e9
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2019
ms.locfileid: "57726932"
---
# <a name="mapping-data-flow-datasets"></a>Veri akış veri kümeleri eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Veri kümeleri, işlem hattınızda çalıştığınız veri şeklini tanımlayan bir Data Factory yapısı var. Veri akışı satır ve sütun düzeyi verileri ince ayrıntılı veri kümesi tanımı gerektirir. Denetim akışı işlem hatlarında kullanılan veri kümelerindeki verileri anlama aynı derinliğini gerektirmez.

Veri akışı veri kümeleri, kaynak ve havuz dönüşümlerini kullanılır. Bunlar, temel veri şemaları tanımlamak için kullanılır. Şeması verinizdeki yoksa, şema değişikliklerini kaynak ve havuz için ayarlayabilirsiniz. Veri kümesinden tanımlanan şemasıyla ilgili veri türleri, veri biçimleri, dosya konumu ve ilişkili bağlı hizmeti bağlantı bilgileri olacaktır. Meta veriler veri kümelerinden, kaynak dönüştürme "Projeksiyon" kaynağı olarak görünür. Projeksiyonun kaynak dönüşümü içinde tanımlanmış adları ve türleri ile verileri veri akışı gösterimi temsil ederken, şema kümesindeki şekli ve fiziksel veri türünü temsil eder.

## <a name="dataset-types"></a>Veri kümesi türleri

Şu anda veri akışı, dört veri kümesi türleri bulacaksınız:

* Azure SQL DB
* Azure SQL DW
* Parquet (Başlangıç, ADLS & Blob)
* Sınırlandırılmış metin (ADLS & Blob)

Veri akış veri kümeleri ayrı *kaynak türünü* gelen *bağlı hizmet bağlantı türü*. Data Factory, genellikle de (Blob, ADLS, vb.) bağlantı türünü seçin ve veri kümesinde dosya türü tanımlayabilirsiniz. Veri akışı içinde farklı bağlantılı hizmet bağlantı türleri ile ilişkili kaynak türlerini seçer.

![Dönüştürme seçenekleri kaynak](media/data-flow/dataset1.png "kaynakları")

## <a name="data-flow-compatible-datasets"></a>Veri akışı uyumlu veri kümeleri

Yeni bir veri kümesi oluştururken, "Veri akışı uyumlu" sağ üst kısımdaki panel etiketli onay kutusu yok. Bu düğmeye tıklayarak veri akışları ile kullanılabilecek veri kümelerini filtreleyin. 

## <a name="import-schemas"></a>Şemaları İçeri Aktar

Veri akışı veri kümesi şemasını içeri aktarılırken bir şemayı İçeri Aktar düğmesi görürsünüz. Bu düğmeye tıklayarak, iki seçenek sunar: Kaynak sunucudan içeri aktarmak veya yerel bir dosyadan içeri aktarın. Çoğu durumda, şema doğrudan kaynaktan alınır. Var olan bir şema dosyası (Parquet dosya veya üst bilgi içeren CSV) varsa, ancak yerel dosya ve Data Factory, şema dosyasını temel alan bir şema tanımlayın gösterebilirsiniz.

## <a name="delimited-text-dataset"></a>Sınırlandırılmış metin veri kümesi

Sınırlandırılmış metin kümesinde ya da tek sınırlayıcılar işlemek için sınırlayıcı ayarlayacak ('\t 'TSV,','for ' csv, ' |'...) veya birden çok karakter için sınırlayıcı kullanın. Üst bilgi satırı getirin ve ardından veri türlerini otomatik olarak algılamak için kaynak dönüşümünü gidin.

## <a name="next-steps"></a>Sonraki adımlar

Başlayın [yeni bir veri akışı oluşturma](data-flow-create.md) ve bir kaynak dönüştürme ekleyin. Daha sonra veri kaynağınız için yapılandırın.

Kullanım [kopyalama etkinliği](copy-activity-overview.md) tüm ADF gelen verileri getirmek için veri kaynağı ve veri akışı tarafından erişime ADLS veya Blob hazırlama.

