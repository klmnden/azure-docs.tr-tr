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
ms.date: 05/20/2019
ms.author: shvija
ms.openlocfilehash: 4e6f16a15547583baab63f452504d36eb2e43b85
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978444"
---
# <a name="managed-identities-for-azure-resources-with-event-hubs"></a>Event Hubs ile Azure kaynakları için yönetilen kimlikleri

[Kimlikler Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md) uygulama kodunuzun çalıştığı dağıtımla ilişkili güvenli bir kimlik oluşturmanızı sağlayan bir çapraz Azure özelliğidir. Ardından, uygulamanızın belirli Azure kaynaklarına erişmek için özel izinler erişim denetimi rolleri kimliğe ilişkilendirebilirsiniz. 

İle yönetilen kimlikleri, Azure platformu bu çalışma zamanı kimlik yönetir. Depolayın ve uygulama kodu veya yapılandırma, kimlik için veya erişmek için ihtiyacınız olan kaynakları için erişim anahtarlarını korumak gerekmez. SAS kuralları ve anahtarlar ya da herhangi bir erişim belirteçleri işlemek Event Hubs, Azure kaynaklarını desteği etkin yönetilen kimliklerle bir Azure App Service uygulama içinde veya bir sanal makinede çalışan bir istemci uygulaması gerekmez. İstemci uygulaması, uç nokta adresi Event Hubs ad alanının yalnızca gerekir. Uygulamaya bağlandığında, Event Hubs yönetilen kimliğin bağlam istemci örneği bu makalenin sonraki bölümlerinde gösterilen işleminde bağlar.

Yönetilen bir kimlikle ilişkili olduğunda, Event Hubs istemcisi tüm yetkili işlemleri de yapabilirsiniz. Event Hubs rolleri ile bir yönetilen kimlik ilişkilendirerek yetkilendirme izni verilir. 

## <a name="event-hubs-roles-and-permissions"></a>Olay hub'ları rolleri ve izinleri
Yönetilen bir kimlik için ekleyebilirsiniz **olay hub'ları veri sahibi** Event Hubs ad alanının rol. Bu rol, kimlik, ad alanındaki tüm varlıklar (Yönetim ve veri işlemleri için) tam denetim verir.

>[!IMPORTANT]
> Biz daha önce desteklenen yönetilen kimliğe ekleme **sahibi** veya **katkıda bulunan** rol. Ancak, veri ayrıcalıklarına erişim **sahibi** ve **katkıda bulunan** rolü artık dikkate alınır. Kullanıyorsanız **sahibi** veya **katkıda bulunan** rolü kullanmanın anahtar **olay hub'ları veri sahibi** rol.

Yeni yerleşik rolü kullanmak için aşağıdaki adımları izleyin: 

1. [Azure portalına](https://portal.azure.com) gidin
2. Event Hubs ad alanına gidin.
3. Üzerinde **olay hub'ları Namespace** sayfasında **erişim Control(IAM)** sol menüden.
4. Üzerinde **erişim denetimi (IAM)** sayfasında **Ekle** içinde **bir rol ataması Ekle** bölümü. 

    ![Rol ataması düğmesi ekleme](./media/event-hubs-managed-service-identity/add-role-assignment-button.png)
5. Üzerinde **rol ataması Ekle** sayfasında, aşağıdaki adımları uygulayın: 
    1. İçin **rol**seçin **Azure olay hub'ları veri sahibi**. 
    2. Seçin **kimlik** role eklenecek.
    3. **Kaydet**’i seçin. 

        ![Olay hub'ları veri sahip rolü](./media/event-hubs-managed-service-identity/add-role-assignment-dialog.png)
6. Geçiş **rol atamaları** sayfa ve kullanıcı eklendiğini onaylayın **Azure olay hub'ları veri sahibi** rol. 

    ![Kullanıcı rolüne eklenir onaylayın](./media/event-hubs-managed-service-identity/role-assignments.png)
 
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

Ardından, [bir Event Hubs ad alanı oluşturma](event-hubs-create.md). 

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
