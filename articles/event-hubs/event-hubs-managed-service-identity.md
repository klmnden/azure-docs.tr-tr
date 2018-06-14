---
title: Azure Event Hubs preview ile hizmet kimliği yönetilen | Microsoft Docs
description: Azure Event Hubs ile yönetilen hizmet kimlikleri kullanın
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2017
ms.author: sethm
ms.openlocfilehash: dd50e4f6ebc5fdf5496a5127fde20bd052087b59
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
ms.locfileid: "26783474"
---
# <a name="managed-service-identity-preview"></a>Yönetilen hizmet kimliği (Önizleme)

Yönetilen hizmet kimliği (MSI), uygulama kodunuzun çalıştığı dağıtımla ilişkili güvenli bir kimlik oluşturmanızı sağlayan bir çapraz Azure özelliğidir. Ardından, uygulamanız gereken belirli Azure kaynaklarına erişmek için özel izinleri erişim denetimi rolleri kimliğe ilişkilendirebilirsiniz. 

MSI ile Azure platformu bu çalışma zamanı kimlik yönetir. Depolamak ve uygulama kodu veya yapılandırma, kimlik için veya erişmek için ihtiyacınız olan kaynakları için erişim tuşları korumak gerekmez. Azure App Service uygulama içinde veya bir sanal makine etkin MSI desteği ile çalışan bir olay hub'ları istemci uygulama tanıtıcısı SAS kuralları ve anahtarları veya başka bir erişim belirteci gerekmez. İstemci uygulama, yalnızca olay hub'ları ad uç noktası adresi gerekir. Uygulama bağlandığında, olay hub'ları MSI bağlam örneği bu makalenin sonraki bölümlerinde gösterilen bir işlem istemcisinde bağlar.

Yönetilen hizmet kimliği ile ilişkili olduğunda, Event Hubs istemcisi tüm yetkili işlemler gerçekleştirebilir. Yetkilendirme, olay hub'ları rolleriyle bir MSI ilişkilendirerek verilir. 

## <a name="event-hubs-roles-and-permissions"></a>Olay hub'ları rolleri ve izinleri

İlk genel Önizleme sürümü için bir yönetilen hizmet kimliği ad alanındaki tüm varlıklara üzerinde kimlik tam denetim verir bir olay hub'ları ad alanı "Sahibi" veya "Katkıda" rolleri yalnızca ekleyebilirsiniz. Ancak, ad alanı topoloji değişikliği operations olan ilk yönetim Azure Resource Manager yalnızca ancak desteklenen yerel olay hub'ları REST yönetim arabirimi üzerinden değil. .NET Framework istemci kullanamazsınız bu desteği de anlamına [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi içinde bir yönetilen hizmet kimliği. 
 
## <a name="use-event-hubs-with-a-managed-service-identity"></a>Yönetilen hizmet kimliği ile olay hub'ları kullanın

Aşağıdaki bölümde oluşturmak ve yönetilen hizmet kimliği, olay hub'ları ad alanı, kimlik erişim vermek nasıl ve uygulaması kullanarak event hubs ile nasıl etkileşim kurduğunu altında çalışan örnek bir uygulama dağıtmak için gereken adımlar açıklanmaktadır kimliği.

Bu giriş barındırılan bir web uygulaması açıklar [Azure App Service](https://azure.microsoft.com/services/app-service/). VM barındırdığı bir uygulama için gerekli adımları benzerdir.

### <a name="create-an-app-service-web-application"></a>Bir App Service web uygulaması oluşturma

İlk adım, bir App Service ASP.NET uygulaması oluşturmaktır. Azure'da nasıl yapılacağı hakkında bilgi sahibi değilseniz izleyin [yapılır bu kılavuzda](../app-service/app-service-web-get-started-dotnet-framework.md). Ancak, öğreticide gösterildiği gibi bir MVC uygulaması oluşturmak yerine, bir Web Forms uygulaması oluşturma.

### <a name="set-up-the-managed-service-identity"></a>Yönetilen hizmet kimliği ayarlayın

Uygulamayı oluşturduktan sonra (aynı zamanda nasıl yapılır gösterilen) Azure portalında yeni oluşturulan web uygulamasına gidin ve ardından gidin **yönetilen hizmet kimliği** sayfasında ve özelliği etkinleştirin: 

![](./media/event-hubs-managed-service-identity/msi1.png)
 
Özellik etkinleştirdikten sonra yeni bir hizmet kimliği Azure Active Directory'yi oluşturulur ve uygulama hizmeti ana bilgisayara yapılandırılmış.

### <a name="create-a-new-event-hubs-namespace"></a>Yeni bir olay hub'ları ad alanı oluşturma

İleri [bir olay hub'ları ad alanı oluşturma](event-hubs-create.md) MSI Önizleme desteğine sahip Azure bölgelerinden birinde: **BİZE Doğu**, **ABD Doğu 2**, veya **Batı Avrupa**. 

Ad alanına gidin **erişim denetimi (IAM)** sayfasında portalda ve ardından **Ekle** yönetilen hizmet kimliği eklemek için **sahibi** rol. Bunu yapmak için web uygulamasının adını arayın **izinleri eklemek** Masası **seçin** alan ve sonra giriş'i tıklatın. Daha sonra **Kaydet**'e tıklayın.

![](./media/event-hubs-managed-service-identity/msi2.png)
 
Web uygulamasının yönetilen hizmet kimliği olay hub'ları ad alanına erişimi artık sahiptir ve daha önce oluşturduğunuz olay hub'ına. 

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Şimdi varsayılan sayfa oluşturduğunuz ASP.NET uygulamasının değiştirin. Ayrıca web uygulama kodundan kullanabilirsiniz [bu GitHub deposunu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp). 

Uygulamanızı başlattıktan sonra tarayıcınızı için EventHubsMSIDemo.aspx gelin. Alternatif olarak, başlangıç sayfanızı ayarlayın. Kod EventHubsMSIDemo.aspx.cs dosyasında bulunabilir. Bir en az bir web uygulaması birkaç giriş alanları ile birlikte sonucudur **Gönder** ve **alma** göndermek veya iletileri almak için Event Hubs bağlanmak düğmeler. 

Not nasıl [Eventhubclient](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) nesne başlatılır. Paylaşılan erişim belirteci (SAS) belirteci sağlayıcısı kullanmak yerine, bir belirteç sağlayıcısı ile yönetilen hizmet kimliği için kod oluşturur `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` çağırın. Bu nedenle, korumak ve kullanmak için hiçbir gizli vardır. Olay hub'ları ve yetkilendirme el sıkışma için Yönetilen hizmet kimlik bağlamını akışını otomatik olarak tarafından işlenmesini SAS kullanmaktan daha basit bir modeldir belirteç sağlayıcısı.

Bu değişiklikleri yaptıktan sonra yayımlama ve uygulamayı çalıştırın. Karşıdan yüklemek ve bir yayımlama profili Visual Studio'da almak için doğru yayımlama verileri elde etmek için kolay bir yoludur.:

![](./media/event-hubs-managed-service-identity/msi3.png)
 
İleti göndermek veya almak için ad alanı ve oluşturduğunuz varlığın adı girin ve ardından ya da **Gönder** veya **almak**. 
 
Yönetilen hizmet kimliği yalnızca Azure ortamı içindeki ve yalnızca içinde yapılandırdığınız uygulama hizmeti dağıtımı çalıştığını unutmayın. Ayrıca, yönetilen hizmet kimlikleri uygulama hizmeti dağıtım yuvası ile şu anda çalışmıyor unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Olay hub'ı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)