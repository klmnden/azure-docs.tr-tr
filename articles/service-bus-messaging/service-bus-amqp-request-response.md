---
title: İstek-yanıt tabanlı Azure Service Bus işlemlerinde AMQP 1.0 | Microsoft Docs
description: Microsoft Azure Service Bus istek/yanıt tabanlı işlemleri listesi.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: sethm
ms.openlocfilehash: cda313085d197558e969309eaed928421b0b1924
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752913"
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>Microsoft Azure hizmet veri yolu AMQP 1.0: istek-yanıt tabanlı işlemleri

Bu makalede, Microsoft Azure Service Bus istek/yanıt tabanlı işlemlerin listesini tanımlar. Bu bilgiler üzerinde AMQP yönetim sürüm 1.0 çalışma taslak dayanır.  
  
Service Bus nasıl uygular ve OASIS AMQP teknik belirtimi derlemeler açıklayan, ayrıntılı Hat düzeyinde AMQP 1.0 protokolü kılavuzu için bkz: [AMQP 1.0 Azure Service Bus ve Event Hubs Protokolü Kılavuzu'nda][amqp 1.0 protokol kılavuzu].  
  
## <a name="concepts"></a>Kavramlar  
  
### <a name="entity-description"></a>Varlık açıklaması  

Bir varlık açıklaması ya da hizmet veri yolu başvuruyor [QueueDescription sınıfı](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [TopicDescription sınıfı](/dotnet/api/microsoft.servicebus.messaging.topicdescription), veya [SubscriptionDescription sınıfı](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) nesnesi.  
  
### <a name="brokered-message"></a>Aracılık edilen ileti  

Hizmet veri yolu AMQP iletisine eşlenen iletisinde temsil eder. Eşleme tanımlanan [hizmet veri yolu AMQP protokolünü Kılavuzu](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-to-entity-management-node"></a>Varlık Yönetimi düğümüne ekleme  

Bu belgede açıklanan tüm işlemleri istek/yanıt desenler izleyen bir varlığa kapsamlı ve bir varlık yönetimi düğümüne bağlanmasını gerektirir.  
  
### <a name="create-link-for-sending-requests"></a>İstekleri göndermek için bağlantı oluşturma  

İstek göndermek için yönetim düğümü için bir bağlantı oluşturur.  
  
```  
requestLink = session.attach(     
role: SENDER,   
    target: { address: "<entity address>/$management" },   
    source: { address: ""<my request link unique address>" }   
)  
  
```  
  
### <a name="create-link-for-receiving-responses"></a>Yanıtları almak için bağlantı oluşturma  

Yönetim düğümden yanıtları almak için bir bağlantı oluşturur.  
  
```  
responseLink = session.attach(    
role: RECEIVER,   
    source: { address: "<entity address>/$management" }   
    target: { address: "<my response link unique address>" }   
)  
  
```  
  
### <a name="transfer-a-request-message"></a>Bir istek iletisi Aktarım  

Bir istek iletisi aktarır.  
İşlem durumu, işlem destekleyen işlemleri için isteğe bağlı olarak eklenebilir.

```  
requestLink.sendTransfer(  
        Message(  
                properties: {  
                        message-id: <request id>,  
                        reply-to: "<my response link unique address>"  
                },  
                application-properties: {  
                        "operation" -> "<operation>",  
                }
        ),
        [Optional] State = transactional-state: {
                txn-id: <txn-id>
        }
)
```  
  
### <a name="receive-a-response-message"></a>Bir yanıt iletisi alıyorsunuz  

Yanıt bağlantısından yanıt iletisini alır.  
  
```  
responseMessage = responseLink.receiveTransfer()  
```  
  
Yanıt iletisi aşağıdaki biçimdedir:
  
```  
Message(  
properties: {     
        correlation-id: <request id>  
    },  
    application-properties: {  
            "statusCode" -> <status code>,  
            "statusDescription" -> <status description>,  
           },         
)  
  
```  
  
### <a name="service-bus-entity-address"></a>Service Bus varlık adresi  

Hizmet veri yolu varlıklarını gibi ele alınması gerekir:  
  
|Varlık türü|Adres|Örnek|  
|-----------------|-------------|-------------|  
|kuyruk|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|konu başlığı|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|aboneliği|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>Mesaj işlemleri  
  
### <a name="message-renew-lock"></a>İleti kilit yenileme  

Varlık açıklamasında belirtilen süreye göre bir ileti kilit genişletir.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
 İstek ileti gövdesi aşağıdaki girişleri ile eşleme içeren bir amqp değer bölümünde oluşması gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|UUID dizisi|Evet|Yenilemek için kilit belirteçleri ileti.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu.|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi aşağıdaki girişleri ile eşleme içeren bir amqp değer bölümünde oluşması gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|süre sonu|zaman damgası dizisi|Evet|Kilit belirteç isteme karşılık gelen ileti kilit belirteci yeni süre sonu.|  
  
### <a name="peek-message"></a>İletiye Gözat  

İletileri kilitlemeden iletiye göz atar.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|boylam|Evet|Peek başlayacağı sıra numarası.|  
|`message-count`|int|Evet|En fazla atmaya ileti sayısı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti sahip<br /><br /> 0xcc: Hayır içerik – daha fazla ileti yok|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her eşleme bir iletiyi temsil eden iletilerinin listesi.|  
  
Bir ileti temsil eden harita aşağıdaki girdileri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|Hat üzeri olarak kodlanmış bir AMQP 1.0 ileti.|  
  
### <a name="schedule-message"></a>Zamanlama iletisi  

İletileri zamanlar. Bu işlem, işlem destekler.
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her eşleme bir iletiyi temsil eden iletilerinin listesi.|  
  
Bir ileti temsil eden harita aşağıdaki girdileri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|ileti kimliği|dize|Evet|`amqpMessage.Properties.MessageId` dize olarak|  
|oturum kimliği|dize|Hayır|`amqpMessage.Properties.GroupId as string`|  
|Bölüm anahtarı|dize|Hayır|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|
|aracılığıyla bölüm-anahtar|dize|Hayır|`amqpMessage.MessageAnnotations."x-opt-via-partition-key"`|
|message|bayt dizisi|Evet|Hat üzeri olarak kodlanmış bir AMQP 1.0 ileti.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu.|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölümüne aşağıdaki girişleri ile eşleme içeren:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|uzun dizi|Evet|Zamanlanmış ileti sıra numarası. Sıra numarası, iptal etmek için kullanılır.|  
  
### <a name="cancel-scheduled-message"></a>Zamanlanmış iletiyi iptal etme  

Zamanlanmış iletileri iptal eder.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|uzun dizi|Evet|İptal etmek için zamanlanmış ileti sıra numarası.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu.|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölümüne aşağıdaki girişleri ile eşleme içeren:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|uzun dizi|Evet|Zamanlanmış ileti sıra numarası. Sıra numarası, iptal etmek için kullanılır.|  
  
## <a name="session-operations"></a>Oturum işlemleri  
  
### <a name="session-renew-lock"></a>Oturum kilidi yenileme  

Varlık açıklamasında belirtilen süreye göre bir ileti kilit genişletir.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|dize|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti sahip<br /><br /> 0xcc: Hayır içerik – daha fazla ileti yok|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölümüne aşağıdaki girişleri ile eşleme içeren:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|süre sonu|timestamp|Evet|Yeni süre sonu.|  
  
### <a name="peek-session-message"></a>Oturum İletiye Gözat  

Oturum iletileri kilitlemeden iletiye göz atar.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sırası-sayı|boylam|Evet|Peek başlayacağı sıra numarası.|  
|ileti sayısı|int|Evet|En fazla atmaya ileti sayısı.|  
|oturum kimliği|dize|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti sahip<br /><br /> 0xcc: Hayır içerik – daha fazla ileti yok|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölümüne aşağıdaki girişleri ile eşleme içeren:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her eşleme bir iletiyi temsil eden iletilerinin listesi.|  
  
 Bir ileti temsil eden harita aşağıdaki girdileri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|Hat üzeri olarak kodlanmış bir AMQP 1.0 ileti.|  
  
### <a name="set-session-state"></a>Oturum durumunu belirle  

Oturum durumunu ayarlar.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|dize|Evet|Oturum kimliği|  
|oturum durumu|bir bayt dizisi|Evet|Donuk ikili veri.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
### <a name="get-session-state"></a>Get oturum durumu  

Oturum durumunu alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|dize|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum durumu|bir bayt dizisi|Evet|Donuk ikili veri.|  
  
### <a name="enumerate-sessions"></a>Oturumları listeleme  

Bir Mesajlaşma varlığı oturumlarını numaralandırır.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Son güncelleştirme saati|timestamp|Evet|Yalnızca belirli bir süre sonra güncelleştirilmiş oturumları göstermek için filtrelenir.|  
|Atla|int|Evet|Oturum sayısını atlayın.|  
|Sayfanın Üstü|int|Evet|En fazla oturum sayısını.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti sahip<br /><br /> 0xcc: Hayır içerik – daha fazla ileti yok|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Atla|int|Evet|Durum kodu 200 ise Atlanan oturum sayısı.|  
|oturumları kimlikleri|dize dizisi|Evet|Oturum durum kodu 200 ise kimlikleri dizisi.|  
  
## <a name="rule-operations"></a>Kuralı işlemleri  
  
### <a name="add-rule"></a>Kural Ekle  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural adı|dize|Evet|Kural adı, abonelik ve konu adları dahil edilmez.|  
|Kural açıklaması|Eşleme|Evet|Sonraki bölümde belirtildiği gibi kural açıklaması.|  
  
**Kural açıklaması** eşlemesi, aşağıdaki girişleri içermelidir nerede **sql filtresi** ve **bağıntı filtresi** karşılıklı olarak birbirini dışlar:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|SQL filtresi|Eşleme|Evet|`sql-filter`, sonraki bölümde belirtildiği gibi.|  
|Bağıntı filtresi|Eşleme|Evet|`correlation-filter`, sonraki bölümde belirtildiği gibi.|  
|SQL kural eylemi|Eşleme|Evet|`sql-rule-action`, sonraki bölümde belirtildiği gibi.|  
  
Sql filtresi eşleme aşağıdaki girdileri şunları içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İfade|dize|Evet|SQL filtre ifadesi.|  
  
**Bağıntı filtresi** harita aşağıdaki girdileri en az birini içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Bağıntı Kimliği|dize|Hayır||  
|ileti kimliği|dize|Hayır||  
|-|dize|Hayır||  
|Yanıtla|dize|Hayır||  
|etiket|dize|Hayır||  
|oturum kimliği|dize|Hayır||  
|yanıt için oturum kimliği|dize|Hayır||  
|içerik türü|dize|Hayır||  
|properties|Eşleme|Hayır|Hizmet veri yolu eşlemelerini [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
**Sql kural eylemi** eşlemesi, aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İfade|dize|Evet|SQL eylem ifade.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
### <a name="remove-rule"></a>Kuralı Kaldır  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural adı|dize|Evet|Kural adı, abonelik ve konu adları dahil edilmez.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
### <a name="get-rules"></a>Kuralları Al

#### <a name="request"></a>İstek

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:

|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:enumerate-rules`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  

İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Sayfanın Üstü|int|Evet|Sayfanın getirmek için kuralları sayısı.|  
|Atla|int|Evet|Atlamak için kuralları sayısı. Başlangıç dizini (+ 1) kurallarının listesini tanımlar. | 

#### <a name="response"></a>Yanıt

Yanıt iletisi aşağıdaki özellikleri içerir:

|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|rules| Harita dizisi|Evet|Kuralları dizisi. Her bir kural tarafından bir harita temsil edilir.|

Dizideki her eşleme girişi aşağıdaki özellikleri içerir:

|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural açıklaması|açıklanan nesneler dizisi|Evet|`com.microsoft:rule-description:list` kod 0x0000013700000004 AMQP ile açıklanan| 

`com.microsoft.rule-description:list` açıklanan nesnelerinin bir dizisidir. Dizi aşağıdakileri içerir:

|Dizin oluşturma|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | açıklanan nesneler dizisi | Evet | `filter` Aşağıda belirtildiği gibi. |
| 1 | açıklanan nesne dizisi | Evet | `ruleAction` Aşağıda belirtildiği gibi. |
| 2 | dize | Evet | Kural adı. |

`filter` şu türlerden birini olabilir:

| Tanımlayıcı adı | Tanımlayıcı kod | Değer |
| --- | --- | ---|
| `com.microsoft:sql-filter:list` | 0x000001370000006 | SQL filtresi |
| `com.microsoft:correlation-filter:list` | 0x000001370000009 | Bağıntı filtresi |
| `com.microsoft:true-filter:list` | 0x000001370000007 | 1 = 1 temsil eden true filtresi |
| `com.microsoft:false-filter:list` | 0x000001370000008 | 1 = 0 temsil eden false filtresi |

`com.microsoft:sql-filter:list` içeren açıklanan bir dizi şöyledir:

|Dizin oluşturma|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | dize | Evet | SQL filtre ifadesi |

`com.microsoft:correlation-filter:list` içeren açıklanan bir dizi şöyledir:

|Dizin (varsa var)|Değer türü|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | dize | Bağıntı Kimliği |
| 1 | dize | İleti Kimliği |
| 2 | dize | Alıcı |
| 3 | dize | Yanıtla |
| 4 | dize | Etiket |
| 5 | dize | Oturum Kimliği |
| 6 | dize | Oturum kimliği Yanıtla|
| 7 | dize | İçerik Türü |
| 8 | Eşleme | Uygulama tanımlı özelliklerinin eşleme |

`ruleAction` Aşağıdaki türlerden biri olabilir:

| Tanımlayıcı adı | Tanımlayıcı kod | Değer |
| --- | --- | ---|
| `com.microsoft:empty-rule-action:list` | 0x0000013700000005 | Boş kural eylemi - mevcut hiçbir kural eylemi |
| `com.microsoft:sql-rule-action:list` | 0x0000013700000006 | SQL kural eylemi |

`com.microsoft:sql-rule-action:list` SQL kural eylemin ifadesi içeren bir dize olan ilk giriştir açıklanan nesnelerinin bir dizisidir.

## <a name="deferred-message-operations"></a>Ertelenmiş ileti işlemleri  
  
### <a name="receive-by-sequence-number"></a>Seri numarasına göre alma  

Sıra numarası tarafından ertelenmiş iletilerini alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|uzun dizi|Evet|Sıra numaraları.|  
|receiver-settle-mode|ubyte|Evet|**Alıcı kapatma** AMQP çekirdek v1.0 belirtildiği gibi modu.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|statusDescription|dize|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her eşleme bir ileti temsil ettiği ileti listesi.|  
  
Bir ileti temsil eden harita aşağıdaki girdileri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|kilit simgesi|UUID|Evet|Kilit belirteci IF `receiver-settle-mode` 1'dir.|  
|message|bayt dizisi|Evet|Hat üzeri olarak kodlanmış bir AMQP 1.0 ileti.|  
  
### <a name="update-disposition-status"></a>Değerlendirme durumunu güncelleştir  

Ertelenmiş iletileri değerlendirme durumunu güncelleştirir. Bu işlem işlemleri destekler.
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlem|dize|Evet|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Değerlendirme durumu|dize|Evet|Tamamlandı<br /><br /> terk<br /><br /> Askıya alındı|  
|Kilit belirteçleri|UUID dizisi|Evet|Değerlendirme durumunu güncelleştirmek için kilit belirteçleri ileti.|  
|sahipsiz nedeni|dize|Hayır|Değerlendirme durumu ayarlanmışsa ayarlanabilir **askıya**.|  
|sahipsiz açıklaması|dize|Hayır|Değerlendirme durumu ayarlanmışsa ayarlanabilir **askıya**.|  
|değiştirme özellikleri|Eşleme|Hayır|Hizmet veri yolu listesini değiştirmek için ileti özellikleri aracılı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|statusDescription|dize|Hayır|Durum açıklaması.|

## <a name="next-steps"></a>Sonraki adımlar

AMQP ve Service Bus hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Hizmet veri yolu AMQP genel bakış]
* [AMQP 1.0 protokol kılavuzu]
* [Windows Server için hizmet veri yolu AMQP]

[Hizmet veri yolu AMQP genel bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md
[Windows Server için hizmet veri yolu AMQP]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)