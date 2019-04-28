---
title: Azure Data Factory'de olay tabanlı Tetikleyicileri oluşturma | Microsoft Docs
description: Azure Data factory'de bir işlem hattı, bir olaya yanıt olarak çalışan bir tetikleyici oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: 94c9c3f997143d72262c1ba3d8dbfea90d6f920c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61347730"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Bir olaya yanıt olarak bir işlem hattı çalıştırmalarını tetiği oluşturma

Bu makale, Data Factory işlem hatlarınızı içinde oluşturabileceğiniz olay tabanlı Tetikleyicileri açıklar.

Olay denetimli mimari (EDA) üretim, algılama, tüketim ve olaylara tepki içeren ortak bir veri tümleştirme desendir. Veri tümleştirme senaryosunu genellikle Data Factory işlem hatları etkinliklere göre tetikleyin müşterilere gerektirir. Veri Fabrikası ile tümleştirilmiş Şimdi [Azure Event Grid](https://azure.microsoft.com/services/event-grid/), tetiklenen olanak sağlayan bir olay üzerinde işlem hatları.

10 dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> Bu makalede açıklanan tümleştirme bağımlı [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Event Grid kaynak sağlayıcısı ile aboneliğinize kayıtlı olduğundan emin olun. Daha fazla bilgi için bkz. [kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md#azure-portal).

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
| **Kapsam** | Depolama hesabı Azure Resource Manager kaynak kimliği. | String | Azure Resource Manager kimliği | Evet |
| **Olayları** | Bu tetikleyici ateşlenmesine neden olayların türü. | Dizi    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Evet, bu değerlerden herhangi bir birleşimi. |
| **blobPathBeginsWith** | Blob yolu tetikleyiciyi harekete geçirmek sağlanan deseni ile başlamalıdır. Örneğin, `/records/blobs/december/` bloblar için yalnızca tetikleyici `december` klasörü altında `records` kapsayıcı. | String   | | Bu özelliklerden en az biri için bir değer sağlamanız gereken: `blobPathBeginsWith` veya `blobPathEndsWith`. |
| **blobPathEndsWith** | Blob yolu tetikleyiciyi harekete geçirmek sağlanan deseni ile bitmelidir. Örneğin, `december/boxes.csv` adlı bloblar için yalnızca tetikleyici `boxes` içinde bir `december` klasör. | String   | | Bu özelliklerden en az biri için bir değer sağlamanız gereken: `blobPathBeginsWith` veya `blobPathEndsWith`. |

## <a name="examples-of-event-based-triggers"></a>Olay tabanlı Tetikleyicileri örnekleri

Bu bölümde, olay tabanlı tetikleyici ayarlarını örnekleri sağlar.

> [!IMPORTANT]
> Dahil etmek zorunda `/blobs/` kapsayıcı ve klasöre, kapsayıcı ve dosya ya da kapsayıcı, klasör belirtin ve dosya olduğunda, aşağıdaki örneklerde gösterildiği gibi yol kesimi.

| Özellik | Örnek | Açıklama |
|---|---|---|
| **BLOB yolu ile başlar** | `/containername/` | Olayları için herhangi bir blob kapsayıcısında alır. |
| **BLOB yolu ile başlar** | `/containername/blobs/foldername/` | Tüm bloblar için olaylarını alır `containername` kapsayıcı ve `foldername` klasör. |
| **BLOB yolu ile başlar** | `/containername/blobs/foldername/subfoldername/` | Bir alt klasör de başvurabilirsiniz. |
| **BLOB yolu ile başlar** | `/containername/blobs/foldername/file.txt` | Adlı bir blob için olaylarını alır `file.txt` içinde `foldername` klasörü altında `containername` kapsayıcı. |
| **Biten BLOB yolu** | `file.txt` | Adlı bir blob için olaylarını alır `file.txt` herhangi bir yolda. |
| **Biten BLOB yolu** | `/containername/blobs/file.txt` | Adlı bir blob için olaylarını alır `file.txt` kapsayıcısı altında `containername`. |
| **Biten BLOB yolu** | `foldername/file.txt` | Adlı bir blob için olaylarını alır `file.txt` içinde `foldername` klasörü altında herhangi bir kapsayıcı. |

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz. [işlem hattı yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers).
