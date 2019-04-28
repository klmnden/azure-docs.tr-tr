---
title: İptal edin ve bir Azure içeri/dışarı aktarma işini silmek | Microsoft Docs
description: İptal etmek ve Microsoft Azure içeri/dışarı aktarma hizmeti için silme hakkında bilgi edinin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 6eb56413319208feef1b4ab51296fe12a1e0bcf2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483154"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>İptal etme ve Azure içeri/dışarı aktarma işleri siliniyor

 Önce bir iş iptal olduğunu istemek için `Packaging` durum, çağrı [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs) işlemi ve kümesi `CancelRequested` öğesine `true`. Bir en iyi çaba ilkesine göre işi iptal edildi. Sürücüleri veri aktarma aşamasında, veri iptal istendi sonra aktarılan devam edebilir.

 İptal edilen bir iş taşınır `Completed` belirtin ve 90 gün boyunca hangi noktada silinmiş tutulur.

 Bir işi silmek için çağrı [silme işi](/rest/api/storageimportexport/jobs) iş sevk edildi önce işlemi (diğer bir deyişle, iş içinde çalışırken `Creating` durumu). İçinde olduğunda bir iş de silebilirsiniz `Completed` durumu. Bir proje silindikten sonra kendi bilgilerini ve durumunu artık REST API'si veya Azure portalı erişilebilir.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'sini kullanma](storage-import-export-using-the-rest-api.md)
