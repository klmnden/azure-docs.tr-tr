---
title: "Azure içeri/dışarı aktarma sürücü bildirimlerini yedekleme | Microsoft Docs"
description: "Sürücü bildirimlerinizi otomatik olarak yedeklenen Microsoft Azure içeri/dışarı aktarma hizmeti için sahip öğrenin."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 594abd80-b834-4077-a474-d8a0f4b7928a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 33eb8e1eea8f8aa7b79ef3e54f2b1ed88dc794ae
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Azure içeri/dışarı aktarma işleri sürücünün yedekleme bildirimleri

Sürücü bildirimleri otomatik olarak yedeklenebilir BLOB'larını ayarlayarak `BackupDriveManifest` özelliğine `true` içinde [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) veya [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) REST API işlemleri. Varsayılan olarak, sürücü bildirimleri yedeklenmez. Sürücü bildirim yedeklemeler, blok bloblar bir kapsayıcıda işle ilişkili depolama hesabı olarak depolanır. Varsayılan olarak, kapsayıcı addır `waimportexport`, ancak farklı bir ad belirtebilirsiniz `DiagnosticsPath` çağrılırken özelliği `Put Job` veya `Update Job Properties` işlemleri. Yedekleme bildirim blob şu biçimde adlandırılır: `waies/jobname_driveid_timestamp_manifest.xml`.

 Çağırarak işi için yedekleme sürücü bildirimleri URI'sini alabilir [alma işi](/rest/api/storageimportexport/jobs#Jobs_Get) işlemi. URI döndürülür blob `ManifestUri` özelliği her bir sürücü için.

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'si kullanma](storage-import-export-using-the-rest-api.md)
