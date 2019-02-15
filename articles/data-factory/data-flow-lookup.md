---
title: Azure veri fabrikası veri akışı arama dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı arama dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 28c8f2b0005641934f7fcd9549f1cf004b206d80
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272141"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure veri fabrikası veri akışı arama dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Arama, veri akışı için başka bir kaynaktan başvuru verilerini eklemek için kullanın. Arama dönüştürme eden başvuru tablonuza ve anahtar alanları üzerinde eşleşen bir tanımlı kaynak gerektirir.

![Arama dönüşümü](media/data-flow/lookup1.png "arama")

Gelen akış alanları ve başvuru kaynağı alanlar arasında eşleştirmek istediğiniz anahtar alanları seçin. Önce yeni bir kaynak sağ taraftaki arama için kullanılacak veri akışı tasarım Tuvali üzerindeki oluşturmuş olmanız gerekir.

Eşleşme bulunduğunda, elde edilen satırları ve sütunları başvuru kaynaktan veri akışınıza eklenir. Havuz veri akışı sonuna dahil etmek istediğiniz ilgi alanları seçebilirsiniz.
