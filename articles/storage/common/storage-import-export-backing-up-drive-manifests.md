---
title: Azure içeri/dışarı aktarma sürücü bildirimlerini yedekleme | Microsoft Docs
description: Otomatik olarak Microsoft Azure içeri/dışarı aktarma hizmeti için sürücü bildirimlerinizi sahip öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 61881508e18a2c7dbe1bc3be72d34423f862437a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55473400"
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Azure içeri/dışarı aktarma işleri için bildirimlerini sürücünün yedekleme

Sürücü bildirimlerini otomatik olarak yedeklenebilir blob'lara ayarlayarak `BackupDriveManifest` özelliğini `true` içinde [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) veya [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) REST API işlemleri. Varsayılan olarak, sürücü bildirimlerini yedeklenmez. Sürücü bildirim yedekleme işi ile ilişkili depolama hesabında bir kapsayıcıda blok blobları olarak depolanır. Varsayılan olarak, kapsayıcı adı olan `waimportexport`, ancak farklı bir ad belirtebilirsiniz `DiagnosticsPath` çağrılırken özellik `Put Job` veya `Update Job Properties` operations. Yedekleme bildirim blob şu biçimde adlandırılır: `waies/jobname_driveid_timestamp_manifest.xml`.

 Bir iş için yedekleme sürücü bildirimlerini URI'sini çağırarak alabilirsiniz [alma işi](/rest/api/storageimportexport/jobs#Jobs_Get) işlemi. URI döndürülür blob `ManifestUri` her sürücü için özellik.

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'sini kullanma](storage-import-export-using-the-rest-api.md)
