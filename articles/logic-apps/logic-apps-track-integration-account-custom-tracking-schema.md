---
title: Özel İzleme şemaları için B2B iletilerini - Azure Logic Apps | Microsoft Docs
description: Oluşturmak için Azure Logic Apps Enterprise Integration Pack ile tümleştirme hesaplarındaki B2B iletilerini izleme özel izleme şemaları
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.date: 01/27/2017
ms.openlocfilehash: 68c5d6e68562d4027c102e1bde42c775648e58c4
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124852"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-apps"></a>Azure Logic Apps iş akışlarında uçtan uca izleme özel izleme şemaları oluşturma

AS2 izleme veya X12 gibi işletmeler arası akışınızın farklı bölümleri için iletileri etkinleştirebilirsiniz, izleme yerleşik yoktur. Olayları ve iş akışınızı sonuna kadar baştan günlükleri özel izleme etkinleştirebilirsiniz sonra iş akışları oluşturduğunuzda bir mantıksal uygulama, BizTalk Server, SQL Server veya diğer herhangi bir katman içerir. 

Bu makalede, mantıksal uygulamanızı dışında Katmanlar kullanabileceğiniz özel kod sağlanır. 

## <a name="custom-tracking-schema"></a>Özel izleme şeması

```json
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
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| Kaynak türü |   | Çalıştırma kaynağı türü. İzin verilen değerler **Microsoft.Logic/workflows** ve **özel**. (Zorunlu) |
| Kaynak |   | Kaynak türü ise **Microsoft.Logic/workflows**, kaynak bilgileri bu şemayı izlemesi gerekir. Kaynak türü ise **özel**, bir JToken şemadır. (Zorunlu) |
| Systemıd | Dize | Mantıksal uygulama sistem kimliği (Zorunlu) |
| Runıd | Dize | Mantıksal uygulama çalıştırması kimliği. (Zorunlu) |
| operationName | Dize | (Örneğin, bir eylem veya tetikleyici) işlemin adı. (Zorunlu) |
| repeatItemScopeName | Dize | Eylem içinde ise öğe adı yineleyin bir `foreach` / `until` döngü. (Zorunlu) |
| repeatItemIndex | Tamsayı | Eylem içinde olup olmadığını bir `foreach` / `until` döngü. Yinelenen öğe dizini belirtir. (Zorunlu) |
| trackingId | Dize | İletileri ilişkilendirmek için izleme kimliği. (İsteğe bağlı) |
| correlationId | Dize | İletileri ilişkilendirmek için bağıntı kimliği. (İsteğe bağlı) |
| Clientrequestıd'ye | Dize | İstemci iletileri ilişkilendirmek için doldurabilirsiniz. (İsteğe bağlı) |
| eventLevel |   | Olay düzeyi. (Zorunlu) |
| eventTime |   | Etkinliğin UTC biçiminde YYYY-AA-DDTHH:MM:SS.00000Z saati. (Zorunlu) |
| RecordType |   | İzleme kayıt türü. Değer izin verilen **özel**. (Zorunlu) |
| kayıt |   | Özel bir kayıt türü. İzin verilen biçim JToken ' dir. (Zorunlu) |
||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B Protokolü izleme şemaları

B2B protokol şemaları izleme hakkında daha fazla bilgi için bkz:

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12 izleme şemaları](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [B2B iletilerini izleme](logic-apps-monitor-b2b-message.md)
* Hakkında bilgi edinin [Log analytics'te B2B iletilerini izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)