---
title: "API Apps hizmetine giriş | Microsoft Belgeleri"
description: "Azure App Service’in RESTful API’lerini geliştirmenize, barındırmanıza ve kullanmanıza nasıl yardımcı olduğunu öğrenin."
services: app-service\api
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: 
ms.assetid: 60049a16-8159-47aa-a34b-110be0d8dab6
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eda73900ded4c587bacfa3b4d4e8465c1de5a5ed


---
# <a name="api-apps-overview"></a>API Apps’e genel bakış
Azure App Service’deki API uygulamaları, API’leri bulutta ve şirket içinde geliştirmeyi, barındırmayı ve kullanmayı kolaylaştıran özellikler sunar. API uygulamaları ile kurumsal düzeyde güvenlik, basit erişim denetimi, karma bağlantı, otomatik SDK oluşturma ve [Logic Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md) ile sorunsuz tümleştirme elde edersiniz.

[Azure App Service](../app-service/app-service-value-prop-what-is.md) web, mobil ve tümleştirme senaryoları için tam yönetilen bir platformdur. API Apps, [Azure App Service](../app-service/app-service-value-prop-what-is.md) tarafından sunulan dört uygulama türünden biridir.

![Azure App Service’deki uygulama türleri](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>API Apps neden kullanılır?
API Apps’in önemli özelliklerinden bazıları şunlardır:

* **Mevcut API’nizi olduğu gibi getirme** - API Apps’ten yararlanabilmek için mevcut API’lerinizdeki herhangi bir kodu değiştirmeniz gerekli değildir; kodunuzu API uygulamasına dağıtmanız yeterlidir. API’niz App Service tarafından desteklenen ASP.NET, C#, Java, PHP, Node.js ve Python gibi bir dili veya çerçeveyi kullanabilir.
* **Kolay kullanım** - [Swagger API’si meta verileri](http://swagger.io/) ile tümleşik destek, API’lerinizi çeşitli istemciler tarafından kullanılabilir hale getirir.  API’leriniz için C#, Java ve Javascript gibi çeşitli dillerde istemci kodlarını otomatik olarak oluşturun. Kodunuzu değiştirmeden [CORS](app-service-api-cors-consume-javascript.md)’u kolayca yapılandırın. Daha fazla bilgi için bkz. [API bulma ve kod oluşturma için App Service API Apps meta verileri](app-service-api-metadata.md) ve [CORS kullanarak JavaScript’ten bir API uygulaması kullanma](app-service-api-cors-consume-javascript.md). 
* **Basit erişim denetimi** - Kodunuzda herhangi bir değişiklik olmadan bir API uygulamasını kimliği doğrulanmamış erişimden koruyun. Yerleşik kimlik doğrulama hizmetleri diğer hizmetlerin veya kullanıcıları temsil eden diğer istemcilerin erişimine karşı API'lerin güvenliğini sağlar. Desteklenen kimlik sağlayıcıları Azure Active Directory, Facebook, Twitter, Google ve Microsoft Hesabı’dır. İstemciler Active Directory Authentication Library (ADAL) ve Mobile Apps SDK’sını kullanabilir. Daha fazla bilgi için bkz. [Azure App Service’de API Apps için kimlik doğrulama ve yetkilendirme](app-service-api-authentication.md).
* **Visual Studio tümleştirmesi** - Visual Studio’daki ayrılmış araçlar API uygulamaları oluşturma, dağıtma, kullanma, hata ayıklama ve yönetme işlemlerini kolaylaştırır. Daha fazla bilgi için bkz. [.NET için Azure SDK 2.8.1 ile tanışın](/blog/announcing-azure-sdk-2-8-1-for-net/).
* **Logic Apps ile tümleştirme** - Oluşturduğunuz API uygulamaları [App Service Logic Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md) tarafından kullanılabilir.  Daha fazla bilgi için bkz. [Logic Apps ile App Service üzerinde barındırılan özel API’nizi kullanma](../app-service-logic/app-service-logic-custom-hosted-api.md) ve [Yeni şema sürüm 2015-08-01-önizlemesi](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Ayrıca, bir API uygulaması [Web Apps](../app-service-web/app-service-web-overview.md) ve [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) tarafından sunulan özelliklerden yararlanabilir. Bunun tersi de geçerlidir; bir API’yi barındırmak için web uygulaması veya mobil uygulama kullanıyorsanız, istemci kodu oluşturmak için Swagger meta verileri ve etki alanları arası tarayıcı erişimi için CORS gibi API Apps özelliklerinden yararlanabilir. Üç uygulama türü (API, web, mobil) arasındaki tek fark Azure portalında bunlar için kullanılan ad ve simgedir.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>API Apps ile Azure API Management arasındaki fark nedir?
API Apps ve [Azure API Management](../api-management/api-management-key-concepts.md) birbirini tamamlayan hizmetlerdir:

* API Management, API’lerin yönetilmesi ile ilgilidir. Kullanımı izlemek ve kısıtlamak, giriş ve çıkışı yönlendirmek, birkaç API’yi bir uç noktada birleştirmek ve benzeri amaçlarla API Management ön ucunu bir API üzerine yerleştirebilirsiniz. Yönetilen API'ler herhangi bir yerde barındırılabilir.
* API Apps, API’lerin barındırılmasıyla ilgilidir. Hizmet, API’lerin geliştirilmesini ve kullanılmasını kolaylaştıran özellikler içerir, ancak API Management’ın yaptığı izleme, kısıtlama, yönlendirme veya birleştirme işlemlerini gerçekleştirmez. API Management özelliklerine ihtiyaç duymuyorsanız uygulamaları API Management kullanmadan API Uygulamaları içinde barındırabilirsiniz.

Aşağıdaki diyagram, API Uygulamaları ve diğer yerlerde barındırılan API’ler için kullanılan API Management’i göstermektedir.

![Azure API Management ve API Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

API Management ve API Apps’in bazı özellikleri benzer işlevlere sahiptir.  Örneğin, her ikisi de CORS desteğini otomatik hale getirebilir. İki hizmeti birlikte gördüğünüzde, API uygulamalarınızın ön ucu olarak görev yaptığından API Management’ı CORS için kullanabilirsiniz. 

## <a name="getting-started"></a>Başlarken
Birine örnek kod dağıtarak API Apps hizmetini kullanmaya başlamak için tercih ettiğiniz çerçeveye ait öğreticiye bakın:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

API uygulamaları hakkında soru sormak için [API Apps forumunda](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps) bir ileti dizisi başlatın. 




<!--HONumber=Nov16_HO2-->


