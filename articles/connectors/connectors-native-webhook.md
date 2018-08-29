---
title: Olay tabanlı iş akışları veya Eylemler - Azure Logic Apps oluşturun | Microsoft Docs
description: Web kancaları ve Azure Logic Apps kullanarak olay-tabanlı iş akışları veya eylemleri otomatikleştirme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.topic: article
tags: connectors
ms.date: 07/21/2016
ms.openlocfilehash: 7b1886321ca4afd4b4710bd9fddf16d2d5eb224b
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126596"
---
# <a name="create-event-based-workflows-or-actions-by-using-webhooks-and-azure-logic-apps"></a>Web kancaları ve Azure Logic Apps kullanarak olay-tabanlı iş akışları veya Eylemler oluşturma

Web kancası eylemi ve tetikleyicisi ile Başlat, duraklatmak ve bu görevleri gerçekleştirmek üzere akışları Sürdür:

* Gelen tetikleyen bir [Azure olay hub'ı](https://github.com/logicappsio/EventHubAPI) öğeyi alındığında
* Bir iş akışı devam etmeden önce bir onay bekleme

Daha fazla bilgi edinin [destekleyen bir Web kancasını özel API oluşturma](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Web kancası tetikleyicisini kullanma

A [ *tetikleyici* ](connectors-overview.md) bir mantıksal uygulama iş akışı başlatan olaylardır. Bir Web kancası tetikleyici olay tabanlı ve yeni öğeler için yoklama içermez. Gibi [istek tetikleyicisi](connectors-native-reqres.md), mantıksal uygulama bir olay meydana anında başlatılır. Web kancası tetikleyicisine kaydeder bir *geri çağırma URL'si* hizmet ve kullandığı için mantıksal uygulama olarak harekete geçirmek için bu URL'yi gerekir.

HTTP tetikleyicisi mantıksal Uygulama Tasarımcısı'nda ayarlama gösteren bir örnek aşağıda verilmiştir. Zaten dağıtmış olan veya izleyen bir API erişme adımlarda varsayılır [Web kancası abone olma ve logic apps'te deseni aboneliği](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). Her bir mantıksal uygulama ile yeni bir Web kancası kaydedildi veya geçiş devre dışı iken etkin olarak abone ol çağrı yapılır. Bir mantıksal uygulama Web kancası tetikleyici kaldırıldı ve kaydedildiğinde veya gelen geçiş unsubscribe çağrı yapılır devre dışı olarak etkinleştirildi.

**Web kancası tetikleyicisine eklemek için**

1. Ekleme **HTTP Web kancası** tetikleyici bir mantıksal uygulama ilk adımı olarak.
2. Web kancası abone olma ve aboneliği çağrıları için parametreleri doldurun.

   Bu adım olarak aynı deseni izler [HTTP eylemi](connectors-native-http.md) biçimi.

     ![HTTP tetikleyicisi](./media/connectors-native-webhook/using-trigger.png)

3. En az bir eylem ekleyin.
4. Tıklayın **Kaydet** mantıksal uygulamayı yayımlamak için. Bu adım, bu mantıksal uygulama tetiklemek için gereken bir geri çağırma URL'si ile abone uç noktası çağırır.
5. Herhangi bir zamanda hizmet yapar bir `HTTP POST` geri çağırma URL'si, mantıksal uygulama tetiklenir ve istek geçirilen tüm verileri içerir.

## <a name="use-the-webhook-action"></a>Web kancası eylemi kullanın

Bir [ *eylem* ](connectors-overview.md) mantıksal uygulamada tanımlanan iş akışı tarafından bir işlemi gerçekleştirilir. Bir Web kancası eylemi kaydeder bir *geri çağırma URL'si* hizmet ve devam etmeden önce URL'yi çağrılana kadar bekler. ["Onay e-posta Gönder"](connectors-create-api-office365-outlook.md) bu yapıdadır bağlayıcıyı örneğidir. Bu düzen, herhangi bir hizmeti aracılığıyla Web kancası eylem halinde genişletebilirsiniz. 

Mantıksal Uygulama Tasarımcısı'nda bir Web kancası eylemi ayarlama gösteren bir örnek aşağıda verilmiştir. Adımları dağıttıktan veya izleyen bir API eriştiğiniz varsayar [Web kancası abone olma ve aboneliği logic apps'te kullanılan desen](../logic-apps/logic-apps-create-api-app.md#webhook-actions). Mantıksal uygulama, Web kancası eylemi yürütüldüğünde abone ol çağrı yapılır. Bir çalışma için bir yanıt beklerken iptal edilir ya da uygulama mantığı önce zaman aşımına olduğunda unsubscribe çağrı yapılır.

**Bir Web kancası eylemi eklemek için**

1. Seçin **yeni adım** > **Eylem Ekle**.

2. Arama kutusuna "Web kancası" bulmak için yazın. **HTTP Web kancası** eylem.

    ![Sorgu eylemi seçin](./media/connectors-native-webhook/using-action-1.png)

3. Web kancası abone olma ve aboneliği çağrıları için parametreleri doldurun

   Bu adım olarak aynı deseni izler [HTTP eylemi](connectors-native-http.md) biçimi.

     ![Tam sorgu eylemi](./media/connectors-native-webhook/using-action-2.png)
   
   Çalışma zamanında, mantıksal uygulama, abone uç noktası bu adımı ulaştıktan sonra çağırır.

4. Tıklayın **Kaydet** mantıksal uygulamayı yayımlamak için.

## <a name="technical-details"></a>Teknik Ayrıntılar

Daha fazla ayrıntı aşağıdadır tetikleyiciler ve eylemler hakkında bu Web kancası destekler.

## <a name="webhook-triggers"></a>Web kancası Tetikleyicileri

| Eylem | Açıklama |
| --- | --- |
| HTTP Web kancası |Bir geri çağırma URL'si yangın gerektiği gibi mantıksal uygulama için URL'yi çağırabilen bir hizmete abone olun. |

### <a name="trigger-details"></a>Tetikleyici ayrıntıları

#### <a name="http-webhook"></a>HTTP Web kancası

Bir geri çağırma URL'si yangın gerektiği gibi mantıksal uygulama için URL'yi çağırabilen bir hizmete abone olun.
Bir * gerekli alan anlamına gelir.

| Görünen Ad | Özellik Adı | Açıklama |
| --- | --- | --- |
| Abone yöntemi * |method |Abonelik isteği için kullanılacak HTTP yöntemi |
| Abone URI * |uri |Abonelik isteği için kullanılacak HTTP URI |
| Aboneliği yöntemi * |method |Aboneliği kaldırma isteği için kullanılacak HTTP yöntemi |
| URI * aboneliği iptal et |uri |Aboneliği kaldırma isteği için kullanılacak HTTP URI |
| Gövde abone olun |body |Abonelik için HTTP istek gövdesi |
| Üstbilgileri abone olun |headers |HTTP isteği üstbilgileri için abone ol |
| Kimlik doğrulaması abone olun |kimlik doğrulaması |Abone için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) için Ayrıntılar |
| Gövde aboneliği iptal et |body |HTTP istek gövdesi için Aboneliği Kaldır |
| Üstbilgileri aboneliği iptal et |headers |HTTP isteği üstbilgileri için Aboneliği Kaldır |
| Kimlik doğrulaması aboneliği iptal et |kimlik doğrulaması |Abonelik iptali için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) için Ayrıntılar |

**Çıkış Ayrıntıları**

Web kancası isteği

| Özellik Adı | Veri Türü | Açıklama |
| --- | --- | --- |
| Üst bilgiler |object |Web kancası isteği üst bilgileri |
| Gövde |object |Web kancası isteği nesnesi |
| Durum Kodu |int |Web kancası isteği durum kodu |

## <a name="webhook-actions"></a>Web kancası eylemleri

| Eylem | Açıklama |
| --- | --- |
| HTTP Web kancası |Bir geri çağırma URL'si gerektiği gibi bir iş akışı adımı sürdürmek için URL çağırabilen bir hizmete abone olun. |

### <a name="action-details"></a>Eylem ayrıntıları

#### <a name="http-webhook"></a>HTTP Web kancası

Bir geri çağırma URL'si gerektiği gibi bir iş akışı adımı sürdürmek için URL çağırabilen bir hizmete abone olun.
Bir * gerekli alan anlamına gelir.

| Görünen Ad | Özellik Adı | Açıklama |
| --- | --- | --- |
| Abone yöntemi * |method |Abonelik isteği için kullanılacak HTTP yöntemi |
| Abone URI * |uri |Abonelik isteği için kullanılacak HTTP URI |
| Aboneliği yöntemi * |method |Aboneliği kaldırma isteği için kullanılacak HTTP yöntemi |
| URI * aboneliği iptal et |uri |Aboneliği kaldırma isteği için kullanılacak HTTP URI |
| Gövde abone olun |body |Abonelik için HTTP istek gövdesi |
| Üstbilgileri abone olun |headers |HTTP isteği üstbilgileri için abone ol |
| Kimlik doğrulaması abone olun |kimlik doğrulaması |Abone için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) için Ayrıntılar |
| Gövde aboneliği iptal et |body |HTTP istek gövdesi için Aboneliği Kaldır |
| Üstbilgileri aboneliği iptal et |headers |HTTP isteği üstbilgileri için Aboneliği Kaldır |
| Kimlik doğrulaması aboneliği iptal et |kimlik doğrulaması |Abonelik iptali için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) için Ayrıntılar |

**Çıkış Ayrıntıları**

Web kancası isteği

| Özellik Adı | Veri Türü | Açıklama |
| --- | --- | --- |
| Üst bilgiler |object |Web kancası isteği üst bilgileri |
| Gövde |object |Web kancası isteği nesnesi |
| Durum Kodu |int |Web kancası isteği durum kodu |

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Diğer bağlayıcıları bulma](apis-list.md)