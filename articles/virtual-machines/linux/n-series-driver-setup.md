---
title: Linux için Azure N serisi GPU sürücü kurulumu | Microsoft Docs
description: Azure'da Linux çalıştıran N serisi VM'ler için NVIDIA GPU sürücülerini ayarlama
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c80b821d6bd0263473ba0178eea148f7a2d5773
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123956"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>Linux çalıştıran N serisi Vm'lerde NVIDIA GPU sürücüleri yükleyin

Linux çalıştıran Azure N serisi sanal makineler, GPU özelliklerine yararlanmak için NVIDIA GPU sürücülerine yüklenmesi gerekir. [NVIDIA GPU sürücüsünün uzantısı](../extensions/hpccompute-gpu-linux.md) bir N serisi sanal makinesinde uygun NVIDIA CUDA veya kılavuz sürücüleri yükler. Yükleme veya uzantısını Azure portalı veya Azure CLI veya Azure Resource Manager şablonları gibi araçları kullanarak yönetin. Bkz: [NVIDIA GPU sürücüsünün uzantı belgesini](../extensions/hpccompute-gpu-linux.md) desteklenen dağıtımları ve dağıtım adımları için.

GPU sürücüleri el ile yüklemeyi seçerseniz, bu makalede, desteklenen dağıtımlar ve sürücüler yükleme ve doğrulama adımlarını sağlar. El ile sürücü kurulum bilgileri, ayrıca kullanılabilir [Windows Vm'leri](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

N serisi sanal makine özellikleri, depolama kapasitesi ve disk ayrıntıları için bkz: [GPU Linux VM boyutları](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-cuda-drivers-on-n-series-vms"></a>N serisi Vm'lerde CUDA sürücüleri yükleyin

N serisi vm'lerde NVIDIA CUDA setinden CUDA sürücülerini yüklemek için gereken adımlar aşağıda verilmiştir. 


C ve C++ geliştiricileri, GPU hızlandırmalı uygulamalar oluşturmak için tam Araç Seti isteğe bağlı olarak yükleyebilirsiniz. Daha fazla bilgi için [CUDA Yükleme Kılavuzu](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).

CUDA sürücüleri yüklemek için her VM için bir SSH bağlantısı olun. Sistem CUDA özellikli GPU olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

```bash
lspci | grep -i NVIDIA
```
(Bir NVIDIA Tesla K80 kartını gösteriliyor) aşağıdaki örneğe benzer bir çıktı görürsünüz:

![lspci komut çıktısı](./media/n-series-driver-setup/lspci.png)

Ardından, dağıtım için belirli çalışma yükleme komutları.

### <a name="ubuntu"></a>Ubuntu 

1. Sürücüleri indirin ve CUDA NVIDIA Web sitesinden yükleyin. Ubuntu 16.04 LTS için örneğin:
   ```bash
   CUDA_REPO_PKG=cuda-repo-ubuntu1604_10.0.130-1_amd64.deb

   wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

   sudo dpkg -i /tmp/${CUDA_REPO_PKG}

   sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub 

   rm -f /tmp/${CUDA_REPO_PKG}

   sudo apt-get update

   sudo apt-get install cuda-drivers

   ```

   Yükleme birkaç dakika sürebilir.

2. İsteğe bağlı olarak tam CUDA Araç Seti'ni yüklemek için şunu yazın:

   ```bash
   sudo apt-get install cuda
   ```

3. VM'yi yeniden başlatın ve yüklemeyi doğrulamak için devam edin.

#### <a name="cuda-driver-updates"></a>CUDA sürücü güncelleştirmeleri

CUDA sürücüleri düzenli aralıklarla dağıtımdan sonra güncelleştirmenizi öneririz.

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```

### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS veya Red Hat Enterprise Linux

1. (Önerilen) çekirdek güncelleştirin. Çekirdek güncelleştirmeyi tercih ederseniz sürümleri emin `kernel-devel` ve `dkms` , çekirdek için uygundur.

   ```
   sudo yum install kernel kernel-tools kernel-headers kernel-devel
  
   sudo reboot

2. Install the latest [Linux Integration Services for Hyper-V and Azure](https://www.microsoft.com/download/details.aspx?id=55106).

   ```bash
   wget https://aka.ms/lis
 
   tar xvzf lis
 
   cd LISISO
 
   sudo ./install.sh
 
   sudo reboot
   ```
 
3. VM yeniden ve aşağıdaki komutları yüklemeye devam edin:

   ```bash
   sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

   sudo yum install dkms

   CUDA_REPO_PKG=cuda-repo-rhel7-10.0.130-1.x86_64.rpm

   wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

   sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

   rm -f /tmp/${CUDA_REPO_PKG}

   sudo yum install cuda-drivers
   ```

   Yükleme birkaç dakika sürebilir. 

4. İsteğe bağlı olarak tam CUDA Araç Seti'ni yüklemek için şunu yazın:

   ```bash
   sudo yum install cuda
   ```

5. VM'yi yeniden başlatın ve yüklemeyi doğrulamak için devam edin.

### <a name="verify-driver-installation"></a>Sürücü yüklemesi doğrulayın

GPU cihaz durumu SSH çalıştırma ve VM için Sorgulanacak [NVIDIA SMI](https://developer.nvidia.com/nvidia-system-management-interface) sürücü ile birlikte yüklenen komut satırı yardımcı programı. 

Sürücü yüklediyseniz aşağıdakine benzer bir çıktı görürsünüz. Unutmayın **GPU kullanımı** VM üzerinde şu anda bir GPU iş yükü çalıştırmıyorsanız, %0 gösterir. Sürücü sürümü ve GPU ayrıntıları gösterilen anlatılanlardan farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi.png)

## <a name="rdma-network-connectivity"></a>RDMA ağ bağlantısı

Aynı kullanılabilirlik kümesine veya VM ölçek kümesindeki bir tek bir yerleştirme grubu NC24r dağıtılmış gibi RDMA özellikli N serisi Vm'lerde RDMA ağ bağlantısı etkin hale getirilebilir. RDMA ağ Intel MPI ile çalışan uygulamalar için ileti geçirme arabirimi (MPI) trafiğini destekleyerek 5.x veya sonraki bir sürümü. Ek gereksinimler izleyin:

### <a name="distributions"></a>Dağıtımlar

RDMA özellikli N serisi sanal makineler N serisi Vm'lerde RDMA bağlantısı destekleyen Azure Marketi'nde görüntülerden birini dağıtın:
  
* **Ubuntu 16.04 LTS** - VM RDMA sürücülerinin yapılandırmak ve Intel MPI indirmek için Intel ile kaydedin:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **CentOS tabanlı 7.4 HPC** -RDMA sürücüleri ve Intel MPI 5.1 sanal makinede yüklü.

## <a name="install-grid-drivers-on-nv-or-nvv2-series-vms"></a>NV veya NVv2 serisi VM'ler GRID sürücüleri yükleyin

NV veya NVv2 serisi VM'ler, NVIDIA GRID sürücüleri yüklemek için her VM için bir SSH bağlantısı ve Linux dağıtımınız için adımları izleyin. 

### <a name="ubuntu"></a>Ubuntu 

1. `lspci` komutunu çalıştırın. NVIDIA M60 kartın veya kartları PCI cihazlar olarak görünür olduğundan emin olun.

2. Güncelleştirmeleri yükleyin.

   ```bash
   sudo apt-get update

   sudo apt-get upgrade -y

   sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. NVIDIA sürücüsü ile uyumsuz Nouveau çekirdek sürücüsü devre dışı bırakın. (Yalnızca NVIDIA sürücüsü NV veya NVv2 Vm'lerinde kullanın.) Bunu yapmak için bir dosyada oluşturmak `/etc/modprobe.d` adlı `nouveau.conf` aşağıdaki içeriklerle:

   ```
   blacklist nouveau

   blacklist lbm-nouveau
   ```


4. VM'yi yeniden başlatın ve yeniden bağlanın. Çıkış X sunucusu:

   ```bash
   sudo systemctl stop lightdm.service
   ```

5. İndirin ve kılavuz sürücüsünü yükleyin:

   ```bash
   wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=874272  

   chmod +x NVIDIA-Linux-x86_64-grid.run

   sudo ./NVIDIA-Linux-x86_64-grid.run
   ``` 

6. Ne zaman sorulan X yapılandırma dosyanızı güncelleştirin, seçmek için NVIDIA xconfig yardımcı programını çalıştırmak isteyip istemediğinizi **Evet**.

7. Yükleme tamamlandıktan sonra yeni bir dosya gridd.conf adresindeki konum/etc/NVIDIA//etc/nvidia/gridd.conf.template kopyalayın

   ```bash
   sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
   ```

8. Ekleyin `/etc/nvidia/gridd.conf`:
 
   ```
   IgnoreSP=FALSE
   ```
9. VM'yi yeniden başlatın ve yüklemeyi doğrulamak için devam edin.


### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS veya Red Hat Enterprise Linux 

1. (Önerilen) DKMS ve çekirdek güncelleştirin. Çekirdek güncelleştirmeyi tercih ederseniz sürümleri emin `kernel-devel` ve `dkms` , çekirdek için uygundur.
 
   ```bash  
   sudo yum update
 
   sudo yum install kernel-devel
 
   sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
   sudo yum install dkms
   ```

2. NVIDIA sürücüsü ile uyumsuz Nouveau çekirdek sürücüsü devre dışı bırakın. (Yalnızca NVIDIA sürücüsü NV veya NV2 Vm'lerinde kullanın.) Bunu yapmak için bir dosyada oluşturmak `/etc/modprobe.d` adlı `nouveau.conf` aşağıdaki içeriklerle:

   ```
   blacklist nouveau

   blacklist lbm-nouveau
   ```
 
3. En yeni VM'yi yeniden başlatın ve yeniden [Linux Tümleştirme hizmetleri Hyper-V ve Azure](https://www.microsoft.com/download/details.aspx?id=55106).
 
   ```bash
   wget https://aka.ms/lis

   tar xvzf lis

   cd LISISO

   sudo ./install.sh

   sudo reboot

   ```
 
4. Çalıştırma ve VM yeniden `lspci` komutu. NVIDIA M60 kartın veya kartları PCI cihazlar olarak görünür olduğundan emin olun.
 
5. İndirin ve kılavuz sürücüsünü yükleyin:

   ```bash
   wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=874272  

   chmod +x NVIDIA-Linux-x86_64-grid.run

   sudo ./NVIDIA-Linux-x86_64-grid.run
   ``` 
6. Ne zaman sorulan X yapılandırma dosyanızı güncelleştirin, seçmek için NVIDIA xconfig yardımcı programını çalıştırmak isteyip istemediğinizi **Evet**.

7. Yükleme tamamlandıktan sonra yeni bir dosya gridd.conf adresindeki konum/etc/NVIDIA//etc/nvidia/gridd.conf.template kopyalayın
  
   ```bash
   sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
   ```
  
8. Ekleyin `/etc/nvidia/gridd.conf`:
 
   ```
   IgnoreSP=FALSE
   ```
9. VM'yi yeniden başlatın ve yüklemeyi doğrulamak için devam edin.

### <a name="verify-driver-installation"></a>Sürücü yüklemesi doğrulayın


GPU cihaz durumu SSH çalıştırma ve VM için Sorgulanacak [NVIDIA SMI](https://developer.nvidia.com/nvidia-system-management-interface) sürücü ile birlikte yüklenen komut satırı yardımcı programı. 

Sürücü yüklediyseniz aşağıdakine benzer bir çıktı görürsünüz. Unutmayın **GPU kullanımı** VM üzerinde şu anda bir GPU iş yükü çalıştırmıyorsanız, %0 gösterir. Sürücü sürümü ve GPU ayrıntıları gösterilen anlatılanlardan farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 sunucusu
Bir X11 gerekiyorsa bir NV veya NVv2 VM uzak bağlantılar için sunucu [x11vnc](http://www.karlrunge.com/x11vnc/) donanım grafik ivmesini izin verdiğinden önerilir. M60 cihazın BusID X11 için el ile eklenmelidir yapılandırma dosyası (genellikle `etc/X11/xorg.conf`). Ekleme bir `"Device"` bölüm aşağıdakine benzer:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "PCI:0@your-BusID:0:0"
EndSection
```
 
Ayrıca, güncelleştirme, `"Screen"` bu cihazı kullanmak için bölümü.
 
Ondalık çalıştırarak BusID bulunabilir

```bash
nvidia-xconfig --query-gpu-info | awk '/PCI BusID/{print $4}'
```
 
Bir VM bırakılan veya yeniden BusID değiştirebilirsiniz. Bu nedenle, BusID X11 içinde güncelleştirmek için bir betik oluşturmak isteyebilirsiniz VM yeniden başlatıldığında yapılandırma. Örneğin, adlı bir komut dosyası oluşturma `busidupdate.sh` (veya seçtiğiniz başka bir ad) ile içeriği aşağıdakine benzer:

```bash 
#!/bin/bash
XCONFIG="/etc/X11/xorg.conf"
OLDBUSID=`awk '/BusID/{gsub(/"/, "", $2); print $2}' ${XCONFIG}`
NEWBUSID=`nvidia-xconfig --query-gpu-info | awk '/PCI BusID/{print $4}'`

if [[ "${OLDBUSID}" == "${NEWBUSID}" ]] ; then
        echo "NVIDIA BUSID not changed - nothing to do"
else
        echo "NVIDIA BUSID changed from \"${OLDBUSID}\" to \"${NEWBUSID}\": Updating ${XCONFIG}" 
        sed -e 's|BusID.*|BusID          '\"${NEWBUSID}\"'|' -i ${XCONFIG}
fi
```

Ardından güncelleştirme betiği için bir giriş oluşturun `/etc/rc.d/rc3.d` için betik önyüklemede kök olarak çağrılır.

## <a name="troubleshooting"></a>Sorun giderme

* Kalıcılık modunu kullanarak ayarlayabilirsiniz `nvidia-smi` sorgu kartlarını gerektiğinde komutunun çıkışı daha hızlı olacak şekilde. Kalıcılık modu ayarlamak için yürütme `nvidia-smi -pm 1`. VM yeniden başlatılırsa, modu ayarını kaybolduktan olduğunu unutmayın. Her zaman modu ayarını başlatma sırasında çalıştırılacak betik oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* NVIDIA sürücülerinizi yüklü bir Linux VM görüntüsü yakalamak için bkz: [Genelleştir ve Linux sanal makinesi yakalama nasıl](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
