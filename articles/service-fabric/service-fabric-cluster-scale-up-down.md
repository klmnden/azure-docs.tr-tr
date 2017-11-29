---
title: "Service Fabric kümesi veya ölçeklendirin | Microsoft Docs"
description: "Service Fabric kümesi içeri veya dışarı isteğe bağlı her düğüm türü/sanal makine ölçek kümesi için otomatik ölçeklendirme kurallarını ayarlayarak eşleşecek şekilde ölçeklendirilir. Service Fabric kümesi düğümlerine Ekle Kaldır"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 249fb4903c7b2de3ce290850a7759a4793f10aa7
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Service Fabric kümesi veya otomatik ölçeklendirme kurallarını kullanarak uzaklaştırma ölçeklendirme
Sanal makine ölçek kümeleri dağıtmak ve sanal makinelerin bir koleksiyon kümesi olarak yönetmek için kullanabileceğiniz bir Azure işlem kaynaktır. Service Fabric kümesi içinde tanımlanan her düğüm türü ayrı bir sanal makine ölçek kümesi ayarlanır. Her düğüm türü, genişletilebilir veya çıkışı bağımsız olarak, farklı bağlantı noktalarının açık olması ve farklı kapasite ölçümlerini olabilir. İçinde hakkında daha fazla bilgiyi [Service Fabric nodetypes](service-fabric-cluster-nodetypes.md) belge. Service Fabric düğüm türleri kümenizdeki arka uç, sanal makine ölçek kümelerinin yapıldıktan sonra otomatik ölçek kuralı her düğüm türü/sanal makine ölçek kümesi için ayarlamanız gerekir.

> [!NOTE]
> Aboneliğiniz bu küme olun yeni VM'ler eklemek için yeterli çekirdek olması gerekir. Olmadığından hiçbir model doğrulama şu anda, kota sınırları isabet durumunda bir dağıtım zaman hatası alır.
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Düğüm türü/sanal makine ölçek ölçeklendirme için ayarlanmış seçin
Şu anda, Portalı'nı kullanarak sanal makine ölçek kümeleri için otomatik ölçeklendirme kurallarını belirtin, böylece bize düğüm türleri listelemek ve bunlara otomatik ölçek kuralı eklemek için Azure PowerShell (1.0 +) kullanın mümkün değildir.

Kümenizi olun sanal makine ölçek kümesini listesini almak için aşağıdaki cmdlet'leri çalıştırın:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Düğüm türü/sanal makine ölçek kümesi için otomatik ölçeklendirme kurallarını ayarlama
Birden çok düğüm türleri kümeniz varsa, bu her düğüm türleri/sanal makine Ölçek (içeri veya dışarı) ölçeklendirmek istediğiniz ayarlar yineleyin. Otomatik ölçeklendirmeyi ayarlamadan önce bilmeniz gereken düğüm sayısını dikkate alın. Birincil düğüm türü için gereken düğüm sayısı alt sınırı seçmiş olduğunuz güvenilirlik düzeyi tarafından yönetilir. Daha fazla bilgi edinin [güvenilirlik düzeyleri](service-fabric-cluster-capacity.md).

> [!NOTE]
> Birincil düğüm Ölçeklendirmesi en küçük sayı yap değerinden kararsız küme yazın veya getir. Bu, uygulamalarınız için ve sistem hizmetleri için veri kaybına neden olabilir.
> 
> 

Şu anda otomatik ölçek özelliği uygulamalarınız için Service Fabric raporlama yükü tarafından yönlendirilir değil. Bu nedenle elde otomatik ölçek tamamen her sanal makine tarafından gösterilen performans sayaçlarını tarafından yönlendirilen şu anda örnekleri ölçek kümesi.  

Bu yönergeleri izleyin [otomatik ölçek her sanal makine ölçek kümesi için ayarlamak üzere](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> Düğüm türünüz altın veya gümüş dayanıklılık düzeyini olmadıkça senaryo aşağı bir ölçek aramanız gereken [Kaldır ServiceFabricNodeState cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) uygun düğüm adı ile.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>Sanal makineleri bir düğüm türü/sanal için makine ölçek kümesini el ile Ekle
Örnek/yönergeleri izleyin [hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) her Nodetype VM'lerin sayısını değiştirmek için. 

> [!NOTE]
> Vm'leri ekleme anlık olarak eklemeleri beklemediğiniz için zaman alır. Bu nedenle VM kapasitesi çoğaltmalar için kullanılabilir olmadan önce üzerinde 10 dakika için izin vermek için de, zaman içinde Kapasite eklemek / yerleştirilen örnekleri service planlayın.
> 
> 

## <a name="manually-remove-vms-from-the-primary-node-typevirtual-machine-scale-set"></a>Sanal makineleri birincil düğüm türü/sanal makine ölçek kümesini el ile kaldırma
> [!NOTE]
> Birincil düğüm türü kümenizdeki service fabric sistem hizmetlerini çalışır. Hiçbir zaman kapatıldı veya bu düğüm türleri sayısının ölçeğini şekilde güvenilirlik katmanını değerinden garanti eder. Başvurmak [burada güvenilirlik katmanları ayrıntıları](service-fabric-cluster-capacity.md). 
> 
> 

Aşağıdaki adımlar, bir VM örneği aynı anda yürütülmesi gerekir. Bu düzgün biçimde kaldırdığınız VM örneği ve diğer düğümlerde oluşturulan yeni yinelemeler kapatılması Sistem Hizmetleri (ve durum bilgisi olan hizmetlerinizi) sağlar.

1. Çalıştırma [devre dışı bırakma ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) düğümü devre dışı bırakmak amacıyla 'RemoveNode' kaldırın (Bu düğüm türü en yüksek örneğinde) oluşturacağız.
2. Çalıştırma [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) düğümü devre dışı olarak gerçekten çözümlemeye geçmiş emin olmak için. Aksi durumda, düğümü devre dışı kadar bekleyin. Bu adım hurry olamaz.
3. Örnek/yönergeleri izleyin [hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) tek bu Nodetype VM'lerin sayısını değiştirmek için. Kaldırılan yüksek VM örneği örneğidir. 
4. 1 ile gerektiğinde 3 arasındaki adımları yineleyin, ancak hiçbir zaman birincil düğüm türleri ne güvenilirlik katmanını sağlayacağını değerinden sayısının ölçeğini. Başvurmak [burada güvenilirlik katmanları ayrıntıları](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevirtual-machine-scale-set"></a>Sanal makineleri birincil olmayan düğüm türü/sanal makine ölçek kümesini el ile kaldırma
> [!NOTE]
> Durum bilgisi olan bir hizmet için belirli bir sayıda olması her zaman en çok kullanılabilirlik korumak ve hizmetinizin durumunu korumak için düğümleri gerekir. Çok düşük, düğüm sayısını bölüm/hizmet hedef çoğaltma kümesi sayısı eşit gerekir. 
> 
> 

Aşağıdaki adımlar, bir VM örneği çalıştırma aynı anda gerekir. Bu kaldırdığınız VM örneğinde düzgün biçimde kapatılması Sistem Hizmetleri (ve durum bilgisi olan hizmetlerinizi) sağlar ve başka bir konumu yeni çoğaltmaları oluşturulur.

1. Çalıştırma [devre dışı bırakma ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) düğümü devre dışı bırakmak amacıyla 'RemoveNode' kaldırın (Bu düğüm türü en yüksek örneğinde) oluşturacağız.
2. Çalıştırma [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) düğümü devre dışı olarak gerçekten çözümlemeye geçmiş emin olmak için. Aksi durumda düğüm devre dışı kadar bekleyin. Bu adım hurry olamaz.
3. Örnek/yönergeleri izleyin [hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) tek bu Nodetype VM'lerin sayısını değiştirmek için. Bu artık yüksek VM örneği kaldırır. 
4. 1 ile gerektiğinde 3 arasındaki adımları yineleyin, ancak hiçbir zaman birincil düğüm türleri ne güvenilirlik katmanını sağlayacağını değerinden sayısının ölçeğini. Başvurmak [burada güvenilirlik katmanları ayrıntıları](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Service Fabric Explorer'da davranışlarla karşılaşabilirsiniz
Bir küme ölçeklendirdiğinizde Service Fabric Explorer kümesinin parçası olan (sanal makine ölçek kümesi örneklerinin) düğüm sayısını yansıtır.  Ölçeklendirmek ancak, bir küme, aşağı, gerektirmediği sürece kötü durumda görüntülenen kaldırılan düğüm/VM örneği görürsünüz [Kaldır ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) uygun düğümü ada sahip.   

Aşağıda, bu davranışı için bir açıklama verilmiştir.

Service Fabric Explorer'da listelenen düğümleri Service Fabric Sistem Hizmetleri yansımasını olan (FM özellikle) küme vardı/sahip düğüm sayısı hakkında bilir. Sanal makine ölçek kümesi ölçeklendirdiğinizde VM silindi ancak FM sistem hizmeti hala düşündüğü (silindi VM eşlendi) düğüme geri dönün. Bu nedenle Service Fabric Explorer (sistem durumu hatası veya bilinmeyen olabilir) bu düğüme görüntülemeye devam eder.

Bir VM kaldırıldığında bir düğüm kaldırıldığından emin olmak için iki seçeneğiniz vardır:

1) Altın veya gümüş dayanıklılık düzeyi düğüm türleri için altyapı tümleştirme sağlar, kümede seçin. Ölçeklendirdiğinizde, ardından otomatik olarak düğümleri bizim Sistem Hizmetleri (FM) durumundan kaldırır.
Başvurmak [burada dayanıklılık düzeyleri ayrıntıları](service-fabric-cluster-capacity.md)

2) VM örneği ölçeklendirilmiş sonra çağırması gerekir [Kaldır ServiceFabricNodeState cmdlet](https://msdn.microsoft.com/library/mt125993.aspx).

> [!NOTE]
> Service Fabric kümeleri belirli sayıda düğümleri yukarı kullanılabilirliği sürdürmek ve "çekirdek koruma olarak." başvurulan durumunu - korumak için her zaman olması gerekir Bu nedenle, ilk yapmadığınız sürece kümedeki tüm makineleri kapatmaya genellikle güvenli olmayan bir [tam yedekleme, durumunun](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Ayrıca küme kapasite planlaması, Küme yükseltme ve Hizmetleri bölümleme hakkında bilgi edinmek için şunu okuyun:

* [Küme kapasitenizi planlama](service-fabric-cluster-capacity.md)
* [Küme yükseltme](service-fabric-cluster-upgrade.md)
* [En fazla ölçek için bölüm durum bilgisi olan hizmetler](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
