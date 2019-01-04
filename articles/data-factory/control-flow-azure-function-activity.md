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
ms.date: 12/20/2018
ms.author: douglasl
ms.openlocfilehash: 4b185236e5925152acb5f8a733e117186a2318cf
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53740901"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Azure Data factory'de bir Azure işlev etkinliği

Azure işlev etkinliği çalıştırmanıza izin veren [Azure işlevleri](../azure-functions/functions-overview.md) Data Factory işlem hattında. Bir Azure işlevi çalıştırmak için bir bağlı hizmet bağlantısı ve yürütme planladığınız Azure işlevi belirten bir etkinlik oluşturmak gerekir.

## <a name="azure-function-linked-service"></a>Azure bağlantılı işlev hizmeti

Azure işlevinin dönüş türü, geçerli bir JObject olması gerekir. Başka bir şey başarısız olur ve genel kullanıcı hatası oluşturuyor *hata arama uç noktası*.

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| type   | Type özelliği ayarlanmalıdır: **AzureFunction** | evet |
| işlevi uygulama URL'si | Azure işlev uygulaması için URL. Biçim `https://<accountname>.azurewebsites.net`. Bu URL'yi altında değerdir **URL** bölümünde Azure portalında işlev uygulamanızı görüntülerken  | evet |
| işlev tuşu | Azure işlevi için erişim anahtarı. Tıklayarak **Yönet** bölüm için ilgili işlevi ve ya da kopyalama **işlev anahtarı** veya **ana bilgisayar anahtarı**. Buradan daha fazla bilgi edinin: [Azure işlevleri HTTP Tetikleyicileri ve bağlamaları](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | evet |
|   |   |   |

## <a name="azure-function-activity"></a>Azure işlev etkinliği

| **Özellik**  | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| ad  | İşlem hattındaki etkinliğin adı  | Dize | evet |
| type  | 'AzureFunctionActivity' etkinlik türünde | Dize | evet |
| Bağlı hizmet | Azure bağlantılı işlev hizmet için karşılık gelen Azure işlev uygulaması  | Bağlı hizmet başvurusu | evet |
| İşlev adı  | Azure işlev uygulaması bu etkinlik çağıran işlevin adı | Dize | evet |
| method  | İşlev çağrısı için REST API yöntemi | Dize türleri desteklenir: "POST", "PUT GET"   | evet |
| üst bilgi  | Gönderilen istek için üstbilgiler. Örneğin, türü ve dili, bir istek üzerinde ayarlanan için: "üst": {"Accept-Language": "en-us", "Content-Type": "application/json"} | Dize (veya dizenin ifadenin resulttype'ı ile) | Hayır |
| body  | işlev API yöntemi istekle birlikte gönderilen gövdesi  | Dize (veya dizenin ifadenin resulttype'ı ile) veya nesne.   | PUT/POST yöntemleri için gerekli |
|   |   |   | |

İstek yükü şemayı [istek yükü şeması](control-flow-web-activity.md#request-payload-schema) bölümü.

## <a name="more-info"></a>Daha fazla bilgi

Azure işlev etkinliği destekleyen **yönlendirme**. Örneğin, uygulamanız aşağıdaki yönlendirme - kullanıyorsa `https://functionAPP.azurewebsites.net/api/functionName/{value}?code=<secret>` - sonra `functionName` olduğu `functionName/{value}`, istenen sağlamak parametreleştirebilirsiniz `functionName` zamanında.

Azure işlev etkinliği de destekler **sorguları**. Bir sorgunun parçası olarak sahip `functionName` - Örneğin, `HttpTriggerCSharp2?name=hello` - burada `function name` olduğu `HttpTriggerCSharp2`.

## <a name="next-steps"></a>Sonraki adımlar

Data Factory'de etkinlikleri hakkında daha fazla bilgi [işlem hatları ve etkinlikler Azure Data factory'de](concepts-pipelines-activities.md).