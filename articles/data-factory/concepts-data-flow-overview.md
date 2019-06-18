---
title: Azure veri fabrikası veri akışı genel bakış eşleme
description: Azure Data factory'de veri akışları eşleme genel bakış açıklaması
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 051886f98d6d35594336291bbb2defb2a4acdfc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233056"
---
# <a name="what-are-mapping-data-flows"></a>Veri akışları eşleme nelerdir?

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Eşleme veri akışları Azure Data factory'de görsel olarak tasarlanmış veri dönüştürme var. Bir veri akışı veri mühendisleri, kod yazmaya gerek kalmadan grafik veri dönüştürme mantığını geliştirmek izin verin. Sonuçta elde edilen bir veri akışı, Azure veri fabrikası genişletilmiş Azure Databricks kümeleri kullanarak işlem hatlarını içindeki etkinlikleri olarak yürütülür.

Azure Data Factory, veri akışı amacı gerekli kodlama ile tam olarak görsel bir deneyim sağlamaktır. Veri akışları, ölçeği genişletilmiş veri işleme için kendi yürütme kümesinde yürütülür. Azure Data Factory tüm kod çevirisi, yolu en iyi duruma getirme ve yürütme veri akışı işlerinizin yürütür.

Bir veri akışı hata ayıklama modunda dönüştürme mantığınızı etkileşimli olarak doğrulayabilmesi oluşturarak başlayın. Ardından, bir veri akış etkinliğini yürütmek için işlem hattını ve veri işlem hattı hata ayıklama akışı veya işlem hattından etkinlik, veri akışı test etmek için "Şimdi Tetikle" işlem hattı kullanın. test ekleyin.

Sonra zamanlama ve yürütme veri akışı etkinliği Azure Data Factory işlem hatlarını kullanarak, veri akışı etkinlikleri izleyin.

Hata ayıklama modu iki durumlu düğme veri akışı tasarım yüzeyinde, etkileşimli veri dönüşümleri oluşturulmasını sağlar. Hata ayıklama modu veri akışı oluşturma için bir veri hazırlık ortamı sağlar.
