---
title: Sanal makine ağ aktarım hızını iyileştirme | Microsoft Docs
description: Azure sanal makine ağ aktarım hızını iyileştirme hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2017
ms.author: steveesp
ms.openlocfilehash: 50d7ca73e5e18f88f5d789e12fc7f26908e8b8f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62125789"
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Azure sanal makineleri için ağ aktarım hızını iyileştirme

Azure sanal makineleri (VM) için ağ aktarım hızı iyileştirilebilir daha fazla varsayılan ağ ayarları vardır. Bu makalede, Microsoft Azure Windows ve Linux Vm'leri, Ubuntu, CentOS ve Red Hat gibi büyük dağıtımları da dahil olmak üzere ağ aktarım hızını iyileştirme açıklar.

## <a name="windows-vm"></a>Windows VM

Windows VM'nizi destekliyorsa [hızlandırılmış ağ](create-vm-accelerated-networking-powershell.md), bu özelliğin etkinleştirilmesi aktarım hızı için en uygun yapılandırma olacaktır. Diğer tüm Windows VM'ler için yüksek düzeyde üretilen işten RSS olmayan bir VM Alma Tarafı Ölçeklendirmesi (RSS) kullanarak ulaşabilirsiniz. Varsayılan olarak bir Windows VM RSS devre dışı bırakılabilir. RSS etkin ve şu anda devre dışıysa etkinleştirmek olup olmadığını belirlemek için aşağıdaki adımları tamamlayın:

1. Bir ağ bağdaştırıcısının RSS etkin olup olmadığını `Get-NetAdapterRss` PowerShell komutu. Aşağıdaki örnek çıktıda döndürüldüğü `Get-NetAdapterRss`, RSS etkin değil.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                 : False
    ```
2. RSS etkinleştirmek için aşağıdaki komutu girin:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    Önceki komut, bir çıkış yok. Komut yaklaşık bir dakika için geçici bağlantı kaybına neden NIC ayarları değiştirildi. Bağlantı kaybı sırasında yeniden bağlanılıyor iletişim kutusu görünür. Bağlantı, genellikle üçüncü girişiminden sonra geri yüklenir.
3. RSS VM'yi girerek etkinleştirildiğini onaylayın `Get-NetAdapterRss` yeniden komutu. Aşağıdaki örnek çıkış, başarılı olursa, döndürülür:

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                  : True
    ```

## <a name="linux-vm"></a>Linux VM

RSS, her zaman Azure Linux VM'de varsayılan olarak etkindir. Yayın Tarihi: Ekim 2017 itibaren Linux çekirdeklerinin daha yüksek ağ verimi elde etmek bir Linux VM sağlayan yeni ağ en iyi duruma getirme seçenekleri içerir.

### <a name="ubuntu-for-new-deployments"></a>Yeni dağıtımlar için ubuntu

Ubuntu Azure çekirdek Azure'da en iyi ağ performansını sağlar ve varsayılan çekirdek 21 Eylül 2017'den itibaren kaldırıldı. Bu çekirdek alabilmek için önce 16.04 LTS, desteklenen son sürümü aşağıda gösterildiği gibi yükleyin:

```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```

Oluşturma işlemi tamamlandıktan sonra en son güncelleştirmeleri almak için aşağıdaki komutları girin. Bu adımlar, Ubuntu Azure çekirdek şu anda çalışan VM'ler için de geçerlidir.

```bash
#run as root or preface with sudo
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

Aşağıdaki isteğe bağlı komut kümesi zaten sahip olan Azure çekirdek ancak hatalarla başka güncelleştirmeler başarısız olan mevcut Ubuntu dağıtımlar için yararlı olabilir.

```bash
#optional steps may be helpful in existing deployments with the Azure kernel
#run as root or preface with sudo
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

#### <a name="ubuntu-azure-kernel-upgrade-for-existing-vms"></a>Var olan VM'ler için ubuntu Azure çekirdek yükseltme

Önemli bir aktarım hızı performansı için Azure Linux çekirdeğinin yükselterek gerçekleştirilebilir. Bu çekirdek yüklü olup olmadığını doğrulamak için çekirdek sürümünüzü kontrol edin.

```bash
#Azure kernel name ends with "-azure"
uname -r

#sample output on Azure kernel:
#4.13.0-1007-azure
```

VM'niz Azure çekirdek yoksa sürüm numarasını genellikle "4.4" ile başlar VM'nin Azure çekirdek yoksa kök olarak aşağıdaki komutları çalıştırın:

```bash
#run as root or preface with sudo
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

En son iyileştirmeler alabilmek için aşağıdaki parametreleri belirterek desteklenen son sürümü ile bir VM oluşturmak idealdir:

```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.4",
"Version": "latest"
```

Yeni ve var olan VM'ler, en son Linux Integration Services (LIS) yüklenmesini yararlı olabilir. Üretilen iş iyileştirme 4.2.2-2 ' sonraki sürümlerde başka geliştirmeler içerir ancak başlangıç LIS içinde var. En son LIS yüklemek için aşağıdaki komutları girin:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

En iyi duruma getirme almak için aşağıdaki parametreleri belirterek bir VM ile desteklenen son sürümü oluşturulması idealdir:

```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7-RAW"
"Version": "latest"
```

Yeni ve var olan VM'ler, en son Linux Integration Services (LIS) yüklenmesini yararlı olabilir. Üretilen iş iyileştirme LIS 4.2 ' başlayarak, bileşenidir. İndirip LIS yüklemek için aşağıdaki komutları girin:

```bash
mkdir lis4.2.3-5
cd lis4.2.3-5
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.3-5.tar.gz
tar xvzf lis-rpms-4.2.3-5.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Görüntüleyerek Hyper-V için Linux Tümleştirme hizmetleri sürüm 4.2 hakkında daha fazla bilgi [indirme sayfası](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Sonraki adımlar
* İle en iyi duruma getirilmiş sonucunu [bant genişliği/aktarım hızı testi Azure VM](virtual-network-bandwidth-testing.md) senaryonuz için.
* Nasıl çalıştıracağınızı okuyun [bant genişliği, sanal makinelere ayrılır](virtual-machine-network-throughput.md)
* Daha fazla bilgi edinin [Azure sanal ağı sık sorulan sorular (SSS)](virtual-networks-faq.md)
