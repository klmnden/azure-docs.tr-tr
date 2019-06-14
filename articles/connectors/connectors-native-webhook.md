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
ms.openlocfilehash: c3047000843e054e71ec1a80313118a25e7c4905
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60447232"
---
# <a name="create-event-based-workflows-or-actions-by-using-webhooks-and-azure-logic-apps"></a>Web kancaları ve Azure Logic Apps kullanarak olay-tabanlı iş akışları veya Eylemler oluşturma

Web kancası eylemi ve tetikleyicisi ile Başlat, duraklatmak ve bu görevleri gerçekleştirmek üzere akışları Sürdür:

* Gelen tetikleyen bir [Azure olay hub'ı](https://github.com/logicappsio/EventHubAPI) öğeyi alındığında
* Bir iş akışı devam etmeden önce bir onay bekleme

Daha fazla bilgi edinin [destekleyen bir Web kancasını özel API oluşturma](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Web kancası tetikleyicisini kullanma

A [ *tetikleyici* ](../connectors/apis-list.md) bir mantıksal uygulama iş akışı başlatan olaylardır. Web kancası tetikleyici yeni öğeler için yoklama hangi bağımlı olmayan olay tabanlı. Ne zaman bir Web kancası tetikleyici ile mantıksal uygulamanızı kaydetmeniz veya etkin devre dışı olarak gelen mantıksal uygulamanızı değiştirdiğinizde, Web kancası tetikleyicisine *abone* belirtilen hizmet ya da kaydederek uç noktası bir *geriçağırmaURL'si* hizmet veya uç nokta. Tetikleyici sonra gerektiği gibi mantıksal uygulamayı çalıştırmak için bu URL'yi kullanır. Gibi [istek tetikleyicisi](connectors-native-reqres.md), beklenen olay durumda hemen mantıksal uygulama başlatılır. Tetikleyici *abonelikten çıkma* tetikleyiciyi kaldırın ve mantıksal uygulamanızı kaydedin veya mantıksal uygulamanızdan etkinken devre dışı olarak değiştirdiğinizde.

HTTP tetikleyicisi mantıksal Uygulama Tasarımcısı'nda ayarlama gösteren bir örnek aşağıda verilmiştir. Zaten dağıtmış olan veya izleyen bir API erişme adımlarda varsayılır [Web kancası abone olma ve logic apps'te deseni aboneliği](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). 

**Web kancası tetikleyicisine eklemek için**

1. Ekleme **HTTP Web kancası** tetikleyici bir mantıksal uygulama ilk adımı olarak.
2. Web kancası abone olma ve aboneliği çağrıları için parametreleri doldurun.

   Bu adım olarak aynı deseni izler [HTTP eylemi](connectors-native-http.md) biçimi.

     ![HTTP Tetikleyicisi](./media/connectors-native-webhook/using-trigger.png)

3. En az bir eylem ekleyin.
4. Tıklayın **Kaydet** mantıksal uygulamayı yayımlamak için. Bu adım, bu mantıksal uygulama tetiklemek için gereken bir geri çağırma URL'si ile abone uç noktası çağırır.
5. Herhangi bir zamanda hizmet yapar bir `HTTP POST` geri çağırma URL'si, mantıksal uygulama tetiklenir ve istek geçirilen tüm verileri içerir.

## <a name="use-the-webhook-action"></a>Web kancası eylemi kullanın

Bir [ *eylem* ](../connectors/apis-list.md) bir tanımlanan işlemi ve mantıksal uygulamanızın iş akışı tarafından Çalıştır. Mantıksal uygulama bir Web kancası eylemi, bu eylem çalıştırıldığında *abone* belirtilen hizmet ya da kaydederek uç noktası bir *geri çağırma URL'si* hizmet veya uç nokta. Web kancası eylemi ardından aramaları çalıştıran logic app sürdürür önce URL'yi hizmet kadar bekler. Mantıksal uygulama hizmet veya uç noktası bu gibi durumlarda aboneliğini iptal eder: 

* Web kancası eylemi başarıyla tamamlandığında
* Bir yanıtı beklenirken mantıksal uygulama çalıştırması iptal edilirse
* Uygulama mantığı önce zaman aşımına uğruyor

Örneğin, [ **onay e-posta Gönder** ](connectors-create-api-office365-outlook.md) eylem Bu desen aşağıdaki Web kancası eylem örneği verilmiştir. Bu düzen, herhangi bir hizmeti aracılığıyla Web kancası eylem halinde genişletebilirsiniz. 

Mantıksal Uygulama Tasarımcısı'nda bir Web kancası eylemi ayarlama gösteren bir örnek aşağıda verilmiştir. Adımları dağıttıktan veya izleyen bir API eriştiğiniz varsayar [Web kancası abone olma ve aboneliği logic apps'te kullanılan desen](../logic-apps/logic-apps-create-api-app.md#webhook-actions). 

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

| Görünen ad | Özellik Adı | Açıklama |
| --- | --- | --- |
| Abone yöntemi * |method |Abonelik isteği için kullanılacak HTTP yöntemi |
| Abone URI * |URI |Abonelik isteği için kullanılacak HTTP URI |
| Aboneliği yöntemi * |method |Aboneliği kaldırma isteği için kullanılacak HTTP yöntemi |
| URI * aboneliği iptal et |URI |Aboneliği kaldırma isteği için kullanılacak HTTP URI |
| Gövde abone olun |Gövde |Abonelik için HTTP istek gövdesi |
| Üstbilgileri abone olun |Üst bilgileri |HTTP isteği üstbilgileri için abone ol |
| Kimlik doğrulaması abone olun |kimlik doğrulaması |Abone için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) için Ayrıntılar |
| Gövde aboneliği iptal et |Gövde |HTTP istek gövdesi için Aboneliği Kaldır |
| Üstbilgileri aboneliği iptal et |Üst bilgileri |HTTP isteği üstbilgileri için Aboneliği Kaldır |
| Kimlik doğrulaması aboneliği iptal et |kimlik doğrulaması |Abonelik iptali için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) için Ayrıntılar |

**Çıkış Ayrıntıları**

Web kancası isteği

| Özellik Adı | Veri Türü | Açıklama |
| --- | --- | --- |
| Üst bilgiler |object |Web kancası isteği üst bilgileri |
| Gövde |object |Web kancası isteği nesnesi |
| Durum kodu |int |Web kancası isteği durum kodu |

## <a name="webhook-actions"></a>Web kancası eylemleri

| Eylem | Açıklama |
| --- | --- |
| HTTP Web kancası |Bir geri çağırma URL'si gerektiği gibi bir iş akışı adımı sürdürmek için URL çağırabilen bir hizmete abone olun. |

### <a name="action-details"></a>Eylem ayrıntıları

#### <a name="http-webhook"></a>HTTP Web kancası

Bir geri çağırma URL'si gerektiği gibi bir iş akışı adımı sürdürmek için URL çağırabilen bir hizmete abone olun.
Bir * gerekli alan anlamına gelir.

| Görünen ad | Özellik Adı | Açıklama |
| --- | --- | --- |
| Abone yöntemi * |method |Abonelik isteği için kullanılacak HTTP yöntemi |
| Abone URI * |URI |Abonelik isteği için kullanılacak HTTP URI |
| Aboneliği yöntemi * |method |Aboneliği kaldırma isteği için kullanılacak HTTP yöntemi |
| URI * aboneliği iptal et |URI |Aboneliği kaldırma isteği için kullanılacak HTTP URI |
| Gövde abone olun |Gövde |Abonelik için HTTP istek gövdesi |
| Üstbilgileri abone olun |Üst bilgileri |HTTP isteği üstbilgileri için abone ol |
| Kimlik doğrulaması abone olun |kimlik doğrulaması |Abone için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) için Ayrıntılar |
| Gövde aboneliği iptal et |Gövde |HTTP istek gövdesi için Aboneliği Kaldır |
| Üstbilgileri aboneliği iptal et |Üst bilgileri |HTTP isteği üstbilgileri için Aboneliği Kaldır |
| Kimlik doğrulaması aboneliği iptal et |kimlik doğrulaması |Abonelik iptali için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) için Ayrıntılar |

**Çıkış Ayrıntıları**

Web kancası isteği

| Özellik Adı | Veri Türü | Açıklama |
| --- | --- | --- |
| Üst bilgiler |object |Web kancası isteği üst bilgileri |
| Gövde |object |Web kancası isteği nesnesi |
| Durum kodu |int |Web kancası isteği durum kodu |

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Diğer bağlayıcıları bulma](apis-list.md)
