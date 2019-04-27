---
title: Azure Service Fabric barındırma modeli | Microsoft Docs
description: Hizmet ana bilgisayar işlemi ile dağıtılan bir Service Fabric hizmeti ve çoğaltmaları (veya örnekleri) arasındaki ilişkiyi açıklar.
services: service-fabric
documentationcenter: .net
author: harahma
manager: chackdan
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: d2d958a89bff40483e1cd473538f7d1a6971d266
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60483638"
---
# <a name="azure-service-fabric-hosting-model"></a>Azure Service Fabric barındırma modeli
Bu makalede, Azure Service Fabric tarafından sağlanan modelleri barındıran uygulama genel bir bakış sağlar ve arasındaki farkları açıklar **paylaşılan işlem** ve **özel işlem** modelleri. Bu, dağıtılan bir uygulama bir Service Fabric düğüm ve hizmet ve hizmet ana bilgisayar işlemi çoğaltmaları (veya örnekleri) arasındaki ilişkiyi nasıl görüneceğini gösteren açıklar.

Devam etmeden önce çeşitli kavramları anlamanıza ve ilişkileri içinde açıklanan emin olun [bir Service Fabric uygulamasında Model][a1]. 

> [!NOTE]
> Bu makalede, farklı bir şey anlamına olarak açıkça belirtilmediği sürece:
>
> - *Çoğaltma* hem bir çoğaltmaya bir durum bilgisi olan hizmeti ve durum bilgisi olmayan hizmet örneğini gösterir.
> - *CodePackage* kabul edilir için eşdeğer bir *ServiceHost* kaydeder işlem bir *ServiceType*ve konaklar çoğaltmaları, söz konusu hizmetlerin *ServiceType*.
>

Barındırma modeli anlamak için bir örnek atalım. Diyelim ki sahip bir *ApplicationType* sahip'MyAppType ' bir *ServiceType* 'MyServiceType'. 'MyServiceType' tarafından sağlanır *ServicePackage* sahip'MyServicePackage ' bir *CodePackage* 'MyCodePackage'. 'MyCodePackage' kaydeder *ServiceType* 'çalıştığında MyServiceType'.

Diyelim ki üç düğümlü bir küme sunuyoruz ve oluşturduğumuz bir *uygulama* **fabric: / App1** 'MyAppType' türünde. Bu uygulama içinde **fabric: / App1**, bir hizmet oluşturacağız **fabric: / App1/ServiceA** 'MyServiceType' türünde. Bu hizmet, iki bölümü vardır (örneğin, **P1** ve **P2**) ve bölüm başına üç kopyaya. Aşağıdaki diyagram görünümü, bu uygulamanın sona eriyor dağıtılan bir düğümde gösterir.


![Dağıtılan uygulama düğüm görünümü diyagramı][node-view-one]


Service Fabric, her iki bölüm çoğaltmaları barındıran'MyCodePackage ' başlatıldı'MyServicePackage ' etkin. Kümedeki düğüm sayısına eşit olacak şekilde bölüm başına çoğaltma sayısı seçtik çünkü kümedeki tüm düğümlerin aynı görünüm vardır. Başka bir hizmete oluşturalım **fabric: / App1/ServiceB**, uygulamadaki **fabric: / App1**. Bu hizmet bir bölümü vardır (örneğin, **P3**) ve bölüm başına üç kopyaya. Yeni Görünüm düğümde Aşağıdaki diyagramda gösterilmiştir:


![Dağıtılan uygulama düğüm görünümü diyagramı][node-view-two]


Service Fabric yeni çoğaltmayı bölümü için yerleştirilmiş **P3** hizmetinin **fabric: / App1/ServiceB** 'MyServicePackage', mevcut etkinleştirme. Şimdi. başka bir uygulama oluşturalım **fabric: / App2** 'MyAppType' türünde. İçinde **fabric: / App2**, bir hizmet oluşturma **fabric: / App2/ServiceA**. Bu hizmet, iki bölümü vardır (**P4** ve **P5**) ve bölüm başına üç kopyaya. Aşağıdaki diyagramda, yeni düğüm görünümü gösterilmektedir:


![Dağıtılan uygulama düğüm görünümü diyagramı][node-view-three]


Service Fabric 'MyCodePackage' yeni bir kopyasını başlar'MyServicePackage ' yeni bir kopyasını etkinleştirir. Her iki hizmet bölümlerini çoğaltmalardan **fabric: / App2/ServiceA** (**P4** ve **P5**) bu yeni kopya 'MyCodePackage' yerleştirilir.

## <a name="shared-process-model"></a>Paylaşılan işlem modeli
Önceki bölümde barındırma modeli paylaşılan işlem modeli olarak başvurulan, Service Fabric tarafından sağlanan varsayılan açıklar. Bu modelde, belirli bir uygulama için yalnızca bir kopyası bir verilen *ServicePackage* bir düğümde etkinleştirilir (tüm başladığı *CodePackages* içinde yer alan). Tüm hizmetleri tüm çoğaltmalar bir verilen *ServiceType* yerleştirilir *CodePackage* , kaydeder *ServiceType*. Diğer bir deyişle, tüm çoğaltmaların düğümünde tüm hizmetlerin bir verilen *ServiceType* aynı işlem paylaşın.

## <a name="exclusive-process-model"></a>Özel işlem modeli
Service Fabric tarafından sağlanan diğer barındırma modeli, özel işlem modelidir. Bu modelde, belirli bir düğümdeki her çoğaltma kendi adanmış bir işlem içinde bulunur. Service Fabric etkinleştiren yeni bir kopyasını *ServicePackage* (tüm başladığı *CodePackages* içinde yer alan). Çoğaltmaları yerleştirildiğinde *CodePackage* kayıtlı *ServiceType* ait olduğu çoğaltma hizmeti. 

Service Fabric 5.6 sürümü kullandığınız ya da daha sonra zaman dışlamalı işlem modeli seçebilirsiniz. bir hizmet oluşturma (kullanarak [PowerShell][p1], [REST] [ r1], veya [FabricClient][c1]). Belirtin **ServicePackageActivationMode** 'ExclusiveProcess' olarak.

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Varsayılan hizmet uygulama bildiriminizi varsa, belirterek özel işlem modeli seçebilirsiniz **ServicePackageActivationMode** özniteliği:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Artık başka bir hizmete oluşturalım **fabric: / App1/ServiceC**, uygulamada **fabric: / App1**. Bu hizmet, iki bölümü vardır (örneğin, **P6** ve **7**) ve bölüm başına üç kopyaya. Ayarladığınız **ServicePackageActivationMode** 'ExclusiveProcess' için. Yeni Görünüm düğümde Aşağıdaki diyagramda gösterilmiştir:


![Dağıtılan uygulama düğüm görünümü diyagramı][node-view-four]


Gördüğünüz gibi Service Fabric iki yeni kopyasını 'MyServicePackage' etkinleştirildi (bir bölümündeki her çoğaltma için **P6** ve **7**). Service Fabric yerleştirilen her yineleme, adanmış kopyasında *CodePackage*. Belirli bir uygulama için özel işlem modeli kullandığınız zaman, birden çok kopya bir verilen *ServicePackage* bir düğümde etkin olabilir. Önceki örnekte, üç kopyasını 'MyServicePackage' etkin **fabric: / App1**. Her etkin kopyalarını 'MyServicePackage' sahip bir **ServicePackageActivationId** ilişkili. Bu kodu, uygulama içinde bu kopyayı tanımlar **fabric: / App1**.

Bir uygulama için yalnızca paylaşılan işlem modeli kullandığınız olduğunda yalnızca bir etkin kopyasını *ServicePackage* bir düğüm üzerinde. **ServicePackageActivationId** bu etkinleştirilmesi için *ServicePackage* boş bir dizedir. Örneğin, durum ile budur **fabric: / App2**.

> [!NOTE]
>- Paylaşılan barındırma modeli karşılık gelen işlem **ServicePackageActivationMode** eşittir **SharedProcess**. Barındırma modeli, varsayılan ve **ServicePackageActivationMode** hizmet oluşturma sırasında belirtilmesi gerekmez.
>
>- Özel barındırma modeli karşılık gelen işlem **ServicePackageActivationMode** eşittir **ExclusiveProcess**. Bu ayarı kullanmak için onu açıkça hizmet oluşturma sırasında belirtmeniz gerekir. 
>
>- Bir hizmet barındırma modeli görüntülemek için sorgu [hizmet açıklaması][p2]ve değerinde **ServicePackageActivationMode**.
>
>

## <a name="work-with-a-deployed-service-package"></a>Dağıtılan hizmet paketi ile çalışma
Etkin bir kopyasını bir *ServicePackage* bir düğümde şeklinde adlandırılan bir [dağıtılan hizmet paketi][p3]. Hizmetleri, belirli bir uygulama oluşturmak için özel işlem modeli kullandığınız zaman birden çok dağıtılmış hizmet paketleri için aynı olabilir *ServicePackage*. Bir dağıtılan hizmet paketi için belirli bir işlem gerçekleştiriyorsanız, sağlamalıdır **ServicePackageActivationId** belirli dağıtılan hizmet paketi belirlemek için. Örneğin, kimliği [dağıtılan hizmet paketi durumunu raporlama] [ p4] veya [dağıtılan hizmet paketi kod sürümü yeniden] [p5].

Bunu öğrenebilirsiniz **ServicePackageActivationId** listesini sorgulamak dağıtılan hizmet paketi [hizmet paketleri dağıtılan] [ p3] bir düğüm üzerinde. Ne zaman, sorguladığınız için [hizmet türlerini dağıtılan][p6], [çoğaltmaları dağıtılan][p7], ve [kod paketleri dağıtılan ] [ p8] bir düğümde, sorgu sonucu da içeren **ServicePackageActivationId** üst dağıtılan hizmet paketi.

> [!NOTE]
>- Belirli bir uygulama için belirli bir düğümde, paylaşılan işlem barındırma modeli altında yalnızca bir kopyası bir *ServicePackage* etkinleştirilir. Bunun bir **ServicePackageActivationId** eşit *boş dize*ve dağıtılan hizmet paketi için ilgili işlemleri gerçekleştirilirken belirtilmesi gerekmez. 
>
> - Özel işlem barındırma altında belirli bir uygulama için belirli bir düğümde, bir veya daha fazla kopyasını model bir *ServicePackage* etkin olabilir. Her etkinleştirme açılmış bir *boş* **ServicePackageActivationId**, belirtilen dağıtılan hizmet paketi için ilgili işlemleri gerçekleştirirken. 
>
> - Varsa **ServicePackageActivationId** olan atlanırsa, varsayılan *boş dize*. Paylaşılan işlem modeli altında etkinleştirildiği bir dağıtılan hizmet paketi varsa, işlemi üzerinde gerçekleştirilir. Aksi takdirde işlem başarısız olur.
>
> - Bir kez ve önbellek sorgulamaz **ServicePackageActivationId**. Kimliği dinamik olarak oluşturulur ve çeşitli nedenlerden dolayı değiştirebilirsiniz. Gerektiren bir işlem gerçekleştirmeden önce **ServicePackageActivationId**, listesini önce sorgu [hizmet paketleri dağıtılan] [ p3] bir düğüm üzerinde. Ardından **ServicePackageActivationId** özgün işlemi gerçekleştirmek için sorgu sonuç.
>
>

## <a name="guest-executable-and-container-applications"></a>Konuk yürütülebilir ve kapsayıcı uygulamaları
Service Fabric işler [Konuk yürütülebilir dosyası] [ a2] ve [kapsayıcı] [ a3] kendine yeterdir durum bilgisi olmayan hizmetler olarak uygulamalar. Hiçbir Service Fabric çalışma zamanı içinde *ServiceHost* (işlem veya kapsayıcı). Bu hizmetler kendi içinde olduğundan çoğaltma sayısı *ServiceHost* bu hizmetler için geçerli değildir. Bu hizmetlerle kullanılan en yaygın yapılandırma ile tek bölümlü [Instancecount] [ c2] -1 (hizmet kodunun kümedeki her düğümde çalışan bir kopya) değerine eşit. 

Varsayılan **ServicePackageActivationMode** bu hizmetler için **SharedProcess**, Service Fabric yalnızca bir kopyasını etkinleştirir, bu durumda *ServicePackage* bir düğümde belirli bir uygulama için.  Başka bir deyişle, bir düğümünde hizmet kodu yalnızca bir kopyasını çalıştırır. Bir düğümü üzerinde çalışacak şekilde hizmet kodunuzun birden çok kopyasını istiyorsanız belirtin **ServicePackageActivationMode** olarak **ExclusiveProcess** zaman hizmeti oluşturma. Birden çok hizmeti oluşturduğunuzda, bunu yapabilirsiniz (*Service1* için *ServiceN*), *ServiceType* (belirtilen *ServiceManifest*), ya da hizmetiniz olduğunda çoklu bölümlenmiş. 

## <a name="change-the-hosting-model-of-an-existing-service"></a>Mevcut bir hizmet barındırma modelini değiştirme
Şu anda mevcut bir hizmet barındırma modeli paylaşılan işlemden özel işlem (veya tersi) değiştiremezsiniz.

## <a name="choose-between-the-hosting-models"></a>Barındırma modeli arasında seçin
En iyi hangi barındırma modeli gereksinimlerinize uyduğuna değerlendirmelidir. Paylaşılan işlem modeli işletim sistemi kaynaklarını daha iyi, daha az işlemleri kökenli ve aynı işlemde birden çok çoğaltma bağlantı noktalarını paylaşabilirsiniz kullanır. Ancak, çoğaltmalarından birini gereken yere gönderilerek hizmet ana bilgisayarınızı getirin bir hata varsa, aynı işlemde tüm çoğaltmaları etkiler.

 Özel işlem modeli de kendi işleminde her yineleme ile daha iyi yalıtım sağlar. Çoğaltmalarından birini bir hata varsa, diğer yinelemeler etkilemez. Bu model, çalışmaları nereye bağlantı noktası paylaşımı iletişim protokolü tarafından desteklenmiyor için kullanışlıdır. Bu yineleme düzeyinde kaynak İdaresi uygulama özelliği kolaylaştırır. Ancak, bu düğümdeki her çoğaltma için bir işlem olarak çoğaltılır gibi özel işlem daha fazla işletim sistemi kaynaklar kullanır.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Özel işlem modeli ve uygulama konuları modeli
Çoğu uygulama için bir otomatik olarak kilitlenmesini sağlayarak uygulamanızın Service fabric'te modelleyebilir *ServiceType* başına *ServicePackage*. 

Bazı durumlarda, Service Fabric ayrıca birden fazla sağlar *ServiceType* başına *ServicePackage* (ve bir *CodePackage* birden fazla kaydedebilirsiniz  *ServiceType*). Burada, bu yapılandırmalar yararlı olabilir senaryolardan bazıları şunlardır:

- UNICODE daha az işlemler ve işlem başına daha yüksek yineleme yoğunluğu olan kaynak kullanımını iyileştirmek istediğinizde.
- Farklı çoğaltmalardan *ServiceTypes* maliyeti yüksek başlatma veya bellek bazı ortak verilere paylaşmayı.
- Ücretsiz bir hizmet olması ve hizmetin tüm çoğaltmalar aynı işlem içinde koyarak kaynak kullanımı bir sınır koymak istediğiniz.

Özel barındırma modeli işlem birden çok sahip bir uygulama modeli ile tutarlı değil *ServiceTypes* başına *ServicePackage*. Bunun nedeni birden çok, *ServiceTypes* başına *ServicePackage* daha yüksek kaynak arasında çoğaltmaları paylaşımını sağlamak için tasarlanmıştır ve işlem başına daha yüksek yineleme yoğunluğu sağlar. Özel işlem modeli farklı sonuçlar elde etmek için tasarlanmıştır.

Birden çok durum düşünün *ServiceTypes* başına *ServicePackage*, farklı bir ile *CodePackage* her kaydetme *ServiceType*. Diyelim ki sahip bir *ServicePackage* iki'MultiTypeServicePackage ' *CodePackages*:

- Kayıtları'MyCodePackageA ' *ServiceType* 'MyServiceTypeA'.
- Kayıtları'MyCodePackageB ' *ServiceType* 'MyServiceTypeB'.

Artık, bir uygulama oluşturacağız varsayalım **fabric: / SpecialApp**. İçinde **fabric: / SpecialApp**, aşağıdaki iki özel işlem modeli hizmetleriyle oluşturun:

- Hizmet **fabric: / SpecialApp/ServiceA** 'MyServiceTypeA' türünde iki bölüm (örneğin, **P1** ve **P2**) ve bölüm başına üç kopyaya.
- Hizmet **fabric: / SpecialApp/ServiceB** 'MyServiceTypeB' türünde iki bölüm (**P3** ve **P4**) ve bölüm başına üç kopyaya.

Belirli bir düğümde iki çoğaltma hizmetlerinin her ikisi de vardır. Service Fabric, biz hizmetleri oluşturmak için özel işlem modeli kullandığından, 'MyServicePackage' ın yeni bir kopyasını her çoğaltma için etkinleştirir. Her etkinleştirme 'MultiTypeServicePackage', 'MyCodePackageA' ve 'MyCodePackageB' bir kopyasını başlatır. Ancak, yalnızca bir 'MyCodePackageA' veya 'MyCodePackageB' 'MultiTypeServicePackage' etkinleştirildiği çoğaltma barındırır. Aşağıdaki diyagramda, düğüm görünümü gösterilmektedir:


![Dağıtılan uygulamanın düğüm görünümü diyagramı][node-view-five]


'MultiTypeServicePackage' etkinleştirme çoğaltmasının bölümünün içinde **P1** hizmetinin **fabric: / SpecialApp/ServiceA**, 'MyCodePackageA' çoğaltma barındırma. 'MyCodePackageB' çalışıyor. Benzer şekilde, 'MultiTypeServicePackage' etkinleştirme çoğaltmasının bölümünün içinde **P3** hizmetinin **fabric: / SpecialApp/ServiceB**, 'MyCodePackageB' çoğaltma barındırma. 'MyCodePackageA' çalışıyor. Bu nedenle, sayısı arttıkça *CodePackages* (farklı kaydetme *ServiceTypes*) başına *ServicePackage*, yedekli kaynak kullanımını daha yüksek. 
 
 Ancak biz Hizmetleri oluşturursanız **fabric: / SpecialApp/ServiceA** ve **fabric: / SpecialApp/ServiceB** paylaşılan işlem modeliyle, Service Fabric yalnızca bir kopyasını etkinleştirir. ' MultiTypeServicePackage' uygulaması için **fabric: / SpecialApp**. 'MyCodePackageA' hizmeti için tüm çoğaltmaları barındıran **fabric: / SpecialApp/ServiceA**. 'MyCodePackageB' hizmeti için tüm çoğaltmaları barındıran **fabric: / SpecialApp/ServiceB**. Aşağıdaki diyagramda, bu ayarda düğüm görünümü gösterilmektedir: 


![Dağıtılan uygulamanın düğüm görünümü diyagramı][node-view-six]


Önceki örnekte, hem 'MyServiceTypeA' hem de 'MyServiceTypeB', 'MyCodePackageA' kaydeder ve hiçbir 'MyCodePackageB' yoktur sonra yoktur Hayır yedekli düşündüğünüzden *CodePackage* çalışıyor. Bu doğru olmasına karşın, bu uygulama modeli ile özel barındırma modeli işlem hizalanmadı. Hedef adanmış kendi işleminde her çoğaltma koymak için ise, her ikisi de kaydetme gerekmez *ServiceTypes* aynı *CodePackage*. Bunun yerine, her basitçe *ServiceType* kendi içinde *ServicePackage*.

## <a name="next-steps"></a>Sonraki adımlar
[Bir uygulama paketi] [ a4] ve dağıtmak hazırlanın.

[Dağıtma ve uygulamaları kaldırma][a5]. Bu makalede, uygulama örneklerini yönetmek için PowerShell kullanmayı açıklar.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-guest-executables-introduction.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/module/servicefabric/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/module/servicefabric/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedcodepackage
