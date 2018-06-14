---
title: Azure geçiş kimlik doğrulama ve yetkilendirme | Microsoft Docs
description: Azure geçişi, paylaşılan erişim imzası (SAS) kimlik doğrulamasına genel bakış
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 86a9cf2c1106180ba5c8c65849042784bfd2afcd
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28018126"
---
# <a name="azure-relay-authentication-and-authorization"></a>Azure geçiş kimlik doğrulama ve yetkilendirme

Uygulamaları Azure geçişi paylaşılan erişim imzası (SAS) kimlik doğrulamasını kullanarak kimlik doğrulaması yapabilir. SAS kimlik doğrulamasını geçiş ad alanında yapılandırılmış bir erişim anahtarı kullanarak Azure geçiş hizmeti kimlik doğrulaması uygulamaları etkinleştirir. Bu anahtar ardından istemcilerin geçiş hizmeti için kimlik doğrulaması yapmak için kullanabileceği bir paylaşılan erişim imzası belirteci üretmek için de kullanabilirsiniz.

## <a name="shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması

[SAS kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) belirli haklar ile Azure geçiş kaynaklarına kullanıcı erişimi sağlar. SAS kimlik doğrulaması bir şifreleme anahtarıyla ilişkili haklar bir kaynakta yapılandırılmasını içerir. İstemciler daha sonra bu kaynak için Kaynak URI erişilen oluşan bir SAS belirteci sunarak erişebilir ve bir süre sonu yapılandırılmış anahtarı ile imzalanmış.

Bir geçiş ad için SAS anahtarları yapılandırabilirsiniz. Service Bus Mesajlaşma, aksine [geçişi karma bağlantılar](relay-hybrid-connections-protocol.md) yetkisiz veya anonim göndericiler destekler. Oluşturduğunuzda portaldan aşağıdaki ekran görüntüsü gösterildiği gibi varlık için anonim erişimi etkinleştirebilirsiniz:

![][0]

SAS kullanmak için yapılandırabileceğiniz bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) aşağıdaki oluşur geçiş ad alanı nesnede:

* *KeyName* kuralı tanımlar.
* *PrimaryKey* oturum/SAS belirteçleri doğrulamak için kullanılan bir şifreleme anahtarıdır.
* *SecondaryKey* oturum/SAS belirteçleri doğrulamak için kullanılan bir şifreleme anahtarıdır.
* *Hakları* dinleme koleksiyonunu temsil eden, Gönder veya haklar verildi yönetin.

Ad alanı düzeyinde yapılandırılan yetkilendirme kuralları, karşılık gelen anahtar kullanılarak imzalanmış belirteçleri ile istemciler için bir ad alanı'ndaki tüm geçiş bağlantılar erişim izni verebilir. En fazla 12 böyle yetkilendirme kuralları bir geçiş ad yapılandırılabilir. Varsayılan olarak, bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ilk sağlandığında tüm haklara sahip her ad alanı için yapılandırılır.

Bir varlık erişmek için istemci belirli bir kullanılarak oluşturulan bir SAS belirteci gerektirir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). SAS belirteci, yetkilendirme kuralı ile ilişkili bir şifreleme anahtarı ile erişim talep kaynak URI'sini içeren bir kaynak dize ve bir süre sonu HMAC SHA256 kullanılarak oluşturulur.

Azure .NET SDK'sı sürüm 2.0 ve sonraki sürümlerinde Azure geçişi için SAS kimlik doğrulama desteği. SAS desteği içeren bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Bir bağlantı dizesi parametresi olarak kabul tüm API'leri SAS bağlantı dizeleri için destek içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma devam [paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) SAS hakkında daha fazla ayrıntı için.
- Bkz: [Azure geçişi karma bağlantılar Protokolü Kılavuzu](relay-hybrid-connections-protocol.md) karma bağlantılar özelliği hakkında ayrıntılı bilgi için.
- Service Bus Mesajlaşma hizmeti kimlik doğrulama ve yetkilendirme hakkında ilgili bilgi için bkz: [Service Bus kimlik doğrulama ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md). 

[0]: ./media/relay-authentication-and-authorization/hcanon.png