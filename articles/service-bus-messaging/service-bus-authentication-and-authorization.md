---
title: Azure Service Bus kimlik doğrulama ve yetkilendirme | Microsoft Docs
description: Service Bus uygulamalara paylaşılan erişim imzası (SAS) kimlik doğrulaması ile kimlik doğrulaması.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 7c5a45504b7c44d97ff2250663ef9c47ef6e3595
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60714515"
---
# <a name="service-bus-authentication-and-authorization"></a>Service Bus kimlik doğrulaması ve yetkilendirme

Uygulamalar, paylaşılan erişim imzası (SAS) belirteci kimlik doğrulamasını kullanarak Azure Service Bus kaynaklarına erişin. SAS ile uygulamaları bir belirteç Service Bus için imzalanmış bir simetrik anahtar belirteci veren her ikisi de bilinen ve Service Bus'ın (Bu nedenle "paylaşılan") ile sunmak ve bu anahtardır doğrudan izni gibi belirli erişim hakları veren bir kural ile ilişkili Alma/dinleme veya ileti gönderme. SAS ya da ad alanı veya doğrudan bir kuyruk veya konuda, ayrıntılı erişim denetimi için izin verme gibi varlıkları yapılandırılmış kurallardır.

SAS belirteçleri ya da bir Service Bus istemci tarafından doğrudan oluşturulabilir veya bazı Ara belirteci veren uç noktası ile etkileşime giren istemci tarafından oluşturulabilir. Örneğin, bir sistem kimliğini ve sistem erişim hakları ve web hizmeti sonra döndürür uygun Service Bus belirteci kanıtlamak için bir Active Directory yetkilendirme korumalı web hizmeti uç noktasını çağırmak istemci gerektirebilir. Bu SAS belirteci, Azure SDK'da bulunan Service Bus belirteci sağlayıcısı kullanılarak kolayca oluşturulabilir. 

> [!IMPORTANT]
> Service Bus ile Azure Active Directory erişim denetimi (Access Control Service veya ACS olarak da bilinir) kullanıyorsanız, sınırlı ve SAS'ı kullanmak için uygulamanızı geçirmelisiniz artık bu yöntem için sağlanan destek olduğunu unutmayın. Daha fazla bilgi için [bu blog gönderisini](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/) ve [bu makalede](service-bus-migrate-acs-sas.md).

## <a name="shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması

[SAS kimlik doğrulaması](service-bus-sas.md) belirli haklar ile Service Bus kaynakları için kullanıcı erişim sağlar. Service Bus SAS kimlik doğrulaması, bir şifreleme anahtarıyla ilişkili bir Service Bus kaynak hakları yapılandırılmasını içerir. İstemciler bu kaynak için Kaynak URI erişilen oluşan bir SAS belirteci sunarak erişebilir ve bir süre sonu yapılandırılmış anahtarla imzalanmış.

Service Bus ad alanınızdaki için SAS anahtarları yapılandırabilirsiniz. Anahtarı, bu ad alanındaki tüm Mesajlaşma varlıkları için geçerlidir. Anahtarlar, Service Bus kuyrukları ve konuları üzerinde de yapılandırabilirsiniz. SAS üzerinde desteklenen ayrıca [Azure geçişi](../service-bus-relay/relay-authentication-and-authorization.md).

SAS'ı kullanmak için yapılandırabileceğiniz bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ad alanı, kuyruk veya konuda nesne. Bu kural aşağıdaki öğelerden oluşur:

* *KeyName*: kural tanımlar.
* *PrimaryKey*: oturum/SAS belirteçleri doğrulamak için kullanılan bir şifreleme anahtarı.
* *İkincil anahtarı oluşturma*: oturum/SAS belirteçleri doğrulamak için kullanılan bir şifreleme anahtarı.
* *Hakları*: koleksiyonunu temsil eder **dinleme**, **Gönder**, veya **Yönet** haklar verildi.

Ad alanı düzeyinde yapılandırılan yetkilendirme kuralları, karşılık gelen anahtar kullanılarak imzalanmış belirteçleri ile istemciler için bir ad alanındaki tüm varlıklara erişim izni verebilirsiniz. Bir Service Bus ad alanı, kuyruk veya konuda en çok 12 yetkilendirme kuralları yapılandırabilirsiniz. Varsayılan olarak, bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ilk sağlandığında tüm haklara sahip her ad alanı için yapılandırılır.

Bir varlık erişmek için belirli bir kullanılarak oluşturulan bir SAS belirteci istemcinin ihtiyaç duyduğu [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). SAS belirteci, Kaynak URI erişim talep edildikten oluşan bir kaynak dizesi ve bir süre sonu HMAC SHA256 yetkilendirme kuralıyla ilişkili bir şifreleme anahtarı kullanarak oluşturulur.

Service Bus için SAS kimlik doğrulaması desteği, dahil edilen Azure .NET SDK'sı sürüm 2.0 ve sonrasında mevcuttur. SAS için destek içerir bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Bir bağlantı dizesi bir parametre olarak kabul tüm API'leri SAS bağlantı dizeleri için destek içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Okumaya devam [paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması](service-bus-sas.md) SAS hakkında daha fazla ayrıntı için.
- Nasıl yapılır [Azure Active Directory erişim denetimi (ACS) için paylaşılan erişim imzası yetkilendirme geçirme](service-bus-migrate-acs-sas.md).
- [Değişiklikleri ACS etkin ad alanları](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).
- İlgili Azure geçişi kimlik doğrulaması ve yetkilendirme hakkında bilgi için [Azure geçişi kimlik doğrulaması ve yetkilendirme](../service-bus-relay/relay-authentication-and-authorization.md). 

