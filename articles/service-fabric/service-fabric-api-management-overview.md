---
title: Azure Service Fabric ile API Management genel bakış | Microsoft Docs
description: Bu makale, Service Fabric uygulamaları için bir ağ geçidi olarak Azure API Yönetimi'ni kullanarak bir giriş niteliğindedir.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: vturecek
ms.openlocfilehash: 0dac2730bcc13b979de6a8faaaa53c0aaf15e902
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60621901"
---
# <a name="service-fabric-with-azure-api-management-overview"></a>Service Fabric ile Azure API Yönetimi'ne genel bakış

Bulut uygulamalarının normalde kullanıcılar, cihazlar ve diğer uygulamalara tek giriş noktası sağlamak için bir ön uç ağ geçidine ihtiyacı vardır. Service Fabric'te, bir ağ geçidi herhangi bir durum bilgisi olmayan hizmet gibi olabilir bir [ASP.NET Core uygulaması](service-fabric-reliable-services-communication-aspnetcore.md), veya başka bir hizmet gibi trafik girişi için tasarlanmış [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/), veya [Azure API Management](https://docs.microsoft.com/azure/api-management/).

Bu makale, Service Fabric uygulamaları için bir ağ geçidi olarak Azure API Yönetimi'ni kullanarak bir giriş niteliğindedir. API Management, zengin bir arka uç Service Fabric hizmetleriniz için yönlendirme kuralları kümesiyle API'ler yayımlamanıza olanak tanıyan, Service Fabric ile doğrudan tümleşir. 

## <a name="availability"></a>Kullanılabilirlik

> [!IMPORTANT]
> Bu özellik kullanılabilir **Premium** ve **Geliştirici** katmanları API Yönetimi'nin gerekli nedeniyle sanal ağ desteği.

## <a name="architecture"></a>Mimari

Ortak bir Service Fabric mimarisi HTTP API'lerini kullanıma sunan arka uç Hizmetleri HTTP çağrıları yapan tek sayfalı web uygulaması kullanır. [Service Fabric kullanmaya başlama örnek uygulamasını](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) Bu mimarinin bir örneği gösterilmektedir.

Bu senaryoda, bir durum bilgisi olmayan web hizmeti, Service Fabric uygulamasına ağ geçidi olarak görev yapar. Bu yaklaşım, arka uç hizmetlerine proxy HTTP isteğinin bir web hizmeti aşağıdaki diyagramda gösterildiği gibi yazmak gerektirir:

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-web-app-stateless-gateway]

Uygulamaların karmaşıklığı arttıkça, bu nedenle çok arka uç Hizmetleri önünde bir API sunması gerekir ağ geçitleri yapın. Azure API Management, karmaşık yönlendirme kuralları, erişim denetimi, hız sınırlaması, izleme, olay günlüğü ve sizin için en az iş ile yanıt önbelleğe alma API'lerle işlemek için tasarlanmıştır. Azure API Management, Service Fabric hizmet bulma, bölümleme çözümlemesi ve çoğaltma seçimi durum bilgisiz kendi API ağ geçidi yazmak zorunda kalmamak için akıllı bir şekilde istekleri Service fabric'teki arka uç hizmetlerine doğrudan yönlendirmeyi destekler. 

HTTP API çağrıları yönetilen ve Azure API Management aracılığıyla Aşağıdaki diyagramda gösterildiği gibi yönlendirilen olsa Bu senaryoda, web kullanıcı Arabirimi yine de bir web hizmeti aracılığıyla sunulur:

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-web-app]

## <a name="application-scenarios"></a>Uygulama senaryoları

Service fabric'te Hizmetleri olabilir durum bilgisi olmayan ya da durum bilgisi olan ve üç düzenlerden birini kullanarak bölümlendirilebilir: tekil, int 64 aralığı ve adlandırılmış. Hizmet uç noktası çözümleme, belirli bir hizmet örneğine belirli bir bölümünü tanımlayan gerektirir. Bir uç nokta bir hizmetin her iki hizmet örneği adı çözülürken (örneğin, `fabric:/myapp/myservice`) hizmetinin belirli bölümü belirtilmelidir olarak, söz konusu olduğunda tek bölüm hariç.

Azure API yönetimi, durum bilgisi olmayan hizmetler, durum bilgisi olan hizmetler ve herhangi bir bölümleme düzeni, herhangi bir birleşimini ile kullanılabilir.

## <a name="send-traffic-to-a-stateless-service"></a>Trafiği bir durum bilgisi olmayan hizmete gönderme

En basit durumda, trafiği durum bilgisi olmayan hizmet örneğine iletilir. Bunu başarmak için bir gelen işlem İlkesi ile bir Service Fabric Service Fabric arka uç, belirli bir durum bilgisi olmayan hizmet örneğine eşleyen uç API Management işlemi içerir. Bu hizmete gönderilen isteklerin rastgele bir durum bilgisi olmayan hizmet örneği kopyasına gönderilir.

#### <a name="example"></a>Örnek
Aşağıdaki senaryoda, bir Service Fabric uygulaması adlı bir durum bilgisi olmayan hizmet içeren `fabric:/app/fooservice`, iç HTTP API sunar. Hizmet örneği adı bilinen ve doğrudan gelen işlem API Management ilkesinde sabit kodlanmış olabilir. 

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-static-stateless]

## <a name="send-traffic-to-a-stateful-service"></a>Bir durum bilgisi olan hizmet için trafiği gönderme

Durum bilgisi olmayan hizmet senaryosu benzer, trafiği durum bilgisi olan hizmet örneğine iletilebilir. Bu durumda, bir gelen işlem İlkesi ile bir Service Fabric, belirli bir bölüme belirli bir istek eşleyen uç bir API Management işlemini içeren *durum bilgisi olan* hizmet örneği. Gelen HTTP istek URL yolu bir değer gibi bazı girişini kullanarak bir lambda yöntemi aracılığıyla hesaplanır üzere her istek eşlemek için bölüm. İlke, yalnızca birincil çoğaltmaya veya okuma işlemleri için rastgele bir çoğaltmaya istekleri göndermek için yapılandırılabilir.

#### <a name="example"></a>Örnek

Aşağıdaki senaryoda, bir Service Fabric uygulaması adlı bölümlendirilmiş bir durum bilgisi olan hizmeti içeren `fabric:/app/userservice` , iç HTTP API sunar. Hizmet örneği adı bilinen ve doğrudan gelen işlem API Management ilkesinde sabit kodlanmış olabilir.  

Hizmet Int64 bölüm şeması iki bölüm ve kapsayan bir anahtar aralığı ile kullanarak bölümlenmiş `Int64.MinValue` için `Int64.MaxValue`. Arka uç İlkesi dönüştürerek bir bölüm anahtarı, aralıktaki hesaplar `id` herhangi bir algoritma burada bölüm anahtarı hesaplamak için kullanılabilir olsa da URL istek yolunda bir 64-bit tamsayıya sağlanan değer. 

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-static-stateful]

## <a name="send-traffic-to-multiple-stateless-services"></a>Birden çok durum bilgisi olmayan hizmetler için trafiği gönderme

Daha gelişmiş senaryolarda, istekleri birden fazla hizmet örneği için eşleşen bir API Management işlemi tanımlayabilirsiniz. Bu durumda, her işlem istekleri için gelen HTTP istek, URL yolu veya sorgu dizesi gibi ve durum bilgisi olan hizmetler söz konusu olduğunda bir bölüm içinde hizmet örneği değerlerine dayalı belirli bir hizmet örneğine eşleyen bir ilke içerir. 

Bunu başarmak için bir gelen işlem İlkesi ile bir Service Fabric gelen HTTP isteğinden alınan değerlere göre Service Fabric arka uç durum bilgisi olmayan hizmet örneğinde eşleyen uç API Management işlemi içerir. Bir hizmet örneği isteklerine hizmet örneği için rastgele bir çoğaltma gönderilir.

#### <a name="example"></a>Örnek

Bu örnekte, bir uygulamanın her bir kullanıcı için aşağıdaki formülü kullanarak dinamik olarak oluşturulan bir adla yeni bir durum bilgisi olmayan hizmet örneği oluşturulur:
 
- `fabric:/app/users/<username>`

  Her hizmet benzersiz bir adı vardır, ancak adlarını ön Hizmetleri yanıt kullanıcı olarak oluşturulur veya yönetici giriş ve bu nedenle APIM ilkeleri ya da yönlendirme kuralları sabit kodlanmış olamaz çünkü bilinmez. Bir istek göndermek hangi hizmetin adını arka uç İlkesi tanımından bunun yerine, oluşturulur `name` URL istek yolunda sağlanan değeri. Örneğin:

  - Bir istek `/api/users/foo` hizmet örneğine yönlendirilir `fabric:/app/users/foo`
  - Bir istek `/api/users/bar` hizmet örneğine yönlendirilir `fabric:/app/users/bar`

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-dynamic-stateless]

## <a name="send-traffic-to-multiple-stateful-services"></a>Trafiği göndermek için birden çok durum bilgisi olan hizmetler

Durum bilgisi olmayan hizmet örneğe benzer bir API Management işlem istekleri birden fazla eşleyebilirsiniz **durum bilgisi olan** hizmet örneği, siz de durumda her bir durum bilgisi olan hizmet örneği için bölümleme çözümlemesi gerçekleştirmek gerekebilir.

Bunu başarmak için bir gelen işlem İlkesi ile bir Service Fabric gelen HTTP isteğinden alınan değerlere göre Service Fabric arka uç durum bilgisi olan hizmet örneğinde eşleyen uç API Management işlemi içerir. Belirli bir hizmet örneği için bir istek eşleme yanı sıra istek Ayrıca hizmet örneği içinde belirli bir bölüme ve isteğe bağlı olarak birincil çoğaltma ya da bölüm içindeki rastgele bir ikincil çoğaltma eşlenebilir.

#### <a name="example"></a>Örnek

Bu örnekte, uygulamanın her bir kullanıcı için aşağıdaki formülü kullanarak dinamik olarak oluşturulan bir adla yeni bir durum bilgisi olan hizmet örneği oluşturulur:
 
- `fabric:/app/users/<username>`

  Her hizmet benzersiz bir adı vardır, ancak adlarını ön Hizmetleri yanıt kullanıcı olarak oluşturulur veya yönetici giriş ve bu nedenle APIM ilkeleri ya da yönlendirme kuralları sabit kodlanmış olamaz çünkü bilinmez. Bir istek göndermek hangi hizmetin adını arka uç İlkesi tanımından bunun yerine, oluşturulur `name` değeri sağlanan URL istek yolu. Örneğin:

  - Bir istek `/api/users/foo` hizmet örneğine yönlendirilir `fabric:/app/users/foo`
  - Bir istek `/api/users/bar` hizmet örneğine yönlendirilir `fabric:/app/users/bar`

Her hizmet örneği, ayrıca Int64 bölüm düzenini kullanarak iki bölüm ve kapsayan bir anahtar aralığı bölümlenen `Int64.MinValue` için `Int64.MaxValue`. Arka uç İlkesi dönüştürerek bir bölüm anahtarı, aralıktaki hesaplar `id` herhangi bir algoritma burada bölüm anahtarı hesaplamak için kullanılabilir olsa da URL istek yolunda bir 64-bit tamsayıya sağlanan değer. 

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-dynamic-stateful]

## <a name="next-steps"></a>Sonraki adımlar

İzleyin [öğretici](service-fabric-tutorial-deploy-api-management.md) ilk Service Fabric kümenizi hizmetleriniz için API yönetimi ve akış isteği API Management aracılığıyla ayarlamak için.

<!-- links -->

<!-- pics -->
[sf-apim-web-app]: ./media/service-fabric-api-management-overview/sf-apim-web-app.png
[sf-web-app-stateless-gateway]: ./media/service-fabric-api-management-overview/sf-web-app-stateless-gateway.png
[sf-apim-static-stateless]: ./media/service-fabric-api-management-overview/sf-apim-static-stateless.png
[sf-apim-static-stateful]: ./media/service-fabric-api-management-overview/sf-apim-static-stateful.png
[sf-apim-dynamic-stateless]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateless.png
[sf-apim-dynamic-stateful]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateful.png