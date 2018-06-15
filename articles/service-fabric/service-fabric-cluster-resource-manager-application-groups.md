---
title: Service Fabric kümesi Kaynak Yöneticisi - uygulama grupları | Microsoft Docs
description: Service Fabric kümesi Kaynak Yöneticisi'nde uygulama grubu işlevlerine genel bakış
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 215efc1f0597f5199dd37baf4b109d7e76040aae
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34213002"
---
# <a name="introduction-to-application-groups"></a>Uygulama grupları giriş
Service Fabric'ın Küme Kaynağı Yöneticisi genellikle yük dengelemesini yaparak küme kaynaklarını yönetir (aracılığıyla temsil [ölçümleri](service-fabric-cluster-resource-manager-metrics.md)) kümesi boyunca eşit. Service Fabric yönetir ve kümesindeki düğümlerin kapasite bir bütün olarak [kapasite](service-fabric-cluster-resource-manager-cluster-description.md). Ölçümleri ve kapasite birçok iş yükü, ancak bazen ek gereksinimleri Getir yoğun olarak kullanılır farklı hizmet doku uygulama örnekleri desenleri harika çalışır. Örneğin, isteyebilirsiniz:

- Yedek kümedeki düğümlere bazı kapasite bazı Adlandırılmış uygulama örneği içindeki hizmetler için
- (Bunları tüm küme yaymak) yerine adlandırılmış uygulama örneği içindeki hizmetler üzerinde çalışan düğümleri toplam sayısını sınırla
- Örneğindeki adlandırılmış uygulama kendisini hizmetlerin veya Hizmetleri içindeki toplam kaynak tüketimini sınırlamak için kapasiteleri tanımlayın

Bu gereksinimleri karşılamak için Service Fabric kümesi Kaynak Yöneticisi uygulama grupları adlı bir özelliği destekler.

## <a name="limiting-the-maximum-number-of-nodes"></a>En fazla düğüm sayısını sınırlandırma
Uygulama örneğini bir belirli en fazla düğüm sayısını için sınırlı olması gereken basit kullanım uygulama kapasite durumdur. Bu uygulama örneği makine kümesi sayısı üzerine içindeki tüm hizmetler birleştirir. Birleştirme, tahmin etmek ya da bu adlandırılmış uygulama örneği içindeki hizmetler tarafından fiziksel kaynak kullanımını cap çalıştığınız yararlıdır. 

Aşağıdaki resimde bir uygulama örneği ile ve tanımlanan düğüm sayısı üst sınırı olmadan gösterilmektedir:

<center>
![Uygulama örneği en fazla düğüm sayısını tanımlama][Image1]
</center>

Sol örnekte, uygulama tanımlı düğüm sayısı üst sınırı yok ve üç hizmeti vardır. Küme Kaynak Yöneticisi'ni tüm çoğaltmaları (varsayılan davranış) kümedeki en iyi dengeyi elde etmek için altı kullanılabilir düğümleri arasında yayılır. Şu örnekte, üç düğümlerine sınırlı aynı uygulama bakın.

Bu davranışı denetler parametre en fazla düğüm adı verilir. Bu parametre uygulama oluşturma sırasında ayarlayın veya zaten çalışan bir uygulama örneği için güncelleştirilmiştir.

Powershell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
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

Düğümleri kümesi içinde Küme Kaynak Yöneticisi'ni hangi hizmet nesnelerin araya yerleştirilen veya hangi düğümlerin alışmanız garanti etmez.

## <a name="application-metrics-load-and-capacity"></a>Uygulama ölçümleri, yükü ve kapasite
Uygulama grupları verilen adlandırılmış uygulama örneği ve bu ölçümleri için o uygulama örneğinin kapasitesi ile ilişkili ölçümleri tanımlamanızı sağlar. Uygulama ölçümleri, izlemek, ayırmak ve Hizmetleri, uygulama örneği içinde kaynak tüketimini sınırlamak izin verir.

Her uygulama ölçümü için ayarlanabilir iki değer vardır:

- **Toplam uygulama kapasitesinin** – Bu ayar uygulama için belirli bir ölçüm toplam kapasitesini temsil eder. Küme Kaynak Yöneticisi'ni yeni hizmetlerin bu değeri aşacak toplam yük neden olacağından bu uygulama örneği içinde oluşturulmasına izin vermez. Örneğin, uygulama örneği 10 kapasitesine sahip ve beş yükünü zaten varsayalım. Bir hizmet oluşturma 10 toplam varsayılan yükü ile izin verilmeyen.
- **En fazla düğüm kapasitesi** – Bu ayar, tek bir düğümünde uygulama için en fazla toplam yükleme belirtir. Yük bu kapasite kalırsa, yükü azaldıktan Küme Kaynak Yöneticisi'ni çoğaltmaları diğer düğümlere taşır.


PowerShell:

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

C# ' TA:

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
Uygulama grupları için başka bir yaygın kullanım kümedeki kaynaklar için belirli uygulama örneği ayrıldığından emin olmaktır. Uygulama örneği oluşturulduğunda, alan her zaman ayrılır.

Bile uygulama hemen gerçekleştiğinden için kümedeki alanı ayırma:
- Uygulama örneği oluşturulur ancak hizmetlerin içindeki henüz yok
- Uygulama örneği değişiklikleri her zaman içindeki Hizmetler sayısı 
- Hizmetler var, ancak kaynak tüketmeye değil 

İki ek parametreler belirtmek uygulama örneğini gerektirir kaynaklarını ayırma: *düğüm* ve *NodeReservationCapacity*

- **En az düğüm** -Uygulama örneğinin çalışması gereken düğüm sayısı alt sınırı tanımlar.  
- **NodeReservationCapacity** -bu uygulama için ölçüm başına ayardır. Değer herhangi bir düğümde uygulama için ayrılmış, ölçüm miktarıdır. Burada, o uygulama hizmetlerini çalıştırmak.

Birleştirme **düğüm** ve **NodeReservationCapacity** küme içindeki uygulama için en düşük yük ayırma güvence altına alır. Kalan kapasite kümedeki toplam ayırma daha az var. gerekli uygulamanın oluşturma başarısız olur. 

Kapasite ayırma bir örneğe bakalım:

<center>
![Uygulama örnekleri ayrılmış Kapasite tanımlama][Image2]
</center>

Sol örnekte, uygulamaları herhangi uygulama tanımlı kapasiteye sahip değil. Küme Kaynak Yöneticisi'ni normal kurallarına göre her şeyi dengeler.

Sağdaki örnekte Application1 aşağıdaki ayarlarla oluşturuldu varsayalım:

- İki düğüm ayarlama
- Bir uygulama ile tanımlanmış ölçümü
  - 20 NodeReservationCapacity

Powershell

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

Service Fabric Application1 için iki düğüm üzerinde kapasite ayırır ve Hizmetleri Uygulama2 dizinlerini olsa bile herhangi bir yük tarafından Application1 içinde Hizmetleri zaman mı tüketiliyor kapasiteyi kullanmasına izin vermez. Bu ayrılmış uygulama kapasite tüketilen ve o düğümde ve kümedeki kalan kapasite karşı sayılarını değerlendirilir.  Ancak, yalnızca en az bir hizmet nesnesi üzerinde yerleştirildiğinde ayrılmış tüketim belirli bir düğümün kapasiteden çıkarılır ayırma kalan küme kapasiteden hemen çıkarılır. Kaynaklar yalnızca gerekli olduğunda düğümlerinde ayrılmış bu daha sonra bu ayırma esneklik ve daha iyi kaynak kullanımı için sağlar.

## <a name="obtaining-the-application-load-information"></a>Uygulama yükleme bilgilerini alma
Her uygulama için bir veya daha fazla ölçümleri hizmetlerinin çoğaltmaları tarafından bildirilen toplam yükleme hakkında bilgi edinmek için tanımlanan bir uygulama kapasitesine sahiptir.

PowerShell:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
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

ApplicationLoad sorgu uygulama için belirtilen uygulama kapasite hakkındaki temel bilgileri döndürür. Bu bilgiler, Minimum düğümleri ve en fazla düğüm bilgileri ve uygulama şu anda kullandığı numarasını içerir. Ayrıca her uygulamayı yük ölçüm hakkında bilgi içerir dahil olmak üzere:

* Ölçüm adı: Ölçüm adı.
* Ayırma kapasitesi: kümede bu uygulama için ayrılmış küme kapasitesi.
* Uygulama yük: Bu uygulamanın alt çoğaltmalarının toplam yük.
* Uygulama kapasitesi: İzin verilen en yüksek uygulama yük değeri.

## <a name="removing-application-capacity"></a>Uygulama kapasite kaldırma
Bir uygulama için uygulama kapasite parametreleri ayarlandıktan sonra güncelleştirme uygulama API'leri veya PowerShell cmdlet'leri kullanılarak kaldırılabilir. Örneğin:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Bu komut tüm uygulama kapasite yönetim parametreleri uygulama örneğinden kaldırır. Bu düğüm, en fazla düğüm ve uygulamanın ölçümleri varsa içerir. Komutun etkisini hemen uygulanır. Bu komut tamamlandıktan sonra Küme Kaynak Yöneticisi'ni varsayılan davranışı uygulamaları yönetmek için kullanır. Uygulama kapasite parametreleri belirtilebilir yeniden aracılığıyla `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.

### <a name="restrictions-on-application-capacity"></a>Uygulama kapasite kısıtlamalar
Dikkate alınması gereken uygulama kapasite parametreleri bazı kısıtlamalar vardır. Doğrulama hatası varsa herhangi bir değişiklik gerçekleşir.

- Tüm tamsayı parametreleri negatif olmayan sayı olması gerekir.
- En az düğüm asla en fazla düğüm büyük olmalıdır.
- Ardından bir yük ölçümü için kapasiteler tanımlanmışsa, bu kurallara uymalıdır:
  - Düğüm ayırma kapasitesi en fazla düğüm kapasitesi büyük olmamalıdır. Örneğin, iki birimlerine düğümde "CPU" ölçüm için kapasite sınırlamak ve üç birimleri her düğümde ayrılacak deneyin.
  - En fazla düğüm belirtilirse, en fazla düğüm ve en fazla düğüm kapasitesi çarpımı toplam uygulama kapasiteden büyük olmamalıdır. Örneğin, "CPU" sekiz için ayarlanmış yük ölçümü için en fazla düğüm kapasitesi varsayalım. Ayrıca en fazla düğüm sayısı 10'a ayarladığınızı varsayalım. Bu durumda, toplam uygulama kapasitesinin Bu yük ölçüm için 80'den büyük olmalıdır.

Kısıtlamaları, hem uygulama oluşturma ve güncelleştirme sırasında zorlanır.

## <a name="how-not-to-use-application-capacity"></a>Uygulama kapasite kullanma değil
- Uygulamaya sınırlamak için uygulama grubu özelliklerini kullanmaya çalışmayın bir _belirli_ alt düğümleri kümesi. Diğer bir deyişle, uygulamanın en fazla beş düğümlerinde çalıştığını belirtebilirsiniz, ancak küme içindeki belirli hangi beş düğümü. Belirli düğümler uygulamaya kısıtlamadır yerleşim kısıtlaması Hizmetleri kullanarak elde edilebilir.
- İki Hizmetleri'nden aynı uygulama aynı düğümlerinde yerleştirilir emin olmak için uygulama kapasite kullanmaya çalışmayın. Bunun yerine kullanın [benzeşim](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) veya [kısıtlamalarından](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Küme Kaynak Yöneticisi'ni yönetir ve yük devretme kümesinde dengeleyen hakkında bilgi almak için makalesine kontrol [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
- En baştan başlatın ve [bir giriş için Service Fabric kümesi Resource Manager Al](service-fabric-cluster-resource-manager-introduction.md)
- Ölçümleri genellikle nasıl çalıştığı hakkında daha fazla bilgi için okumaya devam [Service Fabric yük ölçümleri](service-fabric-cluster-resource-manager-metrics.md)
- Küme Kaynak Yöneticisi'ni küme açıklamak için birçok seçeneğiniz vardır. Bunları hakkında daha fazla bilgi için bu makalede kontrol [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
