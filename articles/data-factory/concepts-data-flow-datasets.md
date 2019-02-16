---
title: Azure veri fabrikası veri akış veri kümeleri eşlemesi
description: Azure Data Factory eşleme veri akışı sepecific veri kümesi uyumluluk sahip
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: ccf4273489d739bb9b0d802b79944efefcd02ff4
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331292"
---
# <a name="mapping-data-flow-datasets"></a>Veri akış veri kümeleri eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Veri kümeleri, işlem hattınızda çalıştığınız veri şeklini tanımlayan bir Data Factory yapısı var. Veri akışında satır ve sütun düzeyinde veri işlem hattı denetim akışı içinde veri kümeleri tarafından gerekenden çok daha ince ayrıntılı tanımı gerektirir.

Temel veri şemasını tanımlamak için akış veri kaynağı ve havuz dönüşümler veri kümelerinde kullanın. Şeması verinizdeki yoksa, şema değişikliklerini kaynak ve havuz için ayarlayabilirsiniz. Veri kümesinden tanımlanan şemasıyla ilgili veri türleri, veri biçimleri, dosya konumu ve ilişkili bağlı hizmeti bağlantı bilgileri olacaktır.

Şu anda veri akışı, dört veri kümeleri bulacaksınız:

* Azure SQL DB
* Azure SQL DW
* Parquet
* Sınırlandırılmış metin

Veri akış veri kümeleri ayrı kaynak *türü* gelen bağlı hizmet bağlantı türü. Data Factory, genellikle de (Blob, ADLS, vb.) bağlantı türünü seçin ve veri kümesinde dosya türü tanımlayabilirsiniz. Veri akışı içinde farklı bağlantılı hizmet bağlantı türleri ile ilişkili kaynak türlerini seçer.

![Dönüştürme seçenekleri kaynak](media/data-flow/dataset1.png "kaynakları")

Yeni bir veri kümesi oluştururken, "Veri akışı uyumlu" sağ üst kısımdaki panel etiketli onay kutusu yok. Bu düğmeye tıklayarak veri akışları ile kullanılabilecek veri kümelerini filtreleyin. 

Şemaları İçeri Aktar

Veri akışı veri kümesi şemasını içeri aktarılırken bir şemayı İçeri Aktar düğmesi görürsünüz. Bu düğmeye tıklayarak, iki seçenek sunar: Kaynak sunucudan içeri aktarmak veya yerel bir dosyadan içeri aktarın. Çoğu durumda, şema doğrudan kaynaktan alınır. Ancak, var olan bir şema dosyası varsa, yerel bir dosyaya işaret edebilir ve Data Factory, şema dosyasını temel alan bir şema tanımlar.

