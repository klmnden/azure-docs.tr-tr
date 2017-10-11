---
title: "Özel İzleme şemaları B2B izleme - Azure Logic Apps | Microsoft Docs"
description: "B2B iletilerden Azure tümleştirme hesabınızda işlemleri izlemek için özel izleme şemalarını oluşturun."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b71a4938dde2a71f1ce29403af7aa9101358d64c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="enable-tracking-to-monitor-your-complete-workflow-end-to-end"></a>İş akışını tam, uçtan uca izlemek izlemeyi etkinleştirme
İzleme AS2 veya X12 gibi iş iş akışınızı farklı kısımlarını iletileri için etkinleştirebileceğiniz olduğunu izleme yerleşik yoktur. Başlangıçtan itibaren akışınıza sonuna olayları kaydeder özel izleme etkinleştirebilirsiniz sonra iş akışları oluşturduğunuzda bir mantıksal uygulama, BizTalk Server, SQL Server veya diğer herhangi bir katman içerir. 

Bu konu, mantıksal uygulamanızı dışında Katmanlar kullanabilirsiniz özel kod sağlar. 

## <a name="custom-tracking-schema"></a>Özel İzleme şeması
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| Kaynak türü |   | Çalışma kaynağının türü. İzin verilen değerler **Microsoft.Logic/workflows** ve **özel**. (Zorunlu) |
| Kaynak |   | Kaynak türü ise **Microsoft.Logic/workflows**, kaynak bilgilerini bu şemayı izlemesi gerekir. Kaynak türü ise **özel**, şemanın bir JToken olduğu. (Zorunlu) |
| SistemKimliği | Dize | Mantıksal uygulama sistem kimliği (Zorunlu) |
| çalıştırma kodu | Dize | Mantıksal uygulama kimliği çalıştırın (Zorunlu) |
| operationName | Dize | (Örneğin, eylem veya tetikleyici) işlemin adı. (Zorunlu) |
| repeatItemScopeName | Dize | Eylem içinde olduğunda öğe adı yineleyin bir `foreach` / `until` döngü. (Zorunlu) |
| repeatItemIndex | Tamsayı | Eylem içinde olup olmadığını bir `foreach` / `until` döngü. Yinelenen öğe dizini belirtir. (Zorunlu) |
| İzleme kodu | Dize | İletileri ilişkilendirmek için izleme kimliği. (İsteğe bağlı) |
| correlationId | Dize | Bağıntı kimliği, iletileri ilişkilendirmek için. (İsteğe bağlı) |
| ClientRequestId | Dize | İstemci iletileri ilişkilendirmek için doldurabilirsiniz. (İsteğe bağlı) |
| eventLevel |   | Olay düzeyi. (Zorunlu) |
| EventTime |   | UTC biçiminde YYYY-AA-DDTHH:MM:SS.00000Z olay zamanı. (Zorunlu) |
| RecordType |   | İzleme kayıt türü. Değer izin verilen **özel**. (Zorunlu) |
| Kayıt |   | Özel bir kayıt türü. İzin verilen JToken biçimindedir. (Zorunlu) |

## <a name="b2b-protocol-tracking-schemas"></a>B2B Protokolü izleme şemaları
B2B Protokolü şemaları izleme hakkında daha fazla bilgi için bkz:
* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12 izleme şemaları](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [B2B iletileri izleme](logic-apps-monitor-b2b-message.md).   
* Hakkında bilgi edinin [Operations Management Suite portalına B2B iletilerinde izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* Daha fazla bilgi edinmek [Kurumsal tümleştirme paketi](../logic-apps/logic-apps-enterprise-integration-overview.md).
