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
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2018
ms.author: labattul
ms.openlocfilehash: d09d2bf7d14508b0b8eac955eb2589c0e7384903
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506517"
---
# <a name="setup-dpdk-in-a-linux-virtual-machine"></a>Bir Linux sanal makinesinde DPDK Kurulumu

Veri düzlemi Geliştirme Seti (DPDK) Azure üzerinde bir Hızlı Kullanıcı alanı paket işleme çerçevesi sanal makinenin çekirdek ağ yığınını atlayan yoğun performans gerektiren uygulamalar için sunar.

Tipik paket Çekirdek Ağ yığını kullanarak işleme temelli kesme ' dir. Gelen paketleri, ağ arabirimini alır her zaman çekirdek alanı paket ve bağlam anahtardan kullanıcı alanına işlemek için bir çekirdek kesme yoktur. DPDK içerik geçişini ve yoklama modu sürücüler için hızlı paket işleme kullanan bir kullanıcı alanı uygulaması yerine yöntemi temelli kesme ortadan kaldırır.

Alt düzey donanım, mantıksal çekirdekler, bellek yönetimi gibi kaynakları ve ağ arabirim kartları için modu sürücüleri yoklamak için erişim sağlayan kullanıcı alanı kitaplıkları kümesinin DPDK oluşur.

DPDK birden çok işletim sistemi dağıtımlarını destekleyen Azure sanal makinelerinde çalıştırabilirsiniz. DPDK, bir sanal yönlendirici, güvenlik duvarı, VPN, yük dengeleyici, sistem gereksinimleri paket çekirdek ve hizmet reddi () gibi ağ sanal Gereçleri (NVA) biçiminde sanallaştırma uygulamaları sürüş ağ işlevi içinde bir ana performans ayrıştırması yapılandırmayı sağlar. DDoS) uygulamaları.

## <a name="benefit"></a>Avantaj

**Daha yüksek Saniyedeki (PPS)**: kullanıcı alanı paketlerin alma denetimi ve çekirdek atlama bağlam ortadan kaldırarak döngüsü sayısını azaltır ve Azure Linux sanal makineler'de saniye başına işlenen paketleri oranını artırır.


## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Azure Galerisi aşağıdaki dağıtımlar desteklenir:

| Linux İşletim Sistemi     | Çekirdek sürümü        |
|--------------|----------------       |
| Ubuntu 16.04 | 4.15.0-1015-Azure     |
| Ubuntu 18.04 | 4.15.0-1015-Azure     |
| SLES 15      | 4.12.14-5.5-Azure     |
| RHEL 7.5     | 3.10.0-862.9.1.el7    |
| CentOS 7.5   | 3.10.0-862.3.3.el7    |

**Özel çekirdek desteği**

Başvurmak [Azure olarak ayarlanmış bir Linux çekirdek oluşturmaya yönelik düzeltme ekleri](https://github.com/microsoft/azure-linux-kernel) listede olmayan herhangi bir Linux çekirdek sürümünü veya daha fazla bilgi için başvurun [ azuredpdk@microsoft.com ](mailto:azuredpdk@microsoft.com). 

## <a name="region-support"></a>Bölge desteği

Tüm Azure bölgelerinde DPDK destekler.

## <a name="prerequisites"></a>Önkoşullar

Bir Linux sanal makinesinde hızlandırılmış ağ etkin olmalıdır. Sanal Makine Yönetimi için bir arabirim ile en az iki ağ arabirimi olması gerekir. Bilgi edinmek için nasıl [hızlandırılmış ağ etkin olan bir Linux sanal makinesi oluşturma](create-vm-accelerated-networking-cli.md).

## <a name="install-dpdk-dependencies"></a>DPDK bağımlılıklarını yükleyin

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev
```

### <a name="rhel75centos-75"></a>RHEL7.5/CentOS 7.5

```bash
yum -y groupinstall "Infiniband Support"
sudo dracut --add-drivers "mlx4_en mlx4_ib mlx5_ib" -f
yum install -y gcc kernel-devel-`uname -r` numactl-devel.x86_64 librdmacm-devel
```

### <a name="sles-15"></a>SLES 15

**Azure çekirdek**

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

## <a name="setup-virtual-machine-environment-once"></a>(Bir kez) sanal makine ortamı Kurulumu

1. [En son DPDK indirme](https://core.dpdk.org/download). Azure için 18.02 veya üzeri bir sürüm gereklidir.
2. Yükleme *libnuma geliştirme* ile paket `sudo apt-get install libnuma-dev`.
3. Varsayılan yapılandırma ile derlediğinizden `make config T=x86_64-native-linuxapp-gcc`.
4. Mellanox PMDs ile oluşturulan yapılandırmasını etkinleştir `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config`.
5. Derleme `make`.
6. İle yükleme `make install DESTDIR=<output folder>`.

# <a name="configure-runtime-environment"></a>Çalışma zamanı ortamı yapılandırma

Sistem yeniden başlatıldıktan sonra aşağıdaki komutları bir kez çalıştırın:

1. Hugepages

   * Tüm numanodes için bir kez aşağıdaki komutu çalıştırarak hugepage yapılandırın:

     ```bash
     echo 1024 | sudo tee
     /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   *  İle bağlama için bir dizin oluşturma `mkdir /mnt/huge`.
   *  Bağlama hugepages ile `mount -t hugetlbfs nodev /mnt/huge`.
   *  Onay hugepages ile ayrılmıştır `grep Huge /proc/meminfo`.

     > [!NOTE]
     > Bir yolu takip ederek önyükleme ayrılmış büyük sayfalar grub dosya değiştirin [yönergeleri](http://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) DPDK için. Sayfanın alt kısmında yönergedir. Bir Azure Linux sanal makinesinde çalışan, yeniden başlatmalar arasında hugepages ayırmak için bunun yerine, /etc/config/grub.d altındaki dosyaları değiştirin.

2. MAC ve IP adresleri: kullanım `ifconfig –a` MAC ve IP adresi ağ arabirimlerinin görüntüleyebilirsiniz. *VF* ağ arabirimi ve *NETVSC* ağ arabirimine sahip olmalı, ancak aynı MAC adresini *NETVSC* ağ arabirimi bir IP adresi vardır.

3. PCI adresleri

   * Kullanılmak üzere hangi PCI adresi öğrenmek *VF* ile `ethtool -i <vf interface name>`.
   * Bu testpmd değil yanlışlıkla konuşturabilirsiniz VF PCI aygıtı sağlamak *eth0*, *eth0* etkin ağ hızlandırdı. DPDK uygulama yönetim ağ arabirimi üzerinden yanlışlıkla sürdü ve SSH bağlantınızı kaybına neden olur, seri konsol DPDK uygulama KILL veya durdurmak veya sanal makineyi başlatmak için kullanın.

4. Yük *ibuverbs* ile her başlatmada `modprobe -a ib_uverbs`. Yalnızca SLES 15 için yük *mlx4_ib* 'modprobe mlx4_ib' ile.

## <a name="failsafe-pmd"></a>Hatasız PMD

Azure'da kullanıma sunulan PMD hatasız üzerinden DPDK uygulamaları çalıştırmanız gerekir. Uygulamayı doğrudan VF PMD üzerinde çalışıyorsa, değil alırsınız **tüm** bazı paketler yapay arabirimi üzerinden görünür olduğundan, VM'ye giden paketler. Ana bilgisayarın bakımda olduğu zaman VF iptal edilir olsa bile DPDK modunda çalışacak şekilde uygulama için hedeflenen tüm paketlerin alır ve ayrıca uygulama sağlar PMD garanti hatasız üzerinde çalışmaya devam edecek. Hatasız PMD hakkında daha fazla bilgi için başvurmak [emniyet yoklama modu sürücüsü kitaplığı](http://doc.dpdk.org/guides/nics/fail_safe.html).

## <a name="run-testpmd"></a>Testpmd çalıştırın

Kullanım `sudo` önce *testpmd* kök modunda çalıştırmak için komutu.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Temel: Sağlamlık onay, hatasız bağdaştırıcısı başlatma

1. Bir tek bağlantı noktası uygulamayı başlatmak için aşağıdaki komutları çalıştırın:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -i \
     --port-topology=chained
    ```

2. Bir çift bağlantı noktası uygulamayı başlatmak için aşağıdaki komutları çalıştırın:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -i
   ```

   2'den fazla NIC ile çalışıyorsa `--vdev` bağımsız değişkeni, bu deseni izler: `net_vdev_netvsc<id>,iface=<vf’s pairing eth>`.

3.  Başlatıldıktan sonra Çalıştır `show port info all` bağlantı noktası bilgileri denetlenecek. Net_failsafe bir veya iki DPDK bağlantı noktalarını görmeniz gerekir (değil *net_mlx4*).
4.  Kullanım `start <port> /stop <port>` trafiği başlatmak için.

Yukarıdaki komutları Başlat *testpmd* etkileşimli modda hangi önerilir, bazı testpmd komutları deneyin.

### <a name="basic-single-sendersingle-receiver"></a>Temel: Tek gönderen/tek alıcısı

Aşağıdaki komutları düzenli aralıklarla paket başına ikinci istatistikleri yazdırın:

1. TX tarafında aşağıdaki komutu çalıştırın:

   ```bash
   Testpmd \
     –l <core-mask> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev=”net_vdev_netvsc<id>,iface=<the iface to attach to>” \
     --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     –eth-peer=<port id>,<peer MAC address> \
     --stats-period <display interval in seconds>
   ```

2. RX tarafında aşağıdaki komutu çalıştırın:

   ```bash
   Testpmd \
     –l <core-mask> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev net_vdev_netvsc<id>,iface=<the iface to attach to>” \
     --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=rxonly \
     –eth-peer=<port id>,<peer MAC address> \
     --stats-period <display interval in seconds>
   ```

Yukarıdaki komutları bir sanal makine üzerinde çalışırken, değiştirme *IP_SRC_ADDR* ve *IP_DST_ADDR* içinde `app/test-pmd/txonly.c` derlemeden önce sanal makinelerin gerçek IP adresini eşleştirmek için. Aksi takdirde, alıcı ulaşmadan önce paketler bırakılır.

### <a name="advanced-single-sendersingle-forwarder"></a>Gelişmiş: Tek gönderen/tek ileticisi
Aşağıdaki komutları düzenli aralıklarla paket başına ikinci istatistikleri yazdırın:

1. TX tarafında aşağıdaki komutu çalıştırın:

   ```bash
   Testpmd \
     –l <core-mask> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev net_vdev_netvsc<id>,iface=<the iface to attach to>” \
     --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     –eth-peer=<port id>,<peer MAC address> \
     --stats-period <display interval in seconds>
    ```

2. İleri Sar tarafında aşağıdaki komutu çalıştırın:

   ```bash
   Testpmd \
     –l <core-mask> \
     -n <num of mem channels> \
     -w <pci address NIC1> \
     -w <pci address NIC2> \
     --vdev=”net_vdev_netvsc<id>,iface=<the iface to attach to>” \
     --vdev=” net_vdev_netvsc<2nd id>,iface=<2nd iface to attach to>” (you need as many --vdev arguments as the number of devices used by testpmd, in this case) \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=io \
     –eth-peer=<recv port id>,<peer MAC address> \
     --stats-period <display interval in seconds>
    ```

Yukarıdaki komutları bir sanal makine üzerinde çalışırken, değiştirme *IP_SRC_ADDR* ve *IP_DST_ADDR* içinde `app/test-pmd/txonly.c` derlemeden önce sanal makinelerin gerçek IP adresini eşleştirmek için. Aksi takdirde, iletici ulaşmadan önce paketler bırakılır. Bir üçüncü makine iletilen trafiği almak için sahip mümkün olmayacaktır *testpmd* bazı kod değişiklikleri yapmanızı sürece, iletici Katman 3 adreslerini değiştirmek değil.

## <a name="references"></a>Başvurular

* [Deniz Mavisi seçenekleri](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options)
* [Testpmd komutları](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options)
