---
title: "Azure VM ad çözümlemesi için iç DNS kullanarak | Microsoft Docs"
description: "Azure VM ad çözümlemesi için iç DNS kullanarak."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.openlocfilehash: bfba2cf38a0624e8480a32bf153f391d820da5a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a>Azure VM ad çözümlemesi için iç DNS kullanma

Bu makalede, sanal NIC kartları (Vnıc) ve DNS etiket adları kullanarak Linux VM'ler için statik iç DNS adlarını ayarlamak nasıl gösterir. Statik DNS adları, bu belge için kullanılan bir Jenkins yapı sunucusu veya Git sunucusu gibi kalıcı altyapı hizmetleri için kullanılır.

Gereksinimler şunlardır:

* [Bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)
* [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#quick-commands) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="quick-commands"></a>Hızlı komutlar

Hızlı bir şekilde görevi gerçekleştirmek gerekiyorsa, aşağıdaki bölümde gerekli komutları ayrıntıları verilmektedir. Her adım, belgenin geri kalanında bulunabilir bilgi ve içerik daha ayrıntılı [burada başlangıç](#detailed-walkthrough).  

Ön gereksinimlerini: SSH ile kaynak grubu, VNet, NSG gelen, alt ağ.

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a>Statik iç DNS adı ile bir Vnıc'teki oluşturma

`-r` CLI bayrağı olduğundan statik Vnıc DNS adını sağlayan DNS etiketi ayarlamak için.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-the-vm-into-the-vnet-nsg-and-connect-the-vnic"></a>VNet, NSG VM dağıtmak ve Vnıc bağlanın

`-N` Vnıc Azure'a dağıtımı sırasında yeni VM bağlanır.

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

Bir tam sürekli tümleştirme ve sürekli dağıtımı (CiCd) altyapısı Azure ile ilgili belirli sunucuların statik veya uzun süreli sunucusu olmasını gerektirir.  Sanal ağlar (Vnet'ler) ve ağ güvenlik grupları (Nsg'ler) gibi Azure varlıklar statik olmalıdır ve nadiren dağıtılan kaynakları uzun ömürlü önerilir.  Bir VNet dağıtıldıktan sonra hiçbir olumsuz etkiler altyapısı olmadan yeni dağıtımlar tarafından yeniden.  Bu statik ağa bir Git deposu sunucusu ve Jenkins Otomasyon sunucusu ekleme CiCd geliştirme veya test ortamınızı sunar.  

İç DNS adları, yalnızca bir Azure sanal ağı içinde çözülebilir.  DNS adları iç olduğundan, bunlar altyapısına ek güvenlik sağlamaya dış internet çözülebilir değildir.

_Tüm örnekleri kendi adlandırma ile değiştirin._

## <a name="create-the-resource-group"></a>Kaynak grubu oluştur

Bir kaynak grubu, bu kılavuzda oluşturuyoruz her şeyi düzenlemek için gereklidir.  Azure kaynak grupları hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-the-vnet"></a>Sanal ağ oluşturma

İlk adım, içine sanal makineleri başlatmak için bir VNet oluşturmaktır.  Sanal ağ Bu izlenecek yol için bir alt ağ içerir.  Azure sanal ağlar hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak bir sanal ağ oluşturma](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-the-nsg"></a>NSG oluşturma

Biz NSG önce alt ağ oluşturmak için varolan bir ağ güvenlik grubu alt oluşturulmuştur.  Azure Nsg'ler bir Güvenlik Duvarı'nı ağ katmanında eşdeğerdir.  Azure Nsg'ler hakkında daha fazla bilgi için bkz: [Nsg'ler Azure CLI oluşturma](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Gelen bir SSH izin ver Kuralı Ekle

Gelen bağlantı noktası 22 trafiği ağ üzerinden bağlantı noktası 22 Linux VM'de geçirilmesine izin veren bir kural gerektiği şekilde Linux VM internet erişimi olmalıdır.

```azurecli
azure network nsg rule create inboundSSH \
--resource-group myResourceGroup \
--nsg-name myNSG \
--access Allow \
--protocol Tcp \
--direction Inbound \
--priority 100 \
--source-address-prefix * \
--source-port-range * \
--destination-address-prefix 10.10.0.0/24 \
--destination-port-range 22
```

## <a name="add-a-subnet-to-the-vnet"></a>Sanal ağ için bir alt ağ Ekle

Sanal ağ içindeki VM'ler bir alt ağda bulunması gerekir.  Her sanal ağ birden çok alt ağa sahip olabilir.  Alt ağ oluşturmak ve alt ağ alt ağı için bir Güvenlik Duvarı'nı eklemek için NSG ile ilişkilendirin.

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

Alt ağ içinde VNet eklenen ve NSG ve NSG kuralı ile ilişkili.

## <a name="creating-static-dns-names"></a>Statik DNS adları oluşturuluyor

Azure çok esnektir, ancak sanal makineleri ad çözümlemesi için DNS adlarını kullanmak için DNS etiketleme kullanarak sanal ağ kartları (VNics) oluşturmanız gerekir.  Bunları bunları farklı VM'ler için sanal makineleri geçici olarak olabileceği, statik bir kaynak olarak Vnıc tutan bağlanarak yeniden gibi VNics önemlidir.  VNic üzerinde etiketleme DNS kullanarak, biz basit ad çözümlemesi VNet içindeki diğer vm'lerden etkinleştiremezsiniz.  Çözümlenebilir adları kullanarak sağlayan Otomasyon sunucusu DNS adı tarafından erişmek diğer VM'ler `Jenkins` veya Git sunucusu olarak `gitrepo`.  Bir Vnıc'teki oluşturun ve önceki adımda oluşturduğunuz alt ağ ile ilişkilendirin.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>VNet ve NSG halinde VM dağıtma

Şimdi bir sanal ağ, bir alt ağ için SSH bağlantı noktası 22 dışındaki tüm gelen trafiği engelleyerek bizim alt korumak için bu sanal ağ ve güvenlik duvarı olarak davranan bir NSG içinde sunuyoruz.  VM şimdi bu mevcut ağ altyapınızda içinde dağıtılabilir.

Azure CLI kullanarak ve `azure vm create` komutu, var olan Azure kaynak grubu, sanal ağ, alt ağ ve Vnıc için Linux VM dağıtılır.  Tam bir VM'yi dağıtmak için CLI kullanma ile ilgili daha fazla bilgi için bkz: [Azure CLI kullanarak eksiksiz bir Linux ortamı oluşturma](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

Var olan kaynakları çağırmak için CLI bayrakları kullanarak, biz VM mevcut bir ağ içinde dağıtmak için Azure isteyin.  Bir VNet ve alt ağ dağıtıldıktan sonra yinelemek için bunlar Azure bölgesi içinde statik veya kalıcı kaynaklar olarak bırakılabilir.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure CLI'si komutlarını doğrudan kullanarak bir Linux VM'si için kendi özel ortamınızı oluşturun](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Şablonları kullanarak Azure'da bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
