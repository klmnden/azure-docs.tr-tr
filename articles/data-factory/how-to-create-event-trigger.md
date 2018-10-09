---
title: Azure Data Factory'de olay tabanlı Tetikleyicileri oluşturma | Microsoft Docs
description: Azure Data factory'de bir işlem hattı, bir olaya yanıt olarak çalışan bir tetikleyici oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2018
ms.author: douglasl
ms.openlocfilehash: 38fbb62de60bc5604210c8ad7339368a04967c27
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867061"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Bir olaya yanıt olarak bir işlem hattı çalıştırmalarını tetiği oluşturma

Bu makale, Data Factory işlem hatlarınızı içinde oluşturabileceğiniz olay tabanlı Tetikleyicileri açıklar.

Olay denetimli mimari (EDA) üretim, algılama, tüketim ve olaylara tepki içeren ortak bir veri tümleştirme desendir. Veri tümleştirme senaryosunu genellikle Data Factory işlem hatları etkinliklere göre tetikleyin müşterilere gerektirir. Veri Fabrikası ile tümleştirilmiş Şimdi [Azure Event Grid](https://azure.microsoft.com/services/event-grid/), tetiklenen olanak sağlayan bir olay üzerinde işlem hatları.

10 dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> Bu makalede açıklanan tümleştirme bağımlı [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Event Grid kaynak sağlayıcısı ile aboneliğinize kayıtlı olduğundan emin olun. Daha fazla bilgi için bkz. [kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md#portal).

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

### <a name="create-a-new-event-trigger"></a>Yeni bir olay tetikleyicisi oluşturma

Tipik bir olay, bir dosya varış veya Azure depolama hesabınızdaki bir dosya silme işlemi ' dir. Bu olay, Data Factory işlem hattı ile yanıt veren bir tetikleyici oluşturabilirsiniz.

> [!NOTE]
> Bu tümleştirme yalnızca sürüm 2 depolama hesapları (genel amaçlı) destekler.

![Yeni bir olay tetikleyicisi oluşturma](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### <a name="configure-the-event-trigger"></a>Olay tetikleyicisi yapılandırın

İle **Blob yolu ile başlayan** ve **Blob yolu ile sona erer** özellikleri, kapsayıcılar, klasörler ve olaylarını almak istediğiniz blob adları belirtebilirsiniz. Her ikisi için desenler çeşitli kullanabilirsiniz **Blob yolu ile başlayan** ve **Blob yolu ile sona erer** bu makalenin ilerleyen bölümlerinde verilen örneklerde gösterildiği gibi özellikleri. Bu özelliklerden en az biri gereklidir.

![Olay tetikleyicisi yapılandırın](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### <a name="select-the-event-trigger-type"></a>Olay Tetikleyici türü seçin

Dosya depolama konumunuz ulaştığında ve karşılık gelen blob oluşturulduktan hemen sonra bu olayı tetikler ve, Data Factory işlem hattı çalıştırır. Bir blob oluşturma olayı, bir blob silme işlemi olay veya her iki olayları, Data Factory işlem hatlarınızı yanıt veren bir tetikleyici oluşturabilirsiniz.

![Olay türü tetikleyiciyi seçin](media/how-to-create-event-trigger/event-based-trigger-image3.png)

### <a name="map-trigger-properties-to-pipeline-parameters"></a>İşlem hattı parametrelerinin harita tetikleyici özellikleri

Belirli bir blob için bir Olay Tetikleyici etkinleştirildiğinde, olay özelliklerini blob klasörü yolu ve dosya adını yakalar `@triggerBody().folderPath` ve `@triggerBody().fileName`. Bir işlem hattı, bu özelliklerin değerlerini kullanmak için işlem hattı parametrelerinin özelliklerini eşlemeniz gerekir. Parametreleri eşleme özellikleri sonra Tetikleyici tarafından yakalanan değerlerine erişebilirsiniz `@pipeline().parameters.parameterName` işlem hattı boyunca ifade.

![İşlem hattı parametrelerinin özellikleri eşleme](media/how-to-create-event-trigger/event-based-trigger-image4.png)

Örneğin, önceki ekran görüntüsünde. Tetikleyici ne zaman sonu bir blob yolu ateşlenmesine yapılandırılmış `.csv` içinde depolama hesabı oluşturulur. Sonuç olarak, bir blob olduğunda `.csv` uzantısı her yerde depolama hesabında oluşturulan `folderPath` ve `fileName` özellikleri yeni blobunun konumunu yakalama. Örneğin, `@triggerBody().folderPath` gibi bir değere sahip `/containername/foldername/nestedfoldername` ve `@triggerBody().fileName` gibi bir değere sahip `filename.csv`. Bu değerleri örnek işlem hattı parametrelerine eşlenen `sourceFolder` ve `sourceFile`. İşlem hattı boyunca kullanabilirsiniz `@pipeline().parameters.sourceFolder` ve `@pipeline().parameters.sourceFile` sırasıyla.

## <a name="json-schema"></a>JSON şeması

Aşağıdaki tabloda, olay tabanlı Tetikleyicileri için ilgili şema öğelerinin genel bir bakış sağlar:

| **JSON öğesi** | **Açıklama** | **Tür** | **İzin verilen değerler** | **Gerekli** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **Kapsam** | Depolama hesabı Azure Resource Manager kaynak kimliği. | Dize | Azure Resource Manager kimliği | Evet |
| **Olayları** | Bu tetikleyici ateşlenmesine neden olayların türü. | Dizi    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Evet, herhangi bir birleşimi. |
| **blobPathBeginsWith** | Blob yolu için harekete geçirmek sağlanan deseni ile başlamalıdır. Örneğin, '/ kayıt / / aralık/blobları' yalnızca tetikleyici kayıt kapsayıcısı altında aralık klasördeki blobları için ateşlenir. | Dize   | | Bu özelliklerden en az biri sağlanmalıdır: blobPathBeginsWith, blobPathEndsWith. |
| **blobPathEndsWith** | Blob yolu için harekete geçirmek sağlanan deseni ile bitmelidir. Örneğin, 'december/boxes.csv' yalnızca tetikleyici bir aralık klasör kutularında adlı BLOB'ları için ateşlenir. | Dize   | | Bu özelliklerden en az biri sağlanmalıdır: blobPathBeginsWith, blobPathEndsWith. |

## <a name="examples-of-event-based-triggers"></a>Olay tabanlı Tetikleyicileri örnekleri

Bu bölümde, olay tabanlı tetikleyici ayarlarını örnekleri sağlar.

-   **BLOB yolu ile başlayan**('/ containername /') – kapsayıcısında tüm bloblar için olayları alır.
-   **BLOB yolu ile başlayan**('/ containername/blobları/foldername') – containername kapsayıcı ve KlasörAdı klasöründe tüm bloblar için olaylarını alır.
-   **BLOB yolu ile başlayan**('/ containername/blobs/foldername/file.txt') – containername kapsayıcısı altında KlasörAdı klasöründeki dosya.txt adlı bir blob için olaylarını alır.
-   **BLOB yolu ile sona erer**('dosya.txt') – alır olayları bir blobun herhangi bir yola dosya.txt adlı.
-   **BLOB yolu ile sona erer**('/ containername/blobs/file.txt') – dosya.txt kapsayıcı containername altında adlı bir blob için olaylarını alır.
-   **BLOB yolu ile sona erer**('foldername/file.txt') – bir blobun alır olayları adlı dosya.txt KlasörAdı klasöründeki tüm kapsayıcı altında.

> [!NOTE]
> Dahil etmek zorunda `/blobs/` kapsayıcı ve klasöre, kapsayıcı ve dosya ya da kapsayıcı, klasör belirtin ve dosya yolunun segmenti.

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz. [işlem hattı yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers).
