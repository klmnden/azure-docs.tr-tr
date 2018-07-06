---
title: Yönetilen hizmet kimliği ile Azure Event Hubs Önizleme | Microsoft Docs
description: Yönetilen hizmet kimlikleri, Azure Event Hubs ile kullanma
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: sethm
ms.openlocfilehash: 7c8f7fff5e3cf7334ce30a3fa90ae950f841662c
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865306"
---
# <a name="managed-service-identity-preview"></a>Yönetilen Hizmet Kimliği (önizleme)

Yönetilen hizmet kimliği (MSI), uygulama kodunuzun çalıştığı dağıtımla ilişkili güvenli bir kimlik oluşturmanızı sağlayan bir arası Azure özelliğidir. Ardından, uygulamanızın belirli Azure kaynaklarına erişmek için özel izinler erişim denetimi rolleri kimliğe ilişkilendirebilirsiniz. 

MSI ile Azure platformu, bu çalışma zamanı kimlik yönetir. Depolayın ve uygulama kodu veya yapılandırma, kimlik için veya erişmek için ihtiyacınız olan kaynakları için erişim anahtarlarını korumak gerekmez. SAS kuralları ve anahtarlar ya da herhangi bir erişim belirteçleri işlemek bir Azure App Service uygulama içinde veya bir sanal makinede etkin MSI desteği ile çalışan bir olay hub'ları istemci uygulamanın gerekmez. İstemci uygulaması, uç nokta adresi Event Hubs ad alanının yalnızca gerekir. Uygulamaya bağlandığında, Event Hubs MSI bağlam istemci örneği bu makalenin sonraki bölümlerinde gösterilen işleminde bağlar.

Yönetilen hizmet kimliği ile ilişkili olduğunda, Event Hubs istemcisi tüm yetkili işlemleri gerçekleştirebilir. Yetkilendirme, bir MSI ile Event Hubs rolleri ile ilişkilendirilmesi yoluyla verilir. 

## <a name="event-hubs-roles-and-permissions"></a>Olay hub'ları rolleri ve izinleri

İlk genel Önizleme sürümünde, yalnızca bir yönetilen hizmet kimliği kimlik ad alanındaki tüm varlıklara tam denetim veren bir Event Hubs ad alanının "Sahip" veya "Katılımcı" rollere de ekleyebilirsiniz. Ancak, ad alanı topoloji değişikliği işlemler: başlangıçta yönetim Azure Resource Manager yalnızca ancak desteklenen yerel Event Hubs REST yönetim arabirimi üzerinden değil. Bu destek, ayrıca .NET Framework istemci kullanamazsınız gelir [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi içinde bir yönetilen hizmet kimliği. 
 
## <a name="use-event-hubs-with-a-managed-service-identity"></a>Yönetilen hizmet kimliği ile Event hubs'ı kullanın

Aşağıdaki bölümde bir yönetilen hizmet kimliği, nasıl bir Event Hubs ad alanı, kimlik erişim vermek ve uygulaması kullanarak event hubs ile nasıl etkileşim kurduğunu altında çalışan bir örnek uygulaması oluşturma ve dağıtma için gereken adımlar açıklanmaktadır. kimlik.

Barındırılan bir web uygulaması bu tanıtımda açıklanmaktadır [Azure App Service](https://azure.microsoft.com/services/app-service/). Bir VM tarafından barındırılan uygulama için gerekli adımlar benzerdir.

### <a name="create-an-app-service-web-application"></a>Bir App Service web uygulaması oluşturma

İlk adım, bir App Service ASP.NET uygulaması oluşturmaktır. Azure'da nasıl yapılacağı hakkında bilgi sahibi değilseniz izleyin [bu nasıl yapılır kılavuzunda](../app-service/app-service-web-get-started-dotnet-framework.md). Ancak, öğreticide gösterilen şekilde bir MVC uygulaması oluşturmak yerine, bir Web Forms uygulaması oluşturun.

### <a name="set-up-the-managed-service-identity"></a>Yönetilen hizmet kimliğini ayarlayın

Uygulamayı oluşturduktan sonra (nasıl yapılır makalesinde de gösterilmiştir) Azure portalında yeni oluşturulan web uygulamasına gidin ve ardından gitmek **yönetilen hizmet kimliği** sayfasını ve özelliğini etkinleştirin: 

![](./media/event-hubs-managed-service-identity/msi1.png)
 
Özelliği etkinleştirdikten sonra yeni bir hizmet kimliği Azure Active Directory'niz içinde oluşturulur ve App Service ana bilgisayar yapılandırılmış.

### <a name="create-a-new-event-hubs-namespace"></a>Yeni Event Hubs ad alanı oluşturma

Ardından, [bir Event Hubs ad alanı oluşturma](event-hubs-create.md) MSI Önizleme desteği olan Azure bölgelerinden birini: **ABD Doğu**, **ABD Doğu 2**, veya **Batı Avrupa**. 

Ad alanınıza gidin **erişim denetimi (IAM)** sayfasında portalda ve ardından **Ekle** için Yönetilen hizmet kimliği eklemek için **sahibi** rol. Bunu yapmak için web uygulamasının adını arayın **izinleri eklemek** paneli **seçin** alan ve sonra giriş'e tıklayın. Daha sonra **Kaydet**'e tıklayın.

![](./media/event-hubs-managed-service-identity/msi2.png)
 
Yönetilen hizmet kimliği artık web uygulaması için Event Hubs ad alanına erişimi vardır ve daha önce oluşturduğunuz olay hub'ına. 

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Artık oluşturduğunuz ASP.NET uygulamasının varsayılan sayfasını değiştirin. Web uygulama kodundan kullanabilirsiniz [bu GitHub deposundan](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp). 

Uygulamayı başlattıktan sonra tarayıcınızı EventHubsMSIDemo.aspx için işaretleyin. Alternatif olarak, başlangıç sayfası olarak ayarlayın. Kod EventHubsMSIDemo.aspx.cs dosyasında bulunabilir. Bazı giriş alanları ile birlikte en az bir web uygulaması sonucudur **Gönder** ve **alma** olaylarını almak veya göndermek için Event Hubs'a bağlanma düğmeleri. 

Not nasıl [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) nesnesi. Paylaşılan erişim belirteci (SAS) belirteç sağlayıcısı kullanmak yerine, kod ile yönetilen hizmet kimliği için bir belirteç sağlayıcısı oluşturur `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` çağırın. Bu nedenle, korumak ve kullanmak için gizli dizi vardır. Olay hub'larına ve yetkilendirme el sıkışması yönetilen hizmet kimliği bağlamı akışını otomatik olarak tarafından işlenmesini SAS kullanmaktan daha basit bir model belirteç sağlayıcısı.

Bu değişiklikleri yaptıktan sonra yayımlama ve uygulamayı çalıştırın. İndirmek ve ardından Visual Studio'da bir yayımlama profilini içeri aktarmak için doğru yayımlama verileri almak için kolay bir yol verilmiştir:

![](./media/event-hubs-managed-service-identity/msi3.png)
 
İleti göndermek veya almak için ad alanının adı ve oluşturduğunuz varlığın adını girin, ardından tıklayın **Gönder** veya **alma**. 
 
Yönetilen hizmet kimliği yalnızca Azure ortamı içinde ve yalnızca içinde yapılandırdığınız App Service dağıtımı çalıştığını unutmayın. Ayrıca, yönetilen hizmet kimlikleri şu anda App Service dağıtım yuvaları ile çalışmaz unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Olay hub'ları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)