---
title: Azure Service Bus Role-Based erişim denetimi (RBAC) Önizleme | Microsoft Docs
description: Azure Service Bus rol tabanlı erişim denetimi
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
ms.date: 09/19/2018
ms.author: aschhab
ms.openlocfilehash: e4571a8918b7877b728b54129e47ffcf4af9b46a
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979636"
---
# <a name="active-directory-role-based-access-control-preview"></a>Etkin Directory Role-Based erişim denetimi (Önizleme)

Microsoft Azure kaynakları ve Azure Active Directory (Azure AD) tabanlı uygulamalar için tümleşik erişim denetimi yönetimi sağlar. Azure AD ile ya da kullanıcı hesaplarını yönetebilir ve uygulamaları için Azure tabanlı uygulamalar veya var olan Active Directory altyapınızı Azure kaynaklarını da kapsayan şirket çapında çoklu oturum açma için Azure AD ile federasyona eklemek ve Azure barındırılan uygulamalar. Ardından, Azure kaynaklarına erişim için genel ve hizmete özgü rollere bu Azure AD kullanıcı ve uygulama kimlikleri atayabilirsiniz.

Azure Service Bus için ad alanları ve Azure portalı üzerinden tüm ilgili kaynakları yönetimini ve Azure kaynak yönetimi API'si zaten kullanarak korumalı *rol tabanlı erişim denetimi* (RBAC) modeli. Çalışma zamanı işlemleri için RBAC özelliği artık genel Önizleme aşamasındadır.

SAS kuralları ve anahtarlar ya da hizmet veri yolu için belirli diğer herhangi bir erişim belirteçleri işlemek Azure AD RBAC kullanan bir uygulamayı gerekmez. İstemci uygulaması kimlik doğrulaması bağlamı'kurmak için Azure AD ile etkileşime geçer ve hizmet veri yolu için bir erişim belirteci alır. Etkileşimli oturum açma gerektiren etki alanı kullanıcı hesaplarını, uygulama hiçbir zaman herhangi bir kimlik bilgisi doğrudan işler.

## <a name="service-bus-roles-and-permissions"></a>Service Bus rolleri ve izinleri

Azure sağlayan yerleşik RBAC rolleri için erişimi bir Service Bus ad alanı yetkilendirme aşağıda:

* [Hizmet veri yolu veri sahibi (Önizleme)](../role-based-access-control/built-in-roles.md#service-bus-data-owner): Service Bus ad alanı ve varlıkları (kuyruklar, konular, abonelikler ve filtreleri) veri erişim sağlar.

>[!IMPORTANT]
> Biz daha önce desteklenen yönetilen kimliğe ekleme **"Sahip"** veya **"Katılımcı"** rol.
>
> Ancak, veri ayrıcalıklarına erişim **"Sahip"** ve **"Katılımcı"** rolü artık kullanılacaktır. Kullandıysanız **"Sahip"** veya **"Katılımcı"** rolünü ve ardından bu gerekecektir yazılımınız için **"Hizmet veri yolu veri sahibi"** rol.

## <a name="use-service-bus-with-an-azure-ad-domain-user-account"></a>Service Bus ile bir Azure AD etki alanı kullanıcı hesabı kullanın.

Aşağıdaki bölümde oluşturmak ve çalıştırmak için etkileşimli bir Azure ister örnek bir uygulama için gereken adımlar açıklanmaktadır. oturum açmak için AD kullanıcı, Service Bus konusu kullanıcı hesabına erişim nasıl ve Event Hubs erişmek için bu kimlik kullanma.

Basit bir konsol uygulaması, bu tanıtımda açıklanmaktadır [kodudur, Github'da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl).

### <a name="create-an-active-directory-user-account"></a>Bir Active Directory kullanıcı hesabı oluşturma

Bu ilk adım isteğe bağlıdır. Her Azure aboneliği bir Azure Active Directory kiracısı ile otomatik olarak eşleştirilir ve bir Azure aboneliğine erişiminiz varsa, kullanıcı hesabınız zaten kaydedildi. Hesabınızı hemen kullanabileceğiniz anlamına gelir.

Yine de bu senaryo için özel bir hesap oluşturmak istiyorsanız [adımları](../automation/automation-create-aduser-account.md). Daha büyük kuruluş senaryolarına yönelik durum geçerli olmayabilir ve Azure Active Directory kiracısı içinde hesapları oluşturma izniniz olmalıdır.

### <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma

Ardından, [Service Bus Mesajlaşması ad alanı oluşturma](service-bus-create-namespace-portal.md).

Ad alanı oluşturduktan sonra gidin, **erişim denetimi (IAM)** sayfasında portalda ve ardından **rol ataması Ekle** sahip rolü için Azure AD kullanıcı hesabı eklemek için. Kendi kullanıcı hesabı kullanıyorsanız ve oluşturduğunuz ad alanı, zaten sahip rolüne sahiptirler. Rolü için farklı bir hesap eklemek için web uygulamasının adını arayın **izinleri eklemek** paneli **seçin** alan ve sonra giriş'e tıklayın. Daha sonra **Kaydet**'e tıklayın.

Kullanıcı hesabı artık Service Bus ad alanı erişimi olan ve daha önce oluşturduğunuz kuyruğa.

### <a name="register-the-application"></a>Uygulamayı kaydetme

Örnek uygulamayı çalıştırmadan önce Azure AD'ye kaydetme ve kendi adına Azure Service Bus erişmesine izin veren bir onay istemi onaylayın.

Örnek uygulama bir konsol uygulaması olduğundan yerel bir uygulamayı kaydetme ve API izinlerini eklemeniz gerekir **Microsoft.ServiceBus** "gerekli izinler" kümesi. Yerel uygulamalar da gereken bir **redırect-URI** ; tanımlayıcı olarak hizmet veren Azure AD'de URI ağ hedef olması gerekmez. Kullanım `https://servicebus.microsoft.com` kod örneği olduğundan bu örnek için bu URI kullanır.

Ayrıntılı kayıt adımları açıklanmıştır [Bu öğreticide](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Kaydetmek için adımları izleyin bir **yerel** uygulama ve ardından eklemek için güncelleştirme talimatları izleyin **Microsoft.ServiceBus** API için gerekli izinleri. Bu adımları izlediğiniz gibi Not **Tenantıd** ve **ApplicationId**gibi uygulamayı çalıştırmak için bu değerlere ihtiyacınız olur.

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Örneği çalıştırmadan önce App.config dosyasını düzenleyin ve senaryonuza bağlı olarak, aşağıdaki değerleri ayarlayın:

- `tenantId`: Kümesine **Tenantıd** değeri.
- `clientId`: Kümesine **ApplicationId** değeri.
- `clientSecret`: İstemci gizli anahtarı kullanarak oturum istiyorsanız, Azure AD'de oluşturun. Ayrıca, bir web uygulaması veya API yerine yerel bir uygulama kullanın. Ayrıca, altında uygulama Ekle **erişim denetimi (IAM)** daha önce oluşturduğunuz ad alanı içinde.
- `serviceBusNamespaceFQDN`: Yeni oluşturulan Service Bus ad alanınız için tam DNS adı ayarlayın; Örneğin, `example.servicebus.windows.net`.
- `queueName`: Oluşturduğunuz sıranın adına ayarlayın.
- Uygulamanıza, önceki adımlarda belirtilen yeniden yönlendirme URI'si.

Konsol uygulamasını çalıştırdığınızda, bir senaryo seçmek istenir; tıklayın **etkileşimli kullanıcı oturum açma** sayısı yazıp ENTER tuşuna basın. Uygulama oturum açma penceresinde görüntüler, Service Bus'a erişmek için için izninizi isteyen ve ardından hizmeti oturum açma kimliğini kullanarak gönderme ve alma senaryosunu çalıştırın için kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
