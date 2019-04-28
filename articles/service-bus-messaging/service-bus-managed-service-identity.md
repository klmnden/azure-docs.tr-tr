---
title: Kimlikler Azure Service Bus Önizleme ile Azure kaynakları için yönetilen | Microsoft Docs
description: Azure Service Bus ile Azure kaynakları için yönetilen kimlikleri kullanmak
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
ms.date: 09/01/2018
ms.author: aschhab
ms.openlocfilehash: 5edeebd9698384785082e5a441c24e136ed22481
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61317085"
---
# <a name="managed-identities-for-azure-resources-with-service-bus"></a>Service Bus ile Azure kaynakları için yönetilen kimlikleri 

[Kimlikler Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md) uygulama kodunuzun çalıştığı dağıtımla ilişkili güvenli bir kimlik oluşturmanızı sağlayan bir çapraz Azure özelliğidir. Ardından, uygulamanızın belirli Azure kaynaklarına erişmek için özel izinler erişim denetimi rolleri kimliğe ilişkilendirebilirsiniz.

İle yönetilen kimlikleri, Azure platformu bu çalışma zamanı kimlik yönetir. Depolayın ve uygulama kodu veya yapılandırma, kimlik için veya erişmek için ihtiyacınız olan kaynakları için erişim anahtarlarını korumak gerekmez. Bir Service Bus varlıkları SAS kuralları ve anahtarlar ya da herhangi bir erişim belirteçleri işlemek için destek gerekmiyor Azure kaynakları için yönetilen bir Azure App Service uygulama içinde veya bir sanal makineyle etkin çalışan istemci uygulaması. İstemci uygulaması Service Bus Mesajlaşması ad alanı uç nokta adresini yeterlidir. Uygulamaya bağlandığında, Service Bus yönetilen bir varlığın bağlam istemci örneği bu makalenin sonraki bölümlerinde gösterilen işleminde bağlar. Yönetilen bir kimlikle ilişkili olduğunda, Service Bus istemci tüm yetkili işlemleri de yapabilirsiniz. Yetkilendirme, Service Bus rolleri ile yönetilen bir varlığın ilişkilendirerek verilir. 

## <a name="service-bus-roles-and-permissions"></a>Service Bus rolleri ve izinleri

Bu gibi durumlarda, yönetilen bir kimlik yalnızca bir Service Bus ad alanı "Sahip" veya "Katılımcı" rolleri ekleyebilirsiniz. Ad alanındaki tüm varlıklar üzerinde kimlik tam denetim verir. Ancak, ad alanı topolojisini değiştirme işlemleri başlangıçta olan yönetim, ancak yalnızca Azure Resource Manager desteklenmiyor. Yerel bir Service Bus REST yönetim arabirimi aracılığıyla değil. Bu destek, ayrıca .NET Framework istemci kullanamazsınız gelir [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi içinde yönetilen bir kimlik.

## <a name="use-service-bus-with-managed-identities-for-azure-resources"></a>Service Bus yönetilen kimliklerle Azure kaynakları için kullanın.

Aşağıdaki bölümde bir yönetilen kimlik, bir Service Bus Mesajlaşması ad alanı, kimlik erişim vermek nasıl ve uygulaması kullanarak Service Bus varlıkları ile nasıl etkileşim kurduğunu altında çalışan bir örnek uygulaması oluşturma ve dağıtma için gereken adımlar açıklanmaktadır. Bu kimliği.

Barındırılan bir web uygulaması bu tanıtımda açıklanmaktadır [Azure App Service](https://azure.microsoft.com/services/app-service/). Bir VM tarafından barındırılan uygulama için gerekli adımlar benzerdir.

### <a name="create-an-app-service-web-application"></a>Bir App Service web uygulaması oluşturma

İlk adım, bir App Service ASP.NET uygulaması oluşturmaktır. Azure'da bunun nasıl yapılacağı hakkında bilgi sahibi değilseniz izleyin [bu nasıl yapılır kılavuzunda](../app-service/app-service-web-get-started-dotnet-framework.md). Ancak, öğreticide gösterilen şekilde bir MVC uygulaması oluşturmak yerine, bir Web Forms uygulaması oluşturun.

### <a name="set-up-the-managed-identity"></a>Yönetilen kimlik

Uygulamayı oluşturduktan sonra (nasıl yapılır makalesinde de gösterilmiştir) Azure portalında yeni oluşturulan web uygulamasına gidin ve ardından gitmek **yönetilen hizmet kimliği** sayfasını ve özelliğini etkinleştirin: 

![](./media/service-bus-managed-service-identity/msi1.png)

Özelliği etkinleştirdikten sonra yeni bir hizmet kimliği Azure Active Directory'niz içinde oluşturulur ve App Service ana bilgisayar yapılandırılmış.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Yeni bir Service Bus Mesajlaşması ad alanı oluştur

Ardından, [Service Bus Mesajlaşması ad alanı oluşturma](service-bus-create-namespace-portal.md) RBAC Önizleme desteğine sahip Azure bölgelerinden birini: **ABD Doğu**, **ABD Doğu 2**, veya **Batı Avrupa**. 

Ad alanınıza gidin **erişim denetimi (IAM)** sayfasında portalda ve ardından **rol ataması Ekle** için yönetilen kimlik eklemek için **sahibi** rol. Bunu yapmak için web uygulamasının adını arayın **izinleri eklemek** paneli **seçin** alan ve sonra giriş'e tıklayın. Daha sonra **Kaydet**'e tıklayın.

Web uygulamasının yönetilen kimlik artık Service Bus ad alanı erişimi olan ve daha önce oluşturduğunuz kuyruğa. 

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Şimdi, oluşturduğunuz ASP.NET uygulamasının varsayılan sayfasını değiştirin. Web uygulama kodundan kullanabileceğiniz [bu GitHub deposundan](https://github.com/Azure-Samples/app-service-msi-servicebus-dotnet).  

Default.aspx sayfasında, giriş sayfasıdır. Kod Default.aspx.cs dosyasında bulunabilir. Bazı giriş alanları ile birlikte en az bir web uygulaması sonucudur **Gönder** ve **alma** iletileri almak veya göndermek için Service Bus'a bağlanmak düğmeleri.

Not nasıl [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) nesnesi. Paylaşılan erişim belirteci (SAS) belirteç sağlayıcısı kullanmak yerine, kod ile yönetilen kimlik için bir belirteç sağlayıcısı oluşturur `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)` çağırın. Bu nedenle, korumak ve kullanmak için gizli dizi vardır. Service Bus ve yetkilendirme el sıkışması yönetilen kimlik bağlamını akışını belirteç sağlayıcısı tarafından otomatik olarak işlenir. SAS kullanarak daha basit bir modeldir.

Bu değişiklikleri yaptıktan sonra yayımlama ve uygulamayı çalıştırın. İndirerek ve ardından Visual Studio'da bir yayımlama profilini içeri aktarma doğru yayımlama verileri kolayca edinebilirsiniz:

![](./media/service-bus-managed-service-identity/msi3.png)
 
İleti göndermek veya almak için ad alanının adı ve oluşturduğunuz varlığın adını girin. Ardından ya da tıklayın **Gönder** veya **alma**.


> [!NOTE]
> - Azure Vm'leri, yalnızca uygulama hizmetlerinde, Azure ortamına içinde yönetilen kimlik çalışır ve ölçek kümeleri. .NET uygulamaları için Service Bus NuGet paketi tarafından kullanılan Microsoft.Azure.Services.AppAuthentication kitaplığını, bu protokolü üzerinden bir Özet sağlar ve bir yerel geliştirme deneyimini destekler. Bu kitaplık kodunuzu Visual Studio, Azure CLI 2.0 veya Active Directory tümleşik kimlik doğrulaması, kullanıcı hesabını kullanarak yerel olarak geliştirme makinenizde, test etmenizi sağlar. Bu kitaplığı ile yerel geliştirme seçenekleri hakkında daha fazla bilgi için bkz. [.NET kullanarak Azure Key Vault hizmetten hizmete kimlik doğrulaması](../key-vault/service-to-service-authentication.md).  
> 
> - Şu anda yönetilen kimlikleri, App Service dağıtım yuvaları ile çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)