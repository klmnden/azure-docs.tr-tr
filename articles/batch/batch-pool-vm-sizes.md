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
ms.date: 06/11/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 033e0865f23034b94e3133e0ba5890eca4e746ea
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080885"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Bir Azure Batch havuzunda işlem düğümleri için VM boyutu seçme

Azure Batch havuzu için bir düğüm boyutu seçtiğinizde, neredeyse tüm VM boyutları Azure'da kullanıma sunulan arasından seçim yapabilirsiniz. Azure farklı iş yükleri için Linux ve Windows Vm'leri için bir dizi boyutları sunar.

Bazı özel durumlar ve bir VM boyutu seçme sınırlamalar vardır:

* Bazı VM serisi veya VM boyutları, Batch hizmetinde desteklenmez.
* Bazı VM boyutları kısıtlı ve bunlar ayrılabilen önce özellikle etkinleştirilmesi gerekir.

## <a name="supported-vm-series-and-sizes"></a>Desteklenen VM serisi ve boyutları

### <a name="pools-in-virtual-machine-configuration"></a>Sanal makine yapılandırmasındaki havuzlarda

Sanal makine yapılandırmasında batch havuzları, neredeyse tüm VM boyutları destekler ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). Desteklenen boyutlar ve kısıtlamaları hakkında daha fazla bilgi edinmek için aşağıdaki tabloya bakın.

Tüm tanıtım veya listede Önizleme VM boyutları için destek garanti değildir.

| VM serisi  | Desteklenen boyutlar | Batch hesabının Havuz ayırma modu<sup>1</sup> |
|------------|---------|-----------------|
| Temel A serisi | Tüm boyutları *dışında* Basic_A0 (A0) | Tüm |
| A Serisi | Tüm boyutları *dışında* standard_a0 = | Tüm |
| Av2 Serisi | Tüm boyutlar | Tüm |
| B serisi | None | Kullanılamaz |
| DC serisi | None | Kullanılamaz |
| Dv2, Dsv2 serisi | Tüm boyutlar | Tüm |
| Dv3, Dsv3 serisi | Tüm boyutlar | Tüm |
| [Bellek için iyileştirilmiş boyutları](../virtual-machines/linux/sizes-memory.md) | None | Kullanılamaz |
| Fsv2-serisi | Tüm boyutlar | Tüm |
| H Serisi | Tüm boyutlar | Tüm |
| Hb serisi | Tüm boyutlar | Kullanıcı aboneliği modu |
| Hc serisi | Tüm boyutlar | Kullanıcı aboneliği modu |
| Ls serisi | Tüm boyutlar | Tüm |
| Lsv2 serisi | None | Kullanılamaz |
| M serisi | İşler için standart_m64ms (düşük öncelikli yalnızca), işler için standart_m128s (yalnızca düşük öncelikli) | Tüm |  
| NCv2 serisi<sup>2</sup> | Tüm boyutlar | Tüm |
| NCv3 serisi<sup>2</sup> | Tüm boyutlar | Tüm |
| ND serisi<sup>2</sup> | Tüm boyutlar | Tüm |
| NDv2 serisi | Tüm boyutlar | Kullanıcı aboneliği modu |
| NV serisi | Tüm boyutlar | Tüm |
| NVv3 serisi | None | Kullanılamaz |
| SAP HANA | None | Kullanılamaz |

<sup>1</sup> bazı yeni VM serisi başlangıçta kısmen desteklenir. Bu VM serisi ile Batch hesaplarını tarafından ayrılabilen **Havuz ayırma modu** kümesine **kullanıcı aboneliği**. Bkz: [yönetme Batch hesapları](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode) Batch hesap yapılandırması hakkında daha fazla bilgi için. Bkz: [kotalar ve sınırlar](batch-quota-limit.md) kısmen desteklenen VM serisi için bu kota isteği öğrenmek için **kullanıcı aboneliği** Batch hesapları.  

<sup>2</sup> bu VM boyutları ayrılan sanal makine yapılandırmasında Batch havuzlarında, ancak belirli bir istemelisiniz [kota artışı](batch-quota-limit.md#increase-a-quota).

### <a name="pools-in-cloud-service-configuration"></a>Bulut hizmeti yapılandırması havuzları

Bulut hizmeti yapılandırmasında batch havuzları destekleyen tüm [bulut Hizmetleri için VM boyutları](../cloud-services/cloud-services-sizes-specs.md) **dışında** aşağıdakiler:

| VM serisi  | Desteklenmeyen boyutları |
|------------|-------------------|
| A Serisi   | Çok küçük       |
| Av2 Serisi | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="size-considerations"></a>Boyutu konuları

* **Uygulama gereksinimleri** -özelliklerini ve düğümler üzerinde çalıştırmanız uygulama gereksinimlerini göz önünde bulundurun. Uygulamanın çok iş parçacıklı olup olmadığı ve ne kadar bellek kullandığı gibi konular en uygun ve ekonomik düğüm boyutunu belirlemeye yardımcı olabilir. Çok örnekli için [MPI iş yükleri](batch-mpi.md) veya özelleştirilmiş göz önünde bulundurun CUDA uygulamaları [HPC](../virtual-machines/linux/sizes-hpc.md) veya [GPU özellikli](../virtual-machines/linux/sizes-gpu.md) VM boyutları, sırasıyla. (Bkz [kullanım RDMA özellikli veya GPU özellikli örnekler Batch havuzlarında](batch-pool-compute-intensive-sizes.md).)

* **Düğüm başına görevleri** -bir düğüm boyutu bir görevi bir düğümde aynı anda çalışır varsayarak normaldir. Ancak, birden fazla görevin (ve dolayısıyla Uygulama örneğinin) için avantajlı olabilir [paralel şekilde çalışan](batch-parallel-node-tasks.md) iş yürütme sırasında işlem düğümleri üzerinde. Bu durumda, paralel görev yürütmeye yönelik artan talebi karşılamak için bir çok çekirdekli düğüm boyutu yaygındır.

* **Yük düzeyleri farklı görevler için** -tüm bir havuzdaki düğümler aynı boyuttadır. Farklı sistem gereksinimlerine ve/veya yük düzeylerine sahip uygulamalar çalıştırmayı planlıyorsanız ayrı havuzlar oluşturmanız önerilir.

* **Bölge kullanılabilirliği** -bir VM serisi veya boyutu gerçekleştirilemeyebilir bölgelerde Batch hesabınızı oluşturduğunuz yerdir. Bir boyut kullanılabilir olduğunu denetlemek için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

* **Kotalar** - [çekirdek kotaları](batch-quota-limit.md#resource-quotas) grubunuzda hesabı için bir Batch havuzu ekleyebileceğiniz verilen boyuta düğümlerinin sayısını sınırlayabilir. Bir kota artırım talebinde bulunmak bkz [bu makalede](batch-quota-limit.md#increase-a-quota). 

* **Havuz yapılandırmasını** -bulut hizmeti yapılandırmasıyla karşılaştırıldığında sanal makine yapılandırma havuzu oluştururken genel olarak, daha fazla VM boyut seçenekleri vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Batch ayrıntılı genel bakış için bkz: [büyük ölçekli paralel işlem çözümleri geliştirme batch'le](batch-api-basics.md).
* Yoğun işlem gücü kullanımlı VM boyutları hakkında daha fazla bilgi için bkz: [kullanım RDMA özellikli veya GPU özellikli örnekler Batch havuzlarında](batch-pool-compute-intensive-sizes.md).
