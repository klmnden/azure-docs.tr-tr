---
title: Azure Service Fabric tek başına küme ölçeklendirme | Microsoft Docs
description: Service Fabric tek başına kümeler veya çıkış ve yukarı veya aşağı ölçeklendirme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/13/2018
ms.author: dekapur
ms.openlocfilehash: 05049b9b08b4630c4299a6d3054c7815b082af52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516045"
---
# <a name="scaling-service-fabric-standalone-clusters"></a>Service Fabric tek başına kümeler ölçeklendirme
Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir düğüm denir. Kümeler, potansiyel olarak binlerce düğümde içerebilir. Service Fabric kümesi oluşturduktan sonra küme yatay yönde ölçeklendirebilirsiniz (düğüm sayısını değiştirme) ya da dikey yönde (düğümlerin kaynakları değiştirin).  Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz.  Küme ölçekler gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

Neden bir kümenin ölçeğini? Uygulama talepleri zamanla değişir.  Daha yüksek uygulama iş yükü veya ağ trafiği karşılamak ya da küme kaynaklarını talep düştüğünde azaltmak için küme kaynaklarını artırmam gerekiyor.

## <a name="scaling-in-and-out-or-horizontal-scaling"></a>Giriş ve çıkış ölçeklendirme ve yatay ölçeklendirme
Kümedeki düğüm sayısını değiştirir.  Yeni düğüm, kümeye katılmak sonra [Küme Kaynak Yöneticisi](service-fabric-cluster-resource-manager-introduction.md) Hizmetleri için var olan düğümleri üzerindeki yükü azaltan taşır.  Küme kaynaklarını verimli bir şekilde kullanılmayan, düğüm sayısını da azaltabilirsiniz.  Küme düğümleri bırakın gibi hizmetleri devre dışı düğümleri taşıyın ve yük arttıkça Kalan düğümlerde.  Sanal makine sayısı için kullanmak ve iş yükü değil Bu vm'lerdeki ödeme olduğundan Azure'da çalışan bir kümedeki düğüm sayısını azaltma, para tasarrufu yapabileceğiniz.  

- Avantajları: Teoride, Ölçek sonsuz.  Uygulamanız için ölçeklenebilirlik tasarlanmışsa, daha fazla düğüm ekleyerek sınırsız büyüme etkinleştirebilirsiniz.  Bulut ortamlarında araçları ekleme veya düğümleri, kapasiteyi ayarlamak daha kolaydır ve yalnızca kullandığınız kaynaklar için ödeme yaparsınız kaldırma daha kolay hale getirir.  
- Olumsuz: Uygulamaları olmalıdır [ölçeklendirilebilirlik için tasarlanmış](service-fabric-concepts-scalability.md).  Uygulama veritabanları ve kalıcı olarak iyi ölçeklendirme yapmasını ek mimari iş gerektirebilir.  [Güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md) Service Fabric durum bilgisi olan hizmetler, ancak çok uygulama verilerinizi ölçeklendirme kolaylaştırır.

Tek başına kümeler, Service Fabric kümenizi şirket içinde dağıtmanıza izin ver veya tercih ettiğiniz bulut sağlayıcısı.  Düğüm türleri, dağıtımınıza bağlı olarak sanal makineleri veya fiziksel makineler oluşur. Azure'da çalışan kümelerle karşılaştırıldığında, tek başına küme ölçeklendirme işleminin biraz daha karmaşıktır.  Kümedeki düğüm sayısını el ile değiştirin ve ardından bir küme yapılandırma yükseltmesi çalıştırın gerekir.

Birden fazla yükseltme düğümleri kaldırma işlemi başlatabilir. Bazı düğümler ile işaretlenmiş `IsSeedNode=”true”` etiketi ve küme sorgulayarak tanımlanabilir kullanarak bildirim [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest). Böyle senaryolarda taşınmasını çekirdek düğümleri olduğundan bu düğümleri kaldırılmasını diğerlerinden daha uzun sürebilir. Küme en az üç birincil düğüm türü düğümünden sürdürmeniz gerekir.

> [!WARNING]
> Aşağıdaki düğüm sayısı alt değil öneririz [güvenilirlik katmanını küme boyutunu](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) küme için. Bu, küme arasında çoğaltılması kararlılığını ve büyük olasılıkla kümeyi yok etmek için Service Fabric sistem hizmetlerinin olanağıyla çalışmasını engeller.
>

Tek başına küme ölçeklendirme, aşağıdaki yönergeleri göz önünde bulundurun:
- Birincil düğüm değiştirme ardına kaldırarak ve ardından toplu olarak ekleme yerine gerçekleştirilen bir düğüm olmalıdır.
- Düğüm türü kaldırmadan önce tüm düğümleri düğüm türüne başvuru olup olmadığını denetleyin. Bu düğümler, ilgili düğüm türü kaldırmadan önce kaldırın. Tüm ilgili düğümleri kaldırdıktan sonra küme yapılandırmasından NodeType kaldırın ve bir yapılandırmaya başlamadan kullanarak yükseltme [başlangıç ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade).

Daha fazla bilgi için [bir tek başına kümenin ölçeğini](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="scaling-up-and-down-or-vertical-scaling"></a>Ölçeği artırmayı veya dikey ölçeklendirme 
Kümedeki düğümler kaynakları (CPU, bellek veya depolama) değiştirir.
- Avantajları: Yazılım ve uygulama mimarisi aynı kalır.
- Olumsuz: Kaynakları tek tek düğümlere ne kadar artırmak için bir sınır olduğundan sınırlı ölçek. Kapalı kalma süresi, fiziksel veya sanal kaynak ekleme veya kaldırma için makineleri çevrimdışına almak ihtiyacınız olacağı için.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
* [Azure kümesine veya dışa ölçeklendirme](service-fabric-tutorial-scale-cluster.md).
* [Azure bir kümeyi programlama yoluyla ölçeklendirme](service-fabric-cluster-programmatic-scaling.md) fluent Azure kullanarak işlem SDK.
* [Tek başına küme içe veya dışa ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md).

