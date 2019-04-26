---
title: Service Fabric programlama modeline genel bakış | Microsoft Docs
description: 'Service Fabric hizmetleri oluşturmak için iki çerçeveleri sunar: aktör çerçevesinde ve Hizmetleri çerçevesi. Bunlar, Basitlik ve denetimi içinde ayrı dengelemeler sağlar.'
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: vturecek
ms.assetid: 974b2614-014e-4587-a947-28fcef28b382
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: d764cbe2df78cb9029a4109caa2998ddded5d6ff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60341973"
---
# <a name="service-fabric-programming-model-overview"></a>Service Fabric programlama modeline genel bakış
Service Fabric, yazma ve hizmetlerinizi yönetmek için birden çok yol sunar. Hizmetleri, tam anlamıyla platformun özellikleri ve uygulama çerçeveleri için Service Fabric API'leri kullanmayı da tercih edebilirsiniz. Hizmetleri, herhangi bir dil veya barındırılan bir Service Fabric kümesindeki bir kapsayıcıda çalışan kodda yazılmış bir derlenmiş yürütülebilir program de olabilir.

## <a name="guest-executables"></a>Konuk yürütülebilir dosyaları
A [Konuk yürütülebilir dosyası](service-fabric-guest-executables-introduction.md) olan mevcut, uygulamanızdaki bir hizmet olarak çalıştırma (herhangi bir dilde yazılmış) rastgele yürütülebilir. Konuk tarafından yürütülebilir uygulama Service Fabric SDK'sı API'leri doğrudan çağırmanız gerekmez. Ancak bunlar yine de bir platform sunar, özel sistem durumu hizmet bulunabilirliği gibi özelliklerinden yararlanmak ve Service Fabric tarafından kullanıma sunulan REST API'lerini çağırarak raporlama yükleyin. Bunlar ayrıca, tam uygulama yaşam döngüsü desteği de vardır.

Konuk tarafından yürütülebilir uygulama ile ilk dağıtmaya başlamak [konuk tarafından yürütülebilir uygulama](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Kapsayıcılar
Varsayılan olarak, Service Fabric dağıtır ve hizmet işlemleri olarak etkinleştirir. Service Fabric ayrıca Hizmetleri dağıtma [kapsayıcıları](service-fabric-containers-overview.md). Service Fabric Linux kapsayıcıları ve Windows kapsayıcıları dağıtımı, Windows Server 2016'da destekler. Kapsayıcı görüntüleri, tüm kapsayıcı depodan çekilir ve makineye dağıtılır. Konuk yürütülebilir dosyaları, Service Fabric durum bilgisi olmayan ya da durum bilgisi olan Reliable services veya kapsayıcılarında Reliable Actors olarak var olan uygulamaları dağıtabilir ve Hizmetleri ve kapsayıcıları aynı uygulama Hizmetleri'nde karıştırabilirsiniz.

[Windows veya Linux hizmetlerinizi kapsayıcılı hale getirmek hakkında daha fazla bilgi edinin](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services
Güvenilir hizmetler, hizmetler Service Fabric platformu ile tümleştirin ve tam platform özelliklerden faydalanır yazmak için bir hafif altyapısıdır. Reliable Services en az bir yaşam döngüsü hizmetlerinizi yönetmek Service Fabric çalışma zamanı izin veren ve hizmetlerinizin çalışma zamanı ile etkileşim kurmak izin veren API kümesi sağlar. Uygulama Çerçevesi, tasarım ve uygulama seçenekleri üzerinde tam denetim veren minimal ve herhangi diğer uygulama çerçevesi, ASP.NET Core gibi barındırmak için kullanılabilir.

Reliable Services, durum bilgisi olmayan, hizmetin her örneği eşit oluşturulur ve durumu Azure DB veya Azure tablo depolama gibi harici bir çözümde kalıcı web sunucuları gibi çoğu hizmet platformu benzer olabilir.

Güvenilir hizmetler ayrıca durum bilgisi olan, bulunabilir dışlayan Service fabric'e burada durumu kalıcıdır hizmeti kendisini güvenilir koleksiyonlar kullanarak doğrudan. Durum çoğaltma ile yüksek kullanılabilirliğe yapılan ve, otomatik olarak Service Fabric tarafından yönetilen tüm bölümleme aracılığıyla dağıtılmış.

[Reliable Services hakkında daha fazla bilgi](service-fabric-reliable-services-introduction.md) veya başlayın [ilk güvenilir hizmetinizi yazma](service-fabric-reliable-services-quick-start.md).

## <a name="aspnet-core"></a>ASP.NET Çekirdeği
ASP.NET Core, modern bulut tabanlı İnternet'e bağlı uygulamalar, web uygulamaları, IOT uygulamaları ve mobil arka uçları gibi oluşturmaya yönelik yeni bir açık kaynaklı ve platformlar arası altyapılardan biridir. Durum bilgisiz ve durum bilgisi olan ASP.NET Core güvenilir koleksiyonlar ve Service Fabric'in Gelişmiş Düzenleme özelliklerinden yararlanan uygulamalar yazmak için Service Fabric ASP.NET Core ile tümleştirilir.

[Service fabric'te ASP.NET Core hakkında daha fazla bilgi](service-fabric-reliable-services-communication-aspnetcore.md) veya başlayın [ilk ASP.NET Core Service Fabric uygulamanızı yazma](service-fabric-tutorial-create-dotnet-app.md).

## <a name="reliable-actors"></a>Reliable Actors
Reliable Services üzerine inşa edilen güvenilir aktör çerçevesinde aktör tasarım deseni temel alınarak sanal gerçekleştiren deseni, uygulayan bir uygulama çerçevesidir. Reliable Actor çerçeve yürütme aktörler olarak adlandırılan tek iş parçacıklı işlem durumu ve bağımsız bir birim kullanır. Güvenilir aktör çerçevesinde aktörler ve önceden ayarlanan durumu kalıcılığını ve genişleme yapılandırmaları için yerleşik iletişim sağlar.

Reliable Actors Reliable Services üzerinde oluşturulmuş bir uygulama çerçevesi olduğundan, Service Fabric platformu ve eksiksiz bir platform tarafından sunulan özelliklerin sağladığı avantajları ile tamamen tümleşiktir.

[Reliable Actors hakkında daha fazla bilgi](service-fabric-reliable-actors-introduction.md) veya başlayın [ilk Reliable Actor hizmetinizi yazma](service-fabric-reliable-actors-get-started.md)


[ASP.NET Core kullanarak ön uç hizmet oluşturma](service-fabric-reliable-services-communication-aspnetcore.md)

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric ve kapsayıcılar genel bakış](service-fabric-containers-overview.md)

[Reliable Services genel bakış](service-fabric-reliable-services-introduction.md)

[Reliable Actors hizmetine genel bakış](service-fabric-reliable-actors-introduction.md)

[Service Fabric ve ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)




