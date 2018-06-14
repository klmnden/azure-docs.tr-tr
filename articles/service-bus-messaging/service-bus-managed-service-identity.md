---
title: Azure Service Bus preview ile hizmet kimliği yönetilen | Microsoft Docs
description: Yönetilen hizmet kimlikleri Azure Service Bus ile kullanmak
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
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 7b9901ee3478cb193c808b65d2dbbcf8b596a3c1
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
ms.locfileid: "29874661"
---
# <a name="managed-service-identity-preview"></a>Yönetilen hizmet kimliği (Önizleme)

Yönetilen hizmet kimliği (MSI), uygulama kodunuzun çalıştığı dağıtımla ilişkili güvenli bir kimlik oluşturmanızı sağlayan bir çapraz Azure özelliğidir. Ardından, uygulamanız gereken belirli Azure kaynaklarına erişmek için özel izinleri erişim denetimi rolleri kimliğe ilişkilendirebilirsiniz.

MSI ile Azure platformu bu çalışma zamanı kimlik yönetir. Depolamak ve uygulama kodu veya yapılandırma, kimlik için veya erişmek için ihtiyacınız olan kaynakları için erişim tuşları korumak gerekmez. Azure App Service uygulama içinde veya bir sanal makine etkin MSI desteği ile çalışan bir Service Bus istemci uygulaması tanıtıcısı SAS kuralları ve anahtarları veya başka bir erişim belirteci gerekmez. İstemci uygulama, yalnızca Service Bus Mesajlaşma hizmeti ad alanı uç noktası adresi gerekir. Uygulama bağlandığında, hizmet veri yolu MSI bağlam örneği bu makalenin sonraki bölümlerinde gösterilen bir işlem istemcisinde bağlar. 

Yönetilen hizmet kimliği ile ilişkili olduğunda, Service Bus istemci tüm yetkili işlemler gerçekleştirebilir. Yetkilendirme, Service Bus rolleriyle bir MSI ilişkilendirerek verilir. 

## <a name="service-bus-roles-and-permissions"></a>Hizmet veri yolu rolleri ve izinleri

İlk genel Önizleme sürümü için bir yönetilen hizmet kimliği ad alanındaki tüm varlıklara üzerinde kimlik tam denetim verir bir hizmet veri yolu ad alanı, "Sahibi" veya "Katkıda" rollerini yalnızca ekleyebilirsiniz. Ancak, ad alanı topoloji değişikliği operations olan ilk yönetim Azure Resource Manager yalnızca ancak desteklenen yerel Service Bus REST yönetim arabirimi üzerinden değil. .NET Framework istemci kullanamazsınız bu desteği de anlamına [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nesnesi içinde bir yönetilen hizmet kimliği.

## <a name="use-service-bus-with-a-managed-service-identity"></a>Hizmet veri yolu bir yönetilen hizmet kimliği kullanın

Aşağıdaki bölümde oluşturmak ve yönetilen hizmet kimliği, nasıl bir Service Bus Mesajlaşma hizmeti ad alanı, kimlik erişim vermek ve uygulama Service Bus ile nasıl etkileşim kurduğu altında çalışan örnek bir uygulama dağıtmak için gereken adımlar açıklanmaktadır Bu kimlik bilgileriniz kullanılarak varlıklar.

Bu giriş barındırılan bir web uygulaması açıklar [Azure App Service](https://azure.microsoft.com/services/app-service/). VM barındırdığı bir uygulama için gerekli adımları benzerdir.

### <a name="create-an-app-service-web-application"></a>Bir App Service web uygulaması oluşturma

İlk adım, bir App Service ASP.NET uygulaması oluşturmaktır. Azure'da nasıl yapılacağı hakkında bilgi sahibi değilseniz izleyin [yapılır bu kılavuzda](../app-service/app-service-web-get-started-dotnet-framework.md). Ancak, öğreticide gösterildiği gibi bir MVC uygulaması oluşturmak yerine, bir Web Forms uygulaması oluşturma.

### <a name="set-up-the-managed-service-identity"></a>Yönetilen hizmet kimliği ayarlayın

Uygulamayı oluşturduktan sonra (aynı zamanda nasıl yapılır gösterilen) Azure portalında yeni oluşturulan web uygulamasına gidin ve ardından gidin **yönetilen hizmet kimliği** sayfasında ve özelliği etkinleştirin: 

![](./media/service-bus-managed-service-identity/msi1.png)

Özellik etkinleştirdikten sonra yeni bir hizmet kimliği Azure Active Directory'yi oluşturulur ve uygulama hizmeti ana bilgisayara yapılandırılmış.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Yeni bir Service Bus Mesajlaşma hizmeti ad alanı oluşturma

İleri [Service Bus Mesajlaşma hizmeti ad alanı oluşturma](service-bus-create-namespace-portal.md) RBAC Önizleme desteğine sahip Azure bölgelerinden birinde: **BİZE Doğu**, **ABD Doğu 2**, veya **Batı Avrupa** . 

Ad alanına gidin **erişim denetimi (IAM)** sayfasında portalda ve ardından **Ekle** yönetilen hizmet kimliği eklemek için **sahibi** rol. Bunu yapmak için web uygulamasının adını arayın **izinleri eklemek** Masası **seçin** alan ve sonra giriş'i tıklatın. Daha sonra **Kaydet**'e tıklayın.

![](./media/service-bus-managed-service-identity/msi2.png)
 
Web uygulamasının yönetilen hizmet kimliği artık Service Bus ad alanı erişimi olur ve daha önce oluşturduğunuz kuyruğa. 

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Şimdi varsayılan sayfa oluşturduğunuz ASP.NET uygulamasının değiştirin. Ayrıca web uygulama kodundan kullanabilirsiniz [bu GitHub deposunu](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ManagedServiceIdentity).

Default.aspx sayfasında, giriş sayfasıdır. Kod Default.aspx.cs dosyasında bulunabilir. En az web uygulaması birkaç giriş alanları ile birlikte sonucudur **Gönder** ve **alma** göndermek veya iletileri almak için Service Bus hizmetine bağlanmak düğmeler.

Not nasıl [Eventhubclient](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) nesne başlatılır. Paylaşılan erişim belirteci (SAS) belirteci sağlayıcısı kullanmak yerine, bir belirteç sağlayıcısı ile yönetilen hizmet kimliği için kod oluşturur `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)` çağırın. Bu nedenle, korumak ve kullanmak için hiçbir gizli vardır. Hizmet veri yolu ve yetkilendirme el sıkışma için Yönetilen hizmet kimlik bağlamını akışını otomatik olarak tarafından işlenmesini SAS kullanmaktan daha basit bir modeldir belirteç sağlayıcısı.

Bu değişiklikleri yaptıktan sonra yayımlama ve uygulamayı çalıştırın. Karşıdan yüklemek ve bir yayımlama profili Visual Studio'da almak için doğru yayımlama verileri elde etmek için kolay bir yoludur.:

![](./media/service-bus-managed-service-identity/msi3.png)
 
İleti göndermek veya almak için ad alanı ve oluşturduğunuz varlığın adı girin ve ardından ya da **Gönder** veya **almak**.
 
Yönetilen hizmet kimliği yalnızca Azure ortamı içindeki ve yalnızca içinde yapılandırdığınız uygulama hizmeti dağıtımı çalıştığını unutmayın. Ayrıca, yönetilen hizmet kimlikleri uygulama hizmeti dağıtım yuvası ile şu anda çalışmıyor unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)