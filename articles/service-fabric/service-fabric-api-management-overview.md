---
title: API Management genel bakış ile Azure Service Fabric | Microsoft Docs
description: Bu makale, Service Fabric uygulamaları için bir ağ geçidi olarak Azure API Yönetimi'ni kullanarak bir giriş var.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: vturecek
ms.openlocfilehash: 6bf7ea90bb5351411984110fd8fb05c2f8cb0650
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-with-azure-api-management-overview"></a>Service Fabric ile Azure API Management genel bakış

Bulut uygulamalarının normalde kullanıcılar, cihazlar ve diğer uygulamalara tek giriş noktası sağlamak için bir ön uç ağ geçidine ihtiyacı vardır. Service Fabric bir ağ geçidi herhangi bir durum bilgisiz hizmeti gibi olabilir bir [ASP.NET Core uygulama](service-fabric-reliable-services-communication-aspnetcore.md), veya başka bir hizmeti gibi trafik giriş için tasarlanmış [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/), veya [Azure API Management](https://docs.microsoft.com/azure/api-management/).

Bu makale, Service Fabric uygulamaları için bir ağ geçidi olarak Azure API Yönetimi'ni kullanarak bir giriş var. API Management Service Fabric, arka uç Service Fabric hizmetlerinize yönlendirme kuralları zengin bir dizi API yayımlamanıza olanak tanıyan ile doğrudan tümleşir. 

## <a name="architecture"></a>Mimari
Ortak bir Service Fabric mimarisi HTTP API'leri arka uç hizmetlerini HTTP çağrıları yapan bir tek sayfalı web uygulaması kullanır. [Service Fabric başlama örnek uygulama](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) Bu mimarinin bir örneği gösterir.

Bu senaryoda, bir durum bilgisi olmayan web hizmeti için Service Fabric uygulamaya ağ geçidi olarak görev yapar. Bu yaklaşım, aşağıdaki çizimde gösterildiği gibi arka uç hizmetlerine proxy HTTP isteğinin bir web hizmeti yazmak gerektirir:

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-web-app-stateless-gateway]

Uygulamaları karmaşıklığı arttıkça, bu nedenle bir API çok arka uç hizmetlerini önünde sunmalıdır ağ geçitleri yapın. Azure API Management yönlendirme kuralları, erişim denetimi, hız sınırlaması, izleme, olay günlüğünü ve bölümünüzün minimum çabayla yanıt önbelleğe alma ile karmaşık API'leri işlemek için tasarlanmıştır. Azure API Management Service Fabric hizmet bulma, bölüm çözümleme ve çoğaltma seçimi durum bilgisiz kendi API ağ geçidi yazmak zorunda kalmamak için akıllıca isteklerini doğrudan arka uç Service Fabric Hizmetleri'nde yönlendirmek için destekler. 

HTTP API çağrıları yönetilir ve aşağıdaki çizimde gösterildiği gibi Azure API Management yönlendirilmiş sırada bu senaryoda, web kullanıcı Arabirimi hala bir web hizmeti aracılığıyla sunulur:

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-web-app]

## <a name="application-scenarios"></a>Uygulama senaryoları

Durum bilgisiz ya da durum bilgisi olan Service Fabric Hizmetleri'nde olabilir ve üç düzenleri birini kullanarak bölümlendirilebilir: singleton, int 64 aralığı ve adlandırılmış. Hizmet uç noktası çözümleme belirli hizmet örneği, belirli bir bölüm tanımlayan gerektirir. Bir uç nokta bir hizmetin her iki hizmet örneği adı çözülürken (örneğin, `fabric:/myapp/myservice`) yanı sıra belirli bir bölüm hizmetinin belirtilmelidir, söz konusu olduğunda tek bölüm dışındaki.

Azure API Management durum bilgisi olmayan hizmetler, durum bilgisi olan hizmetler ve tüm bölümleme düzeni, herhangi bir birleşimini ile kullanılabilir.

## <a name="send-traffic-to-a-stateless-service"></a>Durum bilgisiz hizmete trafiği Gönder

En basit durumda, trafik için bir durum bilgisiz hizmet örneği iletilir. Bunun için bir Service Fabric Service Fabric arka uç özel durum bilgisiz hizmet örneğinde eşleyen uç gelen işleme ilkesiyle bir API Management işlemi içerir. Bu hizmete gönderilen istekleri rastgele bir durum bilgisiz hizmet örneği kopyasına gönderilir.

#### <a name="example"></a>Örnek
Aşağıdaki senaryoda, bir Service Fabric uygulaması adlı bir durum bilgisi olmayan hizmetin içeren `fabric:/app/fooservice`, iç HTTP API gösterir. Hizmet örneği adı tanınmış ve doğrudan gelen işleme API Management ilkesinde sabit kodlanmış olabilir. 

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-static-stateless]

## <a name="send-traffic-to-a-stateful-service"></a>Durum bilgisi olan bir hizmet için trafiği Gönder

Durum bilgisiz servis senaryosu benzer, trafik bir durum bilgisi olan hizmet örneğine iletilebilir. Bu durumda, bir API Management işlem bir Service Fabric belirli bir belirli bir bölüm için bir istek eşleyen uç gelen işleme ilkesiyle içeren *durum bilgisi olan* hizmet örneği. Bazı girdi bir URL yolu değerinde gibi gelen HTTP istek kullanarak bir lambda yöntemle hesaplanan üzere her istek eşlemek için bölüm. İlke, yalnızca birincil çoğaltma veya okuma işlemleri için rastgele bir çoğaltma istekleri göndermek için yapılandırılabilir.

#### <a name="example"></a>Örnek

Aşağıdaki senaryoda, bir Service Fabric uygulaması adlı bölümlenmiş bir durum bilgisi olan hizmeti içeren `fabric:/app/userservice` bir iç HTTP API gösterir. Hizmet örneği adı tanınmış ve doğrudan gelen işleme API Management ilkesinde sabit kodlanmış olabilir.  

Hizmet Int64 bölüm düzenini kullanarak iki bölüm ve kapsayan bir anahtar aralığı bölümlenmiş `Int64.MinValue` için `Int64.MaxValue`. Arka uç İlkesi dönüştürerek bu aralıktaki bir bölüm anahtarı hesaplar `id` herhangi bir algoritma bölüm anahtarı hesaplamak için burada kullanılabilse de URL istek yolunda bir 64-bit tamsayı olarak sağlanan değer. 

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-static-stateful]

## <a name="send-traffic-to-multiple-stateless-services"></a>Birden fazla durum bilgisi olmayan hizmetler için trafiği Gönder

Daha gelişmiş senaryolarda istekleri için birden fazla hizmet örneği eşleyen bir API Management işlemi tanımlayabilirsiniz. Bu durumda, her işlem istekleri Gelen HTTP istek, durum bilgisi olan hizmetler, söz konusu olduğunda ve URL yolu veya sorgu dizesi gibi bir bölüm hizmet örneği içinde değerlere göre belirli hizmet örneği eşleyen bir ilke içerir. 

Bunun için bir Service Fabric gelen HTTP isteğinden alınan değerlere göre Service Fabric arka uç durum bilgisiz service örneğinde eşleyen uç gelen işleme ilkesiyle bir API Management işlemi içerir. Bir hizmet örneği isteklerine hizmet örneğinin rastgele bir kopyasını gönderilir.

#### <a name="example"></a>Örnek

Bu örnekte, aşağıdaki formül kullanılarak dinamik olarak oluşturulan bir adla uygulamanın her bir kullanıcı için yeni bir durum bilgisiz hizmet örneği oluşturulur:
 
 - `fabric:/app/users/<username>`

 Her hizmet için benzersiz bir ad olsa da, adları eylemli veya yönetici giriş ve böylece APIM ilkeleri veya yönlendirme kuralları sabit kodlanmış olamaz Hizmetleri yanıt kullanıcı olarak oluşturulur çünkü bilinmez. Bir isteğin gönderileceği hizmetin adını arka uç ilke tanımı bunun yerine, üretildi `name` URL istek yolu sağlanan değer. Örneğin:

  - Bir istek `/api/users/foo` hizmet örneğine yönlendirilir `fabric:/app/users/foo`
  - Bir istek `/api/users/bar` hizmet örneğine yönlendirilir `fabric:/app/users/bar`

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-dynamic-stateless]

## <a name="send-traffic-to-multiple-stateful-services"></a>Birden fazla durum bilgisi olan hizmet için trafiği Gönder

Durum bilgisiz hizmet örneğe benzer bir API Management işlem istekleri birden fazla eşleyebilirsiniz **durum bilgisi olan** hizmet örneği, bu, aynı zamanda durumda her bir durum bilgisi olan hizmet örneği için bölüm çözümlemesi gerekebilir.

Bunun için bir Service Fabric gelen HTTP isteğinden alınan değerlere göre Service Fabric arka uç durum bilgisi olan hizmet örneğinde eşleyen uç gelen işleme ilkesiyle bir API Management işlemi içerir. Belirli hizmet örneği için bir istek eşleştirmesi yanı sıra, istek Ayrıca hizmet örneği içinde belirli bir bölüm ve isteğe bağlı olarak birincil çoğaltmayı veya bölüm içinde rastgele bir ikincil çoğaltma için eşlenebilir.

#### <a name="example"></a>Örnek

Bu örnekte, aşağıdaki formül kullanılarak dinamik olarak oluşturulan bir adla uygulama her bir kullanıcı için yeni bir durum bilgisi olan hizmet örneği oluşturulur:
 
 - `fabric:/app/users/<username>`

 Her hizmet için benzersiz bir ad olsa da, adları eylemli veya yönetici giriş ve böylece APIM ilkeleri veya yönlendirme kuralları sabit kodlanmış olamaz Hizmetleri yanıt kullanıcı olarak oluşturulur çünkü bilinmez. Bir isteğin gönderileceği hizmetin adını arka uç ilke tanımı bunun yerine, üretildi `name` değeri sağlanan URL istek yolu. Örneğin:

  - Bir istek `/api/users/foo` hizmet örneğine yönlendirilir `fabric:/app/users/foo`
  - Bir istek `/api/users/bar` hizmet örneğine yönlendirilir `fabric:/app/users/bar`

Her hizmet örneği iki bölüm ve kapsayan bir anahtar aralığı Int64 bölüm düzeni kullanılarak da bölümlenmiş `Int64.MinValue` için `Int64.MaxValue`. Arka uç İlkesi dönüştürerek bu aralıktaki bir bölüm anahtarı hesaplar `id` herhangi bir algoritma bölüm anahtarı hesaplamak için burada kullanılabilse de URL istek yolunda bir 64-bit tamsayı olarak sağlanan değer. 

![Service Fabric ile Azure API Management topolojisine genel bakış][sf-apim-dynamic-stateful]

## <a name="next-steps"></a>Sonraki adımlar

İzleyin [öğretici](service-fabric-tutorial-deploy-api-management.md) ilk Service Fabric kümenizle API Management API Management ve akış isteklerini hizmetlerinize ayarlamak için.

<!-- links -->

<!-- pics -->
[sf-apim-web-app]: ./media/service-fabric-api-management-overview/sf-apim-web-app.png
[sf-web-app-stateless-gateway]: ./media/service-fabric-api-management-overview/sf-web-app-stateless-gateway.png
[sf-apim-static-stateless]: ./media/service-fabric-api-management-overview/sf-apim-static-stateless.png
[sf-apim-static-stateful]: ./media/service-fabric-api-management-overview/sf-apim-static-stateful.png
[sf-apim-dynamic-stateless]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateless.png
[sf-apim-dynamic-stateful]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateful.png