---
title: "Azure sanal ağlar ve Linux sanal makineleri | Microsoft Docs"
description: "Öğretici - Azure sanal ağlar ve Azure CLI ile Linux sanal makineleri yönetme"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a49b4c2d4ddd6d686675cee53d46cd4dd6ad3811
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a>Azure sanal ağlar ve Azure CLI ile Linux sanal makineleri yönetme

Azure sanal makineler, iç ve dış ağ iletişimi için Azure ağ kullanın. Bu öğreticide iki sanal makine dağıtma ve bu VM'ler için Azure ağı yapılandırma açıklanmaktadır. Bir uygulama öğreticide dağıtılmamış ancak bu öğreticide örneklerde, sanal makineleri bir veritabanı arka uç, web uygulamasıyla barındırıyorsanız varsayılmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir sanal ağ ve alt ağ oluşturun
> * Genel IP adresi oluşturma
> * Bir ön uç VM oluşturma
> * Ağ trafiğinin güvenliğini sağlayın
> * Bir arka uç VM oluşturma

Bu öğreticiyi tamamlamak sırasında oluşturulan bu kaynakları görebilirsiniz:

![İki alt ağ ile sanal ağ](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* -birbirine ve internet ile iletişim kurmak için sanal makineleri kullanan sanal ağ.
- *myFrontendSubnet* -alt ağda *myVNet* ön uç kaynaklar tarafından kullanılır.
- *myPublicIPAddress* -kullanılan genel IP adresine erişimi *myFrontendVM* internet'ten.
- *myFrontentNic* -tarafından kullanılan ağ arabirimini *myFrontendVM* ile iletişim kurmak için *myBackendVM*.
- *myFrontendVM* -VM Internet arasında iletişim kurmak için kullanılır ve *myBackendVM*.
- *myBackendNSG* -arasındaki iletişimi denetleyen ağ güvenlik grubu *myFrontendVM* ve *myBackendVM*.
- *myBackendSubnet* -alt ağ ile ilişkili *myBackendNSG* ve arka uç kaynaklar tarafından kullanılır.
- *myBackendNic* -tarafından kullanılan ağ arabirimini *myBackendVM* ile iletişim kurmak için *myFrontendVM*.
- *myBackendVM* -bağlantı noktası 22 ve 3306 ile iletişim kurmak için kullandığı VM *myFrontendVM*.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>VM ağ genel bakış

Azure sanal ağlar arasında sanal makineleri, internet ve diğer Azure hizmetleriyle Azure SQL veritabanı gibi güvenli ağ bağlantıları etkinleştirin. Sanal ağlar alt ağ olarak adlandırılan mantıksal parçalara bölünür. Alt ağ akış denetimi ve güvenlik sınırı olarak kullanılır. Bir VM dağıtırken, genellikle bir alt ağa bağlı bir sanal ağ arabirimi içerir.

## <a name="create-a-virtual-network-and-subnet"></a>Bir sanal ağ ve alt ağ oluşturun

Bu öğreticide, tek bir sanal ağı iki alt ağ ile oluşturulur. Bir web uygulamasını barındırmak için bir ön uç alt ağı ve bir veritabanı sunucusunu barındırmak için bir arka uç alt ağ.

Bir sanal ağ oluşturmadan önce bir kaynak grubuyla oluşturmanız [az grubu oluşturma](/cli/azure/group#create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myRGNetwork* eastus konumda.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Sanal ağ oluşturma

Kullanım [az ağ vnet oluşturma](/cli/azure/network/vnet#create) bir sanal ağ oluşturmak için komutu. Bu örnekte, adlandırılmış ağ *mvVNet* ve bir adres öneki belirtilen *10.0.0.0/16*. Bir alt ağ da bir ad oluşturulur *myFrontendSubnet* ve bir önek *10.0.1.0/24*. Daha sonra Bu öğreticide bir ön uç VM bu alt ağa bağlanır. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVNet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myFrontendSubnet \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Alt ağ oluşturma

Sanal ağ kullanmaya yeni bir alt ağ eklenen [az ağ sanal alt oluşturmak](/cli/azure/network/vnet/subnet#create) komutu. Bu örnekte, alt ağ olarak adlandırılır *myBackendSubnet* ve bir adres öneki belirtilen *10.0.2.0/24*. Bu alt ağ ile tüm arka uç hizmetlerini kullanılır.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --address-prefix 10.0.2.0/24
```

Bu noktada, ağ oluşturulduğundan ve iki alt ağ, ön uç Hizmetleri için bir tane ve arka uç hizmetlerini için başka bir içine bölümlenmiş. Sonraki bölümde, sanal makineleri oluşturulur ve bu alt ağlara bağlı.

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Bir ortak IP adresi Internet üzerinden erişilebilir olması Azure kaynaklarını sağlar. Genel IP adresi ayırma yöntemi dinamik veya statik olarak yapılandırılmış olmalıdır. Varsayılan olarak, bir ortak IP adresi dinamik olarak ayrılır. Bir VM serbest bırakıldığında dinamik IP adresleri serbest bırakılır. Bu davranış VM ayırmayı kaldırma içeren herhangi bir işlem sırasında değiştirmek IP adresi neden olur.

IP adresi deallocated durumundayken bile bir VM için atanan kalmasını sağlar statik ayırma yöntemi ayarlanabilir. Statik olarak ayrılmış bir IP adresi kullanıldığında, IP adresi belirtilemez. Bunun yerine, kullanılabilir adresler havuzundan tahsis edilir.

```azurecli-interactive
az network public-ip create --resource-group myRGNetwork --name myPublicIPAddress
```

Bir VM oluştururken [az vm oluşturma](/cli/azure/vm#create) varsayılan genel IP adresi ayırma yöntemi dinamik komutu. Kullanarak bir sanal makine oluştururken [az vm oluşturma](/cli/azure/vm#create) içeriyor, komut `--public-ip-address-allocation static` bir statik genel IP adresi atamak için bağımsız değişken. Sonraki bölümde dinamik olarak ayrılan bir IP adresi statik olarak ayrılan adresine değiştirilir ancak bu öğreticide, bu işlem gösterilmez. 

### <a name="change-allocation-method"></a>Ayırma yöntemini değiştirme

IP adresi ayırma yöntemi kullanılarak değiştirilebilir [az ağ ortak IP güncelleştirmesi](/cli/azure/network/public-ip#update) komutu. Bu örnekte, ön uç sanal IP adresi ayırma yöntemi statik olarak değiştirilir.

İlk olarak, VM serbest bırakma.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontendVM
```

Kullanım [az ağ ortak IP güncelleştirmesi](/cli/azure/network/public-ip#update) ayırma yöntemi güncelleştirmek için komutu. Bu durumda, `--allocation-method` ayarlanmış *statik*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myPublicIPAddress --allocation-method static
```

VM Başlat.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontendVM --no-wait
```

### <a name="no-public-ip-address"></a>Ortak IP adresi yok

Genellikle, bir VM Internet üzerinden erişilebilir olması gerekmez. Bir ortak IP adresi olmadan bir VM oluşturmak için kullanmak `--public-ip-address ""` bağımsız değişkeni çift tırnak işareti boş dizi. Bu yapılandırmayı daha sonra Bu öğreticide gösterilmiştir.

## <a name="create-a-front-end-vm"></a>Bir ön uç VM oluşturma

Kullanım [az vm oluşturma](/cli/azure/vm#create) adlı VM oluşturmak için komutu *myFrontendVM* kullanarak *myPublicIPAddress*.

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontendVM \
  --vnet-name myVNet \
  --subnet myFrontendSubnet \
  --nsg myFrontendNSG \
  --public-ip-address myPublicIPAddress \
  --image UbuntuLTS \
  --generate-ssh-keys
```

## <a name="secure-network-traffic"></a>Ağ trafiğinin güvenliğini sağlayın

Ağ güvenlik grubu (NSG), Azure Sanal Ağlara (VNet) bağlı kaynaklara ağ trafiğine izin veren veya reddeden güvenlik kurallarının listesini içerir. Nsg'ler alt ağları veya tek tek ağ arabirimleri için ilişkili olabilir. Bir NSG'yi bir ağ arabirimi ile ilişkili olduğunda, yalnızca ilişkili VM geçerlidir. Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur. 

### <a name="network-security-group-rules"></a>Ağ güvenlik grubu kuralları

NSG kuralları üzerinden trafik izin verilen veya reddedilen ağ bağlantı noktalarını tanımlar. Böylece belirli sistemleri veya alt ağlar arasında trafiği denetlenir kuralları kaynak ve hedef IP adresi aralıklarını içerebilir. NSG kuralları da dahil bir öncelik (1 arasında — ve 4096). Kurallar öncelik sırasına göre değerlendirilir. 100 önceliğine sahip bir kural 200 önceliğine sahip bir kural önce değerlendirilir.

Tüm NSG'ler bir varsayılan kurallar kümesini içerir. Varsayılan kurallar silinemez ancak en düşük önceliğe atanmış oldukları için sizin oluşturduğunuz kurallar tarafından geçersiz kılınabilirler.

- **Sanal ağ** - kaynaklanan trafiği ve sanal ağ içinde bitiş hem gelen ve giden yönlerde izin verilir.
- **Internet** - giden trafiğe izin verilir, ancak gelen trafik engellenir.
- **Yük Dengeleyici** -VM'ler ve rol örneklerinin durumunu araştırma için izin Azure'nın yük dengeleyici. Yük dengelenmiş bir küme kullanmıyorsanız bu kuralı geçersiz kılabilirsiniz.

### <a name="create-network-security-groups"></a>Ağ güvenlik grupları oluşturma

Kullanarak bir VM olarak aynı zamanda bir ağ güvenlik grubu oluşturulabilir [az vm oluşturma](/cli/azure/vm#create) komutu. Bunun yapılması, NSG VM'ler ağ arabirimiyle ilişkilendirilmiş ve bir NSG kuralı otomatik olarak bağlantı noktası üzerinde trafiğe izin vermek için oluşturulan *22* herhangi bir kaynaktan. Bu öğreticide daha önce ön uç VM ile otomatik olarak oluşturulan ön uç NSG. Bir NSG kuralı da otomatik olarak oluşturulan için bağlantı noktası 22 oluştu. 

Bazı durumlarda, ne zaman varsayılan SSH kuralları oluşturulmaması gerektiğini veya ne zaman NSG bir alt ağa bağlı olması gibi bir NSG önceden oluşturmak yararlı olabilir. 

Kullanım [az ağ nsg oluşturma](/cli/azure/network/nsg#create) bir ağ güvenlik grubu oluşturmak için komutu.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myBackendNSG
```

Bir ağ arabirimi NSG'yi ilişkilendirme yerine bir alt ağ ile ilişkili. Bu yapılandırmada alt ağına bağlı olduğu VM NSG kuralları devralır.

Adlı mevcut alt güncelleştirme *myBackendSubnet* yeni NSG ile.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --network-security-group myBackendNSG
```

### <a name="secure-incoming-traffic"></a>Gelen trafiği güvenli

Ön uç VM oluşturulduğunda, bir NSG kuralı bağlantı noktası 22 gelen trafiğe izin verecek şekilde oluşturuldu. Bu kural, VM SSH bağlantılara izin verir. Bu örnekte, trafiğin de bağlantı noktası izin verilmesi gerektiğini *80*. Bu yapılandırma VM erişilecek bir web uygulaması sağlar.

Kullanım [az ağ nsg kuralını](/cli/azure/network/nsg/rule#create) bağlantı noktası için bir kural oluşturmak için komutu *80*.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myFrontendNSG \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

Ön uç VM yalnızca bağlantı noktası üzerinde erişilebilir *22* ve bağlantı noktası *80*. Diğer tüm gelen trafiği ağ güvenlik grubu engellendi. NSG kuralı yapılandırmaları görselleştirmek yararlı olabilir. NSG kuralının yapılandırmayla dönmek [az ağ kuralı listesi](/cli/azure/network/nsg/rule#list) komutu. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myFrontendNSG --output table
```

### <a name="secure-vm-to-vm-traffic"></a>VM VM trafiğinin güvenliğini

Ağ güvenlik grubu kuralları da VM'ler arasında uygulayabilirsiniz. Bu örnekte, ön uç VM arka uç VM bağlantı noktası ile iletişim kurması gereken *22* ve *3306*. Bu yapılandırma, ön uç sanal makineden SSH bağlantılarına izin veren ve bir uygulama bir arka uç MySQL veritabanı ile iletişim kurmak için ön uç VM'de de olanak sağlar. Diğer tüm trafik ön uç ve arka uç sanal makineler arasında engellenmelidir.

Kullanım [az ağ nsg kuralını](/cli/azure/network/nsg/rule#create) bağlantı noktası 22 için bir kural oluşturmak için komutu. Dikkat `--source-address-prefix` bağımsız değişken değerini belirtir *10.0.1.0/24*. Bu yapılandırma, NSG yalnızca ön uç alt ağından gelen trafiğe izin verildiğini sağlar.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Şimdi 3306 noktasından MySQL trafiği için bir kural ekleyin.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

Son olarak, aynı sanal ağ içindeki VM'ler arasında tüm trafiğe izin veren bir varsayılan kuralı Nsg'ler sahip olduğundan, bir kural tüm trafiği engellemek arka uç Nsg'ler için oluşturulabilir. Burada dikkat `--priority` değerini verilen *300*, hangi, NSG ve MySQL kuralları alt olduğu. Bu yapılandırma, SSH ve MySQL trafiği NSG izin verilir sağlar.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

## <a name="create-back-end-vm"></a>Arka uç VM oluşturma

Şimdi eklendiği bir sanal makine, oluşturmak *myBackendSubnet*. Dikkat `--nsg` bağımsız değişkeni boş çift tırnak işareti değerine sahip. Bir NSG'yi VM ile oluşturulmuş gerekmez. VM ile önceden oluşturulmuş arka uç NSG korumalı arka uç alt ağına bağlı. Bu NSG VM için geçerlidir. Ayrıca, burada dikkat `--public-ip-address` bağımsız değişkeni boş çift tırnak işareti değerine sahip. Bu yapılandırma, bir ortak IP adresi olmadan bir VM oluşturur. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackendVM \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Arka uç VM yalnızca bağlantı noktası üzerinde erişilebilir *22* ve bağlantı noktası *3306* ön uç alt ağından. Diğer tüm gelen trafiği ağ güvenlik grubu engellendi. NSG kuralı yapılandırmaları görselleştirmek yararlı olabilir. NSG kuralının yapılandırmayla dönmek [az ağ kuralı listesi](/cli/azure/network/nsg/rule#list) komutu. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myBackendNSG --output table
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide oluşturduğunuz ve Azure ağları sanal makinelerle ilgili olarak güvenli. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir sanal ağ ve alt ağ oluşturun
> * Genel IP adresi oluşturma
> * Bir ön uç VM oluşturma
> * Ağ trafiğinin güvenliğini sağlayın
> * Arka uç VM oluşturma

Azure Yedekleme'yi kullanarak sanal makinelerde verilerin güvenliğini sağlama hakkında bilgi edinmek için sonraki öğretici ilerleyin. 

> [!div class="nextstepaction"]
> [Azure'daki Linux sanal makineleri yedekleyin](./tutorial-backup-vms.md)
