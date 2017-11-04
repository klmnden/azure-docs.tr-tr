---
title: "Barındırma modeli azure Service Fabric | Microsoft Docs"
description: "Dağıtılan bildirimleri yapı hizmeti ve hizmet ana bilgisayar işlemi çoğaltmaları (veya örnekleri) arasındaki ilişkiyi açıklar."
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: ecc9038cf895ddaeb06dd0e4e9852d5ef4a4513a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="service-fabric-hosting-model"></a>Service Fabric barındırma modeli
Bu makale, Service Fabric tarafından sağlanan modelleri barındıran uygulama genel bir bakış sağlar ve arasındaki farklar açıklanmaktadır **paylaşılan işlem** ve **özel işlem** modeller. Dağıtılan bir uygulama, bir Service Fabric düğümü ve hizmet ve hizmet ana bilgisayar işlemi çoğaltmaları (veya örnekleri) arasındaki ilişkiyi nasıl göründüğünü açıklayan.

Devam etmeden önce aşina olduğundan emin olun [Service Fabric uygulama modeli] [ a1] ve çeşitli kavramlar ve aralarındaki ilişkisi anladığınızdan emin olun. 

> [!NOTE]
> Basitlik için bu makalede, açıkça belirtilmediği sürece:
>
> - Word'ün tüm kullanır *çoğaltma* hem çoğaltmasını durum bilgisi olan hizmet ya da durum bilgisi olmayan bir hizmet örneği için başvuruyor.
> - *CodePackage* kabul eşdeğeridir *ServiceHost* kaydeder işlem bir *ServiceType* ve hizmetlerin, konaklar çoğaltmaları *ServiceType* .
>

Barındırma modeli anlamak için bize bir örnek yol. Bize söyleyin, sahip olduğumuz bir *ApplicationType* 'olan MyAppType' bir *ServiceType* 'tarafından sağlanan MyServiceType' *ServicePackage* 'olan MyServicePackage' bir *CodePackage* 'hangi kaydeder MyCodePackage' *ServiceType* 'çalıştırıldığında MyServiceType'.

Diyelim ki 3 düğümlü bir kümeniz olduğuna ve oluşturuyoruz bir *uygulama* **fabric: / App1** 'MyAppType' türünde. Bu içinde *uygulama* **fabric: / App1** hizmet oluşturuyoruz **doku: / App1/ServiceA** '2 bölümleri olan MyServiceType' türünde (söyleyin **P1**  &  **P2**) ve bölüm başına 3 çoğaltmaları. Aşağıdaki diyagramda, bu uygulama görünümünü sona eriyor dağıtılan bir düğümde gösterir.

<center>
![Dağıtılmış uygulama düğümü görünümü][node-view-one]
</center>

Service Fabric etkinleştirilmiş '', her iki bölüm çoğaltmalardan yani barındırma MyCodePackage' başlatılan MyServicePackage' **P1** & **P2**. Kümedeki düğümler sayıya eşit bölüm başına çoğaltmaların sayısı Seçtiğimiz kümedeki tüm düğümler aynı görünümü sahip gerektiğini unutmayın. Başka bir hizmet oluşturalım **doku: / App1/ServiceB** uygulamada **fabric: / App1** 1 bölümü olan (söyleyin **P3**) ve bölüm başına 3 çoğaltmaları. Aşağıdaki diyagramda düğüm üzerindeki yeni görünüm gösterir:

<center>
![Dağıtılmış uygulama düğümü görünümü][node-view-two]
</center>

Service Fabric bölümü için yeni çoğaltma yerleştirilen görebiliriz gibi **P3** hizmetinin **fabric: / App1/ServiceB** 'MyServicePackage' ın mevcut etkinleştirme. Şimdi sağlar başka oluşturmak *uygulama* **fabric: / App2** 'MyAppType' türünde ve içinde **doku: / App2** hizmet oluşturma **doku: / App2/ServiceA** 2 bölümleri olan (söyleyin **P4** & **P5**) ve bölüm başına 3 çoğaltmaları. Diyagramları aşağıdaki yeni düğümü görünüm gösterir:

<center>
![Dağıtılmış uygulama düğümü görünümü][node-view-three]
</center>

Bu süre Service Fabric etkinleştirilmiş 'hem de hizmet bölümlerden 'MyCodePackage' ve çoğaltmaları yeni bir kopyasını başlatan MyServicePackage' ın yeni bir kopyasını **fabric: / App2/ServiceA** (yani **P4**  &  **P5**) bu yeni kopya 'MyCodePackage' yerleştirilir.

## <a name="shared-process-model"></a>Paylaşılan işlem modeli
Yukarıdaki gördüğümüz ve barındırma modeli Service Fabric tarafından sağlanan varsayılan olarak adlandırılır **paylaşılan işlem** modeli. Bu modelde için bir verilen *uygulama*, yalnızca bir kopya bir verilen *ServicePackage* üzerinde etkinleştirilmiş bir *düğümü* (tüm başladığı *CodePackages* içerdiği) ve tüm hizmetlerin tüm çoğaltmaları bir verilen *ServiceType* yerleştirilir *CodePackage* , kaydeder *ServiceType*. Diğer bir deyişle, tüm çoğaltmaların düğümünde tüm hizmetlerin bir verilen *ServiceType* aynı işlem paylaşın.

## <a name="exclusive-process-model"></a>Özel işlem modeli
Service Fabric tarafından sağlanan diğer barındırma modeli **özel işlem** modeli. Bu modelde, üzerinde bir verilen *düğümü*, her çoğaltma yerleştirmek için Service Fabric yeni bir kopyasını etkinleştirir *ServicePackage* (tüm başladığı *CodePackages* içerdiği) ve çoğaltma yerleştirilir *CodePackage* kayıtlı *ServiceType* hizmetinin hangi çoğaltmaya ait. Diğer bir deyişle, her çoğaltma adanmış kendi işleminde yaşar. 

Bu model, sürüm Service Fabric 5.6 itibaren desteklenmektedir. **Özel işlem** modeli hizmet oluşturma sırasında seçilen (kullanarak [PowerShell][p1], [REST] [ r1] veya [FabricClient][c1]) belirterek **ServicePackageActivationMode** 'ExclusiveProcess' olarak.

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

Varsayılan hizmet uygulama bildiriminizi varsa, seçebileceğiniz **özel işlem** belirterek model **ServicePackageActivationMode** özniteliği aşağıda gösterildiği gibi:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Yukarıdaki örnekle devam edersek, başka bir hizmet oluşturma sağlar **doku: / App1/ServiceC** uygulamada **fabric: / App1** 2 bölümleri olan (söyleyin **P6**  &  **7**) ve bölüm başına 3 çoğaltmaları **ServicePackageActivationMode** 'ExclusiveProcess' ayarlayın. Aşağıdaki diyagramda düğüm üzerindeki yeni bir görünüm gösterir:

<center>
![Dağıtılmış uygulama düğümü görünümü][node-view-four]
</center>

Gördüğünüz gibi iki yeni kopyasını 'MyServicePackage' Service Fabric etkinleştirilmiş (bölümünden her çoğaltma için bir tane **P6** & **7**) ve ayrılmış kopyasınıherçoğaltmayerleştirilmiş*CodePackage*. İşte, not etmek için başka bir şey olduğunda **özel işlem** modeli kullanılır, için bir verilen *uygulama*, birden çok kopya bir verilen *ServicePackage* üzerindeetkinolabilir *Düğüm*. İçinde Yukarıdaki örnek, biz 'MyServicePackage' üç kopyasını için etkin olup olmadığını **fabric: / App1**. Her 'MyServicePackage' etkin bu kopyasını sahip bir **ServicePackageActivationId** içinde bu kopyayı tanımlayan ilişkili *uygulama* **fabric: / App1**.

Sadece **paylaşılan işlem** modeli için kullanılan bir *uygulama*gibi **doku: / App2** , yukarıdaki örnek, yoktur, yalnızca bir etkin kopyanın *ServicePackage*  üzerinde bir *düğümü* ve **ServicePackageActivationId** bu etkinleştirmesi için *ServicePackage* ' boş bir dize '.

> [!NOTE]
>- **İşlem paylaşılan** barındırma modeli karşılık gelen **ServicePackageActivationMode** eşit **SharedProcess**. Bu barındırma modeli varsayılandır ve **ServicePackageActivationMode** hizmet oluşturma sırasında belirtilmesi gerekmez.
>
>- **Özel işlem** barındırma modeli karşılık gelen **ServicePackageActivationMode** eşit **ExclusiveProcess** ve hizmet oluşturma sırasında açıkça belirtilmesi gerekir. 
>
>- Bir hizmet barındırma modeli bilinir sorgulayarak [hizmet açıklaması] [ p2] ve değeri **ServicePackageActivationMode**.
>
>

## <a name="working-with-deployed-service-package"></a>Dağıtılan hizmet paketi ile birlikte çalışma
Etkin bir kopyasını bir *ServicePackage* bir düğüm olarak adlandırılır [hizmet paketi dağıtılan][p3]. Daha önce ne zaman yukarıdaki **özel işlem** modeli için hizmetleri oluşturmak için kullanılan bir verilen *uygulama*, birden çok dağıtılmış hizmet paketleri için aynı olması  *ServicePackage*. Dağıtılan hizmet paketi gibi belirli işlemleri gerçekleştirirken [dağıtılmış hizmet paketi durumunu raporlama] [ p4] veya [dağıtılan hizmet paketikodpaketiyenidenbaşlatılıyor] [ p5] vb. **ServicePackageActivationId** belirli bir tanımlamak için dağıtılan hizmet paketi sağlanması gerekiyor.

 **ServicePackageActivationId** dağıtılan bir hizmet paketi listesini sorgulayarak elde edilebilir [hizmet paketleri dağıtılan] [ p3] bir düğüm üzerinde. Sorgulanırken [hizmet türleri dağıtılan][p6], [çoğaltmaları dağıtılan] [ p7] ve [kodu paketleridağıtılan] [ p8] bir düğümde sorgu sonucu da içeren **ServicePackageActivationId** üst dağıtılmış hizmet paketi.

> [!NOTE]
>- Altında **paylaşılan işlem** üzerinde barındırma modeli, bir verilen *düğümü*, için bir verilen *uygulama*, yalnızca bir kopya bir *ServicePackage* etkinleştirilir . Bunun **ServicePackageActivationId** eşit *boş dize* ve işlemleri gerçekleştirme dağıtılmış hizmet paketi ilişkili belirtilmesi gerekmez. 
>
> - Altında **özel işlem** üzerinde barındırma modeli, bir verilen *düğümü*, için bir verilen *uygulama*, bir veya daha fazla kopyasını içeren bir *ServicePackage* can etkin olması. Her etkinleştirme sahip bir *boş* **ServicePackageActivationId** ve dağıtılan gerçekleştirilirken belirtilmesi için hizmet ilgili işlemleri paket. 
>
> - Varsa **ServicePackageActivationId** olduğundan 'boş dize' varsayılanlara atlanmış. Dağıtılan bir hizmet paketini, altında etkinleştirildi **paylaşılan işlem** modeli varsa, sonra işlemi yapılmalıdır, aksi takdirde işlem başarısız olur.
>
> - Bir kez sorgulamak ve önbellek için önerilmez **ServicePackageActivationId** dinamik olarak oluşturulur ve çeşitli nedenlerle değiştirebilirsiniz. Gereken bir işlem gerçekleştirmeden önce **ServicePackageActivationId**, önce listesini sorgu [hizmet paketleri dağıtılan] [ p3] düğümünü ve ardından kullanıma  **ServicePackageActivationId** özgün işlemi gerçekleştirmek için sorgu sonuç.
>
>

## <a name="guest-executable-and-container-applications"></a>Konuk çalıştırılabilir ve kapsayıcı uygulamaları
Service Fabric değerlendirir [Konuk yürütülebilir] [ a2] ve [kapsayıcı] [ a3] , yani müstakil statless Hizmetleri olarak uygulamaları hiçbir Service Fabric çalışma zamanı yok *ServiceHost* (bir işlem veya kapsayıcı). Bu hizmetler kendi içinde bulunan olduğundan çoğaltmaların sayısı *ServiceHost* bu hizmetler için geçerli değildir. Tek bölümlü hizmetlerle kullanılan en yaygın yapılandırma ile [Instancecount] [ c2] -1 (örn. bir kopyası her bir küme düğümünde servis kodu) değerine eşit. 

Varsayılan **ServicePackageActivationMode** için bu hizmetleri **SharedProcess** Service Fabric yalnızca bir kopyasını etkinleştirir; bu durumda *ServicePackage* bir üzerinde *Düğüm* için bir verilen *uygulama* hizmeti kodunu yalnızca bir kopyasını başka bir deyişle, çalışacak bir *düğümü*. Hizmet kodunuzu çalıştırmak için birden çok kopyasını isteyip istemediğinizi bir *düğümü* birden fazla hizmet oluşturduğunuzda (*Service1* için *ServiceN*), *ServiceType* (belirtilen *ServiceManifest*) ya da hizmetiniz çoklu bölümlenmiş olduğunda belirtmelisiniz **ServicePackageActivationMode** olarak **ExclusiveProcess**hizmet oluşturma zamanında.

## <a name="changing-hosting-model-of-an-existing-service"></a>Var olan bir hizmet barındırma modelini değiştirme
Varolan bir hizmetinden barındırma modelini değiştirme **paylaşılan işlem** için **özel işlem** tersi yükseltme veya güncelleştirme mekanizması (veya varsayılan uygulama belirtiminde hizmet aracılığıyla bildirim) şu anda desteklenmiyor. Bu özellik için destek gelecek sürümlerde gelecektir.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>Paylaşılan işlem ve özel işlem modeli arasında seçim yapma
Bu iki barındırma modelleri, kendi olumlu ve olumsuz ve hangisinin gereksinimlerine en iyi şekilde uyduğunu değerlendirmek üzere kullanıcı gereksinimlerini vardır. **İşlem paylaşılan** modeli sağlayan daha iyi kullanımı, işletim sistemi kaynaklarının daha az işlemleri kökenli olduğundan, aynı işlemde birden çok çoğaltma paylaşabilirsiniz bağlantı noktaları, vs. Ancak, çoğaltmaları birini nerede hizmet ana bilgisayarı getirmek için gereken bir hata değerse, diğer tüm çoğaltmaları aynı işlemde etkiler.

 **Özel işlem** modeli de kendi işleminde her çoğaltma ile daha iyi yalıtımı sağlar ve hatalı davranan çoğaltma diğer çoğaltmaları etkilemez. Bunu burada bağlantı noktası paylaşımı iletişim protokolü tarafından desteklenmeyen durumlarda kullanışlı devreye girer. Kaynak İdaresi çoğaltma düzeyinde uygulama yeteneğini kolaylaştırır. Diğer taraftan, **özel işlem** düğüm üzerindeki her çoğaltma için bir işlem olarak çoğaltılır daha fazla işletim sistemi kaynağı tüketir.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Özel işlem modelini ve uygulama konuları modeli
Service Fabric uygulamanızı modellemek için önerilen yöntem bir korumaktır *ServiceType* başına *ServicePackage* ve bu model iyi uygulamaların çoğu için çalışır. 

Belirli kullanım için hedeflenen, Service Fabric ayrıca birden fazla sağlar *ServiceType* başına *ServicePackage* (ve bir *CodePackage* birden fazla kaydedebilirsiniz *ServiceType*). Burada, bu yapılandırmalar yararlı olabilir senaryolardan bazıları şunlardır:

- Daha az işlemleri örnekten oluşturmak ve işlem başına daha yüksek yineleme yoğunluğu olan işletim sistemi kaynak kullanımını iyileştirmek istediğinizde.
- Farklı ServiceTypes çoğaltmalardan yüksek başlatma veya maliyet bellek varsa bazı ortak verileri paylaşmanız gerekir.
- Ücretsiz hizmet sunumu sahip ve aynı işlemde hizmetinin tüm çoğaltmaları koyarak kaynak kullanımını bir sınır koymak istediğiniz.

**Özel işlem** barındırma modeli değil tutarlı sahip birden çok uygulama modeliyle *ServiceTypes* başına *ServicePackage*. Bunun nedeni birden çok, *ServiceTypes* başına *ServicePackage* daha yüksek kaynak çoğaltmalar arasında paylaşımı elde etmek için tasarlanmıştır ve işlem başına daha yüksek yineleme yoğunluğu sağlar. Bu nedir aykırı **özel işlem** model elde etmek üzere tasarlanmıştır.

Birden çok durumunu göz önünde bulundurun *ServiceTypes* başına *ServicePackage* farklı ile *CodePackage* her kaydetme *ServiceType* . Bizim düşünelim bir *ServicePackage* 'iki olan MultiTypeServicePackge' *CodePackages*:

- 'Da kaydeder MyCodePackageA' *ServiceType* 'MyServiceTypeA'.
- 'Da kaydeder MyCodePackageB' *ServiceType* 'MyServiceTypeB'.

Şimdi sağlar söyleyin oluşturuyoruz bir *uygulama* **fabric: / SpecialApp** ve iç **doku: / SpecialApp** iki Hizmetleri ile aşağıdaki oluşturmak **özel İşlem** modeli:

- Hizmet **fabric: / SpecialApp/ServiceA** iki bölüm 'MyServiceTypeA' türünde (söyleyin **P1** ve **P2**) ve bölüm başına 3 çoğaltmaları.
- Hizmet **fabric: / SpecialApp/ServiceB** iki bölüm 'MyServiceTypeB' türünde (söyleyin **P3** ve **P4**) ve bölüm başına 3 çoğaltmaları.

Belirli bir düğümde her iki hizmet iki çoğaltma sahip olur. Kullandık beri **özel işlem** hizmetleri oluşturmak için model Service Fabric 'MyServicePackge' ın yeni bir kopyasını her çoğaltma için etkinleşir. Her etkinleştirme 'MultiTypeServicePackge', 'MyCodePackageA' ve 'MyCodePackageB' bir kopyasını başlar. Ancak, 'MyCodePackageA' veya 'MyCodePackageB' yalnızca biri 'MultiTypeServicePackge' etkinleştirildiği çoğaltmasını barındıracak. Aşağıdaki diyagramda düğüm görünümü gösterir:

<center>
![Dağıtılmış uygulama düğümü görünümü][node-view-five]
</center>

 'MultiTypeServicePackge' etkinleştirme bölümünün yinelemenin görebiliriz gibi **P1** hizmetinin **fabric: / SpecialApp/ServiceA**, 'MyCodePackageA' çoğaltma barındırma ve 'MyCodePackageB' ise yalnızca yukarı ve çalışıyor. Benzer şekilde, 'MultiTypeServicePackge' etkinleştirme bölümünün yinelemenin içinde **P3** hizmetinin **fabric: / SpecialApp/ServiceB**, 'MyCodePackageB' çoğaltma barındırma ve 'MyCodePackageA' ise yalnızca hazır ve çalışır ve benzeri. Bu nedenle, daha sayısı *CodePackages* (farklı kaydetme *ServiceTypes*) başına *ServicePackage*, yedekli kaynak kullanımı daha yüksek olur. 
 
 Biz Hizmetleri oluşturursanız, diğer yandan **fabric: / SpecialApp/ServiceA** ve **fabric: / SpecialApp/ServiceB** ile **paylaşılan işlem** modeli, Service Fabric etkinleşir 'MultiTypeServicePackge' için yalnızca bir kopyasını *uygulama* **fabric: / SpecialApp** (daha önce gördüğümüz gibi). 'MyCodePackageA' ana bilgisayar hizmeti için tüm çoğaltmaları **fabric: / SpecialApp/ServiceA** (veya daha kesin olarak ' MyServiceTypeA' türünde herhangi bir hizmeti) ve 'MyCodePackageB' ana bilgisayar hizmeti için tüm çoğaltmaları **fabric: / SpecialApp/ServiceB** (veya, herhangi bir hizmet türü 'daha kesin olarak MyServiceTypeB'). Aşağıdaki diyagramda bu ayarda düğümünü görüntüler: 

<center>
![Dağıtılmış uygulama düğümü görünümü][node-view-six]
</center>

Yukarıdaki örnekte 'MyCodePackageA', 'MyServiceTypeA' ve 'MyServiceTypeB' kaydeder ve hiçbir 'MyCodePackageB' yoktur durumunda Hayır yedekli da olacaktır düşünebilirsiniz *CodePackage* çalışıyor. Bu doğru ise, ancak, daha önce belirtildiği gibi bu uygulama modeli ile yeteri kadar uyuşmasa **özel işlem** barındırma modeli. Her çoğaltma kendi adanmış koymak için hedef ise, her ikisi de kaydetme işlem *ServiceTypes* aynı gelen *CodePackage* gerekli değildir ve her koyma *ServiceType* içinde kendi *ServicePacakge* daha doğal bir seçimdir.

## <a name="next-steps"></a>Sonraki adımlar
[Bir uygulama paketi] [ a4] ve dağıtmak hazırlanın.

[Dağıtma ve uygulamaları kaldırma] [ a5] uygulama örnekleri yönetmek için PowerShell kullanmayı açıklar.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-deploy-existing-app.md
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
