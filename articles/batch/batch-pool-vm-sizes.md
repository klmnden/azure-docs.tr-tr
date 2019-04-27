---
title: Havuzlar - Azure Batch için VM boyutları seçin | Microsoft Docs
description: Azure Batch havuzlarında işlem düğümleri için kullanılabilen VM boyutları arasından seçim yapma
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 43094839c9da9b00c97d1dffd53f98a3acd119d5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60775738"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Bir Azure Batch havuzunda işlem düğümleri için VM boyutu seçme

Azure Batch havuzu için bir düğüm boyutu seçtiğinizde, neredeyse tüm VM boyutları Azure'da kullanıma sunulan arasından seçim yapabilirsiniz. Azure farklı iş yükleri için Linux ve Windows Vm'leri için bir dizi boyutları sunar.

Bazı özel durumlar ve bir VM boyutu seçme sınırlamalar vardır:

* Bazı VM aileleri veya VM boyutları, Batch hizmetinde desteklenmez. 
* Bazı VM boyutları kısıtlı ve bunlar ayrılabilen önce özellikle etkinleştirilmesi gerekir.

## <a name="supported-vm-families-and-sizes"></a>Desteklenen VM aileleri ve boyutları

### <a name="pools-in-virtual-machine-configuration"></a>Sanal makine yapılandırmasındaki havuzlarda

Sanal makine yapılandırmasında batch havuzları destekleyen tüm VM boyutları ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)) *dışında* aşağıdakiler:

| Aile  | Desteklenmeyen boyutları  |
|---------|---------|
| Temel A serisi | Basic_A0 (A0) |
| A Serisi | Standard_A0 |
| B serisi | Tümü |
| DC serisi | Tümü |
| Aşırı bellek için iyileştirilmiş | Tümü |
| HB serisi<sup>1,2</sup> | Tümü |
| HC serisi<sup>1,2</sup> | Tümü |
| Lsv2 serisi | Tümü |
| NDv2 serisi<sup>1,2</sup> | Tümü |
| NVv2 serisi<sup>1</sup> | Tümü |
| SAP HANA | Tümü |


<sup>1</sup> planlı desteği.  
<sup>2</sup> tarafından kullanılan kullanıcı aboneliği modunda Batch hesapları; kullanıcı aboneliği modunda Batch hesabı ayarlama çekirdek kotası olması gerekir. Bkz: [kullanıcı aboneliği modu için yapılandırma](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode) daha fazla bilgi için.

Aşağıdaki VM boyutları, yalnızca düşük öncelikli düğümleri için desteklenir:

| Aile  | Desteklenen boyutlar  |
|---------|---------|
| M serisi | Standard_M64ms |
| M serisi | İşler için standart_m128s |

M serisi ailesindeki diğer VM boyutları şu anda desteklenmiyor.

### <a name="pools-in-cloud-service-configuration"></a>Bulut hizmeti yapılandırması havuzları

Bulut hizmeti yapılandırmasında batch havuzları destekleyen tüm [bulut Hizmetleri için VM boyutları](../cloud-services/cloud-services-sizes-specs.md) *dışında* aşağıdakiler:

| Aile  | Desteklenmeyen boyutları  |
|---------|---------|
| A Serisi | ExtraSmall |
| Av2 Serisi | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="restricted-vm-families"></a>Kısıtlı VM aileleri

Batch havuzlarını aşağıdaki VM aileleri ayrılabilir, ancak belirli bir kota artışı isteğinde gerekir (bkz [bu makalede](batch-quota-limit.md#increase-a-quota)):

* NCv2 serisi
* NCv3 serisi
* ND serisi

Bu boyutları, sanal makine yapılandırmasındaki havuzlarda yalnızca kullanılabilir.

## <a name="size-considerations"></a>Boyutu konuları

* **Uygulama gereksinimleri** -özelliklerini ve düğümler üzerinde çalıştırmanız uygulama gereksinimlerini göz önünde bulundurun. Uygulamanın çok iş parçacıklı olup olmadığı ve ne kadar bellek kullandığı gibi konular en uygun ve ekonomik düğüm boyutunu belirlemeye yardımcı olabilir. Çok örnekli için [MPI iş yükleri](batch-mpi.md) veya özelleştirilmiş göz önünde bulundurun CUDA uygulamaları [HPC](../virtual-machines/linux/sizes-hpc.md) veya [GPU özellikli](../virtual-machines/linux/sizes-gpu.md) VM boyutları, sırasıyla. (Bkz [kullanım RDMA özellikli veya GPU özellikli örnekler Batch havuzlarında](batch-pool-compute-intensive-sizes.md).)

* **Düğüm başına görevleri** -bir düğüm boyutu bir görevi bir düğümde aynı anda çalışır varsayarak normaldir. Ancak, birden fazla görevin (ve dolayısıyla Uygulama örneğinin) için avantajlı olabilir [paralel şekilde çalışan](batch-parallel-node-tasks.md) iş yürütme sırasında işlem düğümleri üzerinde. Bu durumda, paralel görev yürütmeye yönelik artan talebi karşılamak için bir çok çekirdekli düğüm boyutu yaygındır.

* **Yük düzeyleri farklı görevler için** -tüm bir havuzdaki düğümler aynı boyuttadır. Farklı sistem gereksinimlerine ve/veya yük düzeylerine sahip uygulamalar çalıştırmayı planlıyorsanız ayrı havuzlar oluşturmanız önerilir. 

* **Bölge kullanılabilirliği** -bir VM ailesi veya boyutu gerçekleştirilemeyebilir bölgelerde Batch hesabınızı oluşturduğunuz yerdir. Bir boyut kullanılabilir olduğunu denetlemek için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

* **Kotalar** - [çekirdek kotaları](batch-quota-limit.md#resource-quotas) grubunuzda hesabı için bir Batch havuzu ekleyebileceğiniz verilen boyuta düğümlerinin sayısını sınırlayabilir. Bir kota artırım talebinde bulunmak bkz [bu makalede](batch-quota-limit.md#increase-a-quota). 

* **Havuz yapılandırmasını** -bulut hizmeti yapılandırmasıyla karşılaştırıldığında sanal makine yapılandırma havuzu oluştururken genel olarak, daha fazla VM boyut seçenekleri vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Batch ayrıntılı genel bakış için bkz: [büyük ölçekli paralel işlem çözümleri geliştirme batch'le](batch-api-basics.md).
* Yoğun işlem gücü kullanımlı VM boyutları hakkında daha fazla bilgi için bkz: [kullanım RDMA özellikli veya GPU özellikli örnekler Batch havuzlarında](batch-pool-compute-intensive-sizes.md).
