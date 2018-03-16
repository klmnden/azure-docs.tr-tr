---
title: "Linux için Azure N-serisi sürücü kurulumu | Microsoft Docs"
description: "Linux Azure'da çalışan N-serisi VM'ler için NVIDIA GPU sürücüleri ayarlama"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/12/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7d353adcafed02832243277118da8480e54544ce
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>Linux çalıştıran N-serisi Vm'lerinde NVIDIA GPU sürücüleri yükleyin

Linux çalıştıran Azure N-serisi VM'ler GPU yeteneklerinden yararlanabilmek için desteklenen NVIDIA grafik sürücüleri yükleyin. N-serisi VM dağıttıktan sonra bu makalede sürücü kurulum adımlarını sağlar. Sürücü Kurulum bilgileri de için kullanılabilir [Windows VM'ler](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

N-serisi VM özellikleri, depolama kapasitesi ve disk Ayrıntılar için bkz: [GPU Linux VM boyutları](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-cuda-drivers-for-nc-ncv2-ncv3-and-nd-series-vms"></a>NC, NCv2, NCv3 ve ND-serisi VM'ler CUDA sürücüleri yükleyin

N-serisi vm'lerde NVIDIA CUDA araç setinden NVIDIA sürücülerini yüklemek için adımlar şunlardır. 

C ve C++ geliştiriciler GPU hızlandırılmış uygulamaları oluşturmak için tam Araç Seti isteğe bağlı olarak yükleyebilirsiniz. Daha fazla bilgi için bkz: [CUDA Yükleme Kılavuzu'na](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).

> [!NOTE]
> CUDA sürücü yükleme bağlantıları burada geçerli yayın zamanında sağlanır. En son CUDA sürücüleri için ziyaret [NVIDIA](https://developer.nvidia.com/cuda-zone) Web sitesi.
>

CUDA Araç Seti yüklemek için her VM için bir SSH bağlantısı oluşturun. Sistem CUDA özellikli GPU sahip olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

```bash
lspci | grep -i NVIDIA
```
(Bir NVIDIA Tesla K80 kartı gösteren) aşağıdaki örneğe benzer bir çıktı görürsünüz:

![lspci komut çıktısı](./media/n-series-driver-setup/lspci.png)

Sonra dağıtım için belirli çalışma yükleme komutları.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. CUDA sürücüleri yükleyip yeniden açın.
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_9.1.85-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub 

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  Yükleme birkaç dakika sürebilir.

2. İsteğe bağlı olarak tam CUDA Araç Seti yüklemek için şunu yazın:

  ```bash
  sudo apt-get install cuda
  ```

3. VM yeniden başlatma ve yüklendiğini doğrulamak için devam edin.

#### <a name="cuda-driver-updates"></a>CUDA sürücü güncelleştirmesi

CUDA sürücüleri düzenli aralıklarla dağıtımdan sonra güncelleştirmenizi öneririz.

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```

### <a name="centos-or-red-hat-enterprise-linux-73-or-74"></a>CentOS veya Red Hat Enterprise Linux 7.3 ya da 7.4

1. Çekirdek güncelleştirin.

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
 
3. VM yeniden bağlanın ve aşağıdaki komutları yüklemeye devam edin:

  ```bash
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

  sudo yum install dkms

  CUDA_REPO_PKG=cuda-repo-rhel7-9.1.85-1.x86_64.rpm

  wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

  sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo yum install cuda-drivers
  ```

  Yükleme birkaç dakika sürebilir. 

4. İsteğe bağlı olarak tam CUDA Araç Seti yüklemek için şunu yazın:

  ```bash
  sudo yum install cuda
  ```

5. VM yeniden başlatma ve yüklendiğini doğrulamak için devam edin.

### <a name="verify-driver-installation"></a>Sürücü yükleme doğrulayın

GPU cihaz durumu, SSH VM ve Çalıştır sorgulamak için [NVIDIA SMI](https://developer.nvidia.com/nvidia-system-management-interface) komut satırı yardımcı programının sürücüsüyle yüklü. 

Sürücü yüklüyse, aşağıdakine benzer bir çıktı görürsünüz. Unutmayın **GPU kul** GPU iş yükü VM çalıştırmakta olduğunuz sürece %0 gösterir. Sürücü sürümü ve GPU ayrıntıları gösterilen olanlardan farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi.png)

## <a name="rdma-network-connectivity"></a>RDMA ağ bağlantısı

Aynı kullanılabilirlik kümesinde veya VM ölçek kümesi NC24r dağıtılmış gibi RDMA ağ bağlantısı RDMA özellikli N-serisi Vm'lerinde etkinleştirilebilir. RDMA ağ Intel MPI ile çalışan uygulamalar için ileti geçirme arabirimi (MPI) trafiğini destekler 5.x veya sonraki bir sürümü. Ek gereksinimler izleyin:

### <a name="distributions"></a>Dağıtımlar

RDMA özellikli N-serisi VM'ler görüntüden RDMA bağlantısı N-serisi Vm'lerinde destekleyen Azure Marketi dağıtın:
  
* **Ubuntu 16.04 LTS** - VM RDMA sürücülerinin yapılandırmak ve Intel MPI indirmek için Intel ile kaydedin:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

> [!NOTE]
> CentOS tabanlı HPC görüntüler şu anda N-serisi vm'lerde RDMA bağlantısı için önerilmez. RDMA NVIDIA GPU destekleyen en son CentOS 7.4 çekirdek desteklenmiyor.
> 


## <a name="install-grid-drivers-for-nv-series-vms"></a>NV-serisi VM'ler için kılavuz sürücüleri yükleyin

NV-serisi Vm'lerinde NVIDIA kılavuz sürücüleri yüklemek için her bir VM için bir SSH bağlantısı ve Linux dağıtımınız için adımları izleyin. 

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Çalıştırma `lspci` komutu. NVIDIA M60 kartı veya kartları PCI cihaz olarak görünür olduğunu doğrulayın.

2. Güncelleştirmeleri yükleyin.

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. NVIDIA sürücüsüyle uyumsuz Nouveau çekirdek sürücüsü devre dışı bırakın. (Yalnızca NV Vm'lerinde NVIDIA sürücüsü kullanın.) Bunu yapmak için bir dosyada oluşturmak `/etc/modprobe.d `adlı `nouveau.conf` aşağıdaki içeriğe sahip:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. VM yeniden başlatın ve yeniden bağlayın. Çıkış X sunucusu:

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. Kılavuz sürücü yükleyip yeniden açın:

  ```bash
  wget -O NVIDIA-Linux-x86_64-384.111-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-384.111-grid.run

  sudo ./NVIDIA-Linux-x86_64-384.111-grid.run
  ``` 

6. Ne zaman sorulan X yapılandırma dosyanızı güncelleştirmek için seçin NVIDIA xconfig yardımcı programı çalıştırmak istediğinizi **Evet**.

7. Yükleme tamamlandıktan sonra yeni bir dosya gridd.conf adresindeki konum/etc/NVIDIA//etc/nvidia/gridd.conf.template kopyalayın

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. Aşağıdakileri ekleyin `/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. VM yeniden başlatma ve yüklendiğini doğrulamak için devam edin.


### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS veya Red Hat Enterprise Linux 

1. Çekirdek ve DKMS güncelleştirin.
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. NVIDIA sürücüsüyle uyumsuz Nouveau çekirdek sürücüsü devre dışı bırakın. (Yalnızca NV Vm'lerinde NVIDIA sürücüsü kullanın.) Bunu yapmak için bir dosyada oluşturmak `/etc/modprobe.d `adlı `nouveau.conf` aşağıdaki içeriğe sahip:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. VM yeniden başlatma, yeniden bağlayın ve en son yükleme [Linux Tümleştirme hizmetleri Hyper-V ve Azure](https://www.microsoft.com/download/details.aspx?id=55106).
 
  ```bash
  wget https://aka.ms/lis

  tar xvzf lis

  cd LISISO

  sudo ./install.sh

  sudo reboot

  ```
 
4. VM ve Çalıştır yeniden `lspci` komutu. NVIDIA M60 kartı veya kartları PCI cihaz olarak görünür olduğunu doğrulayın.
 
5. Kılavuz sürücü yükleyip yeniden açın:

  ```bash
  wget -O NVIDIA-Linux-x86_64-384.111-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-384.111-grid.run

  sudo ./NVIDIA-Linux-x86_64-384.111-grid.run
  ``` 
6. Ne zaman sorulan X yapılandırma dosyanızı güncelleştirmek için seçin NVIDIA xconfig yardımcı programı çalıştırmak istediğinizi **Evet**.

7. Yükleme tamamlandıktan sonra yeni bir dosya gridd.conf adresindeki konum/etc/NVIDIA//etc/nvidia/gridd.conf.template kopyalayın
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. Aşağıdakileri ekleyin `/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. VM yeniden başlatma ve yüklendiğini doğrulamak için devam edin.

### <a name="verify-driver-installation"></a>Sürücü yükleme doğrulayın


GPU cihaz durumu, SSH VM ve Çalıştır sorgulamak için [NVIDIA SMI](https://developer.nvidia.com/nvidia-system-management-interface) komut satırı yardımcı programının sürücüsüyle yüklü. 

Sürücü yüklüyse, aşağıdakine benzer bir çıktı görürsünüz. Unutmayın **GPU kul** GPU iş yükü VM çalıştırmakta olduğunuz sürece %0 gösterir. Sürücü sürümü ve GPU ayrıntıları gösterilen olanlardan farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 sunucu
Bir X11 gerekiyorsa NV VM'ye uzak bağlantılar için sunucu [x11vnc](http://www.karlrunge.com/x11vnc/) donanım hızlandırmasını grafik izin verdiği için önerilir. M60 aygıt BusID xconfig dosyasına el ile eklenmesi gerekir (`etc/X11/xorg.conf` Ubuntu 16.04 LTS üzerinde `/etc/X11/XF86config` CentOS 7.3 veya Red Hat Enterprise Server 7.3). Ekleme bir `"Device"` bölüm aşağıdakine benzer:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
Ayrıca, güncelleştirme, `"Screen"` bu cihazı kullanmak için bölümü.
 
BusID çalıştırarak bulunabilir.

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
Bir VM bırakılan veya yeniden BusID değiştirebilirsiniz. Bu nedenle, X11 içinde BusID güncelleştirmek için bir komut dosyası kullanmayı düşünebilirsiniz VM yeniden başlatıldığında yapılandırma. Örneğin:

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed to ${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

Bu dosya önyüklemede kök olarak içinde için bir giriş oluşturarak çağrılabilir `/etc/rc.d/rc3.d`.

## <a name="troubleshooting"></a>Sorun giderme

* Kalıcılık modunu kullanarak ayarlayabilirsiniz `nvidia-smi` sorgu kartlar gerektiğinde komutunun çıkışını daha hızlı olacak şekilde. Kalıcılık modu ayarlamak için yürütme `nvidia-smi -pm 1`. VM yeniden başlatılırsa, modu ayarı kaybolduktan olduğunu unutmayın. Her zaman modu ayarı başlatma sırasında yürütülecek komut dosyası oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Yüklü NVIDIA sürücülerinizi bir Linux VM görüntü yakalamak için bkz: [nasıl generalize ve Linux sanal makine yakalama](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
