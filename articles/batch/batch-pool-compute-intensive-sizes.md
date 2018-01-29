---
title: "Toplu işlem yoğunluklu Azure VM'ler kullanın | Microsoft Docs"
description: "Azure Batch havuzlarında RDMA özellikli GPU etkinleştirilmiş veya VM boyutları yararlanmak nasıl"
services: batch
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: danlep
ms.openlocfilehash: dc28c3a9d46baa8e8d2136ffccbb4e7ff6675b1e
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Batch havuzları, RDMA özellikli GPU etkinleştirilmiş veya örnekleri kullanın

Belirli toplu işlerini çalıştırmak için Azure VM boyutlarını büyük ölçekli hesaplama için tasarlanmış yararlanmak isteyebilirsiniz. Örneğin, birden çok örneği çalıştırmak için [MPI iş yükleri](batch-mpi.md), A8, A9, seçebilir veya bir ağ sahip H-serisi boyutları arabirim uzaktan doğrudan bellek erişimi (RDMA için). Bu boyutlar MPI uygulamaları hızlandırabilir düğümler arası iletişim için bir InfiniBand ağa bağlayın. Veya CUDA uygulamalar için NVIDIA Tesla grafik işlem birimi (GPU) kartları dahil N-serisi boyutları seçebilirsiniz.

Bu makale, yönergeler ve Azure'nın özelleştirilmiş boyutları bazıları Batch havuzlarında kullanmaya örnekler sağlar. Belirtimlerin ve arka plan için bkz:

* Yüksek performanslı işlem VM boyutları ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* GPU etkin VM boyutları ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Aboneliği ve hesabı sınırları

* **Kotalar** - [toplu işlem hesabı başına ayrılmış çekirdek kotası](batch-quota-limit.md#resource-quotas) sayısını veya bir Batch havuzu ekleyebileceğiniz düğümleri türünü sınırlayabilir. RDMA özelliğine sahip, GPU etkin veya diğer çok çekirdekli VM boyutları seçtiğinizde kota ulaşmak büyük olasılıkla. Varsayılan olarak, bu kota 20 çekirdek ' dir. Ayrı bir kota uygulandığı [düşük öncelikli sanal makineleri](batch-low-pri-vms.md), bunları kullanıyorsanız. 

Kota artışı isteği gerekiyorsa, açık bir [çevrimiçi müşteri destek isteği](../azure-supportability/how-to-create-azure-support-request.md) herhangi bir ücret alınmaz.

* **Bölge kullanılabilirliği** - işlem yoğunluklu VM'ler olabilir kullanılabilir, Batch hesabınızı oluşturduğunuz bölgelerde. Bir boyut kullanılabilir olup olmadığını denetlemek için bkz: [bölgeye göre ürünleri](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Bağımlılıklar

İşlem yoğunluklu boyutları RDMA ve GPU özelliklerini yalnızca belirli işletim sistemlerinde desteklenir. İşletim sistemine bağlı olarak yüklemek veya daha fazla sürücü veya başka bir yazılım yapılandırmanız gerekebilir. Aşağıdaki tablolarda, bu bağımlılıklar özetlenmektedir. Ayrıntılar için bağlantılı makalelerine bakın. Batch havuzlarının yapılandırmak seçenekleri için bu makalenin sonraki bölümlerinde bkz.


### <a name="linux-pools---virtual-machine-configuration"></a>Linux havuzları - sanal makine yapılandırması

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | Ubuntu 16.04 LTS,<br/>SUSE Linux Enterprise Server 12 HPC, veya<br/>CentOS tabanlı HPC<br/>(Azure Market) | Intel MPI 5 | Düğümler arası iletişimi etkinleştirmek, eşzamanlı görev yürütme devre dışı bırakma |
| [NC, NCv2, ND serisi *](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-ncv2-and-nd-vms) | NVIDIA Tesla GPU (serilerine göre farklılık gösterir) | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3, veya<br/>CentOS tabanlı 7.3<br/>(Azure Market) | NVIDIA CUDA Araç Seti 9.1 sürücüleri | Yok | 
| [NV serisi](../virtual-machines/linux/n-series-driver-setup.md#install-grid-drivers-for-nv-vms) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3, veya<br/>CentOS tabanlı 7.3<br/>(Azure Market) | NVIDIA Kılavuz 4.3 sürücüleri | Yok |

* RDMA bağlantısı NC24r, NC24r_v2 ve ND24r VM'ler Ubuntu 16.04 LTS veya CentOS tabanlı 7.3 HPC (Azure Marketi'nden) Intel MPI ile desteklenir.



### <a name="windows-pools---virtual-machine-configuration"></a>Windows havuzları - sanal makine yapılandırması

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2 veya<br/>Windows Server 2012 (Azure Market) | Microsoft MPI 2012 R2 veya sonraki bir sürümü veya<br/> Intel MPI 5<br/><br/>HpcVMDrivers Azure VM uzantısı | Düğümler arası iletişimi etkinleştirmek, eşzamanlı görev yürütme devre dışı bırakma |
| [NC, NCv2, ND serisi *](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla GPU (serilerine göre farklılık gösterir) | Windows Server 2016 veya <br/>Windows Server 2012 R2 (Azure Market) | NVIDIA Tesla sürücüleri veya CUDA Araç Seti 9.1 sürücülerini| Yok | 
| [NV serisi](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 veya<br/>Windows Server 2012 R2 (Azure Market) | NVIDIA Kılavuz 4.3 sürücüleri | Yok |

* RDMA bağlantısı NC24r, NC24r_v2 ve ND24r VM'ler HpcVMDrivers uzantısı ve Microsoft MPI veya Intel MPI ile Windows Server 2012 R2 (Azure Marketi'nden) desteklenir.

### <a name="windows-pools---cloud-services-configuration"></a>Windows havuzları - Cloud services yapılandırması

> [!NOTE]
> N-serisi boyutları, cloud services yapılandırması ile Batch havuzlarında desteklenmez.
>

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2,<br/>Windows Server 2012 veya<br/>Windows Server 2008 R2 (konuk işletim sistemi ailesi) | Microsoft MPI 2012 R2 veya sonraki bir sürümü veya<br/>Intel MPI 5<br/><br/>HpcVMDrivers Azure VM uzantısı | Düğümler arası iletişimi etkinleştirmek,<br/> eşzamanlı görev yürütme devre dışı bırak |





## <a name="pool-configuration-options"></a>Havuz yapılandırma seçenekleri

Batch havuzu için özel bir VM boyutu yapılandırmak için gerekli yazılım ya da dahil olmak üzere sürücüleri yüklemek için birkaç seçenek Batch API'lerini ve araçları sağlar:

* [Başlangıç görevi](batch-api-basics.md#start-task) -bir Azure depolama hesabı toplu işlem hesabı ile aynı bölgede kaynak dosyası olarak bir yükleme paketi yükleyin. Havuzu başladığında kaynak dosyası sessizce yüklemek için bir başlangıç görevi komut satırı oluşturun. Daha fazla bilgi için bkz: [REST API belgeleri](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > Başlangıç görevi yükseltilmiş (Yönetici) izinleriyle çalıştırılmalıdır ve başarı için beklemeniz gerekir.
  >

* [Uygulama paketi](batch-application-packages.md) - Batch hesabınıza sıkıştırılmış yükleme paketi eklemek ve havuzda paketi başvurusu yapılandırın. Bu ayar paket havuzdaki tüm düğümler üzerinde unzips ve yükler. Paket bir yükleyici varsa, tüm havuzu düğümlerinde uygulamayı sessiz yüklemek için bir başlangıç görevi komut satırı oluşturun. İsteğe bağlı olarak, bir görev bir düğümü üzerinde çalışacak şekilde zamanlandığında paketini yükleyin.

* [Özel havuzu görüntü](batch-custom-images.md) - sürücüler, yazılım, içeren özel bir Windows veya Linux VM görüntü oluştur veya VM boyutu için gerekli diğer ayarları. 

* [Toplu Shipyard](https://github.com/Azure/batch-shipyard) GPU ve RDMA saydam kapsayıcılı iş yükleri ile Azure toplu olarak çalışmak için otomatik olarak yapılandırır. Toplu Shipyard tamamen yapılandırma dosyaları ile yönetilir. GPU ve RDMA iş yükleri gibi sağlayan birçok örnek tarif yapılandırmaları kullanılabilir vardır [CNTK GPU tarif](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) N-serisi vm'lerde GPU sürücüleri önceden yapılandırır ve Microsoft Bilişsel Araç Seti yazılım Docker görüntü olarak yükler.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Örnek: Microsoft MPI A8 VM havuzunda

Bir Azure A8 düğümleri havuzunda Windows MPI uygulamaları çalıştırmak için desteklenen bir MPI uygulamasını yüklemeniz gerekir. Yüklemek için örnek adımlar şunlardır [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) Windows havuz Batch uygulama paketini kullanarak.

1. Karşıdan [Kurulum paketini](http://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) Microsoft MPI en son sürümü için.
2. Paket bir zip dosyası oluşturun.
3. Paket Batch hesabınıza yükleyin. Adımlar için bkz: [uygulama paketleri](batch-application-packages.md) Kılavuzu. Bir uygulama kimliği gibi belirtin *MSMPI*ve sürüm gibi *8.1*. 
4. Batch API'leri veya Azure portal kullanarak, düğümleri ve ölçek istenen sayıda bulut Hizmetleri Yapılandırması'nda bir havuzu oluşturun. Aşağıdaki tabloda bir başlangıç görevi kullanarak katılımsız modda MPI ayarlamak için örnek ayarlarını gösterir:

| Ayar | Değer |
| ---- | ----- | 
| **Görüntü türü** | Cloud Services |
| **İşletim sistemi ailesi** | Windows Server 2012 R2 (işletim sistemi ailesi 4) |
| **Düğüm boyutu** | A8 standart |
| **Düğümler arası iletişim etkinleştirildi** | True |
| **Düğüm başına en fazla görev** | 1 |
| **Uygulama paketi başvuruları** | MSMPI |
| **Etkin başlangıç görevi** | True<br>**Komut satırı** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Kullanıcı Kimliği** -havuzu autouser, yönetici<br/>**Başarı için bekleyin** - True

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Örnek: NVIDIA Tesla sürücüleri NC VM havuzu

Bir Linux NC düğümleri havuzunda CUDA uygulamalarını çalıştırmak için CUDA Araç Seti 9.0 düğümlerinde yüklemeniz gerekir. Araç Seti gerekli NVIDIA Tesla GPU sürücüleri yükler. GPU sürücüleri ile özel bir Ubuntu 16.04 LTS görüntü dağıtmak için örnek adımlar şunlardır:

1. Bir Azure NC6 Ubuntu 16.04 LTS çalıştıran VM dağıtın. Örneğin, BİZE Güney merkez bölgede VM oluşturun. Yönetilen bir diskle VM oluşturduğunuzdan emin olun.
2. VM'e bağlanmak için gereken adımları izleyin ve [CUDA sürücülerini yüklemek](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-ncv2-and-nd-vms).
3. Linux Aracısı'nı yetkisini kaldırma ve ardından [Linux VM görüntüsü yakalama](../virtual-machines/linux/capture-image.md).
4. NC sanal makineleri destekleyen bir bölgede bir Batch hesabı oluşturun.
5. Batch API'leri veya Azure portalını kullanarak oluşturduğunuz bir havuzu [özel görüntü kullanarak](batch-custom-images.md) ve istenen sayıda düğümleri ve ölçek sahip. Aşağıdaki tabloda görüntüsü için örnek havuzu ayarları gösterilmektedir:

| Ayar | Değer |
| ---- | ---- |
| **Görüntü türü** | Özel görüntü |
| **Özel görüntü** | Görüntü adı |
| **Düğüm Aracısı SKU** | batch.node.ubuntu 16.04 |
| **Düğüm boyutu** | NC6 standart |



## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure Batch havuzunda MPI işlerini çalıştırmak için bkz: [Windows](batch-mpi.md) veya [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) örnekler.

* Batch GPU iş yüklerinin örnekleri için bkz: [toplu Shipyard](https://github.com/Azure/batch-shipyard/) tarif.