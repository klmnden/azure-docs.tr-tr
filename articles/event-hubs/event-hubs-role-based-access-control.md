---
title: "Azure Event Hubs Role-Based erişim denetimi (RBAC) Önizleme | Microsoft Docs"
description: "Azure Event Hubs rol tabanlı erişim denetimi"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 0d3a779eb2cccf242bcd42d82c1a90048b3512ab
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="active-directory-role-based-access-control-preview"></a>Etkin Directory Role-Based erişim denetimi (Önizleme)

Microsoft Azure kaynakları ve Azure Active Directory (Azure AD) tabanlı uygulamalar için tümleşik erişim denetimi yönetimi sağlar. Azure AD ile ya da kullanıcı hesaplarını yönetebilir ve for applications özellikle Azure tabanlı uygulamalar veya var olan Active Directory altyapınızı şirket çapında çoklu oturum açma için Azure kaynaklarını da yayılan, Azure AD ile birleştirmek ve Azure uygulamaları barındırılır. Azure kaynaklara erişim izni için genel ve hizmete özgü rollerine bu Azure AD kullanıcı ve uygulama kimlikleri sonra atayabilirsiniz.

Azure Event Hubs için ad alanları ve tüm ilişkili kaynakları Azure portalı üzerinden yönetimi ve Azure kaynak yönetimi API zaten kullanarak korumalı *rol tabanlı erişim denetimi* (RBAC) modeli. Çalışma zamanı işlemleri için RBAC bir özellik artık genel önizlemede değil. 

Azure AD RBAC kullanan bir uygulama tanıtıcısı SAS kuralları ve anahtarları veya Event Hubs'a belirli diğer herhangi bir erişim belirteci gerekmez. İstemci uygulama bir kimlik doğrulama bağlamı oluşturmak için Azure AD ile etkileşim kurar ve Event Hubs için bir erişim belirteci alır. Etkileşimli oturum açma gerektiren etki alanı kullanıcı hesapları ile uygulama hiçbir zaman herhangi bir kimlik bilgisi doğrudan işler.

## <a name="event-hubs-roles-and-permissions"></a>Olay hub'ları rolleri ve izinleri

İlk genel Önizleme için bir olay hub'ları ad alanı "Sahibi" veya "Katkıda" rolleri yalnızca Azure AD hesapları ve hizmet asıl adı ekleyebilirsiniz. Bu işlem ad alanındaki tüm varlıklara üzerinde kimlik tam denetim verir. Ad topoloji değişikliği yönetim işlemlerini olan başlangıçta yalnızca desteklenen ancak Azure kaynak yönetimi ve yerel olay hub'ları REST yönetim arabirimi üzerinden değil. Bu desteği de .NET Framework istemci anlamına [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi ile bir Azure AD hesabının kullanılamaz.  

## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>Bir Azure AD etki alanı kullanıcı hesabı ile olay hub'ları kullanın

Aşağıdaki bölümde oluşturmak ve etkileşimli Azure ister bir örnek uygulamayı çalıştırmak için gerekli adımlar tanımlanmaktadır oturum açmak için AD kullanıcı, bu kullanıcı hesabı olay hub'ları erişim vermek nasıl ve Event Hubs erişmek için kimliğini kullanma. 

Basit bir konsol uygulaması, bu giriş açıklar [kodu, Github üzerinde.](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Rbac/EventHubsSenderReceiverRbac/)

### <a name="create-an-active-directory-user-account"></a>Bir Active Directory kullanıcı hesabı oluşturun

Bu ilk adım isteğe bağlıdır. Her Azure aboneliği otomatik olarak Azure Active Directory kiracısı ile eşleştirilmiş ve bir Azure aboneliğine erişiminiz varsa, kullanıcı hesabınız zaten kayıtlı. Hesabınızı hemen kullanabileceğiniz anlamına gelir. 

Hala bu senaryo için özel bir hesap oluşturmak istiyorsanız [adımları](../automation/automation-create-aduser-account.md). Büyük kuruluş senaryolarına yönelik durumda olmayabilir ve Azure Active Directory kiracısı içinde hesapları oluşturma iznine sahip olmalıdır.

### <a name="create-an-event-hubs-namespace"></a>Bir olay hub'ları ad alanı oluşturma

İleri [bir olay hub'ları ad alanı oluşturma](event-hubs-create.md) RBAC için olay hub'ları Önizleme desteğine sahip Azure bölgelerinden birinde: **BİZE Doğu**, **ABD Doğu 2**, veya **Batı Avrupa** . 

Ad alanı oluşturulduğunda gidin, **erişim denetimi (IAM)** sayfasında portalda ve ardından **Ekle** Azure AD kullanıcı hesabı için sahip rolünü eklemek için. Ad alanı oluşturmuş ve kendi kullanıcı hesabınızı kullanın, zaten sahip rolünü demektir. Rolü için farklı bir hesap eklemek için web uygulamasının adını arayın **izinleri eklemek** Masası **seçin** alan ve sonra giriş'i tıklatın. Daha sonra **Kaydet**'e tıklayın.
 
![](./media/event-hubs-role-based-access-control/rbac1.PNG)

Kullanıcı hesabı artık olay hub'ları ad alanına erişimi olur ve daha önce oluşturduğunuz olay hub'ına.
 
### <a name="register-the-application"></a>Uygulamayı Kaydet

Örnek uygulamayı çalıştırmadan önce Azure AD'ye kaydetme ve olay hub'ları kendi adına erişmek için uygulama izin veren onay istemi onaylayabilirsiniz. 

Örnek uygulama bir konsol uygulaması olduğundan, bir yerel uygulamayı kaydedin ve API izinlerini eklemeniz gerekir **Microsoft.EventHub** "izinleri gerekli" kümesi. Yerel uygulamalar da gereken bir **yeniden yönlendirme URI'si** hizmet veren bir tanımlayıcı olarak; Azure AD'de URI ağ hedef olması gerekmez. Kullanım `http://eventhubs.microsoft.com` örnek kodu zaten olduğundan bu örnek için bu URI kullanır.

Ayrıntılı kayıt adımları açıklanmıştır [Bu öğretici](../active-directory/develop/active-directory-integrating-applications.md). Kaydetmek için adımları bir **yerel** uygulama ve ardından eklemek için Güncelleştirme yönergeleri izleyin **Microsoft.EventHub** API için gerekli izinleri. Adımları gibi Not **Tenantıd** ve **ApplicationId**, uygulamayı çalıştırmak için bu değerleri ihtiyaç duyacaksınız.

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Örneği çalıştırmadan önce App.config dosyasını düzenleyin ve senaryonuza bağlı olarak, aşağıdaki değerleri ayarlayın:

- `tenantId`: Kümesine **Tenantıd** değeri.
- `clientId`: Kümesine **ApplicationId** değeri. 
- `clientSecret`: İstemci gizli anahtarı kullanarak oturum açmak istiyorsanız, Azure AD'de oluşturun. Ayrıca, bir web uygulaması veya API yerine yerel bir uygulama kullanın. Ayrıca, uygulama altında ekleme **erişim denetimi (IAM)** daha önce oluşturduğunuz ad.
- `eventHubNamespaceFQDN`: Yeni oluşturulan olay hub'ları ad alanınızın tam DNS adını ayarlayın; Örneğin, `example.servicebus.windows.net`.
- `eventHubName`: Oluşturduğunuz olay hub'ı adına ayarlayın.
- Uygulamanızda, önceki adımlarda belirtilen yeniden yönlendirme URI'si.
 
Konsol uygulamasını çalıştırdığınızda, bir senaryo seçmek istenir; tıklatın **etkileşimli kullanıcı oturum açma** numarasını yazıp ENTER tuşuna basın. Uygulama bir oturum açma pencere görüntüler, onayınızı olay hub'ları erişmek ister ve oturum açma kimliğini kullanarak gönderme ve alma senaryosuyla çalıştırmak için hizmeti kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Olay hub'ı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)