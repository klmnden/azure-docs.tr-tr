---
title: Yoğun işlem gücü kullanımlı Azure VM'ler Batch ile kullanma | Microsoft Docs
description: Azure Batch havuzları, RDMA özellikli veya GPU etkin sanal makine boyutlarını avantajlardan yararlanma
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: danlep
ms.openlocfilehash: 6969f0c6a05ebf5b34fb746d2a83b884687ad710
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258264"
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Batch havuzları, RDMA özellikli veya GPU özellikli örnekler kullanın

Belirli Batch işlerini çalıştırmak için büyük ölçekli hesaplama için tasarlanan Azure VM boyutlarını yararlanmak isteyebilirsiniz. Örneğin, çok örnekli çalıştırılacak [MPI iş yükleri](batch-mpi.md), A8, A9, seçebilirsiniz veya bir ağda H serisi boyutları arabirim Uzak doğrudan bellek erişimi (RDMA için). Bu boyutları MPI uygulamaları hızlandırabilir düğümler arası iletişim için bir InfiniBand ağına bağlayın. Veya CUDA uygulamalarda, NVIDIA Tesla grafik işlem birimi (GPU) kartları içeren N serisi boyutları seçebilirsiniz.

Bu makalede, rehberlik ve Azure'nın özel boyutları bazıları Batch havuzlarını kullanmak için örnekler sağlar. Özellikleri ve arka plan için bkz:

* Yüksek performanslı bilgi işlem, sanal makine boyutları ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* GPU etkin sanal makine boyutları ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Abonelik ve hesap sınırlamaları

* **Kotalar ve sınırlar** - [Batch hesabı başına çekirdek kotası](batch-quota-limit.md#resource-quotas) eklemek için bir Batch havuzu verilen boyuta düğümlerinin sayısını sınırlayabilir. RDMA özellikli, GPU özellikli veya diğer çok çekirdekli VM boyutlarının seçtiğinizde bir kotaya erişmeniz daha büyük olasılıkla. 

  Ayrıca, ND, NCv2 ve NCv3 gibi Batch hesabınızdaki belirli VM aileleri kullanımını sınırlı kapasite nedeniyle sınırlıdır. Bu aileleri kullanımı yalnızca 0 çekirdek varsayılan kota artışı isteğinde bulunarak kullanılabilir.  

  İçin gerekiyorsa [bir kota artırım talebinde](batch-quota-limit.md#increase-a-quota) ücret olmadan.

* **Bölge kullanılabilirliği** - yoğun işlem gücü kullanımlı VM'ler gerçekleştirilemeyebilir Batch hesabınızı oluşturduğunuz yerdir bölgelerde. Bir boyut kullanılabilir olduğunu denetlemek için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Bağımlılıklar

Yoğun işlem gücü kullanımlı boyutlar RDMA ve GPU yeteneklerini, yalnızca belirli işletim sistemlerinde desteklenir. İşletim sisteminize bağlı olarak, yükleme veya daha fazla sürücü veya diğer yazılım yapılandırma gerekebilir. Aşağıdaki tablolarda, bu bağımlılıklar özetlenmektedir. Ayrıntılar için bağlantısı verilen makalelerden bakın. Batch havuzları yapılandırmak daha fazla seçenek için bu makalenin ilerleyen bölümlerinde bkz.


### <a name="linux-pools---virtual-machine-configuration"></a>Linux havuzu - sanal makine yapılandırması

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | Ubuntu 16.04 LTS,<br/>SUSE Linux Enterprise Server 12 HPC, veya<br/>CentOS tabanlı HPC<br/>(Azure Market) | Intel MPI 5 | Düğümler arası iletişimi etkinleştirmek için eşzamanlı görev yürütme devre dışı bırak |
| [NC, NCv2 NCv3, ND serisi *](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla GPU (seriyi göre değişiklik gösterir) | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.4, ya da 7.3 veya<br/>CentOS 7.3 veya 7.4<br/>(Azure Market) | NVIDIA CUDA Araç Seti sürücüleri | Yok | 
| [NV serisi](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3, veya<br/>CentOS 7.3<br/>(Azure Market) | NVIDIA GRID sürücüleri | Yok |

* RDMA bağlantısı RDMA özellikli N serisi vm'lerde gerektirebilir [ek yapılandırma](../virtual-machines/linux/n-series-driver-setup.md#rdma-network-connectivity) dağıtım göre değişir.



### <a name="windows-pools---virtual-machine-configuration"></a>Windows havuzu - sanal makine yapılandırması

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2016, 2012 R2'de, veya<br/>2012 (azure Market) | Microsoft MPI 2012 R2 veya sonrası, veya<br/> Intel MPI 5<br/><br/>HpcVMDrivers Azure VM uzantısı | Düğümler arası iletişimi etkinleştirmek için eşzamanlı görev yürütme devre dışı bırak |
| [NC, NCv2 NCv3, ND serisi *](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla GPU (seriyi göre değişiklik gösterir) | Windows Server 2016 veya <br/>2012 R2 (Azure Market) | NVIDIA Tesla sürücüleri veya CUDA Araç Seti sürücüleri| Yok | 
| [NV serisi](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 veya<br/>2012 R2 (Azure Market) | NVIDIA GRID sürücüleri | Yok |

* RDMA bağlantısı RDMA özellikli N serisi sanal makineler üzerinde Windows Server 2016 veya Windows Server 2012 R2 (Azure Market'ten) HpcVMDrivers uzantısı ve Microsoft MPI veya Intel MPI ile desteklenir.

### <a name="windows-pools---cloud-services-configuration"></a>Windows havuzu - Cloud services yapılandırması

> [!NOTE]
> N serisi boyutları bulut hizmeti yapılandırma ile Batch havuzlarında desteklenir.
>

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2016, 2012 R2, 2012 veya<br/>2008 R2 (konuk işletim sistemi ailesi) | Microsoft MPI 2012 R2 veya sonrası, veya<br/>Intel MPI 5<br/><br/>HpcVMDrivers Azure VM uzantısı | Düğümler arası iletişimi etkinleştirmek,<br/> eşzamanlı görev yürütme devre dışı bırak |





## <a name="pool-configuration-options"></a>Havuz yapılandırma seçenekleri

Özel bir VM boyutu, Batch havuzu için yapılandırmak için Batch API'lerine ve araçlarına gerekli yazılım ya da dahil olmak üzere sürücü yüklemek için birkaç seçenek sağlar:

* [Başlangıç görevi](batch-api-basics.md#start-task) -bir yükleme paketi kaynak dosyası olarak Batch hesabıyla aynı bölgede bir Azure depolama hesabına yükleyin. Havuz başladığında kaynak dosyası sessizce yüklemek için bir başlangıç görevi komut satırı oluşturun. Daha fazla bilgi için bkz: [REST API belgeleri](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > Başlangıç görevi yükseltilmiş (Yönetici) izinleriyle çalıştırmalısınız ve başarı için beklemeniz gerekir.
  >

* [Uygulama paketi](batch-application-packages.md) - Batch hesabınıza bir sıkıştırılmış yükleme paketi eklemek ve paket başvurusu havuzda yapılandırın. Bu ayar, yükler ve havuzdaki tüm düğümlerin paketi unzips. Bir yükleyici paketi ise tüm havuzu düğümlerinde uygulamayı sessiz yüklemek için bir başlangıç görevi komut satırı oluşturun. İsteğe bağlı olarak bir görevi bir düğümde çalışmak üzere zamanlandığında paketi yükleyin.

* [Özel havuz görüntü](batch-custom-images.md) - yazılım sürücüleri içeren özel bir Windows veya Linux VM görüntüsü oluşturun veya gerekli VM boyutu için diğer ayarlar. 

* [Batch Shipyard](https://github.com/Azure/batch-shipyard) GPU ve RDMA ile kapsayıcılı iş yüklerini Azure Batch'te şeffaf bir şekilde çalışması için otomatik olarak yapılandırır. Batch Shipyard tamamen yapılandırma dosyaları ile yönetilir. GPU ve RDMA iş yükleri gibi etkinleştiren birçok örnek tarif yapılandırması kullanılabilir [CNTK GPU tarif](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) GPU sürücüleri N serisi vm'lerde önceden yapılandırır ve bir Docker görüntüsü olarak Microsoft Bilişsel Araç Seti yazılım yükler.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Örnek: Microsoft MPI A8 VM havuzunda

Azure A8 düğümü havuzu üzerinde Windows MPI uygulamalarını çalıştırmak için desteklenen bir MPI uygulamasını yüklemeniz gerekir. Yüklemek için örnek adımlar şunlardır [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) üzerinde bir Windows havuzu Batch uygulama paketini kullanma.

1. İndirme [Kurulum paketini](https://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) Microsoft MPI en son sürümü için.
2. Bir paket zip dosyası oluşturun.
3. Paket Batch hesabınıza yükleyin. Adımlar için bkz: [uygulama paketleri](batch-application-packages.md) Kılavuzu. Bir uygulama kimliği gibi belirtin *MSMPI*ve sürümü gibi *8.1*. 
4. Azure portalı ve Batch API'lerini kullanarak, istenen düğüm sayısını ve ölçek ile bulut Hizmetleri Yapılandırması'nda bir havuz oluşturun. Aşağıdaki tablo, bir başlangıç görevi kullanarak katılımsız modda MPI ayarlamak için örnek ayarlarını gösterir:

| Ayar | Değer |
| ---- | ----- | 
| **Görüntü Türü** | Cloud Services |
| **İşletim sistemi ailesi** | Windows Server 2012 R2 (işletim sistemi ailesi 4) |
| **Düğüm boyutu** | Standart a8 |
| **Etkin düğümler arası iletişim** | True |
| **Düğüm başına en fazla görev** | 1 |
| **Uygulama paketi başvuruları** | MSMPI |
| **Başlangıç görevi, etkin** | True<br>**Komut satırı** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Kullanıcı Kimliği** -havuzu otomatik kullanıcı, yönetici<br/>**Başarı için bekleyin** - True

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Örnek: NC VM havuzunda NVIDIA Tesla sürücüleri

Bir Linux NC düğümleri havuzunda CUDA uygulamaları çalıştırmak için CUDA Araç Seti 9.0 düğümlerinde yüklemeniz gerekir. Araç Seti gerekli NVIDIA Tesla GPU sürücüleri yükler. GPU sürücüleri içeren özel bir Ubuntu 16.04 LTS görüntüsünü dağıtmak için örnek adımlar şunlardır:

1. Ubuntu 16.04 LTS çalıştıran bir Azure NC serisi VM dağıtın. Örneğin, VM Güney Orta ABD bölgesinde oluşturun. Yönetilen disk ile VM oluşturduğunuzdan emin olun.
2. VM'ye bağlanmak için adımları izleyin ve [CUDA sürücülerini yüklemek](../virtual-machines/linux/n-series-driver-setup.md).
3. Linux Aracısı sağlamasını kaldırma ve ardından [Linux VM görüntüsü yakalama](../virtual-machines/linux/capture-image.md).
4. Bir Batch hesabı NC VM'lerin desteklediği bir bölgede oluşturun.
5. Azure portalı ve Batch API'lerini kullanarak bir havuz oluşturma [özel görüntü kullanarak](batch-custom-images.md) ve istenen düğüm sayısını ve ölçek ile. Aşağıdaki tabloda örnek havuzu ayarlarını görüntüsü gösterilmektedir:

| Ayar | Değer |
| ---- | ---- |
| **Görüntü Türü** | Özel görüntü |
| **Özel görüntü** | Görüntü adı |
| **Düğüm Aracısı SKU** | Batch.node.ubuntu 16.04 |
| **Düğüm boyutu** | Standart NC6 |



## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure Batch havuzunda MPI işlerini çalıştırma için bkz: [Windows](batch-mpi.md) veya [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) örnekler.

* Toplu iş yüklerinde GPU örnekleri için bkz: [Batch Shipyard](https://github.com/Azure/batch-shipyard/) tarifleri.