---
title: İptal etmek ve bir Azure içeri/dışarı aktarma işini silmek | Microsoft Docs
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
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>İptal etme ve Azure içeri/dışarı aktarma işleri siliniyor

 Bir işi olarak önce iptal istemek için `Packaging` durum, çağrı [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) işlemi ve kümesi `CancelRequested` öğesine `true`. En iyi çaba ilkesine göre işleri iptal edilir. Veri aktarma işlemi yapıyor sürücülerdir, veri iptal talep edilen sonra aktarılacak devam edebilir.

 İptal edilen işi taşınır `Completed` durum ve 90 gün boyunca, bu noktada, silindiğinden tutulur.

 Bir işi silmek için arama [iş Sil](/rest/api/storageimportexport/jobs#Jobs_Delete) iş sevk edilmiş önce işlemi (diğer bir deyişle, iş, aktarılırken `Creating` durumu). İçinde olduğunda bir iş ayrıca silebilirsiniz `Completed` durumu. Bir işi silindikten sonra kendi bilgilerini ve durumunu artık REST API veya Azure portalı erişilebilir.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'si kullanma](storage-import-export-using-the-rest-api.md)
