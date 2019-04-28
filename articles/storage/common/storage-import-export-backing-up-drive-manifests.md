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
ms.openlocfilehash: ea223ea3ccd113014ceabff34cc4d0174abb1ddf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483137"
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Azure içeri/dışarı aktarma işleri için bildirimlerini sürücünün yedekleme

Sürücü bildirimlerini otomatik olarak yedeklenebilir blob'lara ayarlayarak `BackupDriveManifest` özelliğini `true` içinde [Put işlemini](/rest/api/storageimportexport/jobs) veya [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs) REST API işlemleri. Varsayılan olarak, sürücü bildirimlerini yedeklenmez. Sürücü bildirim yedekleme işi ile ilişkili depolama hesabında bir kapsayıcıda blok blobları olarak depolanır. Varsayılan olarak, kapsayıcı adı olan `waimportexport`, ancak farklı bir ad belirtebilirsiniz `DiagnosticsPath` çağrılırken özellik `Put Job` veya `Update Job Properties` operations. Yedekleme bildirim blob şu biçimde adlandırılır: `waies/jobname_driveid_timestamp_manifest.xml`.

 Bir iş için yedekleme sürücü bildirimlerini URI'sini çağırarak alabilirsiniz [alma işi](/rest/api/storageimportexport/jobs) işlemi. URI döndürülür blob `ManifestUri` her sürücü için özellik.

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'sini kullanma](storage-import-export-using-the-rest-api.md)
