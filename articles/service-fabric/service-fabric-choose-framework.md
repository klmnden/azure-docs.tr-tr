---
title: "Service Fabric programlama modeline genel bakış | Microsoft Docs"
description: "Service Fabric hizmetleri oluşturmak için iki çerçeveleri sunar: aktör çerçevesi ve Hizmetleri çerçevesi. Bunlar ayrı dengelemeler Basitlik ve denetim sağlar."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek
ms.assetid: 974b2614-014e-4587-a947-28fcef28b382
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: a68db62f87bca5c641db310823588df6fb74f75e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="service-fabric-programming-model-overview"></a>Service Fabric programlama modeline genel bakış
Service Fabric yazma ve hizmetlerinizi yönetmek için birden çok yol sunar. Hizmetleri platformun özellikleri ve uygulama çerçeveleri tam anlamıyla yararlanabilmek için Service Fabric API'ları kullanmayı seçebilirsiniz. Hizmetleri herhangi bir dil veya basit bir Service Fabric kümesi üzerinde barındırılan bir kapsayıcıda çalışan kodu yazılmış derlenmiş bir yürütülebilir program da olabilir.

## <a name="guest-executables"></a>Konuk yürütülebilir dosyalar
A [Konuk yürütülebilir](service-fabric-deploy-existing-app.md) mevcut ise, uygulamanızdaki bir hizmet farklı çalıştır (herhangi bir dilde yazılmış) rasgele çalıştırılabilir. Konuk yürütülebilir dosyalar Service Fabric SDK'sı API'lerini doğrudan çağırmayın. Ancak bunlar hala platform, özel durum hizmet bulunabilirliği gibi sunar özelliklerinden yararlanır ve REST API'ları Service Fabric tarafından kullanıma sunulan çağırarak raporlama yükleyin. Aynı zamanda tam uygulama yaşam döngüsü destek sahiptirler.

Konuk yürütülebilir dosyaları ile ilk dağıtmaya başlamak [Konuk yürütülebilir uygulama](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Kapsayıcılar
Varsayılan olarak, Service Fabric dağıtır ve Hizmetleri işlemler olarak etkinleştirir. Service Fabric da Hizmetleri'nde dağıtım [kapsayıcıları](service-fabric-containers-overview.md). Service Fabric, Windows Server 2016 Linux kapsayıcıları ve Windows kapsayıcıları dağıtımını destekler. Kapsayıcı görüntüleri tüm kapsayıcı depodan çekilen ve makineye dağıtıldı. Konuk exectuables, Service Fabric durum bilgisiz veya durum bilgisi olan güvenilir hizmetler ya da kapsayıcılarında Reliable Actors olarak mevcut uygulamaları dağıtabilir ve işlemlerde Hizmetleri ve Hizmetleri aynı uygulamada kapsayıcılardaki karıştırabilirsiniz.

[Windows veya Linux hizmetlerinizi containerizing hakkında daha fazla bilgi edinin](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services
Güvenilir hizmetler Service Fabric platformu ile tümleştirme ve platform özelliklerinin tam kümesinden yararlanabilirsiniz Hizmetleri yazmak için bir çerçevedir hafif. Güvenilir hizmetler en az bir dizi hizmetlerinizi ömrünü yönetmek Service Fabric çalışma zamanını izin ve hizmetlerinizi çalışma zamanı ile etkileşim kurmasına izin veren API sağlar. Uygulama Çerçevesi tasarım ve uygulama seçenekleri üzerinde tam denetim verip, en alt düzeydedir ve herhangi diğer uygulama çerçevesi, ASP.NET Core gibi barındırmak için kullanılır.

Güvenilir hizmetler, durum bilgisiz, her bir hizmet örneği eşit oluşturulur ve durumu Azure DB veya Azure Table Storage gibi harici bir çözümde kalıcı web sunucuları gibi çoğu hizmet platformları benzer olabilir.

Güvenilir hizmetler de durum bilgisi olabilir Service Fabric özel, burada durumu kalıcıdır hizmeti kendisini güvenilir koleksiyonları kullanarak doğrudan. Durumu yüksek oranda çoğaltma yoluyla kullanılabilir hale ve, otomatik olarak Service Fabric tarafından yönetilen tüm bölümleme aracılığıyla dağıtılır.

[Reliable Services hakkında daha fazla bilgi](service-fabric-reliable-services-introduction.md) veya başlayın [ilk güvenilir hizmetiniz yazma](service-fabric-reliable-services-quick-start.md).

## <a name="aspnet-core"></a>ASP.NET Çekirdeği
ASP.NET Core modern bulut tabanlı Internet'e bağlı uygulamaları, web uygulamaları, IOT uygulamaları ve mobil arka uçlarını gibi oluşturmak için yeni bir açık kaynaklı ve çapraz platform framework kümesidir. Durum bilgisiz ve durum bilgisi olan ASP.NET Core güvenilir koleksiyonlarını ve Service Fabric'ın gelişmiş orchestration yetenekleri yararlanan uygulamalar yazma için Service Fabric ASP.NET Core ile tümleşir.

[Service Fabric ASP.NET çekirdek hakkında daha fazla bilgi](service-fabric-reliable-services-communication-aspnetcore.md) veya başlayın [ilk ASP.NET Core Service Fabric uygulamanızı yazma](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="reliable-actors"></a>Reliable Actors
Güvenilir hizmetler en üstünde oluşturulan güvenilir aktör aktör tasarım deseni temel alınarak sanal aktör deseni uygulayan bir uygulama çerçevesi çerçevedir. Güvenilir aktör çerçevesi yürütme aktörler olarak adlandırılan tek iş parçacıklı işlem ve durumunun bağımsız bir birim kullanır. Güvenilir aktör framework aktörler ve önceden ayarlanmış durumu kalıcılığını ve genişleme yapılandırmaları için yerleşik iletişim sağlar.

Reliable Actors kendisini Reliable Services üzerinde bir uygulama altyapısıdır olduğu gibi Service Fabric platformundan ve avantajların platform tarafından sunulan özelliklerden tam kümesinden ile tam olarak tümleşiktir.

[Reliable Actors hakkında daha fazla bilgi](service-fabric-reliable-actors-introduction.md) veya başlayın [ilk güvenilir aktör hizmetiniz yazma](service-fabric-reliable-actors-get-started.md)


[ASP.NET Core kullanarak bir ön uç hizmet oluşturma](service-fabric-reliable-services-communication-aspnetcore.md)

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric ve kapsayıcıları genel bakış](service-fabric-containers-overview.md)

[Güvenilir Hizmetleri'ne Genel Bakış](service-fabric-reliable-services-introduction.md)

[Güvenilir aktörler genel bakış](service-fabric-reliable-actors-introduction.md)

[Service Fabric ve ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)




