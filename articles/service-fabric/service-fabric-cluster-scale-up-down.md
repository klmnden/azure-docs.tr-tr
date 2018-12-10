---
title: İç veya dış bir Service Fabric kümesini ölçekleme | Microsoft Docs
description: Her düğüm türü/sanal makine ölçek kümesi için otomatik ölçeklendirme kurallarını ayarlayarak talebini eşleştirmek için Service Fabric kümesini içe veya dışa ölçeklendirme. Bir Service Fabric küme düğümleri ekleyebilir veya kaldırabilirsiniz
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: aljo
ms.openlocfilehash: 85a1e874ad80d0a3251c93c9c1199f56ab045527
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140583"
---
# <a name="read-before-you-scale"></a>Ölçeği önce okuyun
Uygulama iş yükünüz kasıtlı planlama gerektirir, neredeyse her zaman bir üretim ortamında tamamlamak için bir saatten daha uzun sürer ve iş yükü ve iş bağlamını anlamak ihtiyacınız kaynağına işlem kaynaklarını ölçeklendirme; Bu etkinlik önce hiçbir zaman yaptıysanız, aslında, okuma ve anlama başlattığınız önerilir [Service Fabric kümesi kapasite planlaması konuları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity), bu belgenin geri kalanında devam etmeden önce. Bu istenmeyen LiveSite sorunlarını önlemek için önerilir ve ayrıca bir üretim dışı ortamda karşı gerçekleştirmeye karar işlemleri başarıyla test önerilir. Herhangi bir zamanda yapabilecekleriniz [üretim sorunlarını bildirmek veya Azure için Ücretli destek isteği](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-production-issues-or-request-paid-support-for-azure). Mühendislerin yeterli bağlama sahip bu işlemleri gerçekleştirmek için ayrılan, bu makalede ölçeklendirme işlemleri açıklanmaktadır, ancak karar verin ve işlemleri, kullanım örneği için uygun olduğunu anlamak; hangi kaynakları ölçeklendirme (CPU, depolama, bellek) gibi hangi yönü (yatay veya dikey olarak) ölçeklendirmek için ve hangi işlemleri (kaynak şablonu dağıtımı, Portal, PowerShell/CLI) gerçekleştirin.

# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Bir Service Fabric kümesini otomatik ölçeklendirme kurallarını kullanarak içe veya dışa ölçeklendirme veya el ile
Sanal makine ölçek kümeleri, dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullanabileceğiniz bir Azure işlem kaynağıdır. Bir Service Fabric kümesinde tanımlanan her düğüm türü ayrı bir sanal makine ölçek kümesi ayarlanır. Her düğüm türü, ölçeklendirilebilir veya out bağımsız olarak, farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri yapılabilir. İçinde hakkında daha fazla bilgiyi [Service Fabric NodeType](service-fabric-cluster-nodetypes.md) belge. Kümenizde Service Fabric düğüm türleri arka uçtaki sanal makine ölçek kümelerinin yapılan olduğundan, her düğüm türü/sanal makine ölçek kümesi için otomatik ölçeklendirme kurallarını ayarlamanız gerekir.

> [!NOTE]
> Bu kümeyi oluşturan yeni Vm'leri eklemek için yeterli çekirdek aboneliğinizin olması gerekir. Model doğrulama yoktur şu anda, herhangi bir kota sınırları ulaşırsanız dağıtım zamanı hatası alabilmeniz. Ayrıca tek bir düğüm türü VMSS başına 100 düğüm yalnızca aşamaz. VMSS kullanıcının hedeflenen ölçeğe ulaşmak için ve otomatik ölçeklendirme başlatamaz eklemeniz gerekebilir automagically VMSS'ın ekleyin. Yerinde VMSS'ın dinamik bir kümeye ekleme, zorlu bir görevdir ve yaygın olarak kullanıcıların yeni küme oluşturma sırasında sağlanan uygun düğümü türleri ile sağlama sonuçlanır; [küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) uygun şekilde. 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Düğüm türü/sanal makine ölçek kümesi ölçeği seçin
Şu anda, portalı kullanarak bir Service Fabric kümesi oluşturma, bize bu nedenle düğüm türlerini listelemek ve bunlara otomatik ölçeklendirme kurallarını eklemek için Azure PowerShell (1.0 +) kullanmak için sanal makine ölçek kümeleri için otomatik ölçeklendirme kurallarını belirtmek mümkün değildir.

Kümeyi oluşturan sanal makine ölçek kümesi listesini almak için aşağıdaki cmdlet'leri çalıştırın:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Düğüm türü/sanal makine ölçek kümesi için otomatik ölçeklendirme kurallarını ayarlama
Birden çok düğüm türleri kümenizi varsa, bu her düğüm türü/sanal makine Ölçek (iç veya dış) ölçeklendirmek istediğiniz ayarlar yineleyin. Otomatik ölçeklendirmeyi ayarlamadan önce sahip olmanız gereken düğüm sayısını dikkate alın. Birincil düğüm türü için gereken düğüm sayısı alt sınırı seçmiş olduğunuz güvenilirlik düzeyi tarafından yönetilir. Daha fazla bilgi edinin [güvenilirlik düzeylerini](service-fabric-cluster-capacity.md).

> [!NOTE]
> Birincil düğüm ölçeklendirme değerinden daha düşük sayı yap kararsız küme yazın veya taşıyın. Bu, sistem hizmetleri ve uygulamalarınız için veri kaybına neden olabilir.
> 
> 

Şu anda otomatik ölçeklendirme özelliği için Service Fabric uygulamalarınızı raporlama yükleri tarafından yönlendirilen değil. Bu nedenle şu anda size otomatik ölçeklendirme tamamen tarafından yayılan her sanal makinenin performans sayaçlarını kullanan ölçek kümesi örnekleri.  

Bu yönergeleri izleyin [otomatik ölçeklendirme, her sanal makine ölçek kümesi için ayarlamak için](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> Düğüm türünde bir dayanıklılık düzeyi Silver veya Gold sürece senaryo aşağı bir ölçek kümesinde çağırmanız gerekir [Remove-ServiceFabricNodeState cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) uygun düğümü ada sahip. Bronz dayanıklılık sağlamak için aynı anda birden fazla düğümlerini ölçeklendirmek için önerilmez.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>El ile bir düğüm türü/sanal için makine ölçek kümesi VM ekleme
Örnek/yönergeleri izleyin [hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) her Nodetype VM'lerin sayısını değiştirmek için. 

> [!NOTE]
> Sanal makinelerin eklenmesi anında eklemeleri beklemiyoruz şekilde zaman alır. Bu nedenle VM kapasitesi çoğaltmalar için kullanılabilir olmadan önce tekrar 10 dakika için izin vermek için de, zaman içinde Kapasite eklemek / hizmet örneklerinin yerleştirilen planlayın.
> 
> 

## <a name="manually-remove-vms-from-the-primary-node-typevirtual-machine-scale-set"></a>El ile birincil düğüm türü/sanal makine ölçek kümesi sanal makineleri Kaldır
> [!NOTE]
> Birincil düğüm türü kümenizde service fabric sistem hizmetlerinin çalıştırın. Hiçbir zaman kapatmanız veya bu düğüm türü örneği sayısını ölçeğini için güvenilirlik katmanını değerinden gerektirir. Başvurmak [güvenilirlik katmanları ayrıntıları](service-fabric-cluster-capacity.md). 
> 
> 

Aşağıdaki adımlar bir sanal makine örneğinin aynı anda yürütmek gerekir. Bu sistem hizmetlerini (ve, durum bilgisi olan hizmetler) düzgün bir şekilde kaldırmakta olduğunuz sanal makine örneği ve diğer düğümler üzerinde oluşturulan yeni çoğaltmaları kapatılması için sağlar.

1. Çalıştırma [devre dışı bırak-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) düğümü devre dışı bırakmak amacıyla 'RemoveNode', (Bu düğüm türünde en yüksek örnek) kaldıracaksınız.
2. Çalıştırma [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) düğümü devre dışı olarak gerçekten çözümlemeye geçmiş emin olmak için. Aksi durumda, düğümü devre dışı kadar bekleyin. Bu adım hurry olamaz.
3. Örnek/yönergeleri izleyin [hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) VM tek düğüm türü sayısı değiştirmek için. Kaldırılan en yüksek VM örneği örneğidir. 
4. 1 ile gerektiği gibi 3. adımları yineleyin, ancak hiçbir zaman ölçeğini birincil düğüm türlerinde ne gerektirdiğini güvenilirlik katmanını daha az örnek sayısı. Başvurmak [güvenilirlik katmanları ayrıntıları](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevirtual-machine-scale-set"></a>El ile birincil olmayan düğüm türü/sanal makine ölçek kümesi sanal makineleri Kaldır
> [!NOTE]
> Bir durum bilgisi olan hizmet için belirli bir sayıda olması her zaman en çok kullanılabilirliği sürdürmek ve hizmetinizi durumunu korumak için düğümleri gerekir. Çok az düğüm sayısını bölüm/hizmet hedef çoğaltma kümesi sayısı eşittir ihtiyacınız vardır. 
> 
> 

Aşağıdaki adımlar bir sanal makine örneğini çalıştırma aynı anda ihtiyacınız vardır. Bu sistem hizmetlerini (ve, durum bilgisi olan hizmetler) kaldırmakta olduğunuz VM örneğine göre düzgün bir şekilde kapatılmasını sağlar ve başka bir konumu yeni çoğaltmaları oluşturdunuz.

1. Çalıştırma [devre dışı bırak-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) düğümü devre dışı bırakmak amacıyla 'RemoveNode', (Bu düğüm türünde en yüksek örnek) kaldıracaksınız.
2. Çalıştırma [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) düğümü devre dışı olarak gerçekten çözümlemeye geçmiş emin olmak için. Aksi durumda, düğümü devre dışı kadar bekleyin. Bu adım hurry olamaz.
3. Örnek/yönergeleri izleyin [hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) VM tek düğüm türü sayısı değiştirmek için. Bu, en yüksek VM örneği artık kaldırır. 
4. 1 ile gerektiği gibi 3. adımları yineleyin, ancak hiçbir zaman ölçeğini birincil düğüm türlerinde ne gerektirdiğini güvenilirlik katmanını daha az örnek sayısı. Başvurmak [güvenilirlik katmanları ayrıntıları](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Service Fabric Explorer'ın davranışlarla karşılaşabilirsiniz
Bir kümenin ölçeğini artırdığınızda, Service Fabric Explorer (sanal makine ölçek kümesi örnekleri) ve kümenin parçası olan düğüm sayısını yansıtır.  Ancak, ölçeğini daralttığınızda, kümeyi kötü durumda, çağırmadığınız sürece görüntülenen kaldırılan düğüm/sanal makine örneği görürsünüz [Remove-ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) uygun düğümü ada sahip.   

Bu davranış için açıklaması aşağıda verilmiştir.

Service Fabric Explorer'da listelenen bir yansıma Service Fabric sistem hizmetlerinin düğümlerdir (FM özellikle) kümesi vardı/sahip düğüm sayısını hakkında bilir. Sanal makine ölçek kümesini ölçeklendirme, sanal makine silinmiş, ancak FM sistem hizmeti hala gördüğü (silindikten sonra VM eşlendi) düğüme geri dönün. Bu nedenle Service Fabric Explorer (sistem durumu hata veya bilinmeyen olabilir, ancak) düğüm görüntülemeye devam eder.

Bir düğüm, bir VM kaldırıldığında kaldırıldığını emin olmak için iki seçeneğiniz vardır:

1. Altyapı tümleştirmesinde size, kümedeki düğüm türleri için Silver veya Gold bir dayanıklılık düzeyi seçin. Ölçeği aşağı olduğunda, ardından otomatik olarak düğümleri bizim Sistem Hizmetleri (FM) durumundan kaldırır.
Başvurmak [dayanıklılık düzeyleri ayrıntıları](service-fabric-cluster-capacity.md)

2. Sanal makine örneği ölçeği bir kez çağırmanız gerekir [Remove-ServiceFabricNodeState cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate).

> [!NOTE]
> Service Fabric kümeleri yedekleme kullanılabilirliği sürdürmek ve durumu "çekirdek koruma olarak." başvurulan - korumak için her zaman olması için düğümleri belirli sayıda gerektirir Bu nedenle, kümedeki tüm makinelerin ilk yapmadığınız sürece kapatmak için genellikle güvenli olmayan bir [durumunuzu tam yedekleme](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Küme kapasitesini planlama, Küme yükseltme ve hizmetlerini bölümleme hakkında da bilgi için aşağıdakileri okuyun:

* [Küme kapasitenizi planlama](service-fabric-cluster-capacity.md)
* [Küme yükseltme](service-fabric-cluster-upgrade.md)
* [Bölüm durum bilgisi olan hizmetler için en yüksek ölçek](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
