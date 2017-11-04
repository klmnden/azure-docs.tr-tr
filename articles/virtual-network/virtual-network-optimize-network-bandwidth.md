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
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: 914747983d4d974810836be66d6c6af343f58b60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Azure sanal makineleri için ağ verimliliğini en iyi duruma getirme

Azure sanal makineler (VM) daha fazla ağ verimliliği için iyileştirilmiş varsayılan ağ ayarları vardır. Bu makalede, Microsoft Azure Windows ve Linux VM'ler Ubuntu ve CentOS Red Hat gibi büyük dağıtımlar dahil olmak üzere, ağ verimliliğini en iyi duruma getirme açıklar.

## <a name="windows-vm"></a>Windows VM

Windows VM ile destekleniyorsa [hızlandırılmış ağ](virtual-network-create-vm-accelerated-networking.md), bu özelliğin etkinleştirilmesi verimliliği için en uygun yapılandırma olacaktır. Diğer tüm Windows VM'ler için Alma Tarafı Ölçeklendirmesi (RSS) kullanarak bir VM RSS olmadan daha yüksek düzeyde verimlilik ulaşabilir. Varsayılan olarak bir Windows VM RSS devre dışı olabilir. RSS etkin olup olmadığını belirlemek için ve onu devre dışı bırakılırsa etkinleştirmek için aşağıdaki adımları tamamlayın.

1. Girin `Get-NetAdapterRss` RSS bir ağ bağdaştırıcısı için etkin olup olmadığını görmek için PowerShell komutu. Aşağıdaki örnek çıktıda döndürülen `Get-NetAdapterRss`, RSS etkin değil.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. RSS etkinleştirmek için aşağıdaki komutu girin:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    Önceki komut çıktısı yok. Komut geçici bağlantı kaybı yaklaşık bir dakika boyunca neden NIC ayarları değiştirildi. Bağlantı kaybı sırasında bir Reconnecting iletişim kutusu görüntülenir. Bağlantı genellikle üçüncü girişiminden sonra geri yüklendi.
3. RSS VM'yi girerek etkinleştirildiğini onaylayın `Get-NetAdapterRss` yeniden komutu. Başarılı olursa, aşağıdaki örnek çıkış verilir:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Linux VM

RSS, her zaman bir Azure Linux VM'de varsayılan olarak etkindir. Ocak 2017 bu yana yayımlanan Linux tekrar daha yüksek ağ verimliliği elde etmek bir Linux VM sağlayan yeni ağ en iyi duruma getirme seçenekleri içerir.

### <a name="ubuntu"></a>Ubuntu

En iyi duruma getirme alabilmek için önce en son desteklenen sürümünü, olan Haziran 2017 itibariyle güncelleştirin:
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Güncelleştirme tamamlandıktan sonra son çekirdek almak için aşağıdaki komutları girin:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

İsteğe bağlı komut:

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a>Ubuntu Azure Önizleme çekirdek
> [!WARNING]
> Bu Azure Linux Önizleme çekirdek kullanılabilirliği ve güvenilirliği Market görüntü olarak aynı düzeyde olmayabilir ve genel kullanılabilirlik olan tekrar bırakın. Özellik desteklenmiyor, yetenekleri kısıtlı ve varsayılan çekirdek olarak güvenilir olmayabilir. Bu çekirdek üretim iş yükleri için kullanmayın.

Önemli verimliliği performansından önerilen Azure Linux çekirdek yükleyerek elde edilebilir. Bu çekirdek denemek için /etc/apt/sources.list için bu satırı ekleyin

```bash
#add this to the end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

Daha sonra bu komutları kök olarak çalıştırın.
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

En iyi duruma getirme alabilmek için önce en son desteklenen sürümünü, olan Temmuz 2017 itibariyle güncelleştirin:
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
Güncelleştirme tamamlandıktan sonra son Linux Tümleştirme hizmetleri (LIS) yükleyin.
Üretilen iş iyileştirme 4.2.2-2 başlayarak LIS kullanılıyor. LIS yüklemek için aşağıdaki komutları girin:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

En iyi duruma getirme alabilmek için önce en son desteklenen sürümünü, olan Temmuz 2017 itibariyle güncelleştirin:
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
Güncelleştirme tamamlandıktan sonra son Linux Tümleştirme hizmetleri (LIS) yükleyin.
4.2 başlayarak LIS, üretilen iş iyileştirme kullanılıyor. LIS'yi indirmeniz ve yüklemeniz için aşağıdaki komutları girin:

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Linux Tümleştirme hizmetleri sürüm 4.2 hakkında daha fazla Hyper-V için görüntüleyerek bilgi [indirme sayfasının](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Sonraki adımlar
* VM en iyi duruma getirilmiş, sonuçla bkz [bant genişliği/verimlilik Azure VM sınama](virtual-network-bandwidth-testing.md) senaryonuz için.
* Daha fazla bilgi edinin [Azure Virtual Network sık sorulan sorular (SSS)](virtual-networks-faq.md)
