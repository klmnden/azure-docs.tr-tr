---
title: B2B iletilerini - Azure Logic Apps için şemalar izleme X12 | Microsoft Docs
description: İzleme için Azure Logic Apps Enterprise Integration Pack ile tümleştirme hesaplarındaki B2B iletilerini izleme şemaları X12 oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.date: 01/27/2017
ms.openlocfilehash: cfd195f2f812c8b2e09058d325d0dbb6f7a60d59
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125610"
---
# <a name="create-schemas-for-tracking-x12-messages-in-integration-accounts-for-azure-logic-apps"></a>Oluşturma X12 izleme şemaları tümleştirme hesapları Azure Logic Apps için iletileri

Yardımcı olması için İzleyici başarı, hataları ve işletmeler arası (B2B) işlemleri, ileti özelliklerini şemaları tümleştirme hesabı izleme bu X12 kullanabilirsiniz:

* X12 işlem kümesi izleme şeması
* X12 işlem kümesi bildirim izleme şeması
* X12 değişim izleme şeması
* X12 değişim bildirim izleme şeması
* X12 işlevsel Grup izleme şeması
* İzleme şeması bildirim X12 işlevsel Grup

## <a name="x12-transaction-set-tracking-schema"></a>X12 işlem kümesi izleme şeması

```json
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
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | Gönderen iş ortağı adı X12 ileti. (İsteğe bağlı) |
| receiverPartnerName | Dize | Alıcı iş ortağı adı X12 ileti. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adını, iletileri çözümlenen sözleşme. (İsteğe bağlı) |
| yön | Sabit listesi | İleti akışı yönünü almak veya göndermek. (Zorunlu) |
| interchangeControlNumber | Dize | Değişim denetim numarası. (İsteğe bağlı) |
| functionalGroupControlNumber | Dize | İşlevsel denetim numarası. (İsteğe bağlı) |
| transactionSetControlNumber | Dize | İşlem kümesi denetim numarası. (İsteğe bağlı) |
| CorrelationMessageId | Dize | Bağıntı ileti kimliği. {AgreementName} birleşimi {*GroupControlNumber*} {TransactionSetControlNumber}. (İsteğe bağlı) |
| MessageType | Dize | İşlem kümesi veya belge türü. (İsteğe bağlı) |
| isMessageFailed | Boole | Olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| isTechnicalAcknowledgmentExpected | Boole | Teknik Bildirim X12 içinde yapılandırılmış olup olmadığı sözleşme. (Zorunlu) |
| isFunctionalAcknowledgmentExpected | Boole | İşlev bildirim X12 içinde yapılandırılmış olup olmadığı sözleşme. (Zorunlu) |
| needAk2LoopForValidMessages | Boole | AK2 döngü için geçerli bir ileti gerekli olup olmadığı. (Zorunlu) |
| segmentsCount | Tamsayı | Kesim X12 içinde işlem kümesi. (İsteğe bağlı) |
||||

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>X12 işlem kümesi bildirim izleme şeması

```json
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
      "CorrelationMessageId": "",
      "isMessageFailed": "",
      "ak2Segment": "",
      "ak3Segment": "",
      "ak5Segment": ""
   }
}
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | Gönderen iş ortağı adı X12 ileti. (İsteğe bağlı) |
| receiverPartnerName | Dize | Alıcı iş ortağı adı X12 ileti. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adını, iletileri çözümlenen sözleşme. (İsteğe bağlı) |
| yön | Sabit listesi | İleti akışı yönünü almak veya göndermek. (Zorunlu) |
| interchangeControlNumber | Dize | Değişim denetim numarası, işlev bildirim. Değer doldurur burada işlevsel bildirim alındığında gönderilen iletileri gönderme tarafı için iş ortağı için. (İsteğe bağlı) |
| functionalGroupControlNumber | Dize | İşlev bildirim işlevsel Grup denetim numarası. Değer doldurur burada işlevsel bildirim alındığında gönderilen iletileri gönderme tarafı için iş ortağı için. (İsteğe bağlı) |
| isaSegment | Dize | ISA segmenti iletinin. Değer doldurur burada işlevsel bildirim alındığında gönderilen iletileri gönderme tarafı için iş ortağı için. (İsteğe bağlı) |
| gsSegment | Dize | İletinin GS kesimi. Değer doldurur burada işlevsel bildirim alındığında gönderilen iletileri gönderme tarafı için iş ortağı için. (İsteğe bağlı) |
| respondingfunctionalGroupControlNumber | Dize | Değişim denetim numarası yanıt. (İsteğe bağlı) |
| respondingFunctionalGroupId | Dize | İçinde bildirim için AK101 eşleştiren işlevsel Grup Kimliği yanıt. (İsteğe bağlı) |
| respondingtransactionSetControlNumber | Dize | Yanıt veren işlem kümesi denetim numarası. (İsteğe bağlı) |
| respondingTransactionSetId | Dize | Yanıt veren işlem AK201 için bildirim içinde eşleştiren kimliği ayarlayın. (İsteğe bağlı) |
| statusCode | Boole | Onay durum kodunu işlem kümesi. (Zorunlu) |
| segmentsCount | Sabit listesi | Onay durum kodunu. İzin verilen değerler **kabul edilen**, **reddedildi**, ve **AcceptedWithErrors**. (Zorunlu) |
| processingStatus | Sabit listesi | İşlem durumu alındı. İzin verilen değerler **alınan**, **oluşturulan**, ve **gönderilen**. (Zorunlu) |
| CorrelationMessageId | Dize | Bağıntı ileti kimliği. {AgreementName} birleşimi {*GroupControlNumber*} {TransactionSetControlNumber}. (İsteğe bağlı) |
| isMessageFailed | Boole | Olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| ak2Segment | Dize | Bildirim için bir işlem içinde alınan işlevsel grup kümesi. (İsteğe bağlı) |
| ak3Segment | Dize | Veri segmenti hataları bildirir. (İsteğe bağlı) |
| ak5Segment | Dize | İşlem AK2 segment tanımlanan kümesi kabul ya da reddedilen ve neden bildirir. (İsteğe bağlı) |
||||

## <a name="x12-interchange-tracking-schema"></a>X12 değişim izleme şeması

```json
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
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | Gönderen iş ortağı adı X12 ileti. (İsteğe bağlı) |
| receiverPartnerName | Dize | Alıcı iş ortağı adı X12 ileti. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adını, iletileri çözümlenen sözleşme. (İsteğe bağlı) |
| yön | Sabit listesi | İleti akışı yönünü almak veya göndermek. (Zorunlu) |
| interchangeControlNumber | Dize | Değişim denetim numarası. (İsteğe bağlı) |
| isaSegment | Dize | İleti ISA segmenti. (İsteğe bağlı) |
| isTechnicalAcknowledgmentExpected | Boole | Teknik Bildirim X12 içinde yapılandırılmış olup olmadığı sözleşme. (Zorunlu) |
| isMessageFailed | Boole | Olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| isa09 | Dize | X12 belge değişim tarih. (İsteğe bağlı) |
| isa10 | Dize | Değişim zaman X12 belgeleyin. (İsteğe bağlı) |
| ısa11 | Dize | X12 Değişim Denetimi standartları tanımlayıcısı. (İsteğe bağlı) |
| ısa12 | Dize | X12 değişim denetim sürüm numarası. (İsteğe bağlı) |
| isa14 | Dize | X12 bildirim istendi. (İsteğe bağlı) |
| ısa15 | Dize | Test veya üretim için göstergesi. (İsteğe bağlı) |
| isa16 | Dize | Öğe ayırıcı. (İsteğe bağlı) |
||||

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>X12 değişim bildirim izleme şeması

```json
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
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | Gönderen iş ortağı adı X12 ileti. (İsteğe bağlı) |
| receiverPartnerName | Dize | Alıcı iş ortağı adı X12 ileti. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adını, iletileri çözümlenen sözleşme. (İsteğe bağlı) |
| yön | Sabit listesi | İleti akışı yönünü almak veya göndermek. (Zorunlu) |
| interchangeControlNumber | Dize | Değişim denetim numarası, iş ortaklarından alınan Teknik Bildirim. (İsteğe bağlı) |
| isaSegment | Dize | İş ortaklarından alınan Teknik Bildirim için ISA segmenti. (İsteğe bağlı) |
| respondingInterchangeControlNumber |Dize | Değişim denetim numarası için iş ortaklarından alınan Teknik Bildirim. (İsteğe bağlı) |
| isMessageFailed | Boole | Olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| statusCode | Sabit listesi | Onay durum kodunu değişimi. İzin verilen değerler **kabul edilen**, **reddedildi**, ve **AcceptedWithErrors**. (Zorunlu) |
| processingStatus | Sabit listesi | Bildirim durumu. İzin verilen değerler **alınan**, **oluşturulan**, ve **gönderilen**. (Zorunlu) |
| ta102 | Dize | Tarih değişimi. (İsteğe bağlı) |
| ta103 | Dize | Zaman değişimi. (İsteğe bağlı) |
| ta105 | Dize | Not kod değişim. (İsteğe bağlı) |
||||

## <a name="x12-functional-group-tracking-schema"></a>X12 işlevsel Grup izleme şeması

```json
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
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | Gönderen iş ortağı adı X12 ileti. (İsteğe bağlı) |
| receiverPartnerName | Dize | Alıcı iş ortağı adı X12 ileti. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adını, iletileri çözümlenen sözleşme. (İsteğe bağlı) |
| yön | Sabit listesi | İleti akışı yönünü almak veya göndermek. (Zorunlu) |
| interchangeControlNumber | Dize | Değişim denetim numarası. (İsteğe bağlı) |
| functionalGroupControlNumber | Dize | İşlevsel denetim numarası. (İsteğe bağlı) |
| gsSegment | Dize | İleti GS kesimi. (İsteğe bağlı) |
| isTechnicalAcknowledgmentExpected | Boole | Teknik Bildirim X12 içinde yapılandırılmış olup olmadığı sözleşme. (Zorunlu) |
| isFunctionalAcknowledgmentExpected | Boole | İşlev bildirim X12 içinde yapılandırılmış olup olmadığı sözleşme. (Zorunlu) |
| isMessageFailed | Boole | Olmadığını X12 ileti başarısız oldu. (Zorunlu)|
| gs01 | Dize | İşlev tanımlayıcı kod. (İsteğe bağlı) |
| gs02 | Dize | Uygulama gönderen kodu. (İsteğe bağlı) |
| gs03 | Dize | Uygulama alıcının kodu. (İsteğe bağlı) |
| gs04 | Dize | İşlevsel Grup tarih. (İsteğe bağlı) |
| gs05 | Dize | İşlevsel Grup süre. (İsteğe bağlı) |
| gs07 | Dize | Sorumlu Ajans kodu. (İsteğe bağlı) |
| gs08 | Dize | Sürüm/yayın/sektör tanımlayıcı kod. (İsteğe bağlı) |
||||

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>İzleme şeması bildirim X12 işlevsel Grup

```json
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
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | Gönderen iş ortağı adı X12 ileti. (İsteğe bağlı) |
| receiverPartnerName | Dize | Alıcı iş ortağı adı X12 ileti. (İsteğe bağlı) |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. (Zorunlu) |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. (Zorunlu) |
| receiverQualifier | Dize | İş ortağı niteleyicisi alırsınız. (Zorunlu) |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını alır. (Zorunlu) |
| agreementName | Dize | X12 adını, iletileri çözümlenen sözleşme. (İsteğe bağlı) |
| yön | Sabit listesi | İleti akışı yönünü almak veya göndermek. (Zorunlu) |
| interchangeControlNumber | Dize | İş ortaklarından Teknik Bildirim alındığında için gönderme tarafı doldurur değişim denetim numarası. (İsteğe bağlı) |
| functionalGroupControlNumber | Dize | İş ortaklarından Teknik Bildirim alındığında için gönderme tarafı doldurur Teknik Bildirim işlevsel Grup denetim numarası. (İsteğe bağlı) |
| isaSegment | Dize | Değişim aynı numarası, ancak yalnızca belirli durumlarda doldurulmuş denetler. (İsteğe bağlı) |
| gsSegment | Dize | İşlevsel Grup aynı numarası, ancak yalnızca belirli durumlarda doldurulmuş denetler. (İsteğe bağlı) |
| respondingfunctionalGroupControlNumber | Dize | Özgün işlevsel Grup denetim numarası. (İsteğe bağlı) |
| respondingFunctionalGroupId | Dize | AK101 eşlenir bildirim işlevsel grubun kimliği. (İsteğe bağlı) |
| isMessageFailed | Boole | Olmadığını X12 ileti başarısız oldu. (Zorunlu) |
| statusCode | Sabit listesi | Onay durum kodunu. İzin verilen değerler **kabul edilen**, **reddedildi**, ve **AcceptedWithErrors**. (Zorunlu) |
| processingStatus | Sabit listesi | İşlem durumu alındı. İzin verilen değerler **alınan**, **oluşturulan**, ve **gönderilen**. (Zorunlu) |
| ak903 | Dize | Alınan işlem kümesi sayısı. (İsteğe bağlı) |
| ak904 | Dize | İşlem kümesi sayısı içinde tanımlanan işlevsel Grup kabul edildi. (İsteğe bağlı) |
| ak9Segment | Dize | AK1 kesimdeki tanımlanan işlevsel Grup kabul ya da reddedilen ve neden. (İsteğe bağlı) |
|||| 

## <a name="b2b-protocol-tracking-schemas"></a>B2B Protokolü izleme şemaları

B2B protokol şemaları izleme hakkında daha fazla bilgi için bkz:

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [B2B özel izleme şemaları](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [B2B iletilerini izleme](logic-apps-monitor-b2b-message.md).
* Hakkında bilgi edinin [Log analytics'te B2B iletilerini izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
