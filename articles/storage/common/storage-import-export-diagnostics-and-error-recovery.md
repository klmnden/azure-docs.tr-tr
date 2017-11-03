---
title: "Azure içeri/dışarı aktarma işleri için tanılama ve hata kurtarma | Microsoft Docs"
description: "Ayrıntılı Microsoft Azure içeri/dışarı aktarma hizmeti işleri için günlüğü etkinleştirmek öğrenin."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 096cc795-9af6-4335-9fe8-fffa9f239a17
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 0068aae9d6780aa41a070db0eb191d0d5a165d21
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Azure içeri/dışarı aktarma işleri için tanılama ve hata kurtarma
İşlenen her sürücü için Azure içeri/dışarı aktarma hizmeti ilişkili depolama hesabında bir hata günlüğü oluşturur. Ayarlayarak ayrıntılı günlük kaydını etkinleştirebilirsiniz `LogLevel` özelliğine `Verbose` çağrılırken [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) veya [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) işlemleri.

 Varsayılan olarak, günlükler adlı bir kapsayıcı yazılır `waimportexport`. Farklı bir ad ayarlayarak belirtebilirsiniz `DiagnosticsPath` çağrılırken özelliği `Put Job` veya `Update Job Properties` işlemleri. Günlükleri aşağıdaki adlandırma kuralıyla blok blobları olarak depolanır: `waies/jobname_driveid_timestamp_logtype.xml`.

 Bir işi için kayıtlar URI'sini çağırarak alabilir [alma işi](/rest/api/storageimportexport/jobs#Jobs_Get) işlemi. Ayrıntılı günlük URI'sini döndürülür `VerboseLogUri` özelliği, hata günlüğü için URI döndürürken her sürücü için `ErrorLogUri` özelliği.

Günlük verilerini aşağıdaki sorunları belirlemek için kullanabilirsiniz.

## <a name="drive-errors"></a>Sürücü hataları

Aşağıdaki öğeler sürücüsü hataları sınıflandırılan:

-   Erişimi veya bildirim dosyasını okuma hataları

-   Yanlış BitLocker anahtarları

-   Okuma/yazma hataları sürücü

## <a name="blob-errors"></a>BLOB hataları

Aşağıdaki öğeler blob hataları olarak sınıflandırılan:

-   Yanlış veya çakışan blob veya adları

-   Eksik dosyaları

-   Blob bulunamadı

-   Kesilmiş dosyaları (diskteki dosyaları bildiriminde belirtilenden daha küçük)

-   Bozuk dosya içerik (için içeri aktarma işi ile bir MD5 sağlama toplamı eşleşmezliği algıladı)

-   Bozuk blob meta verileri ve özellik dosyaları (ile bir MD5 sağlama toplamı eşleşmezliği algıladı)

-   Blob özellikleri ve/veya meta veri dosyaları için yanlış şeması

Genel İş hala tamamlanırken burada bazı bölümleri içe veya dışa aktarma işleminin başarıyla tamamlanması değil durumlar olabilir. Bu durumda, karşıya yükleme veya ağ üzerinden veri eksik parçalarını indirin veya verileri aktarmak için yeni bir proje oluşturabilirsiniz. Bkz: [Azure içeri/dışarı aktarma aracı başvurusu](storage-import-export-tool-how-to-v1.md) verileri ağ üzerinden onarmak hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'si kullanma](storage-import-export-using-the-rest-api.md)
