---
title: Hızlandırılmış ağ ile bir Azure sanal makinesi oluşturun | Microsoft Docs
description: Hızlandırılmış etkin ağ ile bir Linux sanal makinesi oluşturmayı öğrenin.
services: virtual-network
documentationcenter: na
author: gsilva5
manager: gedegrac
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/10/2019
ms.author: gsilva
ms.custom: ''
ms.openlocfilehash: 8ea17e5615c0256c084b0745a392fb49f8873f99
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58805372"
---
# <a name="create-a-linux-virtual-machine-with-accelerated-networking"></a>Hızlandırılmış ağ ile bir Linux sanal makinesi oluşturma

Bu öğreticide, hızlandırılmış ağ ile bir Linux sanal makinesini (VM) oluşturma konusunda bilgi edinin. Hızlandırılmış ağ ile bir Windows VM oluşturmak için bkz: [hızlandırılmış ağ ile bir Windows VM oluşturma](create-vm-accelerated-networking-powershell.md). Hızlandırılmış ağ, ağ performansını önemli ölçüde geliştirmek, bir sanal makineye tek kökte g/ç Sanallaştırması (SR-IOV) sağlar. Bu yüksek performanslı yolu, gecikme süresi, değişim ve CPU kullanımı, En zorlu ağ iş yükleri desteklenen VM türleri ile kullanmak için azaltma datapath konaktan atlar. Aşağıdaki resimde, hızlandırılmış ağ bağlantısı olmadan ve iki sanal makineler arasındaki iletişimi gösterir:

![Karşılaştırma](./media/create-vm-accelerated-networking/accelerated-networking.png)

Hızlandırılmış ağ bağlantısı olmadan konak ve sanal anahtar ve VM'ye giden tüm ağ trafiğini çapraz gerekir. Sanal anahtarı, ağ güvenlik grupları, erişim denetim listeleri, yalıtım ve diğer ağ trafiğini sanallaştırılmış Ağ Hizmetleri gibi tüm ilke zorlaması sağlar. Sanal anahtarlar hakkında daha fazla bilgi edinmek için [Hyper-V ağ sanallaştırma ve sanal anahtar](https://technet.microsoft.com/library/jj945275.aspx) makalesi.

Hızlandırılmış ağlı ağ trafiğini sanal makine'nin ağ arabirimine (NIC) ulaşır ve ardından VM iletilir. Sanal anahtarın uygulandığı tüm ağ ilkeleri artık Boşaltılan ve donanım uygulanır. Donanım ilkede uygulama konak ve sanal anahtar konağa uygulanan tüm ilke korurken atlayarak doğrudan VM ağ trafiği NIC'ye sağlar.

Hızlandırılmış ağ avantajları yalnızca üzerinde etkin VM için de geçerlidir. En iyi sonuçlar için en az iki VM aynı Azure sanal ağa (VNet) bağlı bu özelliği etkinleştirmek idealdir. Bu özellik, sanal ağlar veya şirket bağlantı iletişim kurarken, toplam gecikme süresi çok az etkisi sahiptir.

## <a name="benefits"></a>Avantajlar
* **Daha düşük gecikme süresi / daha yüksek paket / saniye (pps):** Sanal anahtar datapath kaldırılıyor ana bilgisayar ilke işleme için paketler için harcadığınız zamanı kaldırır ve sanal makine içinde işlenebilecek paketlerin sayısını artırır.
* **Daha az değişim:** Sanal anahtar işleme uygulanması gereken ilke miktarını ve işleme yapılıyor CPU iş yüküne bağlıdır. İlke zorlaması için donanım yük boşaltma, konak VM iletişim ve tüm yazılım kesmelerini ve bağlam anahtarları kaldırılıyor doğrudan VM paketleri sunarak bu değişkenlik kaldırır.
* **Düşük CPU kullanımı:** Konakta sanal anahtar atlayarak ağ trafiğini işlemek için daha az CPU kullanımına neden olur.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Azure Galerisi hazır aşağıdaki dağıtımlar desteklenir: 
* **Ubuntu 14.04 linux azure çekirdek ile**
* **Ubuntu 16.04 veya üzeri** 
* **SLES12 SP3 veya üstü** 
* **RHEL 7.4 veya üzeri**
* **CentOS 7.4 veya üzeri**
* **CoreOS Linux**
* **Debian "backports çekirdekle Uzat"**
* **Oracle Linux 7.4 ve üzeri ile Red Hat uyumlu çekirdek (RHCK)**
* **Oracle Linux 7.5 ve üzeri ile UEK sürüm 5**
* **FreeBSD 10.4, 11.1 & 12.0**

## <a name="limitations-and-constraints"></a>Sınırlamalar ve kısıtlamalar

### <a name="supported-vm-instances"></a>Desteklenen sanal makine örnekleri
Hızlandırılmış ağ en genel amaçlı ve işlem için iyileştirilmiş örnek boyutları Vcpu, 2 veya daha fazla ile desteklenir.  Bu desteklenen serisi şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destek örneklerinde, hızlandırılmış ağ ile Vcpu, 4 veya daha fazla VM örneklerinde desteklenir. Desteklenen serisi şunlardır: D/Dsv3, E/Esv3, Fsv2, Lsv2, Ms/Mms ve Ms/Mmsv2.

Sanal makine örnekleri hakkında daha fazla bilgi için bkz. [Linux VM boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Bölgeler
Azure kamu Bulutları yanı sıra tüm genel Azure bölgelerinde kullanılabilir.

<!-- ### Network interface creation 
Accelerated networking can only be enabled for a new NIC. It cannot be enabled for an existing NIC.
removed per issue https://github.com/MicrosoftDocs/azure-docs/issues/9772 -->
### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Hızlandırılmış ağ çalışan bir VM'deki etkinleştirme
Desteklenen bir VM boyutu hızlandırılmış ağ etkin olmadan yalnızca özelliğinin durdurulur ve serbest etkin olabilir.  
### <a name="deployment-through-azure-resource-manager"></a>Azure Resource Manager üzerinden dağıtım
Hızlandırılmış ağ ile sanal makineler (Klasik) dağıtılamıyor.

## <a name="create-a-linux-vm-with-azure-accelerated-networking"></a>Azure hızlandırılmış ağ ile bir Linux VM oluşturma
## <a name="portal-creation"></a>Portal oluşturma
Bu makale, hızlandırılmış ağ ile Azure CLI kullanarak bir sanal makine oluşturmak için adımları sağlar ancak ayrıca [hızlandırılmış ağ ile Azure portalını kullanarak bir sanal makine oluşturma](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Portalda, sanal makine oluştururken **sanal makine oluşturma** dikey penceresinde, seçin **ağ** sekmesi.  Bu sekmede için bir seçenek yoktur **hızlandırılmış**.  Seçtiyseniz bir [işletim sistemi desteklenen](#supported-operating-systems) ve [VM boyutu](#supported-vm-instances), bu seçenek "Açık." otomatik olarak doldurulur  Aksi halde, hızlandırılmış ağ için "Kapalı" seçeneği doldurmak ve kullanıcıya neden olan olması etkin bir neden sağlayın.   

* *Not:* Desteklenen işletim sistemleri, portal üzerinden etkinleştirilebilir.  Lütfen özel bir görüntü kullanıyorsanız ve hızlandırılmış ağ görüntünüzü destekliyorsa, CLI veya Powershell kullanarak VM oluşturma. 

Sanal makine oluşturulduktan sonra hızlandırılmış ağ etkin yönergeleri izleyerek doğrulayabilirsiniz [hızlandırılmış ağ etkin olduğunu onaylayın](#confirm-that-accelerated-networking-is-enabled).

## <a name="cli-creation"></a>CLI oluşturma
### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Son yükleme [Azure CLI](/cli/azure/install-azure-cli) ve Azure hesabınızı kullanarak oturum açma [az login](/cli/azure/reference-index). Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *Mynıc*, ve *myVm*.

[az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *centralus* konumu:

```azurecli
az group create --name myResourceGroup --location centralus
```

Listelenen desteklenen bir Linux bölge seçin [Linux hızlandırılmış ağ özelliğini](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview).

[az network vnet create](/cli/azure/network/vnet) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVnet* bir alt ağ ile:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma
Bir ağ güvenlik grubu oluşturun [az ağ nsg oluşturma](/cli/azure/network/nsg). Aşağıdaki örnek *myNetworkSecurityGroup* adında bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Ağ güvenlik grubu, biri de Internet'ten gelen tüm erişimi devre dışı bırakır, birkaç varsayılan kurallar içerir. SSH ile sanal makine erişmesine izin vermek için bağlantı noktası açma [az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule):

```azurecli
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup \
  --name Allow-SSH-Internet \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 22
```

### <a name="create-a-network-interface-with-accelerated-networking"></a>Hızlandırılmış ağ ile bir ağ arabirimi oluşturma

[az network public-ip create](/cli/azure/network/public-ip) komutu ile bir genel IP adresi oluşturun. Internet'ten sanal makineye erişmek için ancak bu makaledeki adımları tamamlayabilmeniz için planlamıyorsanız bir genel IP adresi gerekli değilse, gerekli değildir.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Bir ağ arabirimi ile oluşturma [az ağ NIC oluşturup](/cli/azure/network/nic) , hızlandırılmış ağ bağlantısı etkin. Aşağıdaki örnekte adlı bir ağ arabirimi oluşturur *Mynıc* içinde *mySubnet* alt *myVnet* sanal ağ ile ilişkilendirir  *Vm2* ağ arabirimi için ağ güvenlik grubu:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --accelerated-networking true \
    --public-ip-address myPublicIp \
    --network-security-group myNetworkSecurityGroup
```

### <a name="create-a-vm-and-attach-the-nic"></a>VM oluşturma ve NIC ekleme
NIC belirtin, VM'yi oluştururken oluşturduğunuz `--nics`. Bir boyut seçip listelenen dağıtım [Linux hızlandırılmış ağ özelliğini](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview). 

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* UbuntuLTS görüntüsünü ve hızlandırılmış ağ iletişimi destekleyen bir boyutu ile (*Standard_DS4_v2*):

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS4_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Tüm VM boyutları ve özelliklerini bir listesi için bkz. [Linux VM boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

VM oluşturulduktan sonra aşağıdaki örnek çıktıya benzer bir çıktı döndürülür. **publicIpAddress** değerini not alın. Bu adres, sonraki adımlarda sanal Makineye erişmek için kullanılır.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "centralus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "192.168.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

### <a name="confirm-that-accelerated-networking-is-enabled"></a>Hızlandırılmış ağ etkin olduğunu doğrulayın

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. Değiştirin `<your-public-ip-address>` sanal atanan genel IP adresi ile oluşturduğunuz makine ve Değiştir *azureuser* için farklı bir değer kullandıysanız `--admin-username` VM oluşturduğunuzda.

```bash
ssh azureuser@<your-public-ip-address>
```

Bash Kabuğu'ndan girin `uname -r` ve çekirdek sürümünün bir aşağıdaki sürümleri veya daha büyük olduğundan emin olun:

* **Ubuntu 16.04**: 4.11.0-1013
* **SLES SP3**: 4.4.92-6.18
* **RHEL**: 7.4.2017120423
* **CentOS**: 7.4.20171206


Mellanox VF cihaz VM ile Internet'e açık onaylayın `lspci` komutu. Döndürülen çıkış aşağıdaki çıktıya benzer:

```bash
0000:00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
0000:00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
0000:00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
0000:00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
0000:00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA
0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
```

Denetleme (sanal işlev) VF etkinlik ile `ethtool -S eth0 | grep vf_` komutu. Alırsanız, çıkışı, hızlandırılmış ağ etkin ve çalışma aşağıdaki örneğe benzer çıktı.

```bash
vf_rx_packets: 992956
vf_rx_bytes: 2749784180
vf_tx_packets: 2656684
vf_tx_bytes: 1099443970
vf_tx_dropped: 0
```
Hızlandırılmış ağ, sanal Makineniz için etkinleştirilmiştir.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Hızlandırılmış ağ mevcut vm'lerde etkinleştir
Hızlandırılmış ağ olmayan bir VM oluşturduysanız, varolan bir VM'yi bu özelliği etkinleştirmek mümkündür.  Sanal Makineyi hızlandırılmış ağ, yukarıda özetlenen aşağıdaki gereksinimleri karşılayarak desteklemelidir:

* Sanal Makineyi hızlandırılmış ağ için desteklenen bir boyut olmalıdır
* VM, desteklenen Azure galeri görüntüsü (ve Linux için çekirdek sürümü) olmalıdır.
* Bir kullanılabilirlik kümesindeki tüm sanal makineler veya VMSS hızlandırılmış ağ nıc'lerden birine etkinleştirmeden önce durdurulmuş/serbest bırakılmış olmalıdır

### <a name="individual-vms--vms-in-an-availability-set"></a>Tek Vm'leri & Vm'leri bir kullanılabilirlik kümesi
İlk Durdur/VM'yi serbest bırakın veya bir kullanılabilirlik kümesi, kümedeki tüm sanal makineler:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

Önemli, lütfen ayrı ayrı bir kullanılabilirlik kümesi, yalnızca sanal makinenizin oluşturulduysa Not gerekir hızlandırılmış ağ iletişimi etkinleştirmek için tek tek VM'yi Durdur/serbest bırakın.  Sanal makinenize bir kullanılabilirlik kümesi ile oluşturulmuş olsa bile, kullanılabilirlik kümesinde yer alan tüm Vm'lere hızlandırılmış ağ NIC'lerin herhangi biri üzerinde etkinleştirmeden önce durdurulmuş/serbest bırakılmış olması gerekir. 

Hizmet durdurulduğunda, hızlandırılmış ağ sanal makinenizin NIC'de etkinleştir:

```azurecli
az network nic update \
    --name myNic \
    --resource-group myResourceGroup \
    --accelerated-networking true
```

Bir kullanılabilirlik kümesi, kümedeki tüm sanal makineler, VM veya, eğer yeniden ve hızlandırılmış ağ etkin olduğunu doğrulayın: 

```azurecli
az vm start --resource-group myResourceGroup \
    --name myVM
```

### <a name="vmss"></a>VMSS
VMSS biraz farklıdır ama aynı iş akışını izler.  İlk olarak, Vm'leri durdurun:

```azurecli
az vmss deallocate \
    --name myvmss \
    --resource-group myrg
```

VM durdurulduktan sonra ağ arabirimi'nin altındaki hızlandırılmış ağ özelliğini güncelleştirin:

```azurecli
az vmss update --name myvmss \
    --resource-group myrg \
    --set virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].enableAcceleratedNetworking=true
```

Lütfen bir VMSS Not otomatik, sıralı ve el ile olmak üzere üç farklı ayarları kullanarak güncelleştirmeleri uygulamak VM yükseltmeleri sahiptir.  VMSS hemen yeniden başlattıktan sonra değişiklikleri alır, böylece bu Yönergelerdeki ilke otomatik olarak ayarlanır.  Değişiklikler hemen alınır şekilde otomatik olarak ayarlamak için: 

```azurecli
az vmss update \
    --name myvmss \
    --resource-group myrg \
    --set upgradePolicy.mode="automatic"
```

Son olarak, VMSS yeniden başlatın:

```azurecli
az vmss start \
    --name myvmss \
    --resource-group myrg
```

Bir kez, yeniden, yükseltmeleri tamamlamak ancak bir kez tamamlandı, VF VM içinde görünür.  (Lütfen desteklenen bir işletim sistemi ve VM boyutu kullandığınızdan emin olun.)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Hızlandırılmış ağ ile mevcut Vm'leri yeniden boyutlandırma

Hızlandırılmış ağ etkin Vm'lerle yalnızca VM'ler için hızlandırılmış ağ destekleyen boyutlandırılabilir.  

Hızlandırılmış ağ etkin olan bir VM yeniden boyutlandırma işlemi kullanarak hızlandırılmış ağ desteği olmayan bir VM örneğine yeniden boyutlandırılamaz.  Bunun yerine, bu Vm'lere birini yeniden boyutlandırmak için: 

* VM'yi Durdur/serbest veya bir kullanılabilirlik kümesi/VMSS varsa içinde Durdur/set/VMSS tüm sanal makineler serbest bırakın.
* Hızlandırılmış ağ VM veya bir kullanılabilirlik kümesi/VMSS, küme/VMSS içindeki tüm sanal makineleri sahipse NIC üzerinde devre dışı bırakılmalıdır.
* Hızlandırılmış ağ devre dışı bırakıldıktan sonra VM/kullanılabilirlik kümesi/VMSS hızlandırılmış ağ desteklemez ve yeniden yeni bir boyut için taşınabilir.  

