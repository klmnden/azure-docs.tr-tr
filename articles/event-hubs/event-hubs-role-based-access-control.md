---
title: Rol tabanlı erişim denetimi Önizleme - Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs için rol tabanlı erişim denetimi hakkında bilgi sağlar.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 549cfb84ff247295e01c800aa41ba265bb8921c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60343469"
---
# <a name="active-directory-role-based-access-control-preview"></a>Etkin Directory Role-Based erişim denetimi (Önizleme)

Microsoft Azure kaynakları ve Azure Active Directory (Azure AD) tabanlı uygulamalar için tümleşik erişim denetimi yönetimi sağlar. Azure AD ile özellikle Azure tabanlı uygulamalarınız için kullanıcı hesapları ve uygulamalar ya da yönetebilir veya var olan Active Directory altyapınızı şirket çapında çoklu oturum de kapsayan, Azure kaynaklarını açma için Azure AD ile federasyona eklemek ve Azure barındırılan uygulamalar. Ardından, Azure kaynaklarına erişim için genel ve hizmete özgü rollere bu Azure AD kullanıcı ve uygulama kimlikleri atayabilirsiniz.

Azure Event Hubs için ad alanları ve Azure portalı üzerinden tüm ilgili kaynakları yönetimini ve Azure kaynak yönetimi API'si zaten kullanarak korumalı *rol tabanlı erişim denetimi* (RBAC) modeli. Çalışma zamanı işlemleri için RBAC özelliği artık genel Önizleme aşamasındadır. 

SAS kuralları ve anahtarlar ya da olay hub'ları için belirli diğer herhangi bir erişim belirteçleri işlemek Azure AD RBAC kullanan bir uygulamayı gerekmez. İstemci uygulaması kimlik doğrulaması bağlamı'kurmak için Azure AD ile etkileşime geçer ve olay hub'ları için bir erişim belirteci alır. Etkileşimli oturum açma gerektiren etki alanı kullanıcı hesaplarını, uygulama hiçbir zaman herhangi bir kimlik bilgisi doğrudan işler.

## <a name="event-hubs-roles-and-permissions"></a>Olay hub'ları rolleri ve izinleri

İlk genel Önizleme için Event Hubs ad alanının "Sahip" veya "Katılımcı" rolleri yalnızca Azure AD hesapları ve hizmet sorumlularını ekleyebilirsiniz. Bu işlem ad alanındaki tüm varlıklar üzerinde kimlik tam denetim verir. Ad topoloji değişikliği yönetim işlemlerini olan başlangıçta yalnızca desteklenen ancak Azure kaynak yönetimi yerel Event Hubs REST yönetim arabirimi üzerinden değil. Bu destek de .NET Framework istemci anlamına [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi olan bir Azure AD hesabı kullanılamaz.  

## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>Event Hubs ile bir Azure AD etki alanı kullanıcı hesabı kullanın.

Aşağıdaki bölümde oluşturmak ve çalıştırmak için etkileşimli bir Azure ister örnek bir uygulama için gereken adımlar açıklanmaktadır. oturum açma için AD kullanıcı, söz konusu kullanıcı hesabı için Event Hubs erişim izni verme hakkında ve Event Hubs erişmek için bu kimlik kullanma. 

Basit bir konsol uygulaması, bu tanıtımda açıklanmaktadır [kodu, GitHub üzerinde.](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Rbac/EventHubsSenderReceiverRbac/)

### <a name="create-an-active-directory-user-account"></a>Bir Active Directory kullanıcı hesabı oluşturma

Bu ilk adım isteğe bağlıdır. Her Azure aboneliği bir Azure Active Directory kiracısı ile otomatik olarak eşleştirilir ve bir Azure aboneliğine erişiminiz varsa, kullanıcı hesabınız zaten kaydedildi. Hesabınızı hemen kullanabileceğiniz anlamına gelir. 

Yine de bu senaryo için özel bir hesap oluşturmak istiyorsanız [adımları](../automation/automation-create-aduser-account.md). Daha büyük kuruluş senaryolarına yönelik durum geçerli olmayabilir ve Azure Active Directory kiracısı içinde hesapları oluşturma izniniz olmalıdır.

### <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Ardından, [bir Event Hubs ad alanı oluşturma](event-hubs-create.md) RBAC Event Hubs Önizleme desteğine sahip Azure bölgelerinden birini: **ABD Doğu**, **ABD Doğu 2**, veya **Batı Avrupa**. 

Ad alanı oluşturduktan sonra gidin, **erişim denetimi (IAM)** sayfasında portalda ve ardından **rol ataması Ekle** sahip rolü için Azure AD kullanıcı hesabı eklemek için. Kendi kullanıcı hesabı kullanıyorsanız ve oluşturduğunuz ad alanı, zaten sahip rolüne sahiptirler. Rolü için farklı bir hesap eklemek için web uygulamasının adını arayın **izinleri eklemek** paneli **seçin** alan ve sonra giriş'e tıklayın. Daha sonra **Kaydet**'e tıklayın. Kullanıcı hesabı artık Event Hubs ad alanına erişimi olan ve daha önce oluşturduğunuz olay hub'ına.
 
### <a name="register-the-application"></a>Uygulamayı kaydetme

Örnek uygulamayı çalıştırmadan önce Azure AD'ye kaydetme ve Event Hubs kendi adına erişmesine izin veren bir onay istemi onaylayın. 

Örnek uygulama bir konsol uygulaması olduğundan yerel bir uygulamayı kaydetme ve API izinlerini eklemeniz gerekir **Microsoft.EventHub** "gerekli izinler" kümesi. Yerel uygulamalar da gereken bir **redırect-URI** ; tanımlayıcı olarak hizmet veren Azure AD'de URI ağ hedef olması gerekmez. Kullanım `https://eventhubs.microsoft.com` kod örneği olduğundan bu örnek için bu URI kullanır.

Ayrıntılı kayıt adımları açıklanmıştır [Bu öğreticide](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Kaydetmek için adımları izleyin bir **yerel** uygulama ve ardından eklemek için güncelleştirme talimatları izleyin **Microsoft.EventHub** API için gerekli izinleri. Bu adımları izlediğiniz gibi Not **Tenantıd** ve **ApplicationId**gibi uygulamayı çalıştırmak için bu değerlere ihtiyacınız olur.

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Örneği çalıştırmadan önce App.config dosyasını düzenleyin ve senaryonuza bağlı olarak, aşağıdaki değerleri ayarlayın:

- `tenantId`: Kümesine **Tenantıd** değeri.
- `clientId`: Kümesine **ApplicationId** değeri. 
- `clientSecret`: İstemci gizli anahtarı kullanarak oturum istiyorsanız, Azure AD'de oluşturun. Ayrıca, bir web uygulaması veya API yerine yerel bir uygulama kullanın. Ayrıca, altında uygulama Ekle **erişim denetimi (IAM)** daha önce oluşturduğunuz ad alanı içinde.
- `eventHubNamespaceFQDN`: Yeni oluşturulan Event Hubs ad alanınız için tam DNS adı ayarlayın; Örneğin, `example.servicebus.windows.net`.
- `eventHubName`: Oluşturduğunuz olay hub'ı adına ayarlayın.
- Uygulamanıza, önceki adımlarda belirtilen yeniden yönlendirme URI'si.
 
Konsol uygulamasını çalıştırdığınızda, bir senaryo seçmek istenir; tıklayın **etkileşimli kullanıcı oturum açma** sayısı yazıp ENTER tuşuna basın. Uygulama oturum açma penceresinde görüntülenir, Event Hubs erişmeye için izninizi isteyen ve ardından hizmeti oturum açma kimliğini kullanarak gönderme ve alma senaryosunu çalıştırın için kullanır.

Uygulamanın kullandığı `ServiceAudience.EventHubsAudience` belirteç hedef kitlesi olarak. İzleyici bir sabit olarak kullanılabilir olduğu diğer bir dil ya da SDK'ları kullanarak kullanmak için doğru değeri olur `https://eventhubs.azure.net/`.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Olay hub'ları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)
