---
title: "Hızlandırılmış ağ ile bir Azure sanal makine oluşturma | Microsoft Docs"
description: "Hızlandırılmış ağ ile Linux sanal makine oluşturmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/02/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: e4c875d07905b56c0d3eb346c839f7a4917531de
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-linux-virtual-machine-with-accelerated-networking"></a>Hızlandırılmış ağ ile Linux sanal makine oluşturma

> [!IMPORTANT] 
> Sanal makineler hızlandırılmış etkin ağ ile oluşturulması gerekir. Bu özellik, varolan sanal makinelere etkinleştirilemez. Hızlandırılmış ağ iletişimi etkinleştirmek için aşağıdaki adımları izleyin:
>   1. Sanal makineyi silin.
>   2. Sanal makine hızlandırılmış etkin ağ ile yeniden oluşturun.
>

Bu öğreticide, bir Linux sanal makine (VM) ağ hızlandırılmış oluşturmayı öğrenin. Hızlandırılmış ağ önemli ölçüde ağ performansını iyileştirme, bir VM tek köklü g/ç Sanallaştırması (SR-IOV) sağlar. Bu yüksek performanslı yolu gecikme, değişim ve desteklenen VM türlerinde zorlu ağ iş yükleri ile kullanmak için CPU kullanımını azaltır ancak konaktan datapath atlar. Aşağıdaki resimde, iki VM ile ve hızlandırılmış ağ olmadan arasındaki iletişimi gösterir:

![Karşılaştırma](./media/create-vm-accelerated-networking/accelerated-networking.png)

Hızlandırılmış ağ desteği olmadan, VM ve dışındaki tüm ağ trafiğini konak ve sanal anahtar çapraz geçiş gerekir. Sanal anahtar ağ güvenlik grupları, erişim denetim listeleri, yalıtım ve diğer ağ trafiği sanallaştırılmış Ağ Hizmetleri gibi tüm ilke zorunluluğu sağlar. Sanal anahtarları hakkında daha fazla bilgi için okuma [Hyper-V ağ sanallaştırma ve sanal anahtar](https://technet.microsoft.com/library/jj945275.aspx) makalesi.

Hızlandırılmış ağ ile ağ trafiğini VM ağ arabiriminin (NIC) ulaştığında ve ardından VM iletilir. Sanal anahtar uygular tüm ağ ilkeleri artık Boşaltılan ve donanım uygulanır. Donanım ilkesinde uygulama konak ve sanal anahtarın konak uygulanan tüm ilke korurken atlayarak doğrudan VM'ye iletme ağ trafiğini NIC'ye sağlar.

Hızlandırılmış ağ yararları yalnızca üzerinde etkin VM için de geçerlidir. En iyi sonuçlar için en az iki VM aynı Azure sanal ağa (VNet) bağlı bu özelliği etkinleştirmek idealdir. Sanal ağlar veya içi bağlantı üzerinden iletişim kurarken, bu özellik genel gecikme en az bir etkisi yoktur.

## <a name="benefits"></a>Avantajlar
* **Düşük gecikme / saniye başına daha yüksek paket (pps):** datapath sanal anahtarı kaldırmak ana bilgisayar ilke işleme için paketler harcadığı zamanı kaldırır ve VM içinde işlenebilecek paketlerin sayısını artırır.
* **Değişim azaltılmış:** sanal anahtar işleme uygulanması gereken ilke miktarını ve işlem yaptığını CPU iş yüküne bağlıdır. İlke zorlama donanım yük boşaltma, VM iletişim ve tüm yazılım kesmelerini ve İçerik Geçişi konağa kaldırma doğrudan VM paketleri sunarak bu değişkenlik kaldırır.
* **Düşük CPU kullanımı:** konakta sanal anahtar atlayarak müşteri adayları ağ trafiğini işlemek için daha az CPU kullanımı için.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
* **Ubuntu 16.04**: 4.11.0-1013 veya büyük çekirdek sürümü
* **SLES SP3**: 4.4.92-6.18 veya büyük çekirdek sürümü
* **RHEL 7.4**: 7.4.2017120423 veya büyük çekirdek sürümü
* **CentOS 7.4**: 7.4.20171206 veya büyük çekirdek sürümü

## <a name="supported-vm-instances"></a>Desteklenen VM örnekleri
Hızlandırılmış ağ en genel amaçlı ve işlem iyileştirilmiş örnek boyutları 4 veya daha fazla Vcpu'lar ile desteklenir. Hiper iş parçacığı destekleyen örneklerinde D/DSv3 veya E/ESv3 gibi hızlandırılmış ağ 8 veya daha fazla Vcpu'lar ile VM örneklerinde desteklenir.  Desteklenen serisi şunlardır: D/DSv2, D/DSv3, E/ESv3, Fs/F/Fsv2 ve Ms/Mms. 

VM örnekleri hakkında daha fazla bilgi için bkz: [Linux VM boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Bölgeler
Doğu Asya hariç olmak üzere tüm ortak Azure bölgeleri'nde kullanılabilir.   Azure Bulutu henüz desteklenmiyor.

## <a name="limitations"></a>Sınırlamalar
Bu özelliği kullanırken aşağıdaki sınırlamalar bulunmaktadır:

* **Ağ arabirimi oluşturma:** hızlandırılmış ağ yalnızca yeni bir NIC için etkinleştirilmesi Varolan bir NIC için etkinleştirilemez
* **VM oluşturma:** etkin hızlandırılmış ağ ile bir NIC yalnızca eklenebilir bir VM VM oluşturulduğunda. NIC için mevcut bir VM'yi eklenemiyor. VM için mevcut bir kullanılabilirlik ekleme ayarlarsanız, kullanılabilirlik kümesindeki tüm sanal makineleri de etkin ağ hızlandırılmış gerekir.
* **Yalnızca aracılığıyla Azure Resource Manager dağıtımı:** hızlandırılmış ağ ile sanal makineleri (Klasik) dağıtılamıyor.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

En son yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı kullanarak oturum açma [az oturum açma](/cli/azure/#az_login). Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *myNic*, ve *myVm*.

[az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *centralus* konumu:

```azurecli
az group create --name myResourceGroup --location centralus
```

Listelenen desteklenen bir Linux bölge seçmelisiniz [Linux hızlandırılmış ağ](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview).

[az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVnet* bir alt ağ ile:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma
Bir ağ güvenlik grubu oluşturma [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create). Aşağıdaki örnek *myNetworkSecurityGroup* adında bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Ağ güvenlik grubu biri Internet'ten gelen tüm erişim devre dışı bırakır, çeşitli varsayılan kurallar içerir. Sanal makine ile SSH erişmesine izin vermek için bir bağlantı noktasını açmak [az ağ nsg kuralını](/cli/azure/network/nsg/rule#az_network_nsg_rule_create):

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

## <a name="create-a-network-interface-with-accelerated-networking"></a>Bir ağ arabirimi ile hızlandırılmış ağ oluşturma

[az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) komutu ile bir genel IP adresi oluşturun. Sanal makine Internet'ten erişmek için ancak bu makaledeki adımları tamamlamak için planlamıyorsanız bir ortak IP adresi gerekli değilse, gerekli değildir.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Bir ağ arabirimi oluştur [az ağ NIC oluşturmak](/cli/azure/network/nic#az_network_nic_create) etkin hızlandırılmış ağ ile. Aşağıdaki örnek adlı ağ arabirimi oluşturur *myNic* içinde *mySubnet* alt *myVnet* sanal ağ ile ilişkilendirilmiş bir  *myNetworkSecurityGroup* ağ arabirimi için ağ güvenlik grubu:

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

## <a name="create-a-vm-and-attach-the-nic"></a>Bir VM oluşturun ve NIC ekleme
NIC belirtin VM oluşturduğunuzda ile oluşturulan `--nics`. Bir boyut seçin ve dağıtım listelenen [Linux hızlandırılmış ağ](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview). 

[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* UbuntuLTS görüntü ve hızlandırılmış ağ destekleyen boyutu (*Standard_DS4_v2*):

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

Tüm VM boyutları ve özellikleri listesi için bkz: [Linux VM boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

VM oluşturulduktan sonra aşağıdaki çıktı örneği ile benzer bir çıktı döndürülür. Not edin **Publicıpaddress**. Bu adres, sonraki adımlarda VM erişmek için kullanılır.

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

## <a name="confirm-that-accelerated-networking-is-enabled"></a>Hızlandırılmış ağ etkinleştirilmiş olduğunu doğrulayın

VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. Değiştir `<your-public-ip-address>` sanal için atanan ortak IP adresiyle makine oluşturduğunuz ve Değiştir *azureuser* için farklı bir değer kullandıysanız `--admin-username` VM oluşturduğunuzda.

```bash
ssh azureuser@<your-public-ip-address>
```

Bash Kabuğu'ndan girin `uname -r` ve çekirdek sürümü aşağıdaki sürümlerinden birine veya daha büyük olduğundan emin olun:

* **Ubuntu 16.04**: 4.11.0-1013
* **SLES SP3**: 4.4.92-6.18
* **RHEL**: 7.4.2017120423
* **CentOS**: 7.4.20171206


Mellanox VF aygıt VM ile gösterilir onaylayın `lspci` komutu. Döndürülen çıkışı aşağıdaki çıkış benzer:

```bash
0000:00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
0000:00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
0000:00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
0000:00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
0000:00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA
0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
```

Olup olmadığını denetleyin (sanal işlev) VF faaliyete ile `ethtool -S eth0 | grep vf_` komutu. Alırsanız, çıktı hızlandırılmış ağ devre dışıdır ve çalışma aşağıdaki örneğe benzer çıktı.

```bash
vf_rx_packets: 992956
vf_rx_bytes: 2749784180
vf_tx_packets: 2656684
vf_tx_bytes: 1099443970
vf_tx_dropped: 0
```
Hızlandırılmış ağ, VM için etkinleştirilmiştir.
