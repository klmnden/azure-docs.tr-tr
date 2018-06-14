---
title: VM boyutları için Azure Batch havuzları seçin | Microsoft Docs
description: İşlem düğümlerinin Azure Batch havuzlarında kullanılabilir VM boyutları arasından seçim yapma
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: danlep
ms.openlocfilehash: addd1e9314a754b40cc5d49c0299f007580f512f
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
ms.locfileid: "29765612"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Bir Azure Batch havuzunda işlem düğümlerini için bir VM boyutunu seçin

Bir Azure Batch havuzu için bir düğüm boyutu seçerken, neredeyse tüm VM boyutları mevcut arasından seçim yapabilirsiniz. Azure boyutları Linux ve Windows VM'ler için farklı iş yükleri için sunmaktadır. 

Bazı özel durumlar ve bir VM boyutu seçmek için sınırlamalar vardır:
* Bazı VM aileleri veya VM boyutları toplu işlemde desteklenmez. 
* Bazı VM boyutları sınırlıdır ve bunlar ayrılabilen önce özellikle etkinleştirilmesi gerekir.


## <a name="supported-vm-families-and-sizes"></a>Desteklenen VM aileleri ve boyutları

### <a name="pools-in-virtual-machine-configuration"></a>Sanal Makine Yapılandırması havuzları

Batch havuzlarının sanal makine yapılandırma desteği tüm VM boyutları ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)) *dışında* aşağıdaki için:

| Aile  | Desteklenmeyen boyutları  |
|---------|---------|
| Temel A-series | Basic_A0 (A0) |
| A Serisi | Standard_A0 |
| B serisi | Tümü |
| Fsv2-serisi<sup>*</sup> | Tümü |

<sup>*</sup>Bu serideki sonraki destek için yol haritası üzerinde boyutlarıdır.

### <a name="pools-in-cloud-service-configuration"></a>Bulut hizmeti yapılandırması havuzları

Bulut hizmet yapılandırmasının batch havuzları destekleyen tüm [VM boyutları bulut Hizmetleri için](../cloud-services/cloud-services-sizes-specs.md) *dışında* aşağıdaki için:

| Aile  | Desteklenmeyen boyutları  |
|---------|---------|
| A Serisi | ExtraSmall |
| Av2 Serisi | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="restricted-vm-families"></a>Kısıtlı VM aileleri
Aşağıdaki VM aileleri Batch havuzlarında ayrılabilen, ancak belirli kota artışı isteği gerekir (bkz [bu makalede](batch-quota-limit.md#increase-a-quota)):
* NCv2 serisi
* NCv3 serisi
* ND serisi

Bu boyutlar, yalnızca sanal makinenin yapılandırmasında havuzlarında kullanılabilir.

## <a name="size-considerations"></a>Boyutu konuları

* **Uygulama gereksinimleri** -düğümlerinde çalışması uygulamanın gereksinimlerini ve özelliklerini göz önünde bulundurun. Uygulamanın çok iş parçacıklı olup olmadığı ve ne kadar bellek kullandığı gibi konular en uygun ve ekonomik düğüm boyutunu belirlemeye yardımcı olabilir. Çok örnekli için [MPI iş yükleri](batch-mpi.md) veya CUDA uygulamalar, özelleştirilmiş göz önünde bulundurun [HPC](../virtual-machines/linux/sizes-hpc.md) veya [GPU etkin](../virtual-machines/linux/sizes-gpu.md) sırasıyla VM boyutları. (Bkz [kullanım RDMA özellikli GPU etkinleştirilmiş veya örneklerinde Batch havuzlarında](batch-pool-compute-intensive-sizes.md).) 

* **Düğüm başına görevleri** -bir görev aynı anda bir düğümde çalışan varsayarak bir düğüm boyutu seçmek için genel bir durumdur. Ancak, birden çok görev (ve dolayısıyla Uygulama örneğinin) sağlamak için yararlı olabilir [paralel olarak çalışan](batch-parallel-node-tasks.md) iş yürütme sırasında işlem düğümleri üzerinde. Bu durumda, paralel görev yürütme artan talebi karşılamak için çok çekirdekli düğüm boyutu seçmek için yaygındır.

* **Yük düzeyleri farklı görevler için** -tüm bir havuzdaki düğümler aynı boyuttadır. Farklı sistem gereksinimlerine ve/veya yük düzeylerine sahip uygulamalar çalıştırmayı planlıyorsanız ayrı havuzlar oluşturmanız önerilir. 

* **Bölge kullanılabilirliği** -A VM ailesi veya boyutu olabilir kullanılabilir, Batch hesabınızı oluşturduğunuz bölgelerde. Bir boyut kullanılabilir olup olmadığını denetlemek için bkz: [bölgeye göre ürünleri](https://azure.microsoft.com/regions/services/).

* **Kotalar** - [çekirdek kotaları](batch-quota-limit.md#resource-quotas) , Batch hesabı bir Batch havuzu ekleyebileceğiniz verilen boyuta düğümlerinin sayısını sınırlayabilirsiniz. Kota artışı isteği için bkz: [bu makalede](batch-quota-limit.md#increase-a-quota). 

* **Havuzu yapılandırması** -genel olarak, bulut hizmeti yapılandırma ile karşılaştırıldığında sanal makine yapılandırması, bir havuz oluşturduğunuzda, daha fazla VM boyutu seçeneğiniz vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Toplu ayrıntılı bir bakış için bkz: [geliştirme büyük ölçekli paralel işlem çözümleri yığın](batch-api-basics.md).
* İşlem yoğunluklu VM boyutları kullanma hakkında daha fazla bilgi için bkz: [kullanım RDMA özellikli GPU etkinleştirilmiş veya örneklerinde Batch havuzlarında](batch-pool-compute-intensive-sizes.md). 


