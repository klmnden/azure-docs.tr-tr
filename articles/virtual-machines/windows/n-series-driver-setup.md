---
title: Windows için Azure N-serisi sürücü kurulumu | Microsoft Docs
description: Windows Server veya Windows Azure'da çalışan N-serisi VM'ler için NVIDIA GPU sürücüleri ayarlama
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/29/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bc4b9cb9940f073034df01143f4d9e77a47cb19b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654394"
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows"></a>GPU sürücüleri Windows çalıştıran N-serisi VM'ler için ayarlama 
Windows Server veya Windows desteklenen bir sürümünü çalıştıran Azure N-serisi VM'ler GPU yeteneklerinden yararlanabilmek için NVIDIA grafik sürücüleri yüklenmesi gerekir. N-serisi VM dağıttıktan sonra bu makalede sürücü kurulum adımlarını sağlar. Sürücü Kurulum bilgileri de için kullanılabilir [Linux VM'ler](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Temel özellikleri, depolama kapasitesi ve disk Ayrıntılar için bkz: [GPU Windows VM boyutları](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>Sürücü yükleme

1. Uzak Masaüstü'nü her N-serisi VM tarafından bağlayın.

2. İndirin, ayıklayın ve Windows işletim sistemi için desteklenen bir sürücü yükleyin.

Azure NV Vm'lerinde sürücü yüklendikten sonra bir yeniden başlatma gereklidir. NC VM'ler üzerinde bir yeniden başlatma gerekli değildir.

## <a name="verify-driver-installation"></a>Sürücü yükleme doğrulayın

Sürücü yükleme Aygıt Yöneticisi'nde doğrulayabilirsiniz. Aşağıdaki örnek, bir Azure NC VM Tesla K80 kartı başarılı yapılandırması gösterilmektedir.

![GPU sürücüsünün özellikleri](./media/n-series-driver-setup/GPU_driver_properties.png)

GPU aygıt durumunu sorgulamak için aşağıdaki komutu çalıştırın [NVIDIA SMI](https://developer.nvidia.com/nvidia-system-management-interface) komut satırı yardımcı programının sürücüsüyle yüklü.

1. Bir komut istemi açın ve değiştirmek **C:\Program Files\NVIDIA Corporation\NVSMI** dizini.

2. `nvidia-smi` öğesini çalıştırın. Sürücü yüklüyse, aşağıdakine benzer bir çıktı göreceksiniz. Unutmayın **GPU kul** gösterir **%0** GPU iş yükü VM çalıştırmakta olduğunuz sürece. Sürücü sürümü ve GPU ayrıntıları gösterilen olanlardan farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>RDMA ağ bağlantısı

NC24r aynı kullanılabilirlik kümesi içinde veya VM ölçek kümesi tek yerleştirme grubunda dağıtılan gibi RDMA ağ bağlantısı RDMA özellikli N-serisi Vm'lerinde etkinleştirilebilir. RDMA bağlantısını etkinleştirmek Windows ağ aygıt sürücülerini yüklemek için HpcVmDrivers uzantısı eklenmesi gerekir. RDMA özellikli N-serisi VM için VM uzantısı eklemek için kullanın [Azure PowerShell](/powershell/azure/overview) cmdlet'leri Azure Resource Manager.

En son sürüm 1.1 yüklemek için mevcut bir RDMA özellikli VM'yi HpcVMDrivers uzantısı Batı ABD bölgesi, myVM adlı:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Daha fazla bilgi için bkz: [sanal makine uzantıları ve özellikleri Windows için](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

RDMA ağ ile çalışan uygulamalar için ileti geçirme arabirimi (MPI) trafiğini destekler [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) veya Intel MPI 5.x. 


## <a name="next-steps"></a>Sonraki adımlar

* NVIDIA Tesla GPU GPU hızlandırılmış uygulamaları oluşturan geliştiriciler, ayrıca karşıdan yükle ve en son yükleme [CUDA Araç Seti](https://developer.nvidia.com/cuda-downloads). Daha fazla bilgi için bkz: [CUDA Yükleme Kılavuzu'na](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


