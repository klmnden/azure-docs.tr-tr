---
title: Azure hizmet veri yolu kimlik doğrulama ve yetkilendirme | Microsoft Docs
description: Hizmet veri yolu uygulamaları paylaşılan erişim imzası (SAS) kimlik doğrulaması ile kimlik doğrulaması.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: sethm
ms.openlocfilehash: b8b5887f2003dd793ae7a50f066b893f685002a0
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
ms.locfileid: "24813938"
---
# <a name="service-bus-authentication-and-authorization"></a>Service Bus kimlik doğrulaması ve yetkilendirme

Uygulamaları paylaşılan erişim imzası (SAS) belirteci kimlik doğrulaması kullanarak Azure Service Bus kaynaklarına erişim kazanır. SAS ile uygulamaların bir belirteç Service Bus için imzalanmış bir belirteç verenin hem de bilinen simetrik anahtar ve Service Bus (Bu nedenle "paylaşılan") ile sunmak ve bu anahtarıdır doğrudan izni gibi belirli erişim hakkı verme kuralı ile ilişkili Alma dinleme veya ileti gönderin. SAS ya da ad alanı veya doğrudan bir kuyruk veya konu, hassas erişim denetimi için izin verme gibi varlıkları yapılandırılmış kurallardır.

SAS belirteci ya da hizmet veri yolu istemci tarafından doğrudan oluşturulabilir veya bazı Ara belirteç uç noktası ile etkileşime giren istemci veren tarafından oluşturulabilir. Örneğin, bir sistem kimliğini ve sistem erişim hakları ve web hizmeti sonra döndürür uygun Service Bus belirteci kanıtlamak için bir Active Directory yetkilendirme korumalı web hizmeti uç noktası çağırmak istemci gerektirebilir. Bu SAS belirteci kolayca Azure SDK'da bulunan Service Bus belirteç sağlayıcısı kullanılarak oluşturulabilir. 

> [!IMPORTANT]
> Service Bus ile Azure Active Directory erişim denetimi (erişim denetimi hizmeti veya ACS olarak da bilinir) kullanıyorsanız, sınırlı ve uygulamanızı SAS'ı kullanacak şekilde geçirmelisiniz artık bu yöntem için destek olduğuna dikkat edin. Daha fazla bilgi için bkz: [bu blog gönderisine](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/) ve [bu makalede](service-bus-migrate-acs-sas.md).

## <a name="shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması

[SAS kimlik doğrulaması](service-bus-sas.md) belirli haklar ile Service Bus kaynaklarına kullanıcı erişimi sağlar. Hizmet veri yolu SAS kimlik doğrulaması bir şifreleme anahtarıyla ilişkili haklar bir Service Bus kaynakta yapılandırılmasını içerir. İstemciler daha sonra bu kaynak için Kaynak URI erişilen oluşan bir SAS belirteci sunarak erişebilir ve bir süre sonu yapılandırılmış anahtarı ile imzalanmış.

Üzerinde bir hizmet veri yolu ad alanı için SAS anahtarları yapılandırabilirsiniz. Bu ad alanındaki tüm Mesajlaşma varlıkları anahtar uygular. Anahtarları aynı zamanda, Service Bus kuyrukları ve konuları da yapılandırabilirsiniz. SAS üzerinde desteklenen de [Azure geçiş](../service-bus-relay/relay-authentication-and-authorization.md).

SAS kullanmak için yapılandırabileceğiniz bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ad alanı, kuyruk veya konu nesnesi. Bu kural aşağıdaki öğelerden oluşur:

* *KeyName*: kuralı tanımlar.
* *PrimaryKey*: oturum/SAS belirteçleri doğrulamak için kullanılan bir şifreleme anahtarı.
* *SecondaryKey*: oturum/SAS belirteçleri doğrulamak için kullanılan bir şifreleme anahtarı.
* *Hakları*: koleksiyonunu temsil eder **dinleme**, **Gönder**, veya **Yönet** haklar verildi.

Ad alanı düzeyinde yapılandırılan yetkilendirme kuralları, karşılık gelen anahtar kullanılarak imzalanmış belirteçleri ile istemciler için bir ad alanındaki tüm varlıklara erişim izni verebilir. Bir hizmet veri yolu ad alanı, kuyruk veya konu gibi en fazla 12 yetkilendirme kuralları yapılandırabilirsiniz. Varsayılan olarak, bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ilk sağlandığında tüm haklara sahip her ad alanı için yapılandırılır.

Bir varlık erişmek için istemci belirli bir kullanılarak oluşturulan bir SAS belirteci gerektirir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). SAS belirteci, yetkilendirme kuralı ile ilişkili bir şifreleme anahtarı ile erişim talep kaynak URI'sini içeren bir kaynak dize ve bir süre sonu HMAC SHA256 kullanılarak oluşturulur.

Hizmet veri yolu için SAS kimlik doğrulama desteği dahil Azure .NET SDK'sı sürüm 2.0 ve üzeri. SAS desteği içeren bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Bir bağlantı dizesi parametresi olarak kabul tüm API'leri SAS bağlantı dizeleri için destek içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma devam [paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması](service-bus-sas.md) SAS hakkında daha fazla ayrıntı için.
- Nasıl yapılır [Azure Active Directory erişim denetimi (ACS) için paylaşılan erişim imzası yetkilendirme geçirmek](service-bus-migrate-acs-sas.md).
- [Değişiklikleri ACS etkinleştirilmiş ad alanlarını](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).
- Karşılık gelen Azure geçiş kimlik doğrulama ve yetkilendirme hakkında bilgi için [Azure geçiş kimlik doğrulama ve yetkilendirme](../service-bus-relay/relay-authentication-and-authorization.md). 

