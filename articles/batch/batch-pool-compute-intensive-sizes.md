---
title: Yoğun işlem gücü kullanımlı Azure VM'ler Batch ile kullanma | Microsoft Docs
description: HPC ve GPU VM boyutları Azure Batch havuzlarında yararlanmak nasıl
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/17/2018
ms.author: lahugh
ms.openlocfilehash: 3974be886b57fbf685b211369094edf844d96ab6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776533"
---
# <a name="use-rdma-or-gpu-instances-in-batch-pools"></a>Batch havuzları, RDMA veya GPU örnekleri kullan

Belirli Batch işlerini çalıştırmak için büyük ölçekli hesaplama için tasarlanan Azure VM boyutları yararlanabilirsiniz. Örneğin:

* Çok örnekli çalıştırılacak [MPI iş yükleri](batch-mpi.md), H serisi veya doğrudan uzak bellek erişimi (RDMA için) bir ağ arabirimine sahip diğer boyutlarını seçin. Bu boyutları MPI uygulamaları hızlandırabilir düğümler arası iletişim için bir InfiniBand ağına bağlayın. 

* CUDA gerektiren uygulamalar için NVIDIA Tesla grafik işlem birimi (GPU) kartları içeren N serisi boyutları seçin.

Bu makalede, rehberlik ve Azure'nın özel boyutları bazıları Batch havuzlarını kullanmak için örnekler sağlar. Özellikleri ve arka plan için bkz:

* Yüksek performanslı bilgi işlem, sanal makine boyutları ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* GPU etkin sanal makine boyutları ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 

> [!NOTE]
> Belirli VM boyutları, Batch hesabınızı oluşturduğunuz yerdir bölgelerde kullanılabilir olmayabilir. Bir boyut kullanılabilir olduğunu denetlemek için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/) ve [bir Batch havuzu için bir VM boyutu seçme](batch-pool-vm-sizes.md).

## <a name="dependencies"></a>Bağımlılıklar

Batch'te işlem yoğunluklu boyutları RDMA veya GPU yeteneklerini, yalnızca belirli işletim sistemlerinde desteklenir. (Desteklenen işletim sistemlerinin listesi bu boyutları içinde oluşturulan sanal makineleri için desteklenen bir alt kümesidir.) Batch havuzu oluşturma nasıl bağlı olarak, yükleme veya düğümler üzerinde daha fazla sürücü veya diğer yazılım yapılandırma gerekebilir. Aşağıdaki tablolarda, bu bağımlılıklar özetlenmektedir. Ayrıntılar için bağlantısı verilen makalelerden bakın. Batch havuzları yapılandırmak daha fazla seçenek için bu makalenin ilerleyen bölümlerinde bkz.

### <a name="linux-pools---virtual-machine-configuration"></a>Linux havuzu - sanal makine yapılandırması

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances)<br/>[NC24r, NC24rs_v2, NC24rs_v3, ND24rs<sup>*</sup>](../virtual-machines/linux/n-series-driver-setup.md#rdma-network-connectivity) | RDMA | Ubuntu 16.04 LTS, veya<br/>CentOS tabanlı HPC<br/>(Azure Market) | Intel MPI 5<br/><br/>Linux RDMA sürücüleri | Düğümler arası iletişimi etkinleştirmek için eşzamanlı görev yürütme devre dışı bırak |
| [NC, NCv2 NCv3, NDv2 serisi](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla GPU (seriyi göre değişiklik gösterir) | Ubuntu 16.04 LTS, veya<br/>CentOS 7.3 veya 7.4<br/>(Azure Market) | NVIDIA CUDA veya CUDA Araç Seti sürücüleri | Yok | 
| [NV, NVv2 serisi](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS, veya<br/>CentOS 7.3<br/>(Azure Market) | NVIDIA GRID sürücüleri | Yok |

<sup>*</sup>RDMA özellikli N serisi boyutları NVIDIA Tesla GPU'ları de içerir.

### <a name="windows-pools---virtual-machine-configuration"></a>Windows havuzu - sanal makine yapılandırması

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances)<br/>[NC24r, NC24rs_v2, NC24rs_v3, ND24rs<sup>*</sup>](../virtual-machines/windows/n-series-driver-setup.md#rdma-network-connectivity) | RDMA | Windows Server 2016, 2012 R2'de, veya<br/>2012 (azure Market) | Microsoft MPI 2012 R2 veya sonrası, veya<br/> Intel MPI 5<br/><br/>Windows RDMA sürücüleri | Düğümler arası iletişimi etkinleştirmek için eşzamanlı görev yürütme devre dışı bırak |
| [NC, NCv2, NCv3, ND serisi NDv2](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla GPU (seriyi göre değişiklik gösterir) | Windows Server 2016 veya <br/>2012 R2 (Azure Market) | NVIDIA CUDA veya CUDA Araç Seti sürücüleri| Yok | 
| [NV, NVv2 serisi](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 veya<br/>2012 R2 (Azure Market) | NVIDIA GRID sürücüleri | Yok |

<sup>*</sup>RDMA özellikli N serisi boyutları NVIDIA Tesla GPU'ları de içerir.

### <a name="windows-pools---cloud-services-configuration"></a>Windows havuzu - Cloud services yapılandırması

> [!NOTE]
> N serisi boyutları bulut hizmeti yapılandırma ile Batch havuzlarında desteklenir.
>

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılım | Havuz ayarları |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2016, 2012 R2, 2012 veya<br/>2008 R2 (konuk işletim sistemi ailesi) | Microsoft MPI 2012 R2 veya sonrası, veya<br/>Intel MPI 5<br/><br/>Windows RDMA sürücüleri | Düğümler arası iletişimi etkinleştirmek,<br/> eşzamanlı görev yürütme devre dışı bırak |

## <a name="pool-configuration-options"></a>Havuz yapılandırma seçenekleri

Özel bir VM boyutu, Batch havuzu için yapılandırmak için gerekli yazılımı veya sürücüleri yüklemek için birkaç seçeneğiniz vardır:

* Sanal Makine Yapılandırması havuzları için önceden yapılandırılmış bir seçin [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/) sürücüleri ve yazılımları önceden yüklenmiş olan bir VM görüntüsü. Örnekler: 

  * [CentOS tabanlı 7.4 HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased74HPC?tab=Overview) -RDMA sürücüleri ve Intel MPI 5.1 içerir

  * [Veri bilimi sanal makinesi](../machine-learning/data-science-virtual-machine/overview.md) Linux veya Windows - NVIDIA CUDA sürücülerini içerir.

  * GPU ve RDMA sürücüleri de Batch kapsayıcı iş yükleri için Linux görüntüleri:

    * [Azure Batch kapsayıcı havuzlar için centOS (ile GPU ve RDMA sürücüleri)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-batch.centos-container-rdma?tab=Overview)

    * [Azure Batch kapsayıcı havuzlar için ubuntu Server (ile GPU ve RDMA sürücüleri)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-batch.ubuntu-server-container-rdma?tab=Overview)

* Oluşturma bir [özel Windows veya Linux VM görüntüsünü](batch-custom-images.md) , sürücüleri, yazılım veya VM boyutu için gerekli diğer ayarları yüklemiş olduğunuz üzerinde. 

* Bir toplu işi oluşturma [uygulama paketini](batch-application-packages.md) sıkıştırılmış sürücü veya uygulama yükleyici ve Batch havuz düğümlerine paketi dağıtma ve her düğüm oluşturulduğunda sonra yüklemek için yapılandırın. Örneğin, uygulama paketi yükleyici ise, oluşturun bir [başlangıç görevi](batch-api-basics.md#start-task) tüm havuzu düğümlerinde uygulamayı sessiz yüklemek için komut satırı. Bir uygulama paketi kullanmayı göz önünde bulundurun ve bir havuz, iş yükünüz belirli sürücü sürümü bağlıysa görev başlatın.

  > [!NOTE] 
  > Başlangıç görevi yükseltilmiş (Yönetici) izinleriyle çalıştırmalısınız ve başarı için beklemeniz gerekir. Uzun süre çalışan görevleri Batch havuz sağlama süresini artırır.
  >

* [Batch Shipyard](https://github.com/Azure/batch-shipyard) ile kapsayıcılı iş yüklerini Azure Batch'te şeffaf bir şekilde çalışması için GPU ve RDMA sürücülerini otomatik olarak yapılandırır. Batch Shipyard tamamen yapılandırma dosyaları ile yönetilir. GPU ve RDMA iş yükleri gibi etkinleştiren birçok örnek tarif yapılandırması kullanılabilir [CNTK GPU tarif](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) GPU sürücüleri N serisi vm'lerde önceden yapılandırır ve bir Docker görüntüsü olarak Microsoft Bilişsel Araç Seti yazılım yükler.


## <a name="example-nvidia-gpu-drivers-on-windows-nc-vm-pool"></a>Örnek: Windows NC VM havuzunda NVIDIA GPU sürücüleri

Bir Windows NC düğümleri havuzunda CUDA uygulamalarını çalıştırmak için NVDIA GPU sürücüleri yüklemeniz gerekir. Aşağıdaki örnek adımlarda, NVIDIA GPU sürücülerini yüklemek için bir uygulama paketi kullanın. İş yükünüz belirli bir GPU sürücü sürümü bağlıysa bu seçeneği belirleyebilirsiniz.

1. GPU sürücüleri Windows Server 2016'dan bir Kurulum paketini indirme [NVIDIA Web sitesi](https://www.nvidia.com/Download/index.aspx) - Örneğin, [sürüm 411.82](https://us.download.nvidia.com/Windows/Quadro_Certified/411.82/411.82-tesla-desktop-winserver2016-international.exe). Yerel olarak gibi kısa ad kullanarak dosyayı kaydedin *GPUDriverSetup.exe*.
2. Bir paket zip dosyası oluşturun.
3. Paket Batch hesabınıza yükleyin. Adımlar için bkz: [uygulama paketleri](batch-application-packages.md) Kılavuzu. Bir uygulama kimliği gibi belirtin *GPUDriver*ve sürümü gibi *411.82*.
1. Azure portalı ve Batch API'lerini kullanarak, istenen düğüm sayısını ve ölçek ile sanal makine yapılandırması bir havuz oluşturun. Aşağıdaki tablo, bir başlangıç görevi kullanarak sessiz NVIDIA GPU sürücülerini yüklemek için örnek ayarlarını gösterir:

| Ayar | Değer |
| ---- | ----- | 
| **Görüntü Türü** | Market (Linux/Windows) |
| **Yayımcı** | MicrosoftWindowsServer |
| **Teklif** | WindowsServer |
| **Sku** | 2016-Datacenter |
| **Düğüm boyutu** | Standart NC6 |
| **Uygulama paketi başvuruları** | GPUDriver, sürüm 411.82 |
| **Başlangıç görevi, etkin** | True<br>**Komut satırı** - `cmd /c "%AZ_BATCH_APP_PACKAGE_GPUDriver#411.82%\\GPUDriverSetup.exe /s"`<br/>**Kullanıcı Kimliği** -havuzu otomatik kullanıcı, yönetici<br/>**Başarı için bekleyin** - True

## <a name="example-nvidia-gpu-drivers-on-a-linux-nc-vm-pool"></a>Örnek: Bir Linux NC VM havuzunda NVIDIA GPU sürücüleri

Bir Linux NC düğümleri havuzunda CUDA uygulamaları çalıştırmak için gerekli NVIDIA Tesla GPU sürücüleri CUDA araç setinin yüklemeniz gerekir. Aşağıdaki örnek adımlarda, oluşturun ve özel bir Ubuntu 16.04 LTS görüntü GPU sürücüleri ile dağıtın:

1. Ubuntu 16.04 LTS çalıştıran bir Azure NC serisi VM dağıtın. Örneğin, VM Güney Orta ABD bölgesinde oluşturun. 
2. Ekleme [NVIDIA GPU sürücülerini uzantısı](../virtual-machines/extensions/hpccompute-gpu-linux.md
) Azure portalı kullanarak VM için bir istemci bilgisayara bağlanan Azure aboneliği veya Azure Cloud Shell için. Alternatif olarak, VM'ye bağlanmak için adımları izleyin ve [CUDA sürücülerini yüklemek](../virtual-machines/linux/n-series-driver-setup.md) el ile.
3. Oluşturmak için aşağıdaki adımları izleyerek bir [anlık görüntü ve özel Linux VM görüntüsü](batch-custom-images.md) toplu işlemi için.
4. Bir Batch hesabı NC VM'lerin desteklediği bir bölgede oluşturun.
5. Azure portalı ve Batch API'lerini kullanarak bir havuz oluşturma [özel görüntü kullanarak](batch-custom-images.md) ve istenen düğüm sayısını ve ölçek ile. Aşağıdaki tabloda örnek havuzu ayarlarını görüntüsü gösterilmektedir:

| Ayar | Değer |
| ---- | ---- |
| **Görüntü Türü** | Özel Görüntü |
| **Özel görüntü** | *Görüntü adı* |
| **Düğüm Aracısı SKU** | Batch.node.ubuntu 16.04 |
| **Düğüm boyutu** | Standart NC6 |

## <a name="example-microsoft-mpi-on-a-windows-h16r-vm-pool"></a>Örnek: Bir Windows H16r VM havuzunda Microsoft MPI

Bir Azure H16r VM düğümlerinin havuzunda Windows MPI uygulamalarını çalıştırmak için HpcVmDrivers uzantısını yapılandırın ve yüklemek gereken [Microsoft MPI](https://docs.microsoft.com/message-passing-interface/microsoft-mpi). Özel bir Windows Server 2016 görüntüsü ile gerekli sürücüleri ve yazılım dağıtmak için örnek adımlar şunlardır:

1. Windows Server 2016 çalıştıran bir Azure H16r VM dağıtın. Örneğin, VM, ABD Batı bölgesinde oluşturun. 
2. Sanal makine tarafından HpcVmDrivers uzantısı eklemek [Azure PowerShell komutunu çalıştıran](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances
) bir istemci bilgisayardan Azure aboneliğiniz veya Azure Cloud Shell kullanarak bağlanır. 
1. VM'ye Uzak Masaüstü Bağlantısı.
1. İndirme [Kurulum paketini](https://www.microsoft.com/download/details.aspx?id=57467) (MSMpiSetup.exe) Microsoft MPI Microsoft MPI yükle ve en son sürümü.
1. Oluşturmak için aşağıdaki adımları izleyerek bir [anlık görüntü ve özel bir Windows VM görüntüsü](batch-custom-images.md) toplu işlemi için.
1. Azure portalı ve Batch API'lerini kullanarak bir havuz oluşturma [özel görüntü kullanarak](batch-custom-images.md) ve istenen düğüm sayısını ve ölçek ile. Aşağıdaki tabloda örnek havuzu ayarlarını görüntüsü gösterilmektedir:

| Ayar | Değer |
| ---- | ---- |
| **Görüntü Türü** | Özel Görüntü |
| **Özel görüntü** | *Görüntü adı* |
| **Düğüm Aracısı SKU** | Batch.node.Windows amd64 |
| **Düğüm boyutu** | H16r standart |
| **Etkin düğümler arası iletişim** | True |
| **Düğüm başına en fazla görev** | 1 |

## <a name="example-intel-mpi-on-a-linux-h16r-vm-pool"></a>Örnek: Bir Linux H16r VM havuzunda Intel MPI

Düğümleri havuzunda Linux H-serisi MPI uygulamalarını çalıştırmak için kullanılacak bir seçenek olan [CentOS tabanlı 7.4 HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased74HPC?tab=Overview) Azure Market görüntüsünden. Linux RDMA sürücüleri ve Intel MPI makineleridir. Bu görüntü, Docker kapsayıcı iş yüklerini de destekler.

Azure portalı ve Batch API'lerini kullanarak bu görüntü kullanarak havuz oluşturma ve istenen düğüm sayısını ve ölçek ile. Aşağıdaki tabloda örnek havuzu ayarlarını gösterilmektedir:

| Ayar | Değer |
| ---- | ---- |
| **Görüntü Türü** | Market (Linux/Windows) |
| **Yayımcı** | OpenLogic |
| **Teklif** | CentOS-HPC |
| **Sku** | 7.4 |
| **Düğüm boyutu** | H16r standart |
| **Etkin düğümler arası iletişim** | True |
| **Düğüm başına en fazla görev** | 1 |

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure Batch havuzunda MPI işlerini çalıştırma için bkz: [Windows](batch-mpi.md) veya [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) örnekler.

* Toplu iş yüklerinde GPU örnekleri için bkz: [Batch Shipyard](https://github.com/Azure/batch-shipyard/) tarifleri.