---
title: Azure veri fabrikası veri akışı arama dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı arama dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: b156917a9987b023a9bf94e51c0cc14aebb133c7
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56738394"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure veri fabrikası veri akışı arama dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Arama, veri akışı için başka bir kaynaktan başvuru verilerini eklemek için kullanın. Arama dönüştürme eden başvuru tablonuza ve anahtar alanları üzerinde eşleşen bir tanımlı kaynak gerektirir.

![Arama dönüşümü](media/data-flow/lookup1.png "arama")

Gelen akış alanları ve başvuru kaynağı alanlar arasında eşleştirmek istediğiniz anahtar alanları seçin. Önce yeni bir kaynak sağ taraftaki arama için kullanılacak veri akışı tasarım Tuvali üzerindeki oluşturmuş olmanız gerekir.

Eşleşme bulunduğunda, elde edilen satırları ve sütunları başvuru kaynaktan veri akışınıza eklenir. Havuz veri akışı sonuna dahil etmek istediğiniz ilgi alanları seçebilirsiniz.
