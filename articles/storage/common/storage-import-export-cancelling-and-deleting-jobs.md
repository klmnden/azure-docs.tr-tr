---
title: İptal edin ve bir Azure içeri/dışarı aktarma işini silmek | Microsoft Docs
description: İptal etmek ve Microsoft Azure içeri/dışarı aktarma hizmeti için silme hakkında bilgi edinin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3524f1677baaa218b009b8498b851390c7b9da9a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38698698"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>İptal etme ve Azure içeri/dışarı aktarma işleri siliniyor

 Önce bir iş iptal olduğunu istemek için `Packaging` durum, çağrı [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) işlemi ve kümesi `CancelRequested` öğesine `true`. Bir en iyi çaba ilkesine göre işi iptal edildi. Sürücüleri veri aktarma aşamasında, veri iptal istendi sonra aktarılan devam edebilir.

 İptal edilen bir iş taşınır `Completed` belirtin ve 90 gün boyunca hangi noktada silinmiş tutulur.

 Bir işi silmek için çağrı [silme işi](/rest/api/storageimportexport/jobs#Jobs_Delete) iş sevk edildi önce işlemi (diğer bir deyişle, iş içinde çalışırken `Creating` durumu). İçinde olduğunda bir iş de silebilirsiniz `Completed` durumu. Bir proje silindikten sonra kendi bilgilerini ve durumunu artık REST API'si veya Azure portalı erişilebilir.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'sini kullanma](storage-import-export-using-the-rest-api.md)
