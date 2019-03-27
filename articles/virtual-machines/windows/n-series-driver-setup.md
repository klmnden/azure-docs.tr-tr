---
title: Windows için Azure N serisi GPU sürücü kurulumu | Microsoft Docs
description: Azure'da Windows Server veya Windows çalıştıran N serisi VM'ler için NVIDIA GPU sürücülerini ayarlama
services: virtual-machines-windows
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d173f1f9048a46f3fb2500d225ec121157d5d960
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483555"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-windows"></a>Windows çalıştıran N serisi Vm'lerde NVIDIA GPU sürücüleri yükleyin 

Windows çalıştıran Azure N serisi sanal makineler, GPU özelliklerine yararlanmak için NVIDIA GPU sürücülerine yüklenmesi gerekir. [NVIDIA GPU sürücüsünün uzantısı](../extensions/hpccompute-gpu-windows.md) bir N serisi sanal makinesinde uygun NVIDIA CUDA veya kılavuz sürücüleri yükler. Yükleme veya uzantısını Azure portalı veya Azure PowerShell veya Azure Resource Manager şablonları gibi araçları kullanarak yönetin. Bkz: [NVIDIA GPU sürücüsünün uzantı belgesini](../extensions/hpccompute-gpu-windows.md) desteklenen işletim sistemleri ve dağıtım adımları için.

GPU sürücüleri el ile yüklemeyi seçerseniz, bu makalede, desteklenen işletim sistemleri, sürücüler ve yükleme ve doğrulama adımlarını sağlar. El ile sürücü kurulum bilgileri, ayrıca kullanılabilir [Linux Vm'leri](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Temel özellikleri, depolama kapasitesi ve disk ayrıntıları için bkz. [GPU Windows VM boyutları](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>Sürücü yüklemesi

1. Her N serisi sanal makine için Uzak Masaüstü ile bağlanın.

2. İndirin, ayıklayın ve desteklenen Windows işletim sisteminizi sürücüsünü yükleyin.

Bir VM'de kılavuz sürücü yükleme sonrasında bir yeniden başlatma gereklidir. CUDA sürücü yükleme sonrasında bir yeniden başlatma gerekli değildir.

## <a name="verify-driver-installation"></a>Sürücü yüklemesi doğrulayın

Sürücü yüklemesi cihaz Yöneticisi'nde doğrulayabilirsiniz. Aşağıdaki örnek, bir Azure NC VM Tesla K80 kartın başarılı yapılandırmasını gösterir.

![GPU sürücüsünün özellikleri](./media/n-series-driver-setup/GPU_driver_properties.png)

GPU cihaz durumunu sorgulamak için aşağıdaki komutu çalıştırın [NVIDIA SMI](https://developer.nvidia.com/nvidia-system-management-interface) sürücü ile birlikte yüklenen komut satırı yardımcı programı.

1. Bir komut istemi açın ve değiştirmek **C:\Program Files\NVIDIA Corporation\NVSMI** dizin.

2. `nvidia-smi` öğesini çalıştırın. Sürücü yüklediyseniz aşağıdakine benzer bir çıktı görürsünüz. **GPU kullanımı** gösterir **%0** sürece sanal makinede bir GPU iş yükü şu anda çalışıyor. Sürücü sürümü ve GPU ayrıntıları gösterilen anlatılanlardan farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>RDMA ağ bağlantısı

Aynı kullanılabilirlik kümesinde veya sanal makine ölçek kümesindeki bir tek bir yerleştirme grubu içinde dağıtılan NC24r gibi RDMA özellikli N serisi vm'lerde RDMA ağ bağlantısı etkin hale getirilebilir. RDMA bağlantı sağlayan Windows ağ aygıt sürücülerini yüklemek için HpcVmDrivers uzantısı eklenmesi gerekir. RDMA özellikli bir N-serisi VM için VM uzantısı eklemek için [Azure PowerShell](/powershell/azure/overview) cmdlet'leri için Azure Resource Manager.

En son sürüm 1.1 yüklemek için HpcVMDrivers uzantısı mevcut RDMA özellikli VM'yi Batı ABD bölgesinde myVM adlı:
  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Daha fazla bilgi için [sanal makine uzantıları ve özellikleri Windows için](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

RDMA ağ ile çalışan uygulamalar için ileti geçirme arabirimi (MPI) trafiğini destekleyerek [Microsoft MPI](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) veya Intel MPI 5.x. 


## <a name="next-steps"></a>Sonraki adımlar

* NVIDIA Tesla GPU'ları için GPU hızlandırmalı uygulamalar oluşturan geliştiriciler de indirebilir ve en son yükleme [CUDA Araç Seti](https://developer.nvidia.com/cuda-downloads). Daha fazla bilgi için [CUDA Yükleme Kılavuzu](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


