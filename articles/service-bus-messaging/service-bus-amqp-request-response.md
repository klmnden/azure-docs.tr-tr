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
ms.openlocfilehash: d72a4de8591898a55e4225ace154fd5ed53e6f91
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>Microsoft Azure hizmet veri yolu AMQP 1.0: istek-yanıt tabanlı işlemleri

Bu makalede, Microsoft Azure Service Bus istek/yanıt tabanlı işlemlerin listesini tanımlar. Bu bilgiler üzerinde AMQP yönetim sürüm 1.0 çalışma taslak dayanır.  
  
Service Bus nasıl uygular ve OASIS AMQP teknik belirtimi derlemeler açıklayan, ayrıntılı Hat düzeyinde AMQP 1.0 protokolü kılavuzu için bkz: [AMQP 1.0 Azure Service Bus ve Event Hubs Protokolü Kılavuzu'nda][AMQP 1.0 protokol kılavuzu].  
  
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
  
```  
requestLink.sendTransfer(  
        Message(  
                properties: {  
                        message-id: <request id>,  
                        reply-to: "<my response link unique address>"  
                },  
                application-properties: {  
                        "operation" -> "<operation>",  
                },  
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
|Sırası|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|Konu|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|aboneliği|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>Mesaj işlemleri  
  
### <a name="message-renew-lock"></a>İleti kilit yenileme  

Varlık açıklamasında belirtilen süreye göre bir ileti kilit genişletir.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlemi|string|Evet|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
 İstek ileti gövdesi aşağıdaki girişleri ile eşleme içeren bir amqp değer bölümünde oluşması gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|UUID dizisi|Evet|Yenilemek için kilit belirteçleri ileti.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu.|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
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
|işlemi|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|uzun|Evet|Peek başlayacağı sıra numarası.|  
|`message-count`|Int|Evet|En fazla atmaya ileti sayısı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti sahip<br /><br /> 0xcc: Hayır içerik – daha fazla ileti yok|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her eşleme bir iletiyi temsil eden iletilerinin listesi.|  
  
Bir ileti temsil eden harita aşağıdaki girdileri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|Hat üzeri olarak kodlanmış bir AMQP 1.0 ileti.|  
  
### <a name="schedule-message"></a>Zamanlama iletisi  

İletileri zamanlar.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlemi|string|Evet|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her eşleme bir iletiyi temsil eden iletilerinin listesi.|  
  
Bir ileti temsil eden harita aşağıdaki girdileri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|ileti kimliği|string|Evet|`amqpMessage.Properties.MessageId` dize olarak|  
|oturum kimliği|string|Evet|`amqpMessage.Properties.GroupId as string`|  
|Bölüm anahtarı|string|Evet|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|  
|message|bayt dizisi|Evet|Hat üzeri olarak kodlanmış bir AMQP 1.0 ileti.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu.|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
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
|işlemi|string|Evet|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|uzun dizi|Evet|İptal etmek için zamanlanmış ileti sıra numarası.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu.|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
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
|işlemi|string|Evet|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti sahip<br /><br /> 0xcc: Hayır içerik – daha fazla ileti yok|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
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
|işlemi|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|from-sequence-number|uzun|Evet|Peek başlayacağı sıra numarası.|  
|ileti sayısı|Int|Evet|En fazla atmaya ileti sayısı.|  
|oturum kimliği|string|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti sahip<br /><br /> 0xcc: Hayır içerik – daha fazla ileti yok|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
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
|işlemi|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum kimliği|  
|oturum durumu|bir bayt dizisi|Evet|Donuk ikili veri.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
### <a name="get-session-state"></a>Get oturum durumu  

Oturum durumunu alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlemi|string|Evet|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
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
|işlemi|string|Evet|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|last-updated-time|timestamp|Evet|Yalnızca belirli bir süre sonra güncelleştirilmiş oturumları göstermek için filtrelenir.|  
|Atla|Int|Evet|Oturum sayısını atlayın.|  
|Sayfanın Üstü|Int|Evet|En fazla oturum sayısını.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti sahip<br /><br /> 0xcc: Hayır içerik – daha fazla ileti yok|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
Yanıt ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Atla|Int|Evet|Durum kodu 200 ise Atlanan oturum sayısı.|  
|oturumları kimlikleri|Dize dizisi|Evet|Oturum durum kodu 200 ise kimlikleri dizisi.|  
  
## <a name="rule-operations"></a>Kuralı işlemleri  
  
### <a name="add-rule"></a>Kural Ekle  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlemi|string|Evet|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural adı|string|Evet|Kural adı, abonelik ve konu adları dahil edilmez.|  
|rule-description|eşleme|Evet|Sonraki bölümde belirtildiği gibi kural açıklaması.|  
  
**Kural açıklaması** eşlemesi, aşağıdaki girişleri içermelidir nerede **sql filtresi** ve **bağıntı filtresi** karşılıklı olarak birbirini dışlar:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sql-filter|eşleme|Evet|`sql-filter`, sonraki bölümde belirtildiği gibi.|  
|Bağıntı filtresi|eşleme|Evet|`correlation-filter`, sonraki bölümde belirtildiği gibi.|  
|sql-rule-action|eşleme|Evet|`sql-rule-action`, sonraki bölümde belirtildiği gibi.|  
  
Sql filtresi eşleme aşağıdaki girdileri şunları içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|ifade|string|Evet|SQL filtre ifadesi.|  
  
**Bağıntı filtresi** harita aşağıdaki girdileri en az birini içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Bağıntı Kimliği|string|Hayır||  
|ileti kimliği|string|Hayır||  
|-|string|Hayır||  
|Yanıtla|string|Hayır||  
|Etiket|string|Hayır||  
|oturum kimliği|string|Hayır||  
|yanıt için oturum kimliği|string|Hayır||  
|içerik türü|string|Hayır||  
|properties|eşleme|Hayır|Hizmet veri yolu eşlemelerini [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
**Sql kural eylemi** eşlemesi, aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|ifade|string|Evet|SQL eylem ifade.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
### <a name="remove-rule"></a>Kuralı Kaldır  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlemi|string|Evet|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural adı|string|Evet|Kural adı, abonelik ve konu adları dahil edilmez.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
## <a name="deferred-message-operations"></a>Ertelenmiş ileti işlemleri  
  
### <a name="receive-by-sequence-number"></a>Seri numarasına göre alma  

Sıra numarası tarafından ertelenmiş iletilerini alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlemi|string|Evet|`com.microsoft:receive-by-sequence-number`|  
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
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|StatusDescription|string|Hayır|Durum açıklaması.|  
  
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

Ertelenmiş iletileri değerlendirme durumunu güncelleştirir.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|işlemi|string|Evet|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden işlem sunucusu zaman aşımı.|  
  
İstek ileti gövdesi oluşması gerekir bir **amqp değeri** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Değerlendirme durumu|string|Evet|tamamlandı<br /><br /> abandoned<br /><br /> askıya alındı|  
|Kilit belirteçleri|UUID dizisi|Evet|Değerlendirme durumunu güncelleştirmek için kilit belirteçleri ileti.|  
|deadletter-reason|string|Hayır|Değerlendirme durumu ayarlanmışsa ayarlanabilir **askıya**.|  
|deadletter-description|string|Hayır|Değerlendirme durumu ayarlanmışsa ayarlanabilir **askıya**.|  
|properties-to-modify|eşleme|Hayır|Hizmet veri yolu listesini değiştirmek için ileti özellikleri aracılı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri de eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde başarısız oldu|  
|StatusDescription|string|Hayır|Durum açıklaması.|

## <a name="next-steps"></a>Sonraki adımlar

AMQP ve Service Bus hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Hizmet veri yolu AMQP genel bakış]
* [AMQP 1.0 protokol kılavuzu]
* [Windows Server için hizmet veri yolu AMQP]

[Hizmet veri yolu AMQP genel bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md
[Windows Server için hizmet veri yolu AMQP]: https://msdn.microsoft.com/library/dn574799.asp