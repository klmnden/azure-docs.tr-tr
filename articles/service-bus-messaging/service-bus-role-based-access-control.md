---
title: "Azure Service Bus Role-Based erişim denetimi (RBAC) Önizleme | Microsoft Docs"
description: "Azure Service Bus rol tabanlı erişim denetimi"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 729d6db6b2fc6495ffb0f4fbe4d545d7ad953cef
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="active-directory-role-based-access-control-preview"></a>Etkin Directory Role-Based erişim denetimi (Önizleme)

Microsoft Azure kaynakları ve Azure Active Directory (Azure AD) tabanlı uygulamalar için tümleşik erişim denetimi yönetimi sağlar. Azure AD ile ya da kullanıcı hesaplarını yönetebilir ve for applications özellikle Azure tabanlı uygulamalar veya var olan Active Directory altyapınızı şirket çapında çoklu oturum açma için Azure kaynaklarını da yayılan, Azure AD ile birleştirmek ve Azure uygulamaları barındırılır. Azure kaynaklara erişim izni için genel ve hizmete özgü rollerine bu Azure AD kullanıcı ve uygulama kimlikleri sonra atayabilirsiniz.

Azure hizmet veri yolu için ad alanları ve tüm ilişkili kaynakları Azure portalı üzerinden yönetimi ve Azure kaynak yönetimi API zaten kullanarak korumalı *rol tabanlı erişim denetimi* (RBAC) modeli. Çalışma zamanı işlemleri için RBAC bir özellik artık genel önizlemede değil. 

Azure AD RBAC kullanan bir uygulama tanıtıcısı SAS kuralları ve anahtarları veya Service Bus belirli diğer herhangi bir erişim belirteci gerekmez. İstemci uygulama bir kimlik doğrulama bağlamı oluşturmak için Azure AD ile etkileşim kurar ve hizmet veri yolu için bir erişim belirteci alır. Etkileşimli oturum açma gerektiren etki alanı kullanıcı hesapları ile uygulama hiçbir zaman herhangi bir kimlik bilgisi doğrudan işler.

## <a name="service-bus-roles-and-permissions"></a>Hizmet veri yolu rolleri ve izinleri

İlk genel önizlemesi için Service Bus Mesajlaşma hizmeti ad alanı "Sahibi" veya "Katkıda" rollere yalnızca Azure AD hesapları ve hizmet asıl adı ekleyebilirsiniz. Bu işlem ad alanındaki tüm varlıklara üzerinde kimlik tam denetim verir. Ad topoloji değişikliği yönetim işlemlerini olan başlangıçta yalnızca desteklenen ancak Azure kaynak yönetimi ve yerel Service Bus REST yönetim arabirimi üzerinden değil. Bu desteği de .NET Framework istemci anlamına [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi ile bir Azure AD hesabının kullanılamaz.  

## <a name="use-service-bus-with-an-azure-ad-domain-user-account"></a>Bir Azure AD etki alanı kullanıcı hesabı ile hizmet veri yolu kullanın

Aşağıdaki bölümde oluşturmak ve etkileşimli Azure ister bir örnek uygulamayı çalıştırmak için gerekli adımlar tanımlanmaktadır oturum açmak için AD kullanıcı, bu kullanıcı hesabı Service Bus erişim vermek nasıl ve Event Hubs erişmek için kimliğini kullanma. 

Basit bir konsol uygulaması, bu giriş açıklar [kodudur kendisi için Github'da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl).

### <a name="create-an-active-directory-user-account"></a>Bir Active Directory kullanıcı hesabı oluşturun

Bu ilk adım isteğe bağlıdır. Her Azure aboneliği otomatik olarak Azure Active Directory kiracısı ile eşleştirilmiş ve bir Azure aboneliğine erişiminiz varsa, kullanıcı hesabınız zaten kayıtlı. Hesabınızı hemen kullanabileceğiniz anlamına gelir. 

Hala bu senaryo için özel bir hesap oluşturmak istiyorsanız [adımları](../automation/automation-create-aduser-account.md). Büyük kuruluş senaryolarına yönelik durumda olmayabilir ve Azure Active Directory kiracısı içinde hesapları oluşturma iznine sahip olmalıdır.

### <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma

İleri [Service Bus Mesajlaşma hizmeti ad alanı oluşturma](service-bus-create-namespace-portal.md) RBAC Önizleme desteğine sahip Azure bölgelerinden birinde: **BİZE Doğu**, **ABD Doğu 2**, veya **Batı Avrupa** . 

Ad alanı oluşturulduğunda gidin, **erişim denetimi (IAM)** sayfasında portalda ve ardından **Ekle** Azure AD kullanıcı hesabı için sahip rolünü eklemek için. Ad alanı oluşturmuş ve kendi kullanıcı hesabınızı kullanın, zaten sahip rolünü demektir. Rolü için farklı bir hesap eklemek için web uygulamasının adını arayın **izinleri eklemek** Masası **seçin** alan ve sonra giriş'i tıklatın. Daha sonra **Kaydet**'e tıklayın.

![](./media/service-bus-role-based-access-control/rbac1.PNG)

Kullanıcı hesabı artık Service Bus ad alanı erişimi olur ve daha önce oluşturduğunuz kuyruğa.
 
### <a name="register-the-application"></a>Uygulamayı Kaydet

Örnek uygulamayı çalıştırmadan önce Azure AD'de kaydetmek ve kendi adına Azure Service Bus erişmek için uygulama izin veren onay istemi onaylayabilirsiniz. 

Örnek uygulama bir konsol uygulaması olduğundan, bir yerel uygulamayı kaydedin ve API izinlerini eklemeniz gerekir **Microsoft.ServiceBus** "izinleri gerekli" kümesi. Yerel uygulamalar da gereken bir **yeniden yönlendirme URI'si** hizmet veren bir tanımlayıcı olarak; Azure AD'de URI ağ hedef olması gerekmez. Kullanım `http://servicebus.microsoft.com` örnek kodu zaten olduğundan bu örnek için bu URI kullanır.

Ayrıntılı kayıt adımları açıklanmıştır [Bu öğretici](../active-directory/develop/active-directory-integrating-applications.md). Kaydetmek için adımları bir **yerel** uygulama ve ardından eklemek için Güncelleştirme yönergeleri izleyin **Microsoft.ServiceBus** API için gerekli izinleri. Adımları gibi Not **Tenantıd** ve **ApplicationId**, uygulamayı çalıştırmak için bu değerleri ihtiyaç duyacaksınız.

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Örneği çalıştırmadan önce App.config dosyasını düzenleyin ve senaryonuza bağlı olarak, aşağıdaki değerleri ayarlayın:

- `tenantId`: Kümesine **Tenantıd** değeri.
- `clientId`: Kümesine **ApplicationId** değeri. 
- `clientSecret`: İstemci gizli anahtarı kullanarak oturum açmak istiyorsanız, Azure AD'de oluşturun. Ayrıca, bir web uygulaması veya API yerine yerel bir uygulama kullanın. Ayrıca, uygulama altında ekleme **erişim denetimi (IAM)** daha önce oluşturduğunuz ad.
- `serviceBusNamespaceFQDN`: Yeni oluşturulan hizmet veri yolu ad alanınız tam DNS adını ayarlayın; Örneğin, `example.servicebus.windows.net`.
- `queueName`: Oluşturduğunuz kuyruğu adına ayarlayın.
- Uygulamanızda, önceki adımlarda belirtilen yeniden yönlendirme URI'si.
 
Konsol uygulamasını çalıştırdığınızda, bir senaryo seçmek istenir; tıklatın **etkileşimli kullanıcı oturum açma** numarasını yazıp ENTER tuşuna basın. Uygulama bir oturum açma pencere görüntüler, onayınızı Service Bus'a erişmek ister ve oturum açma kimliğini kullanarak gönderme ve alma senaryosuyla çalıştırmak için hizmeti kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)