---
title: Yönetilen hizmet kimliği ile Azure Service Bus Önizleme | Microsoft Docs
description: Yönetilen hizmet kimlikleri, Azure Service Bus ile kullanma
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
ms.date: 08/01/2018
ms.author: sethm
ms.openlocfilehash: 30df312e349bd6f6ebd1f38141075382be2522a2
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397993"
---
# <a name="managed-service-identity-preview"></a>Yönetilen Hizmet Kimliği (önizleme)

Yönetilen hizmet kimliği (MSI), uygulama kodunuzun çalıştığı dağıtımla ilişkili güvenli bir kimlik oluşturmanızı sağlayan bir arası Azure özelliğidir. Ardından, uygulamanızın belirli Azure kaynaklarına erişmek için özel izinler erişim denetimi rolleri kimliğe ilişkilendirebilirsiniz.

MSI ile Azure platformu, bu çalışma zamanı kimlik yönetir. Depolayın ve uygulama kodu veya yapılandırma, kimlik için veya erişmek için ihtiyacınız olan kaynakları için erişim anahtarlarını korumak gerekmez. Etkin MSI desteğiyle Azure App Service uygulama içinde veya bir sanal makinede çalışan bir Service Bus istemci uygulaması, SAS kuralları ve anahtarlar ya da herhangi bir erişim belirteçleri işlemek gerekmez. İstemci uygulaması Service Bus Mesajlaşması ad alanı uç nokta adresini yeterlidir. Uygulamaya bağlandığında, Service Bus istemci örneği bu makalenin sonraki bölümlerinde gösterilen işleminde MSI bağlam bağlar. 

Yönetilen hizmet kimliği ile ilişkili olduğunda, Service Bus istemci tüm yetkili işlemleri gerçekleştirebilir. Yetkilendirme, bir MSI hizmet veri yolu rolleri ile ilişkilendirerek verilir. 

## <a name="service-bus-roles-and-permissions"></a>Service Bus rolleri ve izinleri

İlk genel Önizleme sürümünde, yalnızca bir yönetilen hizmet kimliği kimlik ad alanındaki tüm varlıklara tam denetim veren bir Service Bus ad alanının "Sahip" veya "Katılımcı" rollere de ekleyebilirsiniz. Ancak, ad alanı topoloji değişikliği işlemler: başlangıçta yönetim Azure Resource Manager yalnızca ancak desteklenen yerel hizmet veri yolu REST yönetim arabirimi üzerinden değil. Bu destek, ayrıca .NET Framework istemci kullanamazsınız gelir [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi içinde bir yönetilen hizmet kimliği.

## <a name="use-service-bus-with-a-managed-service-identity"></a>Hizmet veri yolu ile bir yönetilen hizmet kimliğini kullanma

Aşağıdaki bölümde bir yönetilen hizmet kimliği, nasıl bir Service Bus Mesajlaşması ad alanı, kimlik erişim vermek ve uygulama Service Bus ile nasıl etkileştiğini altında çalışan bir örnek uygulaması oluşturma ve dağıtma için gereken adımlar açıklanmaktadır. Bu kimlik kullanarak varlıkları.

Barındırılan bir web uygulaması bu tanıtımda açıklanmaktadır [Azure App Service](https://azure.microsoft.com/services/app-service/). Bir VM tarafından barındırılan uygulama için gerekli adımlar benzerdir.

### <a name="create-an-app-service-web-application"></a>Bir App Service web uygulaması oluşturma

İlk adım, bir App Service ASP.NET uygulaması oluşturmaktır. Azure'da nasıl yapılacağı hakkında bilgi sahibi değilseniz izleyin [bu nasıl yapılır kılavuzunda](../app-service/app-service-web-get-started-dotnet-framework.md). Ancak, öğreticide gösterilen şekilde bir MVC uygulaması oluşturmak yerine, bir Web Forms uygulaması oluşturun.

### <a name="set-up-the-managed-service-identity"></a>Yönetilen hizmet kimliğini ayarlayın

Uygulamayı oluşturduktan sonra (nasıl yapılır makalesinde de gösterilmiştir) Azure portalında yeni oluşturulan web uygulamasına gidin ve ardından gitmek **yönetilen hizmet kimliği** sayfasını ve özelliğini etkinleştirin: 

![](./media/service-bus-managed-service-identity/msi1.png)

Özelliği etkinleştirdikten sonra yeni bir hizmet kimliği Azure Active Directory'niz içinde oluşturulur ve App Service ana bilgisayar yapılandırılmış.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Yeni bir Service Bus Mesajlaşması ad alanı oluştur

Ardından, [Service Bus Mesajlaşması ad alanı oluşturma](service-bus-create-namespace-portal.md) RBAC Önizleme desteğine sahip Azure bölgelerinden birini: **ABD Doğu**, **ABD Doğu 2**, veya **Batı Avrupa** . 

Ad alanınıza gidin **erişim denetimi (IAM)** sayfasında portalda ve ardından **Ekle** için Yönetilen hizmet kimliği eklemek için **sahibi** rol. Bunu yapmak için web uygulamasının adını arayın **izinleri eklemek** paneli **seçin** alan ve sonra giriş'e tıklayın. Daha sonra **Kaydet**'e tıklayın.

![](./media/service-bus-managed-service-identity/msi2.png)
 
Web uygulamasının yönetilen hizmet kimliği artık Service Bus ad alanı erişimi olan ve daha önce oluşturduğunuz kuyruğa. 

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Şimdi, oluşturduğunuz ASP.NET uygulamasının varsayılan sayfasını değiştirin. Web uygulama kodundan kullanabileceğiniz [bu GitHub deposundan](https://github.com/Azure-Samples/app-service-msi-servicebus-dotnet).  

Default.aspx sayfasında, giriş sayfasıdır. Kod Default.aspx.cs dosyasında bulunabilir. Bazı giriş alanları ile birlikte en az bir web uygulaması sonucudur **Gönder** ve **alma** iletileri almak veya göndermek için Service Bus'a bağlanmak düğmeleri.

Not nasıl [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) nesnesi. Paylaşılan erişim belirteci (SAS) belirteç sağlayıcısı kullanmak yerine, kod ile yönetilen hizmet kimliği için bir belirteç sağlayıcısı oluşturur `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)` çağırın. Bu nedenle, korumak ve kullanmak için gizli dizi vardır. Service Bus ve yetkilendirme el sıkışması yönetilen hizmet kimliği bağlam akışını otomatik olarak tarafından işlenmesini SAS kullanmaktan daha basit bir model belirteç sağlayıcısı.

Bu değişiklikleri yaptıktan sonra yayımlama ve uygulamayı çalıştırın. İndirmek ve ardından Visual Studio'da bir yayımlama profilini içeri aktarmak için doğru yayımlama verileri almak için kolay bir yol verilmiştir:

![](./media/service-bus-managed-service-identity/msi3.png)
 
İleti göndermek veya almak için ad alanının adı ve oluşturduğunuz varlığın adını girin, ardından tıklayın **Gönder** veya **alma**.


> [!NOTE]
> - Azure Vm'leri, yönetilen hizmet kimliği uygulama hizmetlerinde, Azure ortamına yalnızca içinde çalışır ve ölçek kümeleri. .NET uygulamaları için Service Bus NuGet paketi tarafından kullanılan Microsoft.Azure.Services.AppAuthentication kitaplığını, bu protokolü üzerinden bir Özet sağlar ve bir yerel geliştirme deneyimini destekler. Bu kitaplık kodunuzu Visual Studio, Azure CLI 2.0 veya Active Directory tümleşik kimlik doğrulaması, kullanıcı hesabını kullanarak yerel olarak geliştirme makinenizde, test etmenizi sağlar. Bu kitaplığı ile yerel geliştirme seçenekleri hakkında daha fazla bilgi için bkz. [.NET kullanarak Azure Key Vault hizmetten hizmete kimlik doğrulaması](../key-vault/service-to-service-authentication.md).  
> 
> - Şu anda, yönetilen hizmet kimlikleri, App Service dağıtım yuvaları ile çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)