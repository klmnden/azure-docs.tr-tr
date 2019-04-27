---
title: Yönetilen Azure kaynakları - Azure Event Hubs için kimlik | Microsoft Docs
description: Bu makale, Azure Event Hubs ile Azure kaynakları için yönetilen kimliklerini kullanma hakkında bilgi sağlar.
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
ms.openlocfilehash: 784d8c9280aeff7224f90ecee0b16c9c30381aeb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60746921"
---
# <a name="managed-identities-for-azure-resources-with-event-hubs"></a>Event Hubs ile Azure kaynakları için yönetilen kimlikleri

[Kimlikler Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md) uygulama kodunuzun çalıştığı dağıtımla ilişkili güvenli bir kimlik oluşturmanızı sağlayan bir çapraz Azure özelliğidir. Ardından, uygulamanızın belirli Azure kaynaklarına erişmek için özel izinler erişim denetimi rolleri kimliğe ilişkilendirebilirsiniz. 

İle yönetilen kimlikleri, Azure platformu bu çalışma zamanı kimlik yönetir. Depolayın ve uygulama kodu veya yapılandırma, kimlik için veya erişmek için ihtiyacınız olan kaynakları için erişim anahtarlarını korumak gerekmez. SAS kuralları ve anahtarlar ya da herhangi bir erişim belirteçleri işlemek Event Hubs, Azure kaynaklarını desteği etkin yönetilen kimliklerle bir Azure App Service uygulama içinde veya bir sanal makinede çalışan bir istemci uygulaması gerekmez. İstemci uygulaması, uç nokta adresi Event Hubs ad alanının yalnızca gerekir. Uygulamaya bağlandığında, Event Hubs yönetilen kimliğin bağlam istemci örneği bu makalenin sonraki bölümlerinde gösterilen işleminde bağlar.

Yönetilen bir kimlikle ilişkili olduğunda, Event Hubs istemcisi tüm yetkili işlemleri de yapabilirsiniz. Event Hubs rolleri ile bir yönetilen kimlik ilişkilendirerek yetkilendirme izni verilir. 

## <a name="event-hubs-roles-and-permissions"></a>Olay hub'ları rolleri ve izinleri

Yalnızca bir yönetilen kimlik ad alanındaki tüm varlıklar üzerinde kimlik tam denetim veren bir Event Hubs ad alanının "Sahip" veya "Katılımcı" rolleri ekleyebilirsiniz. Ancak, ad alanı topolojisini değiştirme işlemleri başlangıçta olan yönetim, ancak yalnızca Azure Resource Manager desteklenmiyor. Yerel olay hub'ları REST yönetim arabirimi aracılığıyla değil. Bu destek, ayrıca .NET Framework istemci kullanamazsınız gelir [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi içinde yönetilen bir kimlik. 
 
## <a name="use-event-hubs-with-managed-identities-for-azure-resources"></a>Event Hubs, Azure kaynakları için yönetilen kimliklerle kullanın

Aşağıdaki bölümde, aşağıdaki adımları açıklanmaktadır:

1. Oluşturun ve yönetilen bir kimlik altında çalışan örnek uygulama dağıtırsınız.
2. Bir Event Hubs ad alanı, kimlik erişim verin.
3. Uygulama, kimliğini kullanarak event hubs ile nasıl etkileşim girer.

Barındırılan bir web uygulaması bu tanıtımda açıklanmaktadır [Azure App Service](https://azure.microsoft.com/services/app-service/). Bir VM tarafından barındırılan uygulama için gerekli adımlar benzerdir.

### <a name="create-an-app-service-web-application"></a>Bir App Service web uygulaması oluşturma

İlk adım, bir App Service ASP.NET uygulaması oluşturmaktır. Azure'da nasıl yapılacağı hakkında bilgi sahibi değilseniz izleyin [bu nasıl yapılır kılavuzunda](../app-service/app-service-web-get-started-dotnet-framework.md). Ancak, öğreticide gösterilen şekilde bir MVC uygulaması oluşturmak yerine, bir Web Forms uygulaması oluşturun.

### <a name="set-up-the-managed-identity"></a>Yönetilen kimlik

Uygulamayı oluşturduktan sonra (nasıl yapılır makalesinde de gösterilmiştir) Azure portalında yeni oluşturulan web uygulamasına gidin ve ardından gitmek **yönetilen hizmet kimliği** sayfasını ve özelliğini etkinleştirin: 

![Yönetilen hizmet kimliği sayfası](./media/event-hubs-managed-service-identity/msi1.png)
 
Özelliği etkinleştirdikten sonra yeni bir hizmet kimliği Azure Active Directory'niz içinde oluşturulur ve App Service ana bilgisayar yapılandırılmış.

### <a name="create-a-new-event-hubs-namespace"></a>Yeni Event Hubs ad alanı oluşturma

Ardından, [bir Event Hubs ad alanı oluşturma](event-hubs-create.md) Azure kaynakları için yönetilen kimlikleri için Önizleme desteği olan Azure bölgelerinden birini: **ABD Doğu**, **ABD Doğu 2**, veya **Batı Avrupa**. 

Ad alanınıza gidin **erişim denetimi (IAM)** sayfasında portalda ve ardından **rol ataması Ekle** için yönetilen kimlik eklemek için **sahibi** rol. Bunu yapmak için web uygulamasının adını arayın **izinleri eklemek** paneli **seçin** alan ve sonra giriş'e tıklayın. Daha sonra **Kaydet**'e tıklayın. Event Hubs ad alanı web uygulaması artık yönetilen kimlik erişimi ve daha önce oluşturduğunuz olay hub'ına. 

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Artık oluşturduğunuz ASP.NET uygulamasının varsayılan sayfasını değiştirin. Web uygulama kodundan kullanabilirsiniz [bu GitHub deposundan](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp). 

Uygulamayı başlattıktan sonra tarayıcınızı EventHubsMSIDemo.aspx için işaretleyin. Ayrıca, başlangıç sayfası olarak ayarlayabilirsiniz. Kod EventHubsMSIDemo.aspx.cs dosyasında bulunabilir. Bazı giriş alanları ile birlikte en az bir web uygulaması sonucudur **Gönder** ve **alma** olaylarını almak veya göndermek için Event Hubs'a bağlanma düğmeleri. 

Not nasıl [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) nesnesi. Paylaşılan erişim belirteci (SAS) belirteç sağlayıcısı kullanmak yerine, kod ile yönetilen kimlik için bir belirteç sağlayıcısı oluşturur `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` çağırın. Bu nedenle, kaydetmek ve kullanmak için gizli dizi vardır. Olay hub'larına ve yetkilendirme el sıkışması yönetilen kimlik bağlamını akışını otomatik olarak tarafından işlenmesini SAS kullanmaktan daha basit bir model belirteç sağlayıcısı.

Bu değişiklikleri yaptıktan sonra yayımlama ve uygulamayı çalıştırın. İndirerek ve ardından Visual Studio'da bir yayımlama profilini içeri aktarma verileri doğru yayımlama alabilirsiniz:

![Yayımlama profilini İçeri Aktar](./media/event-hubs-managed-service-identity/msi3.png)
 
İleti göndermek veya almak için ad alanının adı ve oluşturduğunuz varlığın adını girin, ardından tıklayın **Gönder** veya **alma**. 
 
Yönetilen kimlik, yalnızca içinde yapılandırdığınız App Service dağıtımı ve yalnızca Azure ortamı içinde çalışır. Yönetilen kimlik şu anda App Service dağıtım yuvaları ile çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Olay hub'ları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)
