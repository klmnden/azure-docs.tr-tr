---
title: "Service Fabric proje oluşturma sonraki adımlar | Microsoft Docs"
description: "Bu makalede bir dizi çekirdek geliştirme görevi bağlantılar için Service Fabric içerir."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/28/2017
ms.author: rwike77
ms.openlocfilehash: 4523c63493bc9cb3f96713c4abbc1dd1da9130d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>Service Fabric uygulaması ve sonraki adımlar
Azure Service Fabric uygulamanızı oluşturuldu. Bu makalede oluşma şekli projenizi ve bazı olası sonraki adımlar açıklanmaktadır.

## <a name="your-application"></a>Uygulamanızı
Her yeni uygulama, bir uygulama projesi içerir. Bir veya iki ek projeleri, seçilen hizmet türüne bağlı olarak olabilir.

### <a name="the-application-project"></a>Uygulama projesi
Uygulama projesi oluşur:

* Başvurular, uygulamayı oluşturan hizmetlerine kümesi.
* Üç--varsayılan olarak bir küme uç noktası ve yükseltme dağıtımları yapılıp yapılmayacağını ilgili tercihleri gibi farklı ortamları ile çalışmaya yönelik tercihlerini korumak için kullanabileceğiniz profilleri (1-düğümü yerel, 5-Node yerel ve bulut) yayımlayın.
* Üç uygulama parametre (bir hizmet oluşturmak için bölüm sayısı gibi ortama özgü uygulama yapılandırmaları korumak için kullanabileceğiniz aynı yukarıda) dosyaları.
* Komut satırından veya bir otomatik sürekli tümleştirme ve dağıtım ardışık parçası olarak, uygulamanızı dağıtmak için kullanabileceğiniz bir dağıtım komut dosyası.
* Uygulama bildirimi uygulamanın açıklar. Bildirim ApplicationPackageRoot klasörü altında bulabilirsiniz.

### <a name="stateless-service"></a>Durum bilgisiz hizmeti
Yeni bir durum bilgisi olmayan hizmet eklediğinizde, Visual Studio çözümünüze gelen descended türünü içeren hizmet projesi ekler `StatelessService`. Hizmet bir sayaç yerel bir değişkende artırır.

### <a name="stateful-service"></a>Durum bilgisi olan hizmet
Yeni bir durum bilgisi olan hizmet eklediğinizde, Visual Studio çözümünüze gelen descended türünü içeren hizmet projesi ekler `StatefulService`. Hizmet, bir sayaç artırılır kendi `RunAsync` yöntemi ve sonuçta depolayan bir `ReliableDictionary`.

### <a name="actor-service"></a>Aktör hizmeti
Yeni bir güvenilir aktör eklediğinizde, Visual Studio çözümünüz için iki proje ekler: aktör projesinde hem de bir arabirim projesi.

Aktör proje ayarı için yöntemler sağlar ve bir sayaç değerinin alınması içinde aktör'in durumu güvenilir kalıcıdır. Arabirim proje diğer hizmetler aktör çağırmak için kullanabileceğiniz bir arabirim sağlar.

### <a name="stateless-web-api"></a>Durum bilgisiz Web API'si
Durum bilgisiz Web API projesi dış istemcilere uygulamanıza açmak için kullanabileceğiniz bir temel web hizmeti sağlar. Yapılandırılmış, proje hakkında daha fazla bilgi için bkz: [OWIN kendi kendine barındırma ile Service Fabric Web API Hizmetleri](service-fabric-reliable-services-communication-webapi.md).


### <a name="aspnet-core"></a>ASP.NET Çekirdeği
Service Fabric SDK ASP.NET Core aynı kümesini tek başına ASP.NET Core projeleri için kullanılabilecek şablonlar sağlar: boş [Web API][aspnet-webapi], ve [Web uygulaması][aspnet-webapp].

### <a name="guest-executables-and-guest-containers"></a>Konuk yürütülebilir dosyalar ve Konuk kapsayıcıları

Service Fabric 'Konuk' platformun programlama modeli ile yerleşik olmayan bir hizmettir. İkili dosyalar ya da bir konuk paketleyebilirsiniz [doğrudan uygulama paketindeki](service-fabric-deploy-existing-app.md) veya [bir kapsayıcı görüntüsü aracılığıyla](service-fabric-deploy-container.md). Her iki durumda da, Visual Studio gerekli yapılar oluşturur **ApplicationPackageRoot** uygulama projesi klasörü. Visual Studio kodu başka bir yerde zaten mevcut olduğundan yeni bir hizmet projesi oluşturmaz. Service Fabric uygulaması projesi yanında Konuk projelerinizi yönetmek istiyorsanız, bunları aynı Visual Studio çözümü ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
### <a name="create-an-azure-cluster"></a>Bir Azure kümesi oluşturma
Service Fabric SDK, geliştirme ve test için yerel bir küme sağlar. Bir küme oluşturmak için bkz: [Azure portalından bir Service Fabric kümesi ayarlama][create-cluster-in-portal].

### <a name="publish-your-application-to-azure"></a>Uygulamanızı Azure'a yayımlama
Uygulamanızı Visual Studio'dan doğrudan Azure küme yayımlayabilirsiniz. Bilgi edinmek için bkz [uygulamanızı Azure'a yayımlama][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Kümenizi görselleştirme için Service Fabric Explorer kullanın
Service Fabric Explorer dağıtılan uygulamalar ve fiziksel düzenini de dahil olmak üzere kümenizi görselleştirme için kolay bir yol sunar. Daha fazla bilgi için bkz: [Service Fabric Explorer kullanarak kümenizi görselleştirme][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Sürümü ve hizmetlerinizi yükseltme
Service Fabric bağımsız sürüm oluşturma ve uygulamada bağımsız Hizmetleri yükseltme sağlar. Daha fazla bilgi için bkz: [sürüm oluşturma ve hizmetlerinizi yükseltme][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Visual Studio Team Services ile sürekli tümleştirme yapılandırın
Service Fabric uygulamanız için bir sürekli tümleştirme işlemini nasıl ayarlayabilirsiniz bilgi edinmek için [sürekli tümleştirme ile Visual Studio Team Services yapılandırma][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
