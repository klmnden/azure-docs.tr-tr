---
title: AMQP 1.0 istek-yanıt tabanlı işlemler Azure Service Bus | Microsoft Docs
description: Microsoft Azure Service Bus istek/yanıt tabanlı işlemler listesi.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: c22ba0b57ed1161e1f7e2082d2ba21f27b656da1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60402692"
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>Microsoft Azure hizmet veri yolu AMQP 1.0: istek-yanıt tabanlı işlemler

Bu makalede, Microsoft Azure Service Bus istek/yanıt tabanlı işlemler listesini tanımlar. Bu bilgiler AMQP yönetim sürüm 1.0 çalışma taslak üzerinde temel alır.  
  
Service Bus nasıl uygular ve OASIS AMQP teknik belirtimi derlemeler açıklayan, ayrıntılı Hat düzeyinde AMQP 1.0 protokolü kılavuzu için bkz: [AMQP 1.0 Azure Service Bus ve Event Hubs Protokolü Kılavuzu'nda][amqp 1.0 protokol kılavuzu].  
  
## <a name="concepts"></a>Kavramlar  
  
### <a name="entity-description"></a>Varlık açıklaması  

Varlık açıklaması ya da bir Service Bus başvuruyor [QueueDescription sınıfı](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [TopicDescription sınıfı](/dotnet/api/microsoft.servicebus.messaging.topicdescription), veya [SubscriptionDescription sınıfı](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) nesne.  
  
### <a name="brokered-message"></a>Aracılı ileti  

Hizmet veri yolu AMQP iletisine eşleşen bir iletiyi temsil eder. Eşleme tanımlanan [hizmet veri yolu AMQP protokol Kılavuzu](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-to-entity-management-node"></a>Varlık Yönetimi düğümüne ekleme  

Bu belgede açıklanan tüm işlemler istek/yanıt deseni izler, bir varlığa kapsamlı ve bir varlık yönetimi düğümüne eklenmesini gerektirir.  
  
### <a name="create-link-for-sending-requests"></a>İstekleri göndermek için bağlantı oluşturma  

Yönetim düğümü istekleri göndermek için bir bağlantı oluşturur.  
  
```
requestLink = session.attach(
role: SENDER,
    target: { address: "<entity address>/$management" },
    source: { address: ""<my request link unique address>" }
)

```
  
### <a name="create-link-for-receiving-responses"></a>Yanıtlar almak için bağlantı oluşturma  

Yönetim düğümünden yanıtlar almak için bir bağlantı oluşturur.  
  
```
responseLink = session.attach(
role: RECEIVER,
    source: { address: "<entity address>/$management" }
    target: { address: "<my response link unique address>" }
)

```
  
### <a name="transfer-a-request-message"></a>Aktarım İsteği iletisi  

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
  
### <a name="receive-a-response-message"></a>Bir yanıt iletisi  

Yanıt bağlantıdan yanıt iletisini alır.  
  
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

Service Bus varlıklarına şu şekilde ele alınması gerekir:  
  
|varlık türü|Adres|Örnek|  
|-----------------|-------------|-------------|  
|queue|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|topic|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|aboneliği|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>İleti işlemleri  
  
### <a name="message-renew-lock"></a>İleti kilidi yenileme  

Varlık açıklamasında belirtilen süreye göre bir iletinin kilit genişletir.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
 İstek iletisi gövdesi aşağıdaki girişlerle eşleme içeren bir amqp değeri bölümü oluşması gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|uuid dizisi|Evet|Yenilemek için ileti kilidi belirteçleri.|  

> [!NOTE]
> Kilit belirteçleri `DeliveryTag` alınan iletiler özelliği. Aşağıdaki örneğe bakın [.NET SDK'sı](https://github.com/Azure/azure-service-bus-dotnet/blob/6f144e91310dcc7bd37aba4e8aebd535d13fa31a/src/Microsoft.Azure.ServiceBus/Amqp/AmqpMessageConverter.cs#L336) Bu alan. Belirteç de görünebilir 'DeliveryAnnotations' 'x-opt-kilit-belirteci ' ancak, bu kesin değildir ve `DeliveryTag` tercih edilmelidir. 
> 
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu.|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi aşağıdaki girişlerle eşleme içeren bir amqp değeri bölümü oluşması gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|süre sonu|zaman damgası dizisi|Evet|İstek kilit belirteçleri karşılık gelen ileti kilit belirteci yeni süre sonu.|  
  
### <a name="peek-message"></a>İletiye Gözat  

Kilitlemeden iletileri göz atar.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|long|Evet|Sıra numarası gözlem başlayacağı.|  
|`message-count`|int|Evet|En fazla göz atmak için ileti sayısı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti yok<br /><br /> 204: Hayır içerik – daha fazla ileti yok|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her harita iletiyi temsil eden iletilerinin listesi.|  
  
Temsil eden bir ileti eşlemesi, aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|AMQP 1.0 kablo ile kodlanmış ileti.|  
  
### <a name="schedule-message"></a>Zamanlama iletisi  

İletileri zamanlar. Bu işlem, işlem destekler.
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her harita iletiyi temsil eden iletilerinin listesi.|  
  
Temsil eden bir ileti eşlemesi, aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|ileti kimliği|string|Evet|`amqpMessage.Properties.MessageId` dize olarak|  
|oturum kimliği|string|Hayır|`amqpMessage.Properties.GroupId as string`|  
|Bölüm anahtarı|string|Hayır|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|
|aracılığıyla-bölüm anahtarı|string|Hayır|`amqpMessage.MessageAnnotations."x-opt-via-partition-key"`|
|message|bayt dizisi|Evet|AMQP 1.0 kablo ile kodlanmış ileti.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu.|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi oluşması gerekir bir **amqp değer** aşağıdaki girdileri içeren bir harita içeren bölümü:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|long dizisi|Evet|Zamanlanan mesajlar dizisi sayısı. Sıra numarası, iptal etmek için kullanılır.|  
  
### <a name="cancel-scheduled-message"></a>Zamanlanmış iletileri iptal et  

Zamanlanmış iletileri iptal eder.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|long dizisi|Evet|İptal etmek için zamanlanmış ileti sıra numarası.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu.|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi oluşması gerekir bir **amqp değer** aşağıdaki girdileri içeren bir harita içeren bölümü:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|long dizisi|Evet|Zamanlanan mesajlar dizisi sayısı. Sıra numarası, iptal etmek için kullanılır.|  
  
## <a name="session-operations"></a>Oturum işlemleri  
  
### <a name="session-renew-lock"></a>Oturum kilidi yenileme  

Varlık açıklamasında belirtilen süreye göre bir iletinin kilit genişletir.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti yok<br /><br /> 204: Hayır içerik – daha fazla ileti yok|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi oluşması gerekir bir **amqp değer** aşağıdaki girdileri içeren bir harita içeren bölümü:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|süre sonu|timestamp|Evet|Yeni süre sonu.|  
  
### <a name="peek-session-message"></a>Oturum İletiye Gözat  

Kilitlemeden oturumu iletileri atar.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|gelen dizisi-sayı|long|Evet|Sıra numarası gözlem başlayacağı.|  
|ileti sayısı|int|Evet|En fazla göz atmak için ileti sayısı.|  
|oturum kimliği|string|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti yok<br /><br /> 204: Hayır içerik – daha fazla ileti yok|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi oluşması gerekir bir **amqp değer** aşağıdaki girdileri içeren bir harita içeren bölümü:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her harita iletiyi temsil eden iletilerinin listesi.|  
  
 Temsil eden bir ileti eşlemesi, aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|AMQP 1.0 kablo ile kodlanmış ileti.|  
  
### <a name="set-session-state"></a>Küme oturum durumu  

Oturum durumunu ayarlar.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:set-session-state`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum kimliği|  
|oturum durumu|bir bayt dizisi|Evet|Donuk ikili veriler.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
### <a name="get-session-state"></a>Oturum durumunu alma  

Oturum durumunu alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum kimliği|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum durumu|bir bayt dizisi|Evet|Donuk ikili veriler.|  
  
### <a name="enumerate-sessions"></a>Oturumlarının listeleme  

Bir Mesajlaşma varlığı oturumları numaralandırır.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Son güncelleştirme saati|timestamp|Evet|Yalnızca belirli bir süre sonra güncelleştirilmiş oturumları içerecek şekilde filtreleyin.|  
|Atla|int|Evet|Oturumlarının sayısını atlayın.|  
|Sayfanın Üstü|int|Evet|Oturumlarının sayısı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti yok<br /><br /> 204: Hayır içerik – daha fazla ileti yok|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Atla|int|Evet|Durum kodu 200 ise Atlanan oturum sayısı.|  
|oturumlarının kimlikleri|dize dizisi|Evet|Oturum durum kodu 200 ise kimlikleri dizisi.|  
  
## <a name="rule-operations"></a>Kural işlemleri  
  
### <a name="add-rule"></a>Kural Ekle  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural adı|string|Evet|Kural adı, abonelik ve konu adı dahil değil.|  
|Kural açıklaması|map|Evet|Sonraki bölümde belirtilen kural açıklaması.|  
  
**Kural açıklaması** harita, aşağıdaki girişleri içermelidir burada **sql filtresi** ve **bağıntı filtresi** karşılıklı olarak birbirini dışlar:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|SQL filtresi|map|Evet|`sql-filter`, sonraki bölümde belirtildiği gibi.|  
|Bağıntı filtresi|map|Evet|`correlation-filter`, sonraki bölümde belirtildiği gibi.|  
|SQL kural eylemi|map|Evet|`sql-rule-action`, sonraki bölümde belirtildiği gibi.|  
  
Sql filtresi eşlemesi aşağıdaki girdileri şunları içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İfade|string|Evet|SQL filtresi ifadesi.|  
  
**Bağıntı filtresi** eşlemesi aşağıdaki girdileri en az birini içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Bağıntı Kimliği|string|Hayır||  
|ileti kimliği|string|Hayır||  
|-|string|Hayır||  
|Yanıtla|string|Hayır||  
|label|string|Hayır||  
|oturum kimliği|string|Hayır||  
|yanıt için oturum kimliği|string|Hayır||  
|içerik türü|string|Hayır||  
|properties|map|Hayır|Service Bus haritalar [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).|  
  
**Sql kural eylemi** harita, aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İfade|string|Evet|SQL eylem ifadesi.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
### <a name="remove-rule"></a>Kuralı Kaldır  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural adı|string|Evet|Kural adı, abonelik ve konu adı dahil değil.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
### <a name="get-rules"></a>Kuralları alma

#### <a name="request"></a>İstek

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:

|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:enumerate-rules`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  

İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Sayfanın Üstü|int|Evet|Sayfanın getirilecek kuralları sayısı.|  
|Atla|int|Evet|Atlamak için kuralları sayısı. Başlangıç dizini (+ 1) üzerinde kurallarının listesini tanımlar. | 

#### <a name="response"></a>Yanıt

Yanıt iletisi, aşağıdaki özellikleri içerir:

|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu|  
|rules| Harita dizisi|Evet|Kuralları dizisi. Her kural, bir eşlemesi tarafından temsil edilir.|

Dizideki her eşleme girişi aşağıdaki özellikleri içerir:

|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural açıklaması|açıklanan nesneler dizisi|Evet|`com.microsoft:rule-description:list` AMQP ile kod 0x0000013700000004 açıklanan| 

`com.microsoft.rule-description:list` açıklanan nesneler dizisidir. Dizi aşağıdakileri içerir:

|Dizin oluşturma|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | açıklanan nesneler dizisi | Evet | `filter` Aşağıda belirtildiği gibi. |
| 1 | açıklandığı gibi bir nesne dizisi | Evet | `ruleAction` Aşağıda belirtildiği gibi. |
| 2 | string | Evet | kuralın adı. |

`filter` şu türlerden birini olabilir:

| Tanımlayıcı adı | Tanımlayıcı kodu | Değer |
| --- | --- | ---|
| `com.microsoft:sql-filter:list` | 0x000001370000006 | SQL filtresi |
| `com.microsoft:correlation-filter:list` | 0x000001370000009 | Bağıntı filtresi |
| `com.microsoft:true-filter:list` | 0x000001370000007 | 1 = 1 gösteren filtre |
| `com.microsoft:false-filter:list` | 0x000001370000008 | 1 = 0'ı temsil eden false filtresi |

`com.microsoft:sql-filter:list` içeren açıklandığı gibi bir dizi şöyledir:

|Dizin oluşturma|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | string | Evet | SQL filtresi ifadesi |

`com.microsoft:correlation-filter:list` içeren açıklandığı gibi bir dizi şöyledir:

|Dizin (varsa var)|Değer türü|Değer içeriği|  
|---------|----------------|--------------|
| 0 | string | Bağıntı Kimliği |
| 1 | string | İleti kimliği |
| 2 | string | Bitiş |
| 3 | string | Yanıtla |
| 4 | string | Etiket |
| 5 | string | Oturum Kimliği |
| 6 | string | Yanıtlanacak oturum kimliği|
| 7 | string | İçerik Türü |
| 8 | Eşleme | Uygulama haritasını tanımlanan özellikler |

`ruleAction` şu türlerden biri olabilir:

| Tanımlayıcı adı | Tanımlayıcı kodu | Değer |
| --- | --- | ---|
| `com.microsoft:empty-rule-action:list` | 0x0000013700000005 | Boş kural eylemi - mevcut hiçbir kural eylemi |
| `com.microsoft:sql-rule-action:list` | 0x0000013700000006 | SQL kural eylemi |

`com.microsoft:sql-rule-action:list` SQL kural eylemin ifadesi içeren bir dize, ilk girdidir açıklandığı gibi nesneleri dizisidir.

## <a name="deferred-message-operations"></a>Ertelenmiş ileti işlemleri  
  
### <a name="receive-by-sequence-number"></a>Seri numarasına göre alma  

Sıra numarası tarafından ertelenmiş ileti alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|long dizisi|Evet|Sıra numaraları.|  
|receiver-settle-mode|ubyte|Evet|**Alıcı kapatma** AMQP çekirdek v1.0 belirtildiği gibi modu.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|  
  
Yanıt iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|eşlemeleri listesi|Evet|Her harita iletiyi temsil ettiği ileti listesi.|  
  
Temsil eden bir ileti eşlemesi, aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Kilit belirteci|uuid|Evet|Kilit belirteci if `receiver-settle-mode` 1'dir.|  
|message|bayt dizisi|Evet|AMQP 1.0 kablo ile kodlanmış ileti.|  
  
### <a name="update-disposition-status"></a>Güncelleştirme değerlendirme durumu  

Ertelenmiş ileti değerlendirme durumunu güncelleştirir. Bu işlem, işlemleri destekler.
  
#### <a name="request"></a>İstek  

İstek iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|İşlemi|string|Evet|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu milisaniye cinsinden zaman aşımı.|  
  
İstek iletisi gövdesi oluşması gerekir bir **amqp değer** bölüm içeren bir **harita** aşağıdaki girişleri:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|Değerlendirme durumu|string|Evet|Tamamlandı<br /><br /> Yayını bırakıldı<br /><br /> Askıya alındı|  
|Kilit belirteçleri|uuid dizisi|Evet|Değerlendirme durumu güncelleştirmek için ileti kilidi belirteçleri.|  
|Teslim edilemeyen iletiler açıklaması|string|Hayır|Değerlendirme durumu ayarlanırsa ayarlanabilir **askıya**.|  
|Teslim edilemeyen iletiler açıklaması|string|Hayır|Değerlendirme durumu ayarlanırsa ayarlanabilir **askıya**.|  
|değiştirilecek özellikleri|map|Hayır|Service Bus listesi, ileti özelliklerini değiştirmek için aracılı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi, aşağıdaki uygulama özellikleri eklemeniz gerekir:  
  
|Anahtar|Değer türü|Gerekli|Değer içeriği|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Evet|HTTP yanıt kodunu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi takdirde'başarısız oldu|  
|Durumaçıklaması|string|Hayır|Durum açıklaması.|

## <a name="next-steps"></a>Sonraki adımlar

AMQP ve Service Bus hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Hizmet veri yolu AMQP genel bakış]
* [AMQP 1.0 protokol kılavuzu]
* [Windows Server için hizmet veri yolu AMQP]

[Hizmet veri yolu AMQP genel bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md
[Windows Server için hizmet veri yolu AMQP]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)
