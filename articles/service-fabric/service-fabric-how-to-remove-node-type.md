---
title: Uzak bir düğüm için Azure Service Fabric'te nasıl yazın | Microsoft Docs
description: Azure Service Fabric'te bir düğüm türü kaldırmayı öğrenin
services: service-fabric
documentationcenter: .net
author: v-steg
manager: JeanPaul.Connick
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/26/2018
ms.author: v-steg
ms.openlocfilehash: 3704a356763b16a30285baee1aabffdd3aa3f8aa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53981086"
---
# <a name="remove-a-service-fabric-node-type"></a>Bir Service Fabric düğüm türü Kaldır

Kullanım [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) bir Service Fabric düğüm türü kaldırmak için.

Remove-AzureRmServiceFabricNodeType çağrıldığında oluşan iki işlemleri şunlardır:
1.  Sanal makine ölçek kümesi (VMSS) düğüm türü arkasında silinir.
2.  Tüm düğümler için düğüm türü içinde bu düğüm için tüm durumu sistemden kaldırılır. Bu düğümde Hizmetleri varsa, ardından Hizmetleri önce başka bir düğüme taşınır. Küme Yöneticisi çoğaltma/hizmet için bir düğüm bulunamıyor, işlemi Gecikmeli/engellenmiş olur.

> [!NOTE]
> Düğüm türü, bir üretim kümesinden kaldırmak için remove-AzureRmServiceFabricNodeType kullanarak sık kullanılan olarak kullanılması önerilmez. Temel bir sanal makine ölçek kümesi kaynak düğüm türü arkasında siler bu durumda, çok tehlikeli komut aynıdır. Remove-AzureRmServiceFabricNodeType çağırdığınızda, Sistem güvenli olan kaldırma hakkında önemli olmadığını bilmek bir yolu yoktur. 

## <a name="durability-characteristics"></a>Dayanıklılık özellikleri
Güvenlik hızından Remove-AzureRmServiceFabricNodeType kullanırken kurtarılmasına öncelik verilir. Silver veya Gold [dayanıklılık özelliklerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) önerilir, çünkü:
- Bronz durumu bilgilerini kaydetme hakkında herhangi bir garanti vermez.
- Silver ve Gold dayanıklılık VMSS değişiklikleri yakalar.
- Altın Ayrıca, size Azure üzerinde denetim VMSS güncelleştirir.

Service Fabric "temel alınan değişiklikleri düzenler" ve böylece veriler kaybolmaz güncelleştirir. Ancak, Bronz dayanıklılığa sahip bir düğümü kaldırdığınızda, durum bilgilerini kaybedebilir. Birincil düğüm türü kaldırırsınız ve uygulamanızın durum bilgisiz olduğundan, Bronz kabul edilebilir. Durum bilgisi olan iş yükleri üretimde çalıştırdığınız zaman, en düşük yapılandırmayı Silver olmalıdır. Benzer şekilde, üretim senaryoları için birincil düğüm türü her zaman Silver veya Gold olmalıdır.

### <a name="more-about-bronze-durability"></a>Bronz dayanıklılık hakkında daha fazla bilgi

Bronz olan bir düğüm türü kaldırma, düğüm türü tüm düğümleri hemen aşağı git. Service Fabric Bronz düğümleri VMSS güncelleştirmelerden tuzak değil, bu nedenle tüm sanal makineleri hemen aşağı git. Durum bilgisi olan herhangi bir şey bu düğümlerde olsaydı, veriler kaybolur. Durum bilgisi olmayan olsa bile, tüm Service Fabric düğümleri halka, şimdi katılmak bir tüm Komşuları kaybolmuş olabilir. Bu nedenle, etkileyebilir kümesi.

Tek bir düğüm kaldırma aksine teorik olarak, bir kerede tek bir düğüm kaldırabilirsiniz çünkü hizmetleri üzerine getirin, sistemin sabitlenmesini, başka bir düğümü çıkarmadan önce bekleyin ve yinelemeler için bekleyin ve benzeri.  Aynı anda aynı anda birkaç düğüm kaldırırsanız, (Service Fabric VMSS güncelleştirmelerden Bronz dayanıklılığa sahip tuzak değil olduğundan) ancak kümeniz hale gelebilir.

## <a name="recommended-node-type-removal-process"></a>Düğüm türü kaldırma işlemi önerilir

Düğüm türü en güvenli ve hızlı bir şekilde kaldırmak için:
1.  Bronz dayanıklılık kullanıyorsanız veya parçası olarak düğüm türü silme, düğüm türü kaldırma işlemi etkilenen düğümleri ilk boş durum bilgisi olan verileri kaybolacak durum bilgilerini içeren uygulamalar geçmek için sistemi istemediğiniz değilse.
2.  Çalıştırma [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) her kaldırılan istediğiniz düğümleri.
3.  Çalıştırma [Remove-AzureRmVmss](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#remove-vms-from-a-scale-set) düğüm türü kaldırma işlemi etkilenecek VM'ler için.
4. Çalıştırma [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) düğüm türü kaldırmak için.

## <a name="next-steps"></a>Sonraki adımlar
- Küme hakkında daha fazla bilgi [dayanıklılık özellikleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).
- Daha fazla bilgi edinin [düğüm türleri ve sanal makine ölçek kümeleri](service-fabric-cluster-nodetypes.md).
- Daha fazla bilgi edinin [Service Fabric kümesini ölçekleme](service-fabric-cluster-scaling.md).