---
title: Service Fabric Küme Kaynak Yöneticisi - uygulama grupları | Microsoft Docs
description: Service Fabric Küme Kaynak Yöneticisi'nde uygulama grubu işlevlerine genel bakış
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7e90dc00a8e042e48d8016e25dda04c15ce9f619
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62114082"
---
# <a name="introduction-to-application-groups"></a>Uygulama gruplarına giriş
Service Fabric'in Küme Kaynak Yöneticisi genellikle yük yayarak küme kaynaklarını yöneten (aracılığıyla temsil [ölçümleri](service-fabric-cluster-resource-manager-metrics.md)) kümesi boyunca eşit. Service Fabric kümesi ve kümedeki düğümlerin kapasitesini bir bütün olarak yöneten [kapasite](service-fabric-cluster-resource-manager-cluster-description.md). Ölçümler ve kapasite çok sayıda iş yükü, ancak bazen ek gereksinimleri Getir ağır kullanan farklı Service Fabric uygulama örneklerine desenleri için çok iyi çalışır. Örneğin, isteyebilirsiniz:

- Bazı adlandırılmış uygulama örneği içindeki hizmetler için kümedeki düğümlere bazı kapasite rezervasyonu
- (Bunları tüm küme yaymak) yerine adlandırılmış uygulama örneği içindeki hizmetler üzerinde çalışan düğümleri toplam sayısını sınırla
- Adlandırılmış uygulama örneğinde kendi Hizmetleri veya hizmetlerin içindeki toplam kaynak tüketimi sayısını sınırlamak için kapasiteler tanımlayın

Bu gereksinimleri karşılamak için Service Fabric Küme Kaynak Yöneticisi uygulama grupları denilen bir özelliği destekler.

## <a name="limiting-the-maximum-number-of-nodes"></a>En fazla düğüm sayısını sınırlandırma
Uygulama kapasitesi için basit kullanım örneği, bir uygulama örneği, belirli bir maksimum sayıda düğüm için sınırlı olması gerekiyor andır. Bu, tüm hizmetleri, uygulama örneğinde makine bir küme sayısı üzerine birleştirir. Birleştirme, tahmin veya fiziksel kaynak kullanımı, adlandırılmış uygulama örneği içindeki hizmetler tarafından cap çalıştığınız yararlıdır. 

Aşağıdaki görüntüde, bir uygulama örneği olan ve olmayan en fazla bir tanımlanan düğüm sayısını gösterir:

<center>

![En fazla düğüm sayısını tanımlayan bir uygulama örneği][Image1]
</center>

Sol örnekte tanımlanan düğümleri en fazla sayıda uygulama yok ve üç hizmet yok. Küme Kaynak Yöneticisi (varsayılan davranış) kümesindeki en iyi dengeyi elde etmek için altı kullanılabilir düğüm tüm çoğaltmaları yayılmış. Şu örnekte, aynı uygulama için üç düğüm sınırlı görüyoruz.

Bu davranışın denetleyen parametresi MaximumNodes çağrılır. Bu parametre uygulama oluşturma sırasında ayarlayın veya zaten çalışan bir uygulama örneği için güncelleştirilmiştir.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –ApplicationName fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

Düğüm kümesi içinde Küme Kaynak Yöneticisi, hangi hizmet nesneleri birlikte yerleştirilen veya hangi düğümleri alışmanız garanti etmez.

## <a name="application-metrics-load-and-capacity"></a>Uygulama ölçümleri, yükü ve kapasite
Uygulama grupları belirtilen adlandırılmış uygulama örneği ve bu uygulama örneğinin kapasitesi Bu ölçümler için ilişkili ölçümleri tanımlamanızı sağlar. Uygulama ölçümlerini izlemek, ayırabilir ve hizmetlerin, Uygulama örneğinin içinde kaynak tüketimini sınırlamak sağlar.

Her uygulama ölçümü için ayarlanabilir iki değer vardır:

- **Toplam uygulama kapasite** – Bu ayar uygulamanın belirli bir ölçüm için toplam kapasite temsil eder. Küme Kaynak Yöneticisi, toplam yük bu değerini aşmasına neden olan bu uygulama örneği içinde yeni hizmetler oluşturulmasını izin vermiyor. Örneğin, Uygulama örneğinin 10 kapasitesine sahipti ve beş yük zaten varsayalım. 10 toplam varsayılan yükü ile hizmet oluşturma izni.
- **En fazla düğüm kapasitesi** – Bu ayar, tek bir düğümde en fazla toplam yükleme uygulaması için belirtir. Bu kapasite aşımı yük aşması durumunda, Küme Kaynak Yöneticisi diğer düğümlere çoğaltmaları yükü azaltır. böylece taşır.


PowerShell:

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

C# İÇİN:

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a>Kapasite ayırma
Uygulama grubu için bir diğer yaygın kullanımı, küme içindeki kaynakların belirli bir uygulama örneği için ayrılmıştır sağlamaktır. Uygulama örneği oluşturulduğunda alan her zaman ayrılır.

Bile uygulama hemen gerçekleştiğinden için kümedeki alanı ayırma:
- Uygulama örneği oluşturulur ancak hizmetlerin içindeki henüz yok
- Uygulama örneği değişiklikleri her zaman içinde hizmet sayısı 
- Hizmetleri var, ancak kaynakları kullanan değil 

Bir uygulama örneği için kaynaklarını ayırma, iki ek parametre belirtilmesi gerekir: *MinimumNodes* ve *NodeReservationCapacity*

- **MinimumNodes** -Uygulama örneğinin çalışması gereken düğüm sayısı alt sınırı tanımlar.  
- **NodeReservationCapacity** -Bu ölçüm uygulamanın ayarıdır. Değer uygulamanın herhangi bir düğümde ayrılmış bu ölçümü miktarıdır. burada uygulama Hizmetleri'nde çalıştırılan.

Birleştirme **MinimumNodes** ve **NodeReservationCapacity** küme içindeki uygulama için minimum yük ayırma garanti eder. Kalan kapasite kümedeki toplam ayırma daha az var. gereken uygulama oluşturma başarısız olur. 

Kapasite ayırma, bir örneğe göz atalım:

<center>

![Uygulama örnekleri ayrılmış Kapasite tanımlama][Image2]
</center>

Sol örnekte, uygulama tanımlı herhangi bir uygulama kapasite yok. Küme Kaynak Yöneticisi, normal kurallara göre her şeyi dengeler.

Sağ taraftaki örnekte Application1 aşağıdaki ayarlarla oluşturulmuş varsayalım:

- İki MinimumNodes ayarlayın
- Bir uygulama ile tanımlanmış bir metrik
  - 20 NodeReservationCapacity

PowerShell

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

C#

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

Service Fabric için Application1 iki düğüm üzerinde kapasite ayırır ve Hizmetleri olsa bile yükü Application1 içindeki Hizmetleri tarafından zaman Tüketilmekte olan bu kapasiteye tüketmeye Uygulama2 dizinlerini izin vermez. Bu uygulama ayrılmış kapasite kullanılacak ve bu düğümde ve küme içindeki sayılır kalan kapasite karşı değerlendirilir.  Ancak, yalnızca en az bir hizmet nesnesi üzerinde yerleştirildiğinde ayrılmış tüketim belirli bir düğümün kapasiteden çıkarılır ayırma kalan küme kapasiteden hemen çıkarılır. Daha sonra bu ayırma, esneklik ve daha iyi kaynak kullanımı için kaynakları yalnızca gerektiğinde düğümlerinde ayrılmış bu sağlar.

## <a name="obtaining-the-application-load-information"></a>Uygulama yük bilgilerini alma
Her uygulama için bir uygulama çoğaltmaları Hizmetleri tarafından bildirilen toplam yükü hakkında bilgi edinebilirsiniz bir veya daha fazla ölçüm için tanımlanan kapasitesi sahip.

PowerShell:

``` posh
Get-ServiceFabricApplicationLoadInformation –ApplicationName fabric:/MyApplication1
```

C#

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

Uygulamayı belirtilen uygulama kapasitesi hakkında temel bilgileri ApplicationLoad sorguyu döndürür. Bu bilgiler, en az düğümler ve en fazla düğüm bilgileri ve uygulamanın şu anda kaplamıyordur sayısını içerir. Ayrıca her uygulama yükü ölçümü hakkında bilgi içerir dahil olmak üzere:

* Ölçüm adı: Ölçüm adı.
* Kapasite ayırma: Bu uygulama için ayrılmış kümedeki küme kapasitesi.
* Uygulama yükleme: Bu uygulamanın alt yineleme toplam yükü.
* Uygulama kapasitesi: Uygulama yük değeri izin verilen üst sınırı.

## <a name="removing-application-capacity"></a>Uygulama kapasitesi kaldırılıyor
Uygulama kapasitesi parametreleri için bir uygulama ayarlandıktan sonra güncelleştirme uygulama API'ler veya PowerShell cmdlet'leri kullanılarak kaldırılabilir. Örneğin:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Bu komut tüm uygulama kapasite yönetimi parametreleri uygulama örneğinden kaldırır. Bu MinimumNodes MaximumNodes ve uygulama ölçümleri varsa içerir. Komut etkisini hemen geçerli olur. Bu komut tamamlandıktan sonra Küme Kaynak Yöneticisi uygulamaları yönetmek için varsayılan davranışı kullanır. Uygulama kapasitesi parametreleri belirtilebilir yeniden aracılığıyla `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.

### <a name="restrictions-on-application-capacity"></a>Uygulama kapasitesi kısıtlamaları
Uygulama kapasitesi parametrelerindeki dikkate alınması gereken birkaç kısıtlama vardır. Hiçbir değişiklik doğrulama hatalar varsa gerçekleşir.

- Tüm tamsayı parametre negatif olmayan sayı olması gerekir.
- MinimumNodes hiçbir zaman MaximumNodes büyük olmalıdır.
- Bir yük ölçüm için kapasiteler tanımlanmışsa, bunlar şu kurallara uymalıdır:
  - Düğüm ayırma kapasitesi en fazla düğüm kapasitesinden büyük olmamalıdır. Örneğin, iki birim için düğümde "CPU" ölçüm için kapasite sınırlandırmak ve üç birimleri her bir düğümde ayrılacak deneyin.
  - MaximumNodes belirtilirse, ürün MaximumNodes ve en fazla düğüm kapasitesi toplam uygulama kapasiteden büyük olmamalıdır. Örneğin, "CPU" sekiz için ayarlanmış yük ölçüm için en fazla düğüm kapasitesi varsayalım. Ayrıca en fazla düğüme 10 olarak ayarladığınızı varsayalım. Bu durumda, toplam uygulama kapasite bu yük ölçüm için 80'den büyük olmalıdır.

Kısıtlamaları, hem uygulama oluşturma ve güncelleştirme sırasında uygulanır.

## <a name="how-not-to-use-application-capacity"></a>Uygulama kapasitesi kullanma değil
- Uygulamaya sınırlamak için uygulama grubu özelliklerini kullanmaya çalışmayın bir _belirli_ düğümlerinin alt kümesi. Diğer bir deyişle, uygulamanın en fazla beş düğüm üzerinde çalıştığını belirtebilirsiniz, ancak küme içindeki belirli hangi beş düğüm. Bir uygulama belirli düğümlere sınırlama, hizmetler için yerleştirme kısıtlamaları kullanılarak gerçekleştirilebilir.
- Aynı uygulamadaki iki hizmet aynı düğümlere yerleştirildiğinden emin olmak için uygulama kapasitesi kullanmaya çalışmayın. Bunun yerine kullanın [benzeşim](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) veya [yerleştirme kısıtlamaları](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Küme Kaynak Yöneticisi yönetir ve kümedeki yük dengeleyen hakkında bilgi almak için makalesine göz atın [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
- En baştan başlatmak ve [için Service Fabric Küme Kaynak Yöneticisi giriş yapın](service-fabric-cluster-resource-manager-introduction.md)
- Ölçümler genellikle nasıl çalıştığı hakkında daha fazla bilgi için okumaya [Service Fabric yük ölçümleri](service-fabric-cluster-resource-manager-metrics.md)
- Küme Kaynak Yöneticisi kümesi tanımlamak için birçok seçenek vardır. Bunlar hakkında daha fazla bilgi için bu makalede atın [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
