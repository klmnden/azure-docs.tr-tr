---
title: Azure veri fabrikası veri akışı seçin dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı seçin dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: bc83b41067d587adce41658a2c4b3d68969750ba
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61364482"
---
# <a name="azure-data-factory-mapping-data-flow-select-transformation"></a>Azure veri fabrikası veri akışı seçin dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Bu dönüştürme için sütun seçiciliği (sütun sayısı azaltılırsa) kullanın veya diğer sütunları ve akışı adları.

Select dönüştürme, diğer tüm bir akışı sağlar veya bu akıştaki sütunları farklı adlardır (takma adlardır) atayın ve ardından bu yeni adları daha sonra veri akışı başvuru. Bu dönüşüm, kendi kendine birleşme senaryoları için kullanışlıdır. Kendi kendine birleşme ADF veri akışı uygulamak için bir akış dalı "Dalı" yazıp hemen sonra ekleyin "Seçin" dönüşüm gerçekleştirilecek yoludur. Bu akış artık kendi kendine birleşme oluşturma geri özgün akışına katılmak için kullanabileceğiniz yeni bir adı olacaktır:

![Kendi kendine birleşme](media/data-flow/selfjoin.png "kendi kendine birleşim")

Yukarıdaki diyagramda, Select dönüştürme en üstünde alır. Diğer ad kullanımı "OrigSourceBatting" özgün akışa budur. Aşağıdaki higlighted birleştirme dönüştürme bu seçim diğer akış sağ birleştirme, aynı anahtarı sol ve sağ tarafında iç birleşim başvurmak bize verme kullandığımızı görebilirsiniz.

Bir yol seçimini gibi veri akışınız sütunları seçin de kullanılabilir. Örneğin, havuzunuzu içinde tanımlanan 6 sütunları vardır, ancak yalnızca dönüştürmek ve ardından havuza akış için belirli bir 3 seçmek istiyorsanız, select dönüştürme kullanarak yalnızca 3 seçebilirsiniz.

> [!NOTE]
> "Yalnızca belirli sütunları seçmek için Tümünü Seç" geçmelidir

Seçenekler

Varsayılan ayar "Seçin" için tüm gelen sütunlarını ekleyin ve bu özgün adlarını saklamak sağlamaktır. Akış diğer adı seçin dönüştürme adını ayarlayarak yapabilirsiniz.

Diğer tek tek sütunlara yapılan "Tümünü Seç" seçimini kaldırın ve sütun eşleme altındaki kullanın.

![Dönüştürme seçin](media/data-flow/select001.png "diğer adı seçin")
