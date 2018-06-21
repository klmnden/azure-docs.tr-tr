---
title: Olay tabanlı tetikleyiciler Azure Data Factory oluşturma | Microsoft Docs
description: Bir olaya yanıt olarak bir ardışık düzen çalıştıran bir Azure Data factory'de bir tetikleyici oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: douglasl
ms.openlocfilehash: 457983021034d83e0eed05bd91eae1ac30c046da
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296158"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Bir olaya yanıt olarak bir ardışık düzen çalışır bir Tetikleyici oluşturma

Bu makalede, veri fabrikası hatlarınızı oluşturabilirsiniz olay tabanlı tetikleyiciler açıklanmaktadır.

Olay kaynaklı (EDA) üretim, algılama, tüketim ve olaylarına tepki içeren ortak bir veri tümleştirme deseni mimarisidir. Veri tümleştirme senaryolarına genellikle olaylara dayanarak ardışık düzen tetiklemek Data Factory müşteriler gerektirir.

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

### <a name="create-a-new-event-trigger"></a>Yeni bir olay tetikleyicisi oluşturma

Tipik bir olay, bir dosya gelmesi veya Azure Storage hesabınızdaki bir dosya silme işlemi ' dir. Bu durumda, Data Factory işlem hattı yanıtlayan tetikleyici oluşturabilirsiniz.

![Yeni olay tetikleyicisi oluşturma](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### <a name="select-the-event-trigger-type"></a>Olay Tetikleyici türünü seçin

Dosya depolama konumu olarak ulaştığında ve karşılık gelen blob oluşturulduktan hemen sonra bu olay tetiklenir ve Data Factory işlem hattı çalıştırır. Bir blob oluşturma olayı, bir blob silme olayı veya her iki olayların, Data Factory işlem hatlarınızı yanıtlayan tetikleyici oluşturabilirsiniz.

![Olay Seç tetikleyici türü](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### <a name="configure-the-event-trigger"></a>Olay tetikleyicisi yapılandırın

İle **Blob yolu başlıyorsa** ve **Blob yolu bitiyor ile** özelliklerini kapsayıcıları, klasörler ve olayları almak istediğiniz blob adları belirtebilirsiniz. Her ikisi için desenleri çeşitli kullanabilirsiniz **Blob yolu başlıyorsa** ve **Blob yolu bitiyor ile** daha sonra bu makaledeki örneklerde gösterildiği gibi özellikler. Bu özelliklerden en az biri gereklidir.

![Olay tetikleyicisi yapılandırın](media/how-to-create-event-trigger/event-based-trigger-image3.png)

## <a name="json-schema"></a>JSON şeması

Aşağıdaki tabloda, olay tabanlı tetikleyiciler ilgili şema öğeleri genel bir bakış sağlar:

| **JSON öğesi** | **Açıklama** | **Tür** | **İzin verilen değerler** | **Gerekli** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **Kapsam** | Depolama hesabı Azure Resource Manager kaynak kimliği. | Dize | Azure Resource Manager kimliği | Evet |
| **Olayları** | Bu tetikleyici harekete neden olayların türünü. | Dizi    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Evet, herhangi bir birleşimi. |
| **blobPathBeginsWith** | Blob yolu tetiklenecek tetikleyici için sağlanan desen ile başlaması gerekir. Örneğin, '/ kayıt / / aralık/BLOB' yalnızca tetikleyici kayıtları kapsayıcısı altında aralık klasöründeki BLOB'lar için ateşlenir. | Dize   | | Bu özelliklerden en az biri sağlanmalıdır: blobPathBeginsWith, blobPathEndsWith. |
| **blobPathEndsWith** | Blob yolu tetiklenecek tetikleyici için sağlanan desen ile bitmelidir. Örneğin, 'december/boxes.csv' yalnızca bir aralık klasör kutularında adlı BLOB'ları için tetikleyici ateşlenir. | Dize   | | Bu özelliklerden en az biri sağlanmalıdır: blobPathBeginsWith, blobPathEndsWith. |

## <a name="examples-of-event-based-triggers"></a>Olay tabanlı tetikleyiciler örnekleri

Bu bölümde, olay tabanlı tetikleyici ayarları örnekleri sağlar.

-   **BLOB yolu başlıyorsa**('/ containername /') – olayları için herhangi bir blob kapsayıcısında alır.
-   **BLOB yolu başlıyorsa**('/ containername/KlasörAdı') – containername kapsayıcı ve KlasörAdı klasöründe BLOB olaylarını alır.
-   **BLOB yolu başlıyorsa**('/ containername/foldername/file.txt') – containername kapsayıcısı altında KlasörAdı klasöründeki dosya.txt'yi adlı bir blob için olayları alır.
-   **BLOB yolu bitiyor ile**('dosya.txt'yi') – bir blob Receives olaylarını adlı dosya.txt'yi herhangi yolundaki.
-   **BLOB yolu bitiyor ile**('/ containername/file.txt') – kapsayıcı containername altında dosya.txt'yi adlı bir blob için olayları alır.
-   **BLOB yolu bitiyor ile**('foldername/file.txt') – bir blob Receives olaylarını adlı dosya.txt'yi KlasörAdı klasöründeki tüm kapsayıcısı altında.

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz: [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers).
