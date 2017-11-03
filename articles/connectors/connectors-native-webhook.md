---
title: "Azure Logic Apps için Web kancası Bağlayıcısı | Microsoft Docs"
description: "Web kancası eylemleri ve Tetikleyicileri mantığı uygulamalardan filtre dizisi gibi eylemleri gerçekleştirmek için nasıl kullanılacağını"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: fbfef291334109c6dcfcde80741874549fb7929f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-webhook-connector"></a>Web kancası Bağlayıcısı ile çalışmaya başlama

Web kancası eylem ve tetikleyici ile başlatmak, duraklatmak ve bu görevleri gerçekleştirmek için akışları Sürdür:

* Gelen tetikleyen bir [Azure olay hub'ı](https://github.com/logicappsio/EventHubAPI) öğeyi alındığında
* Bir iş akışı devam etmeden önce bir onay beklemek

Daha fazla bilgi edinmek [bir Web kancası destekleyen özel API oluşturma](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Web kancası tetikleyici kullanın

A [ *tetikleyici* ](connectors-overview.md) mantığı uygulama akışı başlar bir olaydır. Bir Web kancası tetikleyici olay tabanlı ve yeni öğeler için yoklama kullanmaz. Gibi [isteği tetikleyici](connectors-native-reqres.md), bir olay gerçekleştiğinde anlık mantıksal uygulama ateşlenir. Web kancası tetikleyici kaydeder bir *geri çağırma URL'si* olarak mantıksal uygulama tetiklenecek bu URL'yi kullanır ve bir hizmeti için gerekli.

Bir HTTP tetikleyicisi mantığı Uygulama Tasarımcısı'nda ayarlama gösteren örnek aşağıda verilmiştir. Zaten dağıttıysanız veya izleyen bir API erişme adımlarının varsayın [Web kancası abone olma ve logic apps desende aboneliği](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). Her bir mantıksal uygulama sahip yeni bir Web kancası kaydedildi veya devre dışı iken etkin anahtarlı abone ol çağrı yapılır. Logic app Web kancası tetikleyicisi kaldırılan ve kaydedildiğinde veya gelen geçiş abonelikten çağrı yapılır devre dışı olarak etkinleştirilmiş.

**Web kancası tetikleyici eklemek için**

1. Ekleme **HTTP Web kancası** tetikleyici bir mantıksal uygulama ilk adımı olarak.
2. Web kancası abone olma ve çağrıları aboneliği için parametrelerini doldurun.

   Bu adımı olarak aynı deseni takip [HTTP eylemi](connectors-native-http.md) biçimi.

     ![HTTP tetikleyici](./media/connectors-native-webhook/using-trigger.png)

3. En az bir eylem ekleyin.
4. Tıklatın **kaydetmek** mantıksal uygulama yayımlamak için. Bu adım, bu mantıksal uygulama tetiklemek için gereken bir geri çağırma URL'si ile abone ol endpoint çağırır.
5. Her hizmet yapar bir `HTTP POST` geri çağırma URL'si, mantıksal uygulama için ateşlenir ve istek geçirilen verileri içerir.

## <a name="use-the-webhook-action"></a>Web kancası eylemi kullanın

Bir [ *eylem* ](connectors-overview.md) bir işlem bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilir. Bir Web kancası eylemi kaydeder bir *geri çağırma URL'si* bir hizmet ve devam etmeden önce URL'yi çağrılıncaya kadar bekler. ["Onay e-posta Gönder"](connectors-create-api-office365-outlook.md) bu deseni izler bir bağlayıcı örneğidir. Web kancası eylem aracılığıyla herhangi bir hizmete bu deseni genişletebilirsiniz. 

Burada, bir Web kancası eylemi mantığı Uygulama Tasarımcısı'nda ayarlamak nasıl oluşturulduğunu gösteren bir örnek verilmiştir. Zaten dağıttıysanız veya izleyen bir API'sine erişim bu adımları varsayın [Web kancası abone olma ve logic apps içinde kullanılan Düzen aboneliği](../logic-apps/logic-apps-create-api-app.md#webhook-actions). Bir mantıksal uygulama Web kancası eylem yürüttüğünde abone ol çağrı yapılır. Bir çalıştırma için bir yanıt beklerken iptal ya da uygulama mantığını önce zaman aşımına uğruyor abonelikten çağrı yapılır.

**Bir Web kancası eylemi eklemek için**

1. Seçin **yeni adım** > **Eylem Ekle**.

2. Arama kutusuna bulmak için "Web kancası" yazın **HTTP Web kancası** eylem.

    ![Sorgu eylemi seçin](./media/connectors-native-webhook/using-action-1.png)

3. Web kancası abone olma ve çağrıları aboneliği için parametrelerini doldurun

   Bu adımı olarak aynı deseni takip [HTTP eylemi](connectors-native-http.md) biçimi.

     ![Tam bir sorgu eylemi](./media/connectors-native-webhook/using-action-2.png)
   
   Çalışma zamanında, bu adımı eriştikten sonra mantıksal uygulama abone ol endpoint çağırır.

4. Tıklatın **kaydetmek** mantıksal uygulama yayımlamak için.

## <a name="technical-details"></a>Teknik Ayrıntılar

Daha fazla ayrıntı aşağıdadır tetikleyiciler ve eylemler hakkında Web kancası destekler.

## <a name="webhook-triggers"></a>Web kancası Tetikleyicileri

| Eylem | Açıklama |
| --- | --- |
| HTTP Web kancası |Bir geri çağırma URL'si gerektiği gibi mantıksal uygulama tetiklenecek URL çağırabilirsiniz bir hizmete abone olun. |

### <a name="trigger-details"></a>Tetikleyici ayrıntıları

#### <a name="http-webhook"></a>HTTP Web kancası

Bir geri çağırma URL'si gerektiği gibi mantıksal uygulama tetiklenecek URL çağırabilirsiniz bir hizmete abone olun.
Bir * gerekli alan anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Abone yöntemi * |Yöntemi |Abone ol istek için kullanılacak HTTP yöntemi |
| Abone URI * |URI |Abone ol istek için kullanılacak HTTP URI |
| Aboneliği yöntemi * |Yöntemi |Aboneliği kaldırma isteği için kullanılacak HTTP yöntemi |
| Aboneliği URI * |URI |HTTP abonelikten istek için kullanılacak URI |
| Gövde abone olma |Gövde |Abonelik için HTTP istek gövdesi |
| Üstbilgiler abone olma |Üstbilgileri |Abonelik için HTTP isteği üstbilgileri |
| Kimlik doğrulama abone olma |Kimlik doğrulaması |Abonelik için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) Ayrıntılar için |
| Gövde aboneliği |Gövde |HTTP isteği gövdesinin abonelikten için |
| Üstbilgiler aboneliği |Üstbilgileri |HTTP istek üstbilgilerinin abonelikten için |
| Kimlik doğrulama aboneliği |Kimlik doğrulaması |Aboneliği kaldırma için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) Ayrıntılar için |

**Çıkış Ayrıntıları**

Web kancası isteği

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Üstbilgileri |Nesne |Web kancası istek üstbilgileri |
| Gövde |Nesne |Web kancası istek nesnesi |
| Durum kodu |Int |Web kancası isteği durum kodu |

## <a name="webhook-actions"></a>Web kancası eylemleri

| Eylem | Açıklama |
| --- | --- |
| HTTP Web kancası |Bir geri çağırma URL'si gerektiği gibi bir iş akışı adımı sürdürmek için URL çağırabilirsiniz bir hizmete abone olun. |

### <a name="action-details"></a>Eylem ayrıntıları

#### <a name="http-webhook"></a>HTTP Web kancası

Bir geri çağırma URL'si gerektiği gibi bir iş akışı adımı sürdürmek için URL çağırabilirsiniz bir hizmete abone olun.
Bir * gerekli alan anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Abone yöntemi * |Yöntemi |Abone ol istek için kullanılacak HTTP yöntemi |
| Abone URI * |URI |Abone ol istek için kullanılacak HTTP URI |
| Aboneliği yöntemi * |Yöntemi |Aboneliği kaldırma isteği için kullanılacak HTTP yöntemi |
| Aboneliği URI * |URI |HTTP abonelikten istek için kullanılacak URI |
| Gövde abone olma |Gövde |Abonelik için HTTP istek gövdesi |
| Üstbilgiler abone olma |Üstbilgileri |Abonelik için HTTP isteği üstbilgileri |
| Kimlik doğrulama abone olma |Kimlik doğrulaması |Abonelik için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) Ayrıntılar için |
| Gövde aboneliği |Gövde |HTTP isteği gövdesinin abonelikten için |
| Üstbilgiler aboneliği |Üstbilgileri |HTTP istek üstbilgilerinin abonelikten için |
| Kimlik doğrulama aboneliği |Kimlik doğrulaması |Aboneliği kaldırma için kullanılacak HTTP kimlik doğrulaması. [HTTP Bağlayıcısı bakın](connectors-native-http.md#authentication) Ayrıntılar için |

**Çıkış Ayrıntıları**

Web kancası isteği

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Üstbilgileri |Nesne |Web kancası istek üstbilgileri |
| Gövde |Nesne |Web kancası istek nesnesi |
| Durum kodu |Int |Web kancası isteği durum kodu |

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama oluşturun.](../logic-apps/logic-apps-create-a-logic-app.md)
* [Diğer bağlayıcıları Bul](apis-list.md)