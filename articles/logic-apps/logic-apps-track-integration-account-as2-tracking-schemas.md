---
title: "AS2 izleme şemaları B2B izleme - Azure Logic Apps | Microsoft Docs"
description: "AS2 izleme şemaları B2B iletilerden Azure tümleştirme hesabınızda işlemleri izlemek için kullanın."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 31bd296dc5ed5ac6998a6c05ee80fd38b12d662c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-to-monitor-success-errors-and-message-properties"></a>Başlat veya İzleyici başarı, hata ve ileti özellikleri AS2 iletileri ve MDNs izlemeyi etkinleştirme
İşletmeden işletmeye (B2B) işlemleri izlemenize yardımcı olması için Azure tümleştirme hesabınızı bu AS2 izleme şemaları kullanabilirsiniz:

* AS2 ileti izleme şeması
* AS2 MDN izleme şeması

## <a name="as2-message-tracking-schema"></a>AS2 ileti izleme şeması
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | AS2 iletiyi gönderenin ortak adı. (İsteğe bağlı) |
| receiverPartnerName | Dize | AS2 ileti alıcının ortak adı. (İsteğe bağlı) |
| as2To | Dize | AS2 iletisinin başlıklarından AS2 ileti alıcının adı. (Zorunlu) |
| as2From | Dize | AS2 iletisinin başlıklarından AS2 iletiyi gönderenin adı. (Zorunlu) |
| agreementName | Dize | İletileri çözümlendiği AS2 sözleşmesi adı. (İsteğe bağlı) |
| Yönü | Dize | İleti akış yönünü alma veya gönderme. (Zorunlu) |
| MessageID | Dize | (İsteğe bağlı) AS2 iletisinin başlıklarından AS2 ileti kimliği |
| dispositionType |Dize | İleti değerlendirme bildirim (MDN) değerlendirme türü değeri. (İsteğe bağlı) |
| fileName | Dize | AS2 iletisinin üstbilgisinden dosya adı. (İsteğe bağlı) |
| isMessageFailed |Boole değeri | Olup AS2 iletisi başarısız oldu. (Zorunlu) |
| isMessageSigned | Boole değeri | AS2 iletisi olup olmadığını imzalandı. (Zorunlu) |
| isMessageEncrypted | Boole değeri | AS2 iletisi olup olmadığını şifrelenmiş. (Zorunlu) |
| isMessageCompressed |Boole değeri | AS2 iletisi olup olmadığını sıkıştırılmış. (Zorunlu) |
| correlationMessageId | Dize | İletileri MDNs ile ilişkilendirmek için AS2 ileti kimliği. (İsteğe bağlı) |
| incomingHeaders |JToken sözlüğü | Gelen AS2 ileti üstbilgisi ayrıntıları. (İsteğe bağlı) |
| outgoingHeaders |JToken sözlüğü | Giden AS2 ileti üstbilgisi ayrıntıları. (İsteğe bağlı) |
| isNrrEnabled | Boole değeri | Değer bilinmiyor, varsayılan değeri kullanın. (Zorunlu) |
| isMdnExpected | Boole değeri | Değer bilinmiyor, varsayılan değeri kullanın. (Zorunlu) |
| mdnType | Enum | İzin verilen değerler **NotConfigured**, **eşitleme**, ve **zaman uyumsuz**. (Zorunlu) |

## <a name="as2-mdn-tracking-schema"></a>AS2 MDN izleme şeması
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | AS2 iletiyi gönderenin ortak adı. (İsteğe bağlı) |
| receiverPartnerName | Dize | AS2 ileti alıcının ortak adı. (İsteğe bağlı) |
| as2To | Dize | AS2 ileti alır ortak adı. (Zorunlu) |
| as2From | Dize | AS2 ileti gönderen ortak adı. (Zorunlu) |
| agreementName | Dize | İletileri çözümlendiği AS2 sözleşmesi adı. (İsteğe bağlı) |
| Yönü |Dize | İleti akış yönünü alma veya gönderme. (Zorunlu) |
| MessageID | Dize | AS2 ileti kimliği. (İsteğe bağlı) |
| OriginalMessageId |Dize | AS2 özgün ileti kimliği. (İsteğe bağlı) |
| dispositionType | Dize | MDN değerlendirme türü değeri. (İsteğe bağlı) |
| isMessageFailed |Boole değeri | Olup AS2 iletisi başarısız oldu. (Zorunlu) |
| isMessageSigned |Boole değeri | AS2 iletisi olup olmadığını imzalandı. (Zorunlu) |
| isNrrEnabled | Boole değeri | Değer bilinmiyor, varsayılan değeri kullanın. (Zorunlu) |
| statusCode | Enum | İzin verilen değerler **kabul edilen**, **reddedildi**, ve **AcceptedWithErrors**. (Zorunlu) |
| micVerificationStatus | Enum | İzin verilen değerler **Notapplıcable**, **başarılı**, ve **başarısız**. (Zorunlu) |
| correlationMessageId | Dize | Bağıntı Kimliği Özgün kimliği messaged (MDN yapılandırılır iletinin ileti kimliği). (İsteğe bağlı) |
| incomingHeaders | JToken sözlüğü | Gelen ileti üstbilgisi ayrıntılarını gösterir. (İsteğe bağlı) |
| outgoingHeaders |JToken sözlüğü | Giden ileti üstbilgisi ayrıntılarını gösterir. (İsteğe bağlı) |

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [Kurumsal tümleştirme paketi](../logic-apps/logic-apps-enterprise-integration-overview.md).    
* Daha fazla bilgi edinmek [B2B iletileri izleme](logic-apps-monitor-b2b-message.md).   
* Daha fazla bilgi edinmek [B2B şemaları izleme özel](logic-apps-track-integration-account-custom-tracking-schema.md).   
* Daha fazla bilgi edinmek [şemaları izleme X12](logic-apps-track-integration-account-x12-tracking-schema.md).   
* Hakkında bilgi edinin [Operations Management Suite portalına B2B iletilerinde izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
