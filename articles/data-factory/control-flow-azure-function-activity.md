---
title: Azure Data factory'de bir Azure işlev etkinliği | Microsoft Docs
description: Azure işlev etkinliği bir Data Factory işlem hattı, bir Azure işlevi çalıştırmak için kullanmayı öğrenin
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: douglasl
ms.openlocfilehash: ef93c62a2e2084a43eeda578c889a568d04db4f1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856907"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Azure Data factory'de bir Azure işlev etkinliği

Azure işlev etkinliği çalıştırmanıza izin veren [Azure işlevleri](../azure-functions/functions-overview.md) Data Factory işlem hattında. Bir Azure işlevi çalıştırmak için bir bağlı hizmet bağlantısı ve yürütme planladığınız Azure işlevi belirten bir etkinlik oluşturmak gerekir.

## <a name="azure-function-linked-service"></a>Azure bağlantılı işlev hizmeti

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| type   | Type özelliği ayarlanmalıdır: **AzureFunction** | evet |
| işlevi uygulama URL'si | Azure işlev uygulaması için URL. Biçim `https://<accountname>.azurewebsites.net`. Bu URL'yi altında değerdir **URL** bölümünde Azure portalında işlev uygulamanızı görüntülerken  | evet |
| işlev tuşu | Azure işlevi için erişim anahtarı. Tıklayarak **Yönet** bölüm için ilgili işlevi ve ya da kopyalama **işlev anahtarı** veya **ana bilgisayar anahtarı**. Daha fazla buradan edinin: [Azure işlevleri HTTP Tetikleyicileri ve bağlamaları](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | evet |
|   |   |   |

## <a name="azure-function-activity"></a>Azure işlev etkinliği

| **Özellik**  | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| ad  | İşlem hattındaki etkinliğin adı  | Dize | evet |
| type  | 'AzureFunctionActivity' etkinlik türünde | Dize | evet |
| Bağlı hizmet | Azure bağlantılı işlev hizmet için karşılık gelen Azure işlev uygulaması  | Bağlı hizmet başvurusu | evet |
| İşlev adı  | Azure işlev uygulaması bu etkinlik çağıran işlevin adı | Dize | evet |
| method  | İşlev çağrısı için REST API yöntemi | Dize türleri desteklenir: "GET", "POST", "PUT"   | evet |
| üst bilgi  | Gönderilen istek için üstbilgiler. Örneğin, türü ve dili, bir istek üzerinde ayarlanan için: "üst": {"Accept-Language": "en-us", "Content-Type": "application/json"} | Dize (veya dizenin ifadenin resulttype'ı ile) | Hayır |
| body  | işlev API yöntemi istekle birlikte gönderilen gövdesi  | Dize (veya dizenin ifadenin resulttype'ı ile).   | PUT/POST yöntemleri için gerekli |
|   |   |   | |

İstek yükü şemayı [istek yükü şeması](control-flow-web-activity.md#request-payload-schema) bölümü.

## <a name="next-steps"></a>Sonraki adımlar

Data Factory'de etkinlikleri hakkında daha fazla bilgi [işlem hatları ve etkinlikler Azure Data factory'de](concepts-pipelines-activities.md).