---
title: Azure geçişi kimlik doğrulaması ve yetkilendirme | Microsoft Docs
description: Azure geçişi, paylaşılan erişim imzası (SAS) kimlik doğrulamasına genel bakış
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: spelluru
ms.openlocfilehash: 206cca95c590a01f69d3664fb87398bc2fcb4ad9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60595504"
---
# <a name="azure-relay-authentication-and-authorization"></a>Azure geçişi kimlik doğrulaması ve yetkilendirme

Uygulamaları, Azure geçişi paylaşılan erişim imzası (SAS) kimlik doğrulamasını kullanarak kimlik doğrulaması yapabilir. SAS kimlik doğrulaması, Azure geçişi hizmetini kullanarak geçiş ad alanı üzerinde yapılandırılmış bir erişim anahtarı kimlik doğrulaması uygulamaları etkinleştirir. Ardından, istemcilerin alan geçiş hizmetine kimliğini doğrulamak için kullanabileceği bir paylaşılan erişim imzası belirtecini oluşturmak için bu anahtarı kullanabilirsiniz.

## <a name="shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması

[SAS kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) Azure geçişi kaynakları belirli haklarına sahip bir kullanıcı erişim sağlar. SAS kimlik doğrulaması, bir şifreleme anahtarıyla ilişkili haklar kaynakta yapılandırılmasını içerir. İstemciler bu kaynak için Kaynak URI erişilen oluşan bir SAS belirteci sunarak erişebilir ve bir süre sonu yapılandırılmış anahtarla imzalanmış.

Bir geçiş ad alanı üzerinde için SAS anahtarları yapılandırabilirsiniz. Service Bus Mesajlaşma, aksine [geçişi karma bağlantıları](relay-hybrid-connections-protocol.md) yetkisiz veya anonim gönderenlerin destekler. Oluşturduğunuzda, varlık için anonim erişim Portalı'ndan aşağıdaki ekran görüntüsünde gösterilen şekilde etkinleştirebilirsiniz:

![][0]

SAS'ı kullanmak için yapılandırabileceğiniz bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesne aşağıdakilerden oluşan bir geçiş ad alanı üzerinde:

* *KeyName* kural tanımlar.
* *PrimaryKey* oturum/SAS belirteçleri doğrulamak için kullanılan bir şifreleme anahtarıdır.
* *İkincil anahtarı oluşturma* oturum/SAS belirteçleri doğrulamak için kullanılan bir şifreleme anahtarıdır.
* *Hakları* dinleme koleksiyonunu temsil eden, gönderin veya haklar verildi yönetin.

Ad alanı düzeyinde yapılandırılan yetkilendirme kuralları, karşılık gelen anahtar kullanılarak imzalanmış belirteçleri ile tüm geçiş bağlantıları ad alanında istemciler için erişim verebilirsiniz. En fazla 12 gibi yetkilendirme kuralları, bir geçiş ad alanı üzerinde yapılandırılabilir. Varsayılan olarak, bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ilk sağlandığında tüm haklara sahip her ad alanı için yapılandırılır.

Bir varlık erişmek için belirli bir kullanılarak oluşturulan bir SAS belirteci istemcinin ihtiyaç duyduğu [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). SAS belirteci, Kaynak URI erişim talep edildikten oluşan bir kaynak dizesi ve bir süre sonu HMAC SHA256 yetkilendirme kuralıyla ilişkili bir şifreleme anahtarı kullanarak oluşturulur.

Azure geçişi için SAS kimlik doğrulaması desteği dahil edilen Azure .NET SDK'sı sürüm 2.0 ve sonrasında mevcuttur. SAS için destek içerir bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Bir bağlantı dizesi bir parametre olarak kabul tüm API'leri SAS bağlantı dizeleri için destek içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Okumaya devam [paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) SAS hakkında daha fazla ayrıntı için.
- Bkz: [Azure geçiş karma bağlantıları protokol Kılavuzu](relay-hybrid-connections-protocol.md) karma bağlantılar özelliği hakkında ayrıntılı bilgi için.
- Service Bus Mesajlaşma hizmeti kimlik doğrulama ve yetkilendirme hakkında ilgili bilgi için bkz: [Service Bus kimlik doğrulama ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md). 

[0]: ./media/relay-authentication-and-authorization/hcanon.png