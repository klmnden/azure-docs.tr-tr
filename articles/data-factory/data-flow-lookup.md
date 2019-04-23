---
title: Azure veri fabrikası veri akışı arama dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı arama dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: ef473ea5f88b9108894787785fe1e9083fab1b0a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59788012"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure veri fabrikası veri akışı arama dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Arama, veri akışı için başka bir kaynaktan başvuru verilerini eklemek için kullanın. Arama dönüştürme eden başvuru tablonuza ve anahtar alanları üzerinde eşleşen bir tanımlı kaynak gerektirir.

![Arama dönüşümü](media/data-flow/lookup1.png "arama")

Gelen akış alanları ve başvuru kaynağı alanlar arasında eşleştirmek istediğiniz anahtar alanları seçin. Önce yeni bir kaynak sağ taraftaki arama için kullanılacak veri akışı tasarım Tuvali üzerindeki oluşturmuş olmanız gerekir.

Eşleşme bulunduğunda, elde edilen satırları ve sütunları başvuru kaynaktan veri akışınıza eklenir. Havuz veri akışı sonuna dahil etmek istediğiniz ilgi alanları seçebilirsiniz.

## <a name="optimizations"></a>En iyi duruma getirme

Data Factory'de veri akışları, ölçeği genişletilen Spark ortamlarda yürütün. Veri kümenizde çalışan düğümü bellek alanına sığıyorsa biz arama performansınızı en iyi duruma getirebilirsiniz.

![Birleştirme yayını](media/data-flow/broadcast.png "yayın birleştirme")

### <a name="broadcast-join"></a>Yayın birleştirme

Sol seçin ve/veya veri kümesinin tamamının belleğe arama ilişkisi her iki taraftan göndermek için ADF istemek için birleştirme sağ tarafındaki yayın.

### <a name="data-partitioning"></a>Veri bölümleme

"Ayarlamak daha iyi çalışan her belleğe sığması veri kümelerini oluşturmak için bölümleme" iyileştirme sekmesinde arama dönüşümü seçerek verilerinizi bölümlemeyi de belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Birleştirme](data-flow-join.md) ve [EXISTS](data-flow-exists.md) dönüştürmeleri ADF eşleme veri akışları'nda benzer görevleri gerçekleştirebilirsiniz. Bu dönüştürme işlemleri bir sonraki göz atın.
