---
title: B2B için şemalar izleme X12 izleme - Azure Logic Apps | Microsoft Docs
description: Şemalar izleme X12 B2B iletilerden Azure tümleştirme hesabınızda işlemleri izlemek için kullanın.
author: padmavc
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e5a43b9bdf522b6b26f27c082f5cb623f7a76a8b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="start-or-enable-tracking-of-x12-messages-to-monitor-success-errors-and-message-properties"></a>İzleyici başarı, hata ve ileti özellikleri iletileri başlangıç veya X12 izlemeyi etkinleştir
İşletmeden işletmeye (B2B) işlemleri izlemenize yardımcı olması için Azure tümleştirme hesabınızda şemaları izleme bu X12 kullanabilirsiniz:

* X12 işlem izleme şema ayarlayın
* X12 işlem şema izleme onayı ayarlama
* X12 izleme şeması değişimi
* Şema izleme bildirim X12 değişimi
* İzleme şema X12 işlev grubu
* Şema izleme bildirim X12 işlev grubu

## <a name="x12-transaction-set-tracking-schema"></a>X12 işlem izleme şema ayarlayın
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "transactionSetControlNumber": "",
                "CorrelationMessageId": "",
                "messageType": "",
                "isMessageFailed": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isFunctionalAcknowledgmentExpected": "",
                "needAk2LoopForValidMessages":  "",
                "segmentsCount": ""
            }
    }
````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 gönderenin ortak adı iletisi. (İsteğe bağlı) |
| receiverPartnerName | Dize | X12 alıcının ortak adı iletisi. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| senderIdentifier | Dize | İş ortağı tanımlayıcı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| receiverIdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adı olduğu iletileri çözümlenmiş anlaşma. (İsteğe bağlı) |
| yön | Enum | İleti akış yönünü alma veya gönderme. (Zorunlu) |
| interchangeControlNumber | Dize | Değiş tokuş denetim numarası. (İsteğe bağlı) |
| functionalGroupControlNumber | Dize | İşlev denetim sayısı. (İsteğe bağlı) |
| transactionSetControlNumber | Dize | İşlem Denetim numarası ayarlayın. (İsteğe bağlı) |
| correlationMessageId | Dize | Bağıntı ileti kimliği. {AgreementName} bileşimini {*GroupControlNumber*} {TransactionSetControlNumber}. (İsteğe bağlı) |
| messageType | Dize | İşlem ayarlayın veya belge türü. (İsteğe bağlı) |
| isMessageFailed | Boole | Olup olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| isTechnicalAcknowledgmentExpected | Boole | Teknik Bildirim X12 yapılandırılmış olup olmadığı anlaşma. (Zorunlu) |
| isFunctionalAcknowledgmentExpected | Boole | İşlev bildirim X12 yapılandırılmış olup olmadığı anlaşma. (Zorunlu) |
| needAk2LoopForValidMessages | Boole | AK2 döngü için geçerli bir ileti gerekli olup olmadığı. (Zorunlu) |
| segmentsCount | Tamsayı | X12 segmentlerinde sayısı işlem kümesi. (İsteğe bağlı) |

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>X12 işlem şema izleme onayı ayarlama
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "isaSegment": "",
                "gsSegment": "",
                "respondingfunctionalGroupControlNumber": "",
                "respondingFunctionalGroupId": "",
                "respondingtransactionSetControlNumber": "",
                "respondingTransactionSetId": "",
                "statusCode": "",
                "processingStatus": "",
                "CorrelationMessageId": ""
                "isMessageFailed": "",
                "ak2Segment": "",
                "ak3Segment": "",
                "ak5Segment": ""
            }
    }
````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 gönderenin ortak adı iletisi. (İsteğe bağlı) |
| receiverPartnerName | Dize | X12 alıcının ortak adı iletisi. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| senderIdentifier | Dize | İş ortağı tanımlayıcı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| receiverIdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adı olduğu iletileri çözümlenmiş anlaşma. (İsteğe bağlı) |
| yön | Enum | İleti akış yönünü alma veya gönderme. (Zorunlu) |
| interchangeControlNumber | Dize | İşlev bildirim denetimi sayısı değişim. Değer doldurur burada işlevsel bildirim alındığında gönderilen iletiler için gönderme tarafı için iş ortağı için. (İsteğe bağlı) |
| functionalGroupControlNumber | Dize | İşlev bildirim işlevsel Grup denetimi sayısı. Değer doldurur burada işlevsel bildirim alındığında gönderilen iletiler için gönderme tarafı için iş ortağı için. (İsteğe bağlı) |
| isaSegment | Dize | İleti ISA kesimi. Değer doldurur burada işlevsel bildirim alındığında gönderilen iletiler için gönderme tarafı için iş ortağı için. (İsteğe bağlı) |
| gsSegment | Dize | İleti GS kesimi. Değer doldurur burada işlevsel bildirim alındığında gönderilen iletiler için gönderme tarafı için iş ortağı için. (İsteğe bağlı) |
| respondingfunctionalGroupControlNumber | Dize | Değişim kontrol numarası yanıt. (İsteğe bağlı) |
| respondingFunctionalGroupId | Dize | Bildirim için AK101 eşlemeleri işlevsel Grup Kimliği yanıt. (İsteğe bağlı) |
| respondingtransactionSetControlNumber | Dize | Yanıt veren işlem denetim numarası ayarlayın. (İsteğe bağlı) |
| respondingTransactionSetId | Dize | Yanıt verme işlemi için AK201 bildirim eşlemeleri kimliği ayarlayın. (İsteğe bağlı) |
| statusCode | Boole | İşlem bildirim durum kodu ayarlayın. (Zorunlu) |
| segmentsCount | Enum | Bildirim durum kodu. İzin verilen değerler **kabul edilen**, **reddedildi**, ve **AcceptedWithErrors**. (Zorunlu) |
| processingStatus | Enum | Bildirim işlem durumu. İzin verilen değerler **alınan**, **oluşturulan**, ve **gönderilen**. (Zorunlu) |
| correlationMessageId | Dize | Bağıntı ileti kimliği. {AgreementName} bileşimini {*GroupControlNumber*} {TransactionSetControlNumber}. (İsteğe bağlı) |
| isMessageFailed | Boole | Olup olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| ak2Segment | Dize | Alınan işlev grubu içinde bir işlem için onay. (İsteğe bağlı) |
| ak3Segment | Dize | Veri segmenti hatalarını bildirir. (İsteğe bağlı) |
| ak5Segment | Dize | AK2 kesimdeki tanımlanan ayarlama işlemi kabul edilen veya reddedilen olup olmadığını ve bu nedenle raporlar. (İsteğe bağlı) |

## <a name="x12-interchange-tracking-schema"></a>X12 izleme şeması değişimi
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "isaSegment": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isMessageFailed": "",
                "isa09": "",
                "isa10": "",
                "isa11": "",
                "isa12": "",
                "isa14": "",
                "isa15": "",
                "isa16": ""
            }
    }
````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 gönderenin ortak adı iletisi. (İsteğe bağlı) |
| receiverPartnerName | Dize | X12 alıcının ortak adı iletisi. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| senderIdentifier | Dize | İş ortağı tanımlayıcı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| receiverIdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adı olduğu iletileri çözümlenmiş anlaşma. (İsteğe bağlı) |
| yön | Enum | İleti akış yönünü alma veya gönderme. (Zorunlu) |
| interchangeControlNumber | Dize | Değiş tokuş denetim numarası. (İsteğe bağlı) |
| isaSegment | Dize | İleti ISA kesimi. (İsteğe bağlı) |
| isTechnicalAcknowledgmentExpected | Boole | Teknik Bildirim X12 yapılandırılmış olup olmadığı anlaşma. (Zorunlu) |
| isMessageFailed | Boole | Olup olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| isa09 | Dize | X12 belge değişim tarih. (İsteğe bağlı) |
| isa10 | Dize | X12 değişim zaman belge. (İsteğe bağlı) |
| isa11 | Dize | X12 Değişim Denetimi standartları tanımlayıcısı. (İsteğe bağlı) |
| isa12 | Dize | X12 Değişim Denetimi sürüm numarası. (İsteğe bağlı) |
| isa14 | Dize | X12 bildirim istendi. (İsteğe bağlı) |
| isa15 | Dize | Test veya üretim için göstergesi. (İsteğe bağlı) |
| isa16 | Dize | Öğe ayırıcı. (İsteğe bağlı) |

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>Şema izleme bildirim X12 değişimi
````java
    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "isaSegment": "",
                "respondingInterchangeControlNumber": "",
                "isMessageFailed": "",
                "statusCode": "",
                "processingStatus": "",
                "ta102": "",
                "ta103": "",
                "ta105": ""
            }
    }
````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 gönderenin ortak adı iletisi. (İsteğe bağlı) |
| receiverPartnerName | Dize | X12 alıcının ortak adı iletisi. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| senderIdentifier | Dize | İş ortağı tanımlayıcı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| receiverIdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adı olduğu iletileri çözümlenmiş anlaşma. (İsteğe bağlı) |
| yön | Enum | İleti akış yönünü alma veya gönderme. (Zorunlu) |
| interchangeControlNumber | Dize | Değişim Denetimi ortaklarından alınan teknik bildirim sayısı. (İsteğe bağlı) |
| isaSegment | Dize | Ortaklarından alınan teknik onayı için ISA kesimi. (İsteğe bağlı) |
| respondingInterchangeControlNumber |Dize | Değiş tokuş ortaklarından alınan teknik onayı için Denetim numarası. (İsteğe bağlı) |
| isMessageFailed | Boole | Olup olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| statusCode | Enum | Bildirim durum kodu değişim. İzin verilen değerler **kabul edilen**, **reddedildi**, ve **AcceptedWithErrors**. (Zorunlu) |
| processingStatus | Enum | Onay durumu. İzin verilen değerler **alınan**, **oluşturulan**, ve **gönderilen**. (Zorunlu) |
| ta102 | Dize | Tarih değişim. (İsteğe bağlı) |
| ta103 | Dize | Zaman değişim. (İsteğe bağlı) |
| ta105 | Dize | Not kodu değişim. (İsteğe bağlı) |

## <a name="x12-functional-group-tracking-schema"></a>İzleme şema X12 işlev grubu
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "gsSegment": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isFunctionalAcknowledgmentExpected": "",
                "isMessageFailed": "",
                "gs01": "",
                "gs02": "",
                "gs03": "",
                "gs04": "",
                "gs05": "",
                "gs07": "",
                "gs08": ""
            }
    }
````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 gönderenin ortak adı iletisi. (İsteğe bağlı) |
| receiverPartnerName | Dize | X12 alıcının ortak adı iletisi. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| senderIdentifier | Dize | İş ortağı tanımlayıcı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| receiverIdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adı olduğu iletileri çözümlenmiş anlaşma. (İsteğe bağlı) |
| yön | Enum | İleti akış yönünü alma veya gönderme. (Zorunlu) |
| interchangeControlNumber | Dize | Değiş tokuş denetim numarası. (İsteğe bağlı) |
| functionalGroupControlNumber | Dize | İşlev denetim sayısı. (İsteğe bağlı) |
| gsSegment | Dize | İleti GS kesimi. (İsteğe bağlı) |
| isTechnicalAcknowledgmentExpected | Boole | Teknik Bildirim X12 yapılandırılmış olup olmadığı anlaşma. (Zorunlu) |
| isFunctionalAcknowledgmentExpected | Boole | İşlev bildirim X12 yapılandırılmış olup olmadığı anlaşma. (Zorunlu) |
| isMessageFailed | Boole | Olup olmadığını X12 ileti başarısız oldu. (Zorunlu)|
| gs01 | Dize | İşlev tanımlayıcı kod. (İsteğe bağlı) |
| gs02 | Dize | Uygulama gönderenin kodu. (İsteğe bağlı) |
| gs03 | Dize | Uygulama alıcının kodu. (İsteğe bağlı) |
| gs04 | Dize | İşlev grubunu tarih. (İsteğe bağlı) |
| gs05 | Dize | İşlev grubunu süre. (İsteğe bağlı) |
| gs07 | Dize | Sorumlu Teşkilatı kodu. (İsteğe bağlı) |
| gs08 | Dize | Yayın/sürüm/endüstri tanımlayıcı kod. (İsteğe bağlı) |

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>Şema izleme bildirim X12 işlev grubu
````java
    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "isaSegment": "",
                "gsSegment": "",
                "respondingfunctionalGroupControlNumber": "",
                "respondingFunctionalGroupId": "",
                "isMessageFailed": "",
                "statusCode": "",
                "processingStatus": "",
                "ak903": "",
                "ak904": "",
                "ak9Segment": ""
            }
    }
````

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 gönderenin ortak adı iletisi. (İsteğe bağlı) |
| receiverPartnerName | Dize | X12 alıcının ortak adı iletisi. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| senderIdentifier | Dize | İş ortağı tanımlayıcı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| receiverIdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adı olduğu iletileri çözümlenmiş anlaşma. (İsteğe bağlı) |
| yön | Enum | İleti akış yönünü alma veya gönderme. (Zorunlu) |
| interchangeControlNumber | Dize | İş ortaklarının sunduğu Teknik Bildirim alındığında gönderme tarafı için doldurur değişim denetimi bir sayıdır. (İsteğe bağlı) |
| functionalGroupControlNumber | Dize | İş ortaklarının sunduğu Teknik Bildirim alındığında gönderme tarafı için doldurur Teknik Bildirim işlevsel Grup denetimi sayısı. (İsteğe bağlı) |
| isaSegment | Dize | Değişim aynı numarası, ancak belirli durumlarda yalnızca doldurulan kontrol eder. (İsteğe bağlı) |
| gsSegment | Dize | İşlevsel grubuyla aynı numarası, ancak belirli durumlarda yalnızca doldurulan kontrol eder. (İsteğe bağlı) |
| respondingfunctionalGroupControlNumber | Dize | Özgün işlevsel grup sayısını denetler. (İsteğe bağlı) |
| respondingFunctionalGroupId | Dize | AK101 eşlenir bildirim işlevsel grubundaki kimliği (İsteğe bağlı) |
| isMessageFailed | Boole | Olup olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| statusCode | Enum | Bildirim durum kodu. İzin verilen değerler **kabul edilen**, **reddedildi**, ve **AcceptedWithErrors**. (Zorunlu) |
| processingStatus | Enum | Bildirim işlem durumu. İzin verilen değerler **alınan**, **oluşturulan**, ve **gönderilen**. (Zorunlu) |
| ak903 | Dize | İşlem kümesi alınan sayısı. (İsteğe bağlı) |
| ak904 | Dize | İşlem kümesi sayısı tanımlanan işlevsel grubunda kabul edildi. (İsteğe bağlı) |
| ak9Segment | Dize | İşlevsel AK1 kesimdeki belirlenen grubu kabul edilen veya reddedilen olup olmadığını ve neden. (İsteğe bağlı) |

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [B2B iletileri izleme](logic-apps-monitor-b2b-message.md).
* Daha fazla bilgi edinmek [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md).
* Daha fazla bilgi edinmek [B2B şemaları izleme özel](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md).
* Hakkında bilgi edinin [günlük analizi B2B iletilerinde izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* Daha fazla bilgi edinmek [Kurumsal tümleştirme paketi](../logic-apps/logic-apps-enterprise-integration-overview.md).  
