---
title: Azure Linux VM'de DPDK | Microsoft Docs
description: Bir Linux sanal makinesinde DPDK ayarlamayı öğrenin.
services: virtual-network
documentationcenter: na
author: laxmanrb
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2018
ms.author: labattul
ms.openlocfilehash: c5cb840035c5d0d5694982324c7237c58001e689
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60731609"
---
# <a name="set-up-dpdk-in-a-linux-virtual-machine"></a>Bir Linux sanal makinesinde DPDK ayarlayın

Azure'da veri düzlemi Geliştirme Seti (DPDK) performans bakımından yoğun uygulamalar için daha hızlı bir kullanıcı alanı paket işleme çerçevesi sağlar. Bu çerçeve, sanal makinenin çekirdek ağ yığınını atlar.

Çekirdek Ağ yığını kullanması tipik paket işlemede, kesme temelli işlemidir. Ağ arabirimine gelen paketleri aldığında, bir çekirdek kesme çekirdek alanından, paket ve bağlamı, kullanıcı alanı alanıma geçiş yap işlem yoktur. DPDK içerik geçişini ve kesme temelli yöntemi yerine bir kullanıcı alanı uygulaması kullanan modu sürücüleri hızlı paket işleme için yoklama ortadan kaldırır.

Alt düzey kaynaklarına erişim sağlama kullanıcı alanı kitaplıklar kümesi DPDK oluşur. Bu kaynaklar, donanım, mantıksal çekirdekler, bellek yönetimi ve ağ arabirim kartları için yoklama modu sürücüleri içerebilir.

DPDK birden çok işletim sistemi dağıtımlarını destekleyen Azure sanal makinelerinde çalıştırabilirsiniz. DPDK ağ işlevi sanallaştırma uygulamalarına yönlendiren, ana performans ayrım sağlar. Bu uygulamalar, ağ sanal Gereçleri (Nva) biçiminde sanal yönlendiriciler, güvenlik duvarları, VPN'ler, yük Dengeleyiciler, sistem gereksinimleri paket çekirdek ve hizmet reddi (DDoS) uygulamaları gibi alabilir.

## <a name="benefit"></a>Avantaj

**Daha yüksek Saniyedeki (PPS)**: Kullanıcı alanı paketlerin alma denetimi ve çekirdek atlama, bağlam anahtarları ortadan kaldırarak döngü sayısı azaltır. Ayrıca, Azure Linux sanal makineler'de saniye başına işlenen paket oranını artırır.


## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Azure Galerisi aşağıdaki dağıtımlar desteklenir:

| Linux İşletim Sistemi     | Çekirdek sürümü        |
|--------------|----------------       |
| Ubuntu 16.04 | 4.15.0-1015-azure     |
| Ubuntu 18.04 | 4.15.0-1015-azure     |
| SLES 15      | 4.12.14-5.5-azure     |
| RHEL 7.5     | 3.10.0-862.9.1.el7    |
| CentOS 7.5   | 3.10.0-862.3.3.el7    |

**Özel çekirdek desteği**

Listede olmayan tüm Linux çekirdek sürümü için bkz: [Azure olarak ayarlanmış bir Linux çekirdek oluşturmaya yönelik düzeltme ekleri](https://github.com/microsoft/azure-linux-kernel). Daha fazla bilgi için de başvurabilirsiniz [ azuredpdk@microsoft.com ](mailto:azuredpdk@microsoft.com). 

## <a name="region-support"></a>Bölge desteği

Tüm Azure bölgelerinde DPDK destekler.

## <a name="prerequisites"></a>Önkoşullar

Bir Linux sanal makinesinde hızlandırılmış ağ etkin olmalıdır. Sanal Makine Yönetimi için bir arabirim ile en az iki ağ arabirimi olması gerekir. Bilgi edinmek için nasıl [hızlandırılmış ağ etkin olan bir Linux sanal makinesi oluşturma](create-vm-accelerated-networking-cli.md).

## <a name="install-dpdk-dependencies"></a>DPDK bağımlılıklarını yükleyin

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="rhel75centos-75"></a>RHEL7.5/CentOS 7.5

```bash
yum -y groupinstall "Infiniband Support"
sudo dracut --add-drivers "mlx4_en mlx4_ib mlx5_ib" -f
yum install -y gcc kernel-devel-`uname -r` numactl-devel.x86_64 librdmacm-devel libmnl-devel
```

### <a name="sles-15"></a>SLES 15

**Azure kernel**

```bash
zypper  \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-azure kernel-devel-azure gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

**Varsayılan çekirdek**

```bash
zypper \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-default-devel gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

## <a name="set-up-the-virtual-machine-environment-once"></a>(Bir kez) sanal makine ortamı ayarlama

1. [En son DPDK indirme](https://core.dpdk.org/download). Azure için 18.02 veya üzeri bir sürüm gereklidir.
2. Varsayılan yapılandırma ile derleme `make config T=x86_64-native-linuxapp-gcc`.
3. Mellanox PMDs ile oluşturulan yapılandırmasını etkinleştir `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config`.
4. Derleme `make`.
5. İle yükleme `make install DESTDIR=<output folder>`.

## <a name="configure-the-runtime-environment"></a>Çalışma zamanı ortamı yapılandırma

Yeniden başlattıktan sonra aşağıdaki komutları bir kez çalıştırın:

1. Hugepages

   * Tüm numanodes için bir kez aşağıdaki komutu çalıştırarak hugepage yapılandırın:

     ```bash
     echo 1024 | sudo tee
     /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   * İle bağlama için bir dizin oluşturma `mkdir /mnt/huge`.
   * Bağlama hugepages ile `mount -t hugetlbfs nodev /mnt/huge`.
   * Hugepages ile ayrılmıştır işaretleyin `grep Huge /proc/meminfo`.

     > [!NOTE]
     > Bir yolu takip ederek önyükleme ayrılmış hugepages grub dosya değiştirin [yönergeleri](https://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) DPDK için. Sayfanın altındaki yönergeleri verilmiştir. Bir Azure Linux sanal makinesi kullanırken, dosyaları altındaki değiştirme **/etc/config/grub.d** bunun yerine, yeniden başlatmalar arasında hugepages ayırmak için.

2. MAC ve IP adresleri: Kullanım `ifconfig –a` MAC ve IP adresi ağ arabirimlerinin görüntüleyebilirsiniz. *VF* ağ arabirimi ve *NETVSC* ağ arabirimine sahip olmalı, ancak aynı MAC adresini *NETVSC* ağ arabirimi bir IP adresi vardır. VF arabirimleri NETVSC arabirimleri bağımlı arabirimleri olarak çalışıyor.

3. PCI adresleri

   * Kullanma `ethtool -i <vf interface name>` kullanılmak üzere hangi PCI adresini bulmak için *VF*.
   * Varsa *eth0* accelerated networking etkin bu testpmd değil yanlışlıkla için VF PCI cihaz üzerinde dikkate aldığınızdan emin olun *eth0*. DPDK uygulama yanlışlıkla yönetim ağ arabirimi üzerinden alır ve SSH bağlantınızı kaybetmenize neden olur, seri konsol DPDK uygulamayı durdurmak için kullanın. Seri konsol, sanal makineyi başlatmak veya durdurmak için de kullanabilirsiniz.

4. Yük *ibuverbs* ile her başlatmada `modprobe -a ib_uverbs`. Yalnızca SLES 15 için aynı zamanda yük *mlx4_ib* ile `modprobe -a mlx4_ib`.

## <a name="failsafe-pmd"></a>Hatasız PMD

Azure'da kullanıma sunulan PMD hatasız üzerinden DPDK uygulamaları çalıştırmanız gerekir. Uygulamayı doğrudan VF PMD üzerinde çalışıyorsa, almaz **tüm** yapay arabirimi üzerinde bazı paketler görünmesini olduğundan VM gidecek paketler. 

DPDK uygulama hatasız PMD çalıştırırsanız, uygulama için hedeflenen tüm paketleri alır garanti eder. Ana bilgisayarın bakımda olduğu zaman VF iptal edilir olsa da uygulama DPDK modunda çalışmaya devam eder emin olur. Hatasız PMD hakkında daha fazla bilgi için bkz: [emniyet yoklama modu sürücüsü kitaplığı](https://doc.dpdk.org/guides/nics/fail_safe.html).

## <a name="run-testpmd"></a>Testpmd çalıştırın

Testpmd kök modunda çalıştırmak için kullanın `sudo` önce *testpmd* komutu.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Temel: Sağlamlık onay, hatasız bağdaştırıcısı başlatma

1. Bir tek bağlantı noktası testpmd uygulamayı başlatmak için aşağıdaki komutları çalıştırın:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -- -i \
     --port-topology=chained
    ```

2. Bir çift bağlantı noktası testpmd uygulamayı başlatmak için aşağıdaki komutları çalıştırın:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -- -i
   ```

   İkiden fazla NIC ile testpmd çalıştırıyorsanız `--vdev` bağımsız değişkeni, bu deseni izler: `net_vdev_netvsc<id>,iface=<vf’s pairing eth>`.

3.  Başlatıldıktan sonra Çalıştır `show port info all` bağlantı noktası bilgileri denetlenecek. Net_failsafe bir veya iki DPDK bağlantı noktalarını görmeniz gerekir (değil *net_mlx4*).
4.  Kullanım `start <port> /stop <port>` trafiği başlatmak için.

Yukarıdaki komutları Başlat *testpmd* etkileşimli modda testpmd komutları denediğiniz için önerilir.

### <a name="basic-single-sendersingle-receiver"></a>Temel: Tek gönderen/tek alıcısı

Aşağıdaki komutları düzenli aralıklarla paket başına ikinci istatistikleri yazdırın:

1. TX tarafında aşağıdaki komutu çalıştırın:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
   ```

2. RX tarafında aşağıdaki komutu çalıştırın:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=rxonly \
     --eth-peer=<port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
   ```

Yukarıdaki komutları bir sanal makine üzerinde çalıştırırken, değiştirme *IP_SRC_ADDR* ve *IP_DST_ADDR* içinde `app/test-pmd/txonly.c` derlemeden önce sanal makinelerin gerçek IP adresini eşleştirmek için. Aksi takdirde, alıcı ulaşmadan önce paketler bırakılır.

### <a name="advanced-single-sendersingle-forwarder"></a>Gelişmiş: Tek gönderen/tek ileticisi
Aşağıdaki komutları düzenli aralıklarla paket başına ikinci istatistikleri yazdırın:

1. TX tarafında aşağıdaki komutu çalıştırın:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
    ```

2. İleri Sar tarafında aşağıdaki komutu çalıştırın:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address NIC1> \
     -w <pci address NIC2> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     --vdev="net_vdev_netvsc<2nd id>,iface=<2nd iface to attach to>" (you need as many --vdev arguments as the number of devices used by testpmd, in this case) \
     -- --nb-cores <number of cores to use for test pmd> \
     --forward-mode=io \
     --eth-peer=<recv port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
    ```

Yukarıdaki komutları bir sanal makine üzerinde çalıştırırken, değiştirme *IP_SRC_ADDR* ve *IP_DST_ADDR* içinde `app/test-pmd/txonly.c` derlemeden önce sanal makinelerin gerçek IP adresini eşleştirmek için. Aksi takdirde, iletici ulaşmadan önce paketler bırakılır. Bir üçüncü makine iletilen trafiği almak için sahip mümkün olmayacaktır *testpmd* bazı kod değişiklikleri yapmanızı sürece, iletici Katman 3 adreslerini değiştirmek değil.

## <a name="references"></a>Başvurular

* [Deniz Mavisi seçenekleri](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options)
* [Testpmd komutları](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options)
