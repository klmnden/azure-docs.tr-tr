---
title: AS2 ileti ayarları - Azure Logic Apps
description: AS2 için başvuru kılavuzu gönderin ve Azure Logic Apps ile Enterprise Integration Pack ayarlarını alır
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: ead92094b9af1dff56ff68e1ff58a3a4cdd9dca5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63769459"
---
# <a name="reference-for-as2-message-settings-in-azure-logic-apps-with-enterprise-integration-pack"></a>Azure Logic Apps Enterprise Integration Pack ile AS2 iletisi ayarları başvurusu

Bu başvuru, AS2 sözleşmesi gönderilen ve alınan ticaret iş ortakları arasında iletilerini nasıl işlediğini belirtmek için ayarlayabileceğiniz özellikleri tanımlar. İş ortağı ile olan sözleşmenizde göre bu özellikleri ayarlama, sizinle iletiler birbiriyle değiştirir.

<a name="AS2-incoming-messages"></a>

## <a name="as2-receive-settings"></a>AS2 alma ayarları

!["Alma ayarlarını" seçin](./media/logic-apps-enterprise-integration-as2-message-settings/receive-settings.png)

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **İleti özelliklerini geçersiz kıl** | Hayır | Gelen iletiler özellikleri özelliği ayarlarınızı geçersiz kılar. |
| **İletinin imzalanmış olması gerekir** | Hayır | Tüm gelen iletilerin dijital olarak imzalanmış olup olmaması gerektiğini belirtir. İmzalama, gerekiyorsa gelen **sertifika** listesinde, iletileri imzayı doğrulamak için mevcut bir konuk iş ortağı ortak sertifikayı seçin. Bir sertifika yoksa, daha fazla bilgi edinin [sertifikaları ekleyerek](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **İletinin şifrelenmiş olması gerekir** | Hayır | Tüm gelen iletilerin şifrelenmesinin gerekip gerekmediğini belirtir. Olmayan şifrelenmiş iletileri reddedilir. Öğesinden, şifreleme gerekiyorsa **sertifika** listesinde, gelen iletilerin şifresini çözmek için var olan konak iş ortağı özel bir sertifikayı seçin. Bir sertifika yoksa, daha fazla bilgi edinin [sertifikaları ekleyerek](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **İletinin sıkıştırılmış olması gerekir** | Hayır | Tüm gelen iletilerin sıkıştırılmış olup olmadığını belirtir. İletileri olmayan sıkıştırılmış reddedilir. |
| **İleti kimliği yinelenmesine izin verme** | Hayır | Yinelenen kimlikleri olan iletileri izin verilip verilmeyeceğini belirtir. Yinelenen kimlikleri devre dışı bırakırsanız, denetimleri arasındaki gün sayısını seçin. Ayrıca, kullanılıp çoğaltmaları askıya almak de seçebilirsiniz. |
| **MDN metni** | Hayır | İletiyi gönderenin için gönderilen istediğiniz varsayılan ileti değerlendirme bildirimi (MDN) belirtir. |
| **MDN Gönder** | Hayır | Alınan iletiler için zaman uyumlu çok Mdn'leri gönderilip gönderilmeyeceğini belirtir.  |
| **İmzalı MDN Gönder** | Hayır | Alınan iletilerin imzalı Mdn'leri gönderilip gönderilmeyeceğini belirtir. İmzalama, gerekiyorsa gelen **MIC algoritması** listesinde, iletileri imzalamak için kullanılacak algoritmayı seçin. |
| **Zaman uyumsuz MDN Gönder** | Hayır | Zaman uyumsuz Mdn'leri gönderilip gönderilmeyeceğini belirtir. İçinde zaman uyumsuz Mdn'leri seçerseniz **URL** kutusunda Mdn'leri gönderileceği URL belirtin. |
||||

<a name="AS2-outgoing-messages"></a>

## <a name="as2-send-settings"></a>AS2 gönderme ayarları

!["Ayarları Gönder" öğesini seçin](./media/logic-apps-enterprise-integration-as2-message-settings/send-settings.png)

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **İleti imzalamayı etkinleştir** | Hayır | Tüm giden iletiler dijital olarak imzalanmış olup olmaması gerektiğini belirtir. İmzalama gerektiriyorsa, bu değerleri seçin: <p>-From **imzalama algoritması** listesinde, iletileri imzalamak için kullanılacak algoritmayı seçin. <br>-From **sertifika** listesinde, iletileri imzalamak için var olan konak iş ortağı özel bir sertifikayı seçin. Bir sertifika yoksa, daha fazla bilgi edinin [sertifikaları ekleyerek](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **İleti şifrelemeyi etkinleştir** | Hayır | Tüm giden iletilerin şifrelenmesinin gerekip gerekmediğini belirtir. Şifreleme gerekiyorsa, bu değerleri seçin: <p>-From **şifreleme algoritması** listesinde, iletileri şifrelemek için kullanılacak Konuk iş ortağı ortak sertifika algoritmasını seçin. <br>-From **sertifika** listesinde, giden iletileri şifrelemek için mevcut bir konuk iş ortağı özel sertifikayı seçin. Bir sertifika yoksa, daha fazla bilgi edinin [sertifikaları ekleyerek](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **İleti sıkıştırmayı etkinleştir** | Hayır | Tüm giden iletiler sıkıştırılmasının olup olmadığını belirtir. |
| **HTTP üst bilgilerini Aç** | Hayır | HTTP koyar `content-type` üstbilgisi tek bir satıra sürükleyin. |
| **Dosya adı MIME üst bilgisindeki iletme** | Hayır | Dosya adını arar eklenip eklenmeyeceğini belirtir. |
| **MDN iste** | Hayır | İleti değerlendirme bildirimleri (Mdn'leri) tüm giden iletileri alacak şekilde belirtir. |
| **İmzalı MDN iste** | Hayır | Tüm giden iletiler için imzalı Mdn'leri almak isteyip belirtir. İmzalama, gerekiyorsa gelen **MIC algoritması** listesinde, iletileri imzalamak için kullanılacak algoritmayı seçin. |
| **Zaman uyumsuz MDN iste** | Hayır | Zaman uyumsuz Mdn'leri almayı belirtir. İçinde zaman uyumsuz Mdn'leri seçerseniz **URL** kutusunda Mdn'leri gönderileceği URL belirtin. |
| **NRR'yi etkinleştir** | Hayır | İnkar edilemez makbuz (NRR) gerekip gerekmediğini belirtir. Bu iletişim öznitelik kanıt sağlar. veriler olarak ele alınan. |
| **SHA2 algoritması biçimi** | Hayır | Giden AS2 iletileri veya MDN üst bilgisindeki imzalama için kullanılacak MIC algoritması biçimi belirtir. |
||||

## <a name="next-steps"></a>Sonraki adımlar

[AS2 iletilerini paylaşma](../logic-apps/logic-apps-enterprise-integration-as2.md)
