---
title: "Linux için Azure N-serisi sürücü kurulumu | Microsoft Docs"
description: "Linux Azure'da çalışan N-serisi VM'ler için NVIDIA GPU sürücüleri ayarlama"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/09/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99989d57a0a9b9cc50d0fd9067e6cb9622764965
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>Linux çalıştıran N-serisi Vm'lerinde NVIDIA GPU sürücüleri yükleyin

Linux çalıştıran Azure N-serisi VM'ler GPU yeteneklerinden yararlanabilmek için desteklenen NVIDIA grafik sürücüleri yükleyin. N-serisi VM dağıttıktan sonra bu makalede sürücü kurulum adımlarını sağlar. Sürücü Kurulum bilgileri de için kullanılabilir [Windows VM'ler](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


N-serisi VM özellikleri, depolama kapasitesi ve disk Ayrıntılar için bkz: [GPU Linux VM boyutları](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a>NV VM'ler için kılavuz sürücüleri yükleyin

NV Vm'lerinde NVIDIA kılavuz sürücüleri yüklemek için her bir VM için bir SSH bağlantısı ve Linux dağıtımınız için adımları izleyin. 

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
  wget -O NVIDIA-Linux-x86_64-384.73-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-384.73-grid.run

  sudo ./NVIDIA-Linux-x86_64-384.73-grid.run
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


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>CentOS tabanlı 7.3 veya Red Hat Enterprise Linux 7.3

> [!IMPORTANT]
> Çalıştırmayan `sudo yum update` CentOS 7.3 veya Red Hat Enterprise Linux 7.3 Çekirdek sürümüne güncelleştirmek için. Çekirdek güncel değilse, sürücü yükleme ve güncelleştirmeleri şu anda çalışmıyor.
>

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
 
3. VM yeniden başlatma, yeniden bağlanma ve Hyper-V: için son Linux Tümleştirme hizmetlerini yükleyin
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.3.tar.gz

  tar xvzf lis-rpms-4.2.3.tar.gz

  cd LISISO

  sudo ./install.sh

  sudo reboot

  ```
 
4. VM ve Çalıştır yeniden `lspci` komutu. NVIDIA M60 kartı veya kartları PCI cihaz olarak görünür olduğunu doğrulayın.
 
5. Kılavuz sürücü yükleyip yeniden açın:

  ```bash
  wget -O NVIDIA-Linux-x86_64-384.73-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-384.73-grid.run

  sudo ./NVIDIA-Linux-x86_64-384.73-grid.run
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

Çıktı aşağıdakine benzer görünür. Sürücü sürümü ve GPU ayrıntıları gösterilen olanlardan farklı olabilir.

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


## <a name="install-cuda-drivers-for-nc-vms"></a>NC VM'ler için CUDA sürücüleri yükleyin

Linux NC VM'ler NVIDIA CUDA araç setinden NVIDIA sürücülerini yüklemek için adımlar şunlardır. 

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
  CUDA_REPO_PKG=cuda-9-0_9.0.176-1_amd64.deb

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

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>CentOS tabanlı 7.3 veya Red Hat Enterprise Linux 7.3

1. Hyper-V için en son Linux Tümleştirme hizmetlerini yükleyin.

  > [!IMPORTANT]
  > CentOS tabanlı HPC görüntü NC24r VM üzerinde yüklü değilse, adım 3'e geçin. Azure RDMA sürücüleri ve Linux Tümleştirme hizmetleri HPC görüntüde önceden yüklenmiş olduğundan, LIS yükseltilmez ve çekirdek güncelleştirmeler varsayılan olarak devre dışıdır.
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.3-1.tar.gz
 
  tar xvzf lis-rpms-4.2.3-1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. VM yeniden bağlanın ve aşağıdaki komutları yüklemeye devam edin:

  ```bash
  sudo yum install kernel-devel

  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

  sudo yum install dkms

  CUDA_REPO_PKG=cuda-repo-rhel7-9.0.176-1.x86_64.rpm

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

Aşağıdakine benzer bir çıktı görünür:

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi.png)



## <a name="rdma-network-for-nc24r-vms"></a>RDMA ağ NC24r VM'ler için

RDMA ağ bağlantısı NC24r aynı kullanılabilirlik kümesinde dağıtılan VM'ler üzerinde etkinleştirilebilir. RDMA ağ Intel MPI ile çalışan uygulamalar için ileti geçirme arabirimi (MPI) trafiğini destekler 5.x veya sonraki bir sürümü. Ek gereksinimler izleyin:

### <a name="distributions"></a>Dağıtımları

RDMA bağlantısı destekleyen Azure Marketi aşağıdaki görüntüleri birinden NC24r VM'ler dağıtın:
  
* **Ubuntu** -Ubuntu Server 16.04 LTS. RDMA sürücüleri VM yapılandırın ve Intel MPI indirmek için Intel ile kaydedin:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **CentOS tabanlı HPC** -CentOS tabanlı 7.3 HPC. RDMA sürücüler ve Intel MPI 5.1 VM yüklenir. 


## <a name="troubleshooting"></a>Sorun giderme

* CUDA sürücüleri 4.4.0-75 Linux çekirdek Ubuntu 16.04 LTS üzerinde çalışan Azure N-serisi vm'lerde bilinen bir sorun yoktur. Çekirdek bir sürümden yükseltme yapıyorsanız, en az yükseltme çekirdek sürüm 4.4.0-77.

* Kalıcılık modunu kullanarak ayarlayabilirsiniz `nvidia-smi` sorgu kartlar gerektiğinde komutunun çıkışını daha hızlı olacak şekilde. Kalıcılık modu ayarlamak için yürütme `nvidia-smi -pm 1`. VM yeniden başlatılırsa, modu ayarı kaybolur olduğunu unutmayın. Her zaman modu ayarı başlatma sırasında yürütülecek komut dosyası oluşturabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

* N-serisi VM'ler NVIDIA GPU hakkında daha fazla bilgi için bkz:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (için Azure NC VM'ler)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (için Azure NV VM'ler)

* Yüklü NVIDIA sürücülerinizi bir Linux VM görüntü yakalamak için bkz: [nasıl generalize ve Linux sanal makine yakalama](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
