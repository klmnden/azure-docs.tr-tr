---
title: Azure Service Fabric barındırma modeli | Microsoft Docs
description: Dağıtılan bir Service Fabric hizmeti ve hizmet ana bilgisayar işlemi çoğaltmaları (veya örnekleri) arasındaki ilişkiyi açıklar.
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: d56bb10041e3baffddf6fd4121a6e1f7ba8e0632
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="azure-service-fabric-hosting-model"></a>Azure Service Fabric barındırma modeli
Bu makale Azure Service Fabric tarafından sağlanan modelleri barındıran uygulama genel bir bakış sağlar ve arasındaki farklar açıklanmaktadır **paylaşılan işlem** ve **özel işlem** modeller. Dağıtılan bir uygulama bir Service Fabric düğümü ve hizmet ve hizmet ana bilgisayar işlemi çoğaltmaları (veya örnekleri) arasındaki ilişkiyi nasıl göründüğünü açıklayan.

Devam etmeden önce çeşitli kavramları anlamanız ilişkileri içinde açıklanan emin olun ve [Service Fabric uygulamada Model][a1]. 

> [!NOTE]
> Bu makalede, farklı bir şey anlamına olarak açıkça belirtilmediği sürece:
>
> - *Çoğaltma* hem bir çoğaltmaya durum bilgisi olan hizmeti ve durum bilgisiz Hizmeti'nin bir örneğini gösterir.
> - *CodePackage* kabul edilir için eşdeğer bir *ServiceHost* kaydeder işlem bir *ServiceType*ve konaklar çoğaltmalar, hizmetlerin *ServiceType*.
>

Barındırma modeli anlamak için şirketinizdeki örneği yol. Bizim düşünelim bir *ApplicationType* sahip'MyAppType ' bir *ServiceType* 'MyServiceType'. 'MyServiceType' tarafından sağlanan *ServicePackage* sahip'MyServicePackage ' bir *CodePackage* 'MyCodePackage'. 'MyCodePackage' kaydeder *ServiceType* 'çalıştırıldığında MyServiceType'.

Diyelim ki üç düğümlü bir küme sahibiz ve oluşturuyoruz bir *uygulama* **fabric: / App1** 'MyAppType' türünde. Bu uygulama içinde **fabric: / App1**, bir hizmet oluşturuyoruz **doku: / App1/ServiceA** 'MyServiceType' türünde. Bu hizmet iki bölümü vardır (örneğin, **P1** ve **P2**) ve bölüm başına üç çoğaltmaları. Aşağıdaki diyagramda, bu uygulama görünümünü sona eriyor dağıtılan bir düğümde gösterir.


![Dağıtılmış uygulama düğümü görünümü diyagramı][node-view-one]


Service Fabric her iki bölüm çoğaltmalardan barındırma'MyCodePackage ' Başlangıç'MyServicePackage ' etkinleştirildi. Kümedeki düğüm sayısına eşit olacak şekilde bölüm başına çoğaltmaların sayısı seçtik çünkü kümedeki tüm düğümler aynı görünüm vardır. Başka bir hizmet oluşturalım **fabric: / App1/ServiceB**, uygulamada **fabric: / App1**. Bu hizmet bir bölümü vardır (örneğin, **P3**) ve bölüm başına üç çoğaltmaları. Aşağıdaki diyagramda düğüm üzerindeki yeni görünüm gösterir:


![Dağıtılmış uygulama düğümü görünümü diyagramı][node-view-two]


Service Fabric bölümü için yeni çoğaltma yerleştirilen **P3** hizmetinin **fabric: / App1/ServiceB** 'MyServicePackage' ın mevcut etkinleştirme. Şimdi. başka bir uygulama oluşturalım **fabric: / App2** 'MyAppType' türünde. İçinde **fabric: / App2**, bir hizmet oluşturma **fabric: / App2/ServiceA**. Bu hizmet iki bölümü vardır (**P4** ve **P5**) ve bölüm başına üç çoğaltmaları. Aşağıdaki diyagramda, yeni düğümü görünüm gösterir:


![Dağıtılmış uygulama düğümü görünümü diyagramı][node-view-three]


Service Fabric 'MyCodePackage' ın yeni bir kopyasını başlatır'MyServicePackage ' ın yeni bir kopyasını etkinleştirir. Her iki hizmet bölümlerini çoğaltmalardan **fabric: / App2/ServiceA** (**P4** ve **P5**) bu yeni kopya 'MyCodePackage' yerleştirilir.

## <a name="shared-process-model"></a>Paylaşılan işlem modeli
Önceki bölümde barındırma modeli paylaşılan işlem modeli başvurulan Service Fabric tarafından sağlanan varsayılan açıklar. Bu modelde, belirli bir uygulamada yalnızca bir kopyası bir verilen *ServicePackage* bir düğümde etkinleştirilir (tüm başladığı *CodePackages* içerdiği). Tüm hizmetlerin tüm çoğaltmalar bir verilen *ServiceType* yerleştirilir *CodePackage* , kaydeder *ServiceType*. Diğer bir deyişle, tüm çoğaltmaların düğümünde tüm hizmetlerin bir verilen *ServiceType* aynı işlem paylaşın.

## <a name="exclusive-process-model"></a>Özel işlem modeli
Service Fabric tarafından sağlanan diğer barındırma modeli özel işlem modelidir. Bu modelde, belirtilen bir düğüm üzerindeki her çoğaltma adanmış kendi işleminde yaşar. Service Fabric etkinleştiren yeni bir kopyasını *ServicePackage* (tüm başladığı *CodePackages* içerdiği). Çoğaltmaları yerleştirilir *CodePackage* kayıtlı *ServiceType* ait olduğu çoğaltma hizmeti. 

Service Fabric sürüm 5.6 kullanıyorsanız veya özel işlem modeli aynı anda daha sonra seçebileceğiniz bir hizmet oluşturma (kullanarak [PowerShell][p1], [REST] [ r1], veya [FabricClient][c1]). Belirtin **ServicePackageActivationMode** 'ExclusiveProcess' olarak.

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

Varsayılan hizmet uygulama bildiriminizi varsa, özel işlem modeli belirterek seçebileceğiniz **ServicePackageActivationMode** özniteliği:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Artık başka bir hizmet oluşturalım **fabric: / App1/ServiceC**, uygulamada **fabric: / App1**. Bu hizmet iki bölümü vardır (örneğin, **P6** ve **7**) ve bölüm başına üç çoğaltmaları. Ayarladığınız **ServicePackageActivationMode** 'ExclusiveProcess' için. Aşağıdaki diyagramda düğüm üzerindeki yeni bir görünüm gösterir:


![Dağıtılmış uygulama düğümü görünümü diyagramı][node-view-four]


Gördüğünüz gibi iki yeni kopyasını 'MyServicePackage' Service Fabric etkinleştirilmiş (bölümünden her çoğaltma için bir tane **P6** ve **7**). Service Fabric yerleştirilen her çoğaltma kendi adanmış kopyasında *CodePackage*. Belirli bir uygulama için özel işlem modeli kullandığınızda, birden çok kopya bir verilen *ServicePackage* bir düğümde etkin olabilir. Önceki örnekte, üç kopyasını 'MyServicePackage' için etkin olan **fabric: / App1**. Her 'MyServicePackage' etkin bu kopyasını sahip bir **ServicePackageActivationId** kendisiyle ilişkilendirilmiş. Bu kodu, uygulama içinde bu kopyayı tanımlar **fabric: / App1**.

Yalnızca paylaşılan işlem modeli için bir uygulama kullandığınızda, yalnızca bir etkin kopyası yok *ServicePackage* bir düğüm üzerinde. **ServicePackageActivationId** bu etkinleştirmesi için *ServicePackage* boş bir dize. Örneğin, ile böyledir **fabric: / App2**.

> [!NOTE]
>- Paylaşılan barındırma modeli karşılık gelen işlemi **ServicePackageActivationMode** eşittir **SharedProcess**. Bu model, barındırma varsayılandır ve **ServicePackageActivationMode** hizmet oluşturma sırasında belirtilmesi gerekmez.
>
>- Barındırma modeli karşılık gelen özel işlem **ServicePackageActivationMode** eşittir **ExclusiveProcess**. Bu ayarı kullanmak için açıkça hizmet oluşturma sırasında belirtmeniz gerekir. 
>
>- Bir hizmet barındırma modeli görüntülemek için sorgu [hizmet açıklaması][p2]ve değeri, Ara **ServicePackageActivationMode**.
>
>

## <a name="work-with-a-deployed-service-package"></a>Dağıtılan hizmet paketi ile çalışma
Etkin bir kopyasını bir *ServicePackage* bir düğüm olarak adlandırılır bir [hizmet paketi dağıtılan][p3]. Belirli bir uygulamada hizmetleri oluşturmak için özel işlem modeli kullandığınızda aynı için birden fazla dağıtılmış hizmet paketleri olabilir *ServicePackage*. Dağıtılan hizmet paketi için belirli işlemleri gerçekleştiriyorsanız sağlamalıdır **ServicePackageActivationId** belirli dağıtılan hizmet paketi belirlemek için. Kullanıyorsanız, örneğin, kimliği sağlayın [dağıtılmış hizmet paketi durumunu raporlama] [ p4] veya [dağıtılan hizmet paketi kod paketi yeniden] [p5].

Bulma **ServicePackageActivationId** listesini sorgulama tarafından dağıtılan hizmet paketi [hizmet paketleri dağıtılan] [ p3] bir düğüm üzerinde. Ne zaman, sorgulamaya [hizmet türleri dağıtılan][p6], [çoğaltmaları dağıtılan][p7], ve [kodu paketleri dağıtılan ] [ p8] bir düğümde sorgu sonucu da içeren **ServicePackageActivationId** üst dağıtılmış hizmet paketi.

> [!NOTE]
>- Belirli bir uygulama için belirtilen bir düğüm üzerindeki işlem paylaşılan barındırma modeli altında yalnızca bir kopyası bir *ServicePackage* etkinleştirilir. Bunun bir **ServicePackageActivationId** eşit *boş dize*ve dağıtılan hizmet paketi ile ilgili işlemleri gerçekleştirirken belirtilmesi gerekmez. 
>
> - Barındırma özel işlemi altında belirli bir uygulama için belirtilen bir düğüm üzerindeki bir veya daha fazla kopyasını model bir *ServicePackage* etkin olabilir. Her etkinleştirme sahip bir *boş* **ServicePackageActivationId**, belirtilen dağıtılan hizmet paketi ilgili işlemleri gerçekleştirirken. 
>
> - Varsa **ServicePackageActivationId** olan atlanırsa, varsayılan olarak *boş dize*. Paylaşılan işlem modeli altında etkinleştirildiği bir dağıtılan hizmet paketi varsa, işlemi üzerinde gerçekleştirilir. Aksi takdirde işlem başarısız olur.
>
> - Bir kez ve önbellek sorgulamaz **ServicePackageActivationId**. Kimliği dinamik olarak oluşturulur ve çeşitli nedenlerle değiştirebilirsiniz. Gerektiren bir işlem gerçekleştirmeden önce **ServicePackageActivationId**, önce listesini sorgu [hizmet paketleri dağıtılan] [ p3] bir düğüm üzerinde. Ardından **ServicePackageActivationId** özgün işlemi gerçekleştirmek için sorgu sonuç.
>
>

## <a name="guest-executable-and-container-applications"></a>Konuk çalıştırılabilir ve kapsayıcı uygulamaları
Service Fabric değerlendirir [Konuk yürütülebilir] [ a2] ve [kapsayıcı] [ a3] uygulamaları kendi içindeki durum bilgisi olmayan hizmetler olarak. Hiçbir Service Fabric çalışma zamanı yok *ServiceHost* (bir işlem veya kapsayıcı). Bu hizmetler kendi içinde bulunan olduğundan çoğaltmaların sayısı *ServiceHost* bu hizmetler için geçerli değildir. Bu Hizmetleri ile kullanılan en yaygın yapılandırma ile tek bölümlü [Instancecount] [ c2] -1 (her bir küme düğümünde servis kodu bir kopyasını) değerine eşit. 

Varsayılan **ServicePackageActivationMode** için bu hizmetleri **SharedProcess**, Service Fabric yalnızca bir kopyasını etkinleştirir; bu durumda *ServicePackage* bir düğümde belirli bir uygulama için.  Bu hizmet kod yalnızca bir kopyasını bir düğüm çalışacak anlamına gelir. Bir düğümü üzerinde çalışacak şekilde hizmet kodunuzun birden çok kopyasını isterseniz, belirtmeniz **ServicePackageActivationMode** olarak **ExclusiveProcess** hizmet oluşturma zamanında. Birden çok hizmet oluşturduğunuzda, örneğin, bunu yapabilirsiniz (*Service1* için *ServiceN*), *ServiceType* (belirtilen *ServiceManifest*), ya da hizmetiniz olduğunda çoklu bölümlenmiş. 

## <a name="change-the-hosting-model-of-an-existing-service"></a>Var olan bir hizmet barındırma modeli
Şu anda mevcut bir hizmet barındırma modeli paylaşılan işleminden özel işlem (veya tersi) değiştiremezsiniz.

## <a name="choose-between-the-hosting-models"></a>Barındırma modelleri arasında seçin
Gereksinimlerinizi en iyi hangi barındırma modeli uygun değerlendirmelisiniz. Paylaşılan işlem modeli işletim sistemi kaynaklarını daha iyi ve daha az işlemleri kökenli ve aynı işlemde birden çok çoğaltma bağlantı noktalarını paylaşabilirsiniz çünkü kullanır. Ancak, çoğaltmaları birini nerede hizmet ana bilgisayarı getirmek gereken bir hata varsa, diğer tüm çoğaltmaları aynı işlemde etkiler.

 Özel işlem modeli de kendi işleminde her çoğaltma ile daha iyi yalıtım sağlar. Bir çoğaltma bir hata varsa, diğer çoğaltmaları etkilemez. Bu model, burada bağlantı noktası paylaşımı iletişim protokolü tarafından desteklenmeyen durumlar için yararlıdır. Kaynak İdaresi çoğaltma düzeyinde uygulama yeteneğini kolaylaştırır. Ancak, düğüm üzerindeki her çoğaltma için bir işlem olarak çoğaltılır gibi özel işlem daha fazla işletim sistemi kaynağı tüketir.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Özel işlem modelini ve uygulama konuları modeli
Çoğu uygulama için Service Fabric uygulamanızı bir tutarak model oluşturabilirsiniz *ServiceType* başına *ServicePackage*. 

Bazı durumlarda, Service Fabric ayrıca birden fazla sağlar *ServiceType* başına *ServicePackage* (ve bir *CodePackage* birden fazla kaydedebilirsiniz  *ServiceType*). Burada, bu yapılandırmalar yararlı olabilir senaryolardan bazıları şunlardır:

- Daha az işlemleri örnekten oluşturmak ve işlem başına daha yüksek yineleme yoğunluğu olan kaynak kullanımını iyileştirmek istediğinizde.
- Farklı çoğaltmalardan *ServiceTypes* maliyeti yüksek başlatma veya bellek varsa bazı ortak veri paylaşmak için.
- Boş bir hizmet teklifi sahip ve aynı işlemde hizmetinin tüm çoğaltmaları koyarak kaynak kullanımını bir sınır koymak istediğiniz.

Barındırma modeli özel işlem birden çok sahip bir uygulama modeli ile tutarlı değil *ServiceTypes* başına *ServicePackage*. Bunun nedeni birden çok, *ServiceTypes* başına *ServicePackage* daha yüksek kaynak çoğaltmalar arasında paylaşımı elde etmek için tasarlanmıştır ve işlem başına daha yüksek yineleme yoğunluğu sağlar. Özel işlem modeli farklı sonuçlar elde etmek için tasarlanmıştır.

Birden çok durumunu göz önünde bulundurun *ServiceTypes* başına *ServicePackage*, farklı bir sahip *CodePackage* her kaydetme *ServiceType*. Bizim düşünelim bir *ServicePackage* iki sahip'MultiTypeServicePackge ' *CodePackages*:

- Kayıtları'MyCodePackageA ' *ServiceType* 'MyServiceTypeA'.
- Kayıtları'MyCodePackageB ' *ServiceType* 'MyServiceTypeB'.

Şimdi, bir uygulama oluşturuyoruz düşünelim **fabric: / SpecialApp**. İçinde **fabric: / SpecialApp**, aşağıdaki iki özel işlem modeli hizmetleriyle oluşturun:

- Hizmet **fabric: / SpecialApp/ServiceA** iki bölüm 'MyServiceTypeA' türünde (örneğin, **P1** ve **P2**) ve bölüm başına üç çoğaltmaları.
- Hizmet **fabric: / SpecialApp/ServiceB** iki bölüm 'MyServiceTypeB' türünde (**P3** ve **P4**) ve bölüm başına üç çoğaltmaları.

Belirli bir düğümde hem hizmetleri her iki çoğaltma sahiptir. Service Fabric hizmetleri oluşturmak için özel işlem modeli kullandık olduğundan, 'MyServicePackage' ın yeni bir kopyasını her çoğaltma için etkinleştirir. Her etkinleştirme 'MultiTypeServicePackge', 'MyCodePackageA' ve 'MyCodePackageB' bir kopyasını başlatır. Ancak, 'MyCodePackageA' veya 'MyCodePackageB' yalnızca biri 'MultiTypeServicePackge' etkinleştirildiği çoğaltma barındırır. Aşağıdaki diyagramda düğümü görünüm gösterir:


![Dağıtılmış uygulama düğümü görünümünü diyagramı][node-view-five]


'MultiTypeServicePackge' etkinleştirme bölümünün yinelemenin **P1** hizmetinin **fabric: / SpecialApp/ServiceA**, 'MyCodePackageA' çoğaltma barındırma. 'MyCodePackageB' çalışıyor. Benzer şekilde, 'MultiTypeServicePackge' etkinleştirme bölümünün yinelemenin içinde **P3** hizmetinin **fabric: / SpecialApp/ServiceB**, 'MyCodePackageB' çoğaltma barındırma. 'MyCodePackageA' çalışıyor. Bu nedenle, daha fazla sayıda *CodePackages* (farklı kaydetme *ServiceTypes*) başına *ServicePackage*, o kadar yüksektir yedekli kaynak kullanımı. 
 
 Ancak, biz Hizmetleri oluşturursanız **fabric: / SpecialApp/ServiceA** ve **fabric: / SpecialApp/ServiceB** paylaşılan işlem modeliyle Service Fabric yalnızca bir kopyasını etkinleştirir. ' MultiTypeServicePackge' uygulaması için **fabric: / SpecialApp**. 'MyCodePackageA' hizmeti için tüm çoğaltmaları barındıran **fabric: / SpecialApp/ServiceA**. 'MyCodePackageB' hizmeti için tüm çoğaltmaları barındıran **fabric: / SpecialApp/ServiceB**. Aşağıdaki diyagramda bu ayarda düğümünü görüntüler: 


![Dağıtılmış uygulama düğümü görünümünü diyagramı][node-view-six]


Önceki örnekte, 'MyCodePackageA', 'MyServiceTypeA' ve 'MyServiceTypeB' kaydeder ve hiçbir 'MyCodePackageB' yoktur ve yeniden yoktur Hayır yedekli düşündüğünüzden *CodePackage* çalışıyor. Bu doğru olmasına karşın, bu uygulama modeli ile özel barındırma modeli işlem yeteri kadar uyuşmasa. Her çoğaltma kendi özel işleme yerleştirmek için hedef ise, her ikisi de kaydetmeniz gerekmez *ServiceTypes* aynı gelen *CodePackage*. Bunun yerine, her basitçe *ServiceType* kendi içinde *ServicePackage*.

## <a name="next-steps"></a>Sonraki adımlar
[Bir uygulama paketi] [ a4] ve dağıtmak hazırlanın.

[Dağıtma ve uygulamaları kaldırma][a5]. Bu makalede, uygulama örnekleri yönetmek için PowerShell kullanmayı açıklar.

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

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage
