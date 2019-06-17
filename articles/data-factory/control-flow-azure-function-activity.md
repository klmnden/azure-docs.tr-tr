---
title: Azure Data factory'de bir Azure işlev etkinliği | Microsoft Docs
description: Azure işlev etkinliği bir Data Factory işlem hattı, bir Azure işlevi çalıştırmak için kullanmayı öğrenin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: 82786b8f01ce409179f4ddd37127679f9357cd0e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64727041"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Azure Data factory'de bir Azure işlev etkinliği

Azure işlev etkinliği çalıştırmanıza izin veren [Azure işlevleri](../azure-functions/functions-overview.md) Data Factory işlem hattında. Bir Azure işlevi çalıştırmak için bir bağlı hizmet bağlantısı ve yürütme planladığınız Azure işlevi belirten bir etkinlik oluşturmak gerekir.

Bir sekiz dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Run-Azure-Functions-from-Azure-Data-Factory-pipelines/player]

## <a name="azure-function-linked-service"></a>Azure bağlantılı işlev hizmeti

Geçerli bir Azure işlev dönüş türü olan `JObject`. (Aklınızda [JArray](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JArray.htm) olduğu *değil* bir `JObject`.) Herhangi bir başka tür dönüş `JObject` başarısız olur ve kullanıcı hatası oluşturuyor *yanıt içeriği değil geçerli JObject*.

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| type   | Type özelliği ayarlanmalıdır: **AzureFunction** | evet |
| işlevi uygulama URL'si | Azure işlev uygulaması için URL. Biçim `https://<accountname>.azurewebsites.net`. Bu URL'yi altında değerdir **URL** bölümünde Azure portalında işlev uygulamanızı görüntülerken  | evet |
| işlev tuşu | Azure işlevi için erişim anahtarı. Tıklayarak **Yönet** bölüm için ilgili işlevi ve ya da kopyalama **işlev anahtarı** veya **ana bilgisayar anahtarı**. Buradan daha fazla bilgi edinin: [Azure işlevleri HTTP Tetikleyicileri ve bağlamaları](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | evet |
|   |   |   |

## <a name="azure-function-activity"></a>Azure işlev etkinliği

| **Özellik**  | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| name  | İşlem hattındaki etkinliğin adı  | String | evet |
| türü  | 'AzureFunctionActivity' etkinlik türünde | String | evet |
| Bağlı hizmet | Azure bağlantılı işlev hizmet için karşılık gelen Azure işlev uygulaması  | Bağlı hizmet başvurusu | evet |
| İşlev adı  | Azure işlev uygulaması bu etkinlik çağıran işlevin adı | String | evet |
| method  | İşlev çağrısı için REST API yöntemi | Dize türleri desteklenir: "POST", "PUT GET"   | evet |
| üst bilgi  | Gönderilen istek için üstbilgiler. Örneğin, türü ve dili, bir istek üzerinde ayarlanan için: "üst": {"Accept-Language": "en-us", "Content-Type": "application/json"} | Dize (veya dizenin ifadenin resulttype'ı ile) | Hayır |
| Gövde  | işlev API yöntemi istekle birlikte gönderilen gövdesi  | Dize (veya dizenin ifadenin resulttype'ı ile) veya nesne.   | PUT/POST yöntemleri için gerekli |
|   |   |   | |

İstek yükü şemayı [istek yükü şeması](control-flow-web-activity.md#request-payload-schema) bölümü.

## <a name="routing-and-queries"></a>Yönlendirme ve sorguları

Azure işlev etkinliği destekleyen **yönlendirme**. Örneğin, Azure işlevinizi uç nokta varsa `https://functionAPP.azurewebsites.net/api/<functionName>/<value>?code=<secret>`, ardından `functionName` Azure işlevi etkinliğinde kullanmaktır `<functionName>/<value>`. İstenen sağlamak için bu işlevi parametreleştirebilirsiniz `functionName` zamanında.

Azure işlev etkinliği de destekler **sorguları**. Bir sorgunun parçası olarak dahil olmak zorundadır `functionName`. Örneğin, işlev adını olduğunda `HttpTriggerCSharp` ve dahil etmek istediğiniz sorgu `name=hello`, sonra da oluşturulabilir `functionName` Azure işlevi etkinlik `HttpTriggerCSharp?name=hello`. Bu işlev için değerin çalışma zamanında belirlenebilir parametreli olabilir.

## <a name="timeout-and-long-running-functions"></a>Zaman aşımı ve uzun süre çalışan işlevleri

Azure işlevleri zaman aşımına açmamasından 230 saniye sonra `functionTimeout` ayarlarında yapılandırdığınız ayar. Daha fazla bilgi için [bu makaleye](../azure-functions/functions-versions.md#timeout) bakın. Bu davranışa geçici bir çözüm için bir zaman uyumsuz desen izleyin veya dayanıklı işlevler kullanın. Dayanıklı İşlevler, kendi uygulamak zorunda kalmamanız için bunlar kendi durumu izleme mekanizması sağlar avantajdır.

Dayanıklı işlevler hakkında daha fazla bilgi [bu makalede](../azure-functions/durable/durable-functions-overview.md). Dayanıklı gibi farklı bir URI ile bir yanıt döndürür işlevi çağırmak için bir Azure işlevi faaliyet ayarlayabilirsiniz [Bu örnek](../azure-functions/durable/durable-functions-http-api.md#http-api-url-discovery). Çünkü `statusQueryGetUri` işlevi sırasında HTTP durum 202 çalıştığından, bir Web etkinliği kullanarak işlev durumunu yoklamak döndürür. Bir Web etkinliği ile yalnızca ayarlama `url` alan kümesine `@activity('<AzureFunctionActivityName>').output.statusQueryGetUri`. Dayanıklı işlevi tamamlandığında, bu işlevin çıktısı Web etkinliğinin çıkış olacaktır.


## <a name="next-steps"></a>Sonraki adımlar

Data Factory'de etkinlikleri hakkında daha fazla bilgi [işlem hatları ve etkinlikler Azure Data factory'de](concepts-pipelines-activities.md).
