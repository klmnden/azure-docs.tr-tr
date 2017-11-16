---
title: "VM ağ verimliliğini en iyi duruma getirme | Microsoft Docs"
description: "Azure sanal makine ağ verimliliğini en iyi duruma getirme hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2017
ms.author: steveesp
ms.openlocfilehash: 2f7a65d32f662d7e265e58c5fe7d9dea81a4e63c
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Azure sanal makineleri için ağ verimliliğini en iyi duruma getirme

Azure sanal makineler (VM) daha fazla ağ verimliliği için iyileştirilmiş varsayılan ağ ayarları vardır. Bu makalede, Microsoft Azure Windows ve Linux VM'ler Ubuntu ve CentOS Red Hat gibi büyük dağıtımlar dahil olmak üzere, ağ verimliliğini en iyi duruma getirme açıklar.

## <a name="windows-vm"></a>Windows VM

Windows VM ile destekleniyorsa [hızlandırılmış ağ](virtual-network-create-vm-accelerated-networking.md), bu özelliğin etkinleştirilmesi verimliliği için en uygun yapılandırma olacaktır. Diğer tüm Windows VM'ler için Alma Tarafı Ölçeklendirmesi (RSS) kullanarak bir VM RSS olmadan daha yüksek düzeyde verimlilik ulaşabilir. Varsayılan olarak bir Windows VM RSS devre dışı olabilir. RSS etkin olup olmadığını belirlemek için ve onu devre dışı bırakılırsa etkinleştirmek için aşağıdaki adımları tamamlayın.

1. Girin `Get-NetAdapterRss` RSS bir ağ bağdaştırıcısı için etkin olup olmadığını görmek için PowerShell komutu. Aşağıdaki örnek çıktıda döndürülen `Get-NetAdapterRss`, RSS etkin değil.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                 : False
    ```
2. RSS etkinleştirmek için aşağıdaki komutu girin:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    Önceki komut çıktısı yok. Komut geçici bağlantı kaybı yaklaşık bir dakika boyunca neden NIC ayarları değiştirildi. Bağlantı kaybı sırasında bir Reconnecting iletişim kutusu görüntülenir. Bağlantı genellikle üçüncü girişiminden sonra geri yüklendi.
3. RSS VM'yi girerek etkinleştirildiğini onaylayın `Get-NetAdapterRss` yeniden komutu. Başarılı olursa, aşağıdaki örnek çıkış verilir:

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Linux VM

RSS, her zaman bir Azure Linux VM'de varsayılan olarak etkindir. Ekim 2017 bu yana yayımlanan Linux tekrar daha yüksek ağ verimliliği elde etmek bir Linux VM sağlayan yeni ağ en iyi duruma getirme seçenekleri içerir.

### <a name="ubuntu-for-new-deployments"></a>Ubuntu yeni dağıtımlar için

Ubuntu Azure çekirdek Azure üzerinde en iyi ağ performansı sağlar ve varsayılan çekirdek 21 Eylül 2017'den bu yana bırakıldı. Bu çekirdek alabilmek için önce en son desteklenen sürümünü 16.04-LTS, aşağıda açıklandığı gibi yükleyin:
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Oluşturma işlemi tamamlandıktan sonra en son güncelleştirmeleri almak için aşağıdaki komutları girin. Bu adımlar, Ubuntu Azure çekirdek çalışmakta VM'ler için de geçerlidir.

```bash
#run as root or preface with sudo
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

Aşağıdaki isteğe bağlı komut kümesini Azure çekirdek zaten sahip ancak, daha fazla güncelleştirme hataları ile başarısız olan mevcut Ubuntu dağıtımlar için yararlı olabilir.

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

Önemli verimliliği performansından Azure Linux çekirdeğe yükselterek elde edilebilir. Bu çekirdek yüklü olup olmadığını doğrulamak için çekirdek sürümünü denetleyin.

```bash
#Azure kernel name ends with "-azure"
uname -r

#sample output on Azure kernel:
#4.11.0-1014-azure
```

VM Azure çekirdek yoksa, sürüm numarasını genellikle "4.4" ile başlar. Bu durumda, kök olarak aşağıdaki komutları çalıştırın.
```bash
#run as root or preface with sudo
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

En son iyileştirmeler alabilmek için şu parametreleri belirterek bir VM ile en son desteklenen sürümünü oluşturmak en iyisidir:
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.4",
"Version": "latest"
```
Yeni ve mevcut sanal makineleri son Linux Tümleştirme hizmetleri (LIS) yüklenmesini yararlı olabilir.
Üretilen iş iyileştirme geliştirmeleri daha sonraki sürümlerinde içerse de 4.2.2-2 başlayarak LIS içinde ' dir. En son LIS yüklemek için aşağıdaki komutları girin:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

En iyi duruma getirme alabilmek için şu parametreleri belirterek bir VM ile en son desteklenen sürümünü oluşturmak en iyisidir:
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7-RAW"
"Version": "latest"
```
Yeni ve mevcut sanal makineleri son Linux Tümleştirme hizmetleri (LIS) yüklenmesini yararlı olabilir.
4.2 başlayarak LIS, üretilen iş iyileştirme kullanılıyor. LIS'yi indirmeniz ve yüklemeniz için aşağıdaki komutları girin:

```bash
mkdir lis4.2.3-1
cd lis4.2.3-1
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.3-1.tar.gz
tar xvzf lis-rpms-4.2.3-1.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Linux Tümleştirme hizmetleri sürüm 4.2 hakkında daha fazla Hyper-V için görüntüleyerek bilgi [indirme sayfasının](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Sonraki adımlar
* VM en iyi duruma getirilmiş, sonuçla bkz [bant genişliği/verimlilik Azure VM sınama](virtual-network-bandwidth-testing.md) senaryonuz için.
* Daha fazla bilgi edinin [Azure Virtual Network sık sorulan sorular (SSS)](virtual-networks-faq.md)
