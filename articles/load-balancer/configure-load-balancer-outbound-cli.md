---
title: Yük Dengeleme ve Azure CLI'yı kullanarak giden kuralları yapılandırın
titlesuffix: Azure Load Balancer
description: Bu makalede, Azure CLI kullanarak nasıl standart yük dengeleyici Yük Dengeleme veya giden kuralları yapılandırılacağı gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: kumud
ms.openlocfilehash: f28088a1a0586964092a0b5f86ce8bf0f95402cd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66122438"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-cli"></a>Standart yük Azure CLI kullanarak dengeleyici Yük Dengeleme ve giden kuralları yapılandırma

Bu hızlı başlangıçta Azure CLI kullanarak nasıl standart yük dengeleyici giden kuralları yapılandırılacağını gösterir.  

İşiniz bittiğinde, yük dengeleyici kaynağı iki ön uçlar ve bunlarla ilişkili kurallar içeriyor: için bir gelen ve giden.  Her ön uç genel IP adresi ve bu senaryo kullanır, farklı bir genel IP adresi için gelen ve giden bir başvuru içeriyor.   Yük Dengeleme kuralını yalnızca gelen Yük Dengeleme sağlar ve giden NAT VM için sağlanan giden kuralı denetler.  Bu hızlı başlangıç kullanan iki ayrı arka uç havuzları için bir gelen, diğeri giden yeteneğini gösterir ve bu senaryo için esneklik sağlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myresourcegroupoutbound* içinde *eastus2* konumu:

```azurecli-interactive
  az group create \
    --name myresourcegroupoutbound \
    --location eastus2
```
## <a name="create-virtual-network"></a>Sanal ağ oluşturma
Adlı bir sanal ağ oluşturma *myvnetoutbound* adlı bir alt ağ ile *mysubnetoutbound* içinde *myresourcegroupoutbound* kullanarak [az ağ vnet oluşturma](https://docs.microsoft.com/cli/azure/network/vnet).

```azurecli-interactive
  az network vnet create \
    --resource-group myresourcegroupoutbound \
    --name myvnetoutbound \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mysubnetoutbound
    --subnet-prefix 192.168.0.0/24
```

## <a name="create-inbound-public-ip-address"></a>Gelen genel IP adresi oluşturma 

Web uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. Standart Yük Dengeleyici yalnızca Standart Genel IP adreslerini destekler. Kullanım [az network public-IP oluşturma](https://docs.microsoft.com/cli/azure/network/public-ip) adlı bir standart genel IP adresi oluşturmak için *mypublicipinbound* içinde *myresourcegroupoutbound*.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipinbound --sku standard
```

## <a name="create-outbound-public-ip-address"></a>Giden genel IP adresi oluşturma 

Load Balancer'ın ön uç giden yapılandırmasını kullanmak için standart IP adresi oluşturma [az network public-IP oluşturma](https://docs.microsoft.com/cli/azure/network/public-ip).

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipoutbound --sku standard
```

## <a name="create-azure-load-balancer"></a>Azure yük dengeleyici oluşturma

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:
  - Yük dengeleyicideki gelen ağ trafiğini alan bir ön uç IP.
  - Bir arka uç havuzu ön uç IP göndereceği yeri Yük Dengelemesi yapılmış ağ trafiğini.
  - Giden bağlantı için bir arka uç havuzu. 
  - arka uç sanal makine örneklerinin durumunu belirleyen bir durum araştırması.
  - Trafiğin sanal makinelere dağıtımını tanımlayan bir yük dengeleyici gelen kuralı.
  - Trafiğin sanal makinelerinden dağıtımını tanımlayan bir yük dengeleyici giden kuralı.

### <a name="create-load-balancer"></a>Yük Dengeleyici oluşturma

Gelen IP adresi kullanarak bir yük dengeleyici oluşturma [az ağ lb oluşturma](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) adlı *lb* gelen ön uç IP yapılandırmasını ve arka uç havuzu içeren *bepoolinbound*genel IP adresiyle ilişkili *mypublicipinbound* , önceki adımda oluşturduğunuz.

```azurecli-interactive
  az network lb create \
    --resource-group myresourcegroupoutbound \
    --name lb \
    --sku standard \
    --backend-pool-name bepoolinbound \
    --frontend-ip-name myfrontendinbound \
    --location eastus2 \
    --public-ip-address mypublicipinbound   
  ```

### <a name="create-outbound-pool"></a>Çıkış havuzu oluşturma

Bir havuz ile VM'lerin için giden bağlantı tanımlamak için bir ek arka uç adres havuzu oluşturma [az ağ lb adres havuzu oluşturma](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) adıyla *bepooloutbound*.  Ayrı bir giden havuz oluşturmak, en üst düzeyde esneklik sağlar, ancak bu adımı atlayabilir ve yalnızca gelen kullanın *bepoolinbound* de.

```azurecli-interactive
  az network lb address-pool create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name bepooloutbound
```

### <a name="create-outbound-frontend-ip"></a>Giden ön uç IP oluşturma
Yük Dengeleyici ile giden ön uç IP yapılandırmasını oluşturun [az network lb frontend-IP oluşturma](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) içeren ve giden ön uç IP yapılandırması adlı *myfrontendoutbound* diğer bir deyişle genel IP adresine ilişkili *mypublicipoutbound*

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myresourcegroupoutbound \
    --name myfrontendoutbound \
    --lb-name lb \
    --public-ip-address mypublicipoutbound 
  ```

### <a name="create-health-probe"></a>Durum araştırması oluşturma

Sistem durumu araştırması tüm sanal makine örneklerini denetleyerek ağ trafiği gönderdiklerinden emin olur. Sistem durumu denetimi başarısız olan sanal makine örnekleri tekrar çevrimiçi olana ve sistem durumu denetimi iyi olduğuna karar verene kadar yük dengeleyiciden kaldırılır. Sanal makinelerin durumunu izlemek için [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) ile bir durum araştırması oluşturun. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name http \
    --protocol http \
    --port 80 \
    --path /  
```

### <a name="create-load-balancing-rule"></a>Yük Dengeleme kuralı oluşturma

Yük Dengeleyici kuralı, gelen trafik ve gerekli kaynak ve hedef bağlantı noktalarının yanı sıra trafiği almak için arka uç havuzu için ön uç IP yapılandırması tanımlar. Bir yük dengeleyici kuralı oluşturun *myinboundlbrule* ile [az ağ lb kuralı oluşturma](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) ön uç havuzunda 80 numaralı bağlantı noktasını dinlemek *myfrontendinbound* ve gönderme arka uç adres havuzuna Yük Dengelemesi yapılmış ağ trafiğini *bepool* de 80 numaralı bağlantı noktasını kullanıyor. 

>[!NOTE]
>Devre dışı bırakma giden-snat parametresiyle birlikte, bu kural nedeniyle otomatik giden (S) NAT bu Yük Dengeleme kuralı devre dışı bırakır. Giden NAT, yalnızca giden kuralı tarafından sağlanır.

```azurecli-interactive
az network lb rule create \
--resource-group myresourcegroupoutbound \
--lb-name lb \
--name inboundlbrule \
--protocol tcp \
--frontend-port 80 \
--backend-port 80 \
--probe http \
--frontend-ip-name myfrontendinbound \
--backend-pool-name bepoolinbound \
--disable-outbound-snat
```

### <a name="create-outbound-rule"></a>Giden kuralı oluşturma

Ön uç tarafından temsil edilen ön uç genel IP, bir giden kuralı tanımlayan *myfrontendoutbound*, doğrulayacak bu kuralın uygulandığı arka uç havuzu yanı sıra tüm giden NAT trafiği için.  Bir giden kuralı oluşturma *myoutboundrule* ağdaki tüm sanal makinelerin (NIC IP yapılandırmaları) giden ağ çeviri *bepool* arka uç havuzu.  Aşağıdaki komutta giden boşta kalma zaman aşımı 4'ten 15 dakika olarak değiştirir ve 10000 SNAT 1024 yerine bağlantı noktası ayırır.  Gözden geçirme [giden kuralları](https://aka.ms/lboutboundrules) daha fazla ayrıntı için.

```azurecli-interactive
az network lb outbound-rule create \
 --resource-group myresourcegroupoutbound \
 --lb-name lb \
 --name outboundrule \
 --frontend-ip-configs myfrontendoutbound \
 --protocol All \
 --idle-timeout 15 \
 --outbound-ports 10000 \
 --address-pool bepooloutbound
```

Ayrı bir giden havuzunu kullanmayı istemiyorsanız, adres havuzu bağımsız değişken belirlemek için önceki komutta değiştirebilirsiniz *bepoolinbound* yerine.  Esneklik ve sonuçta elde edilen yapılandırma okunabilirliğini için ayrı havuzlar kullanılması önerilir.

Bu noktada, sanal makinenizin arka uç havuzuna eklemeye devam *bepoolinbound* __ve__ *bepooloutbound* güncelleştirerek ilgili NIC IP yapılandırması kullanarak kaynakları [az ağ NIC IP-config adres havuzu ekleme](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurecli-interactive 
  az group delete --name myresourcegroupoutbound
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, standart yük dengeleyici oluşturdunuz, hem gelen yük dengeleyici trafik kuralları, yapılandırılmış ve durum yoklaması, arka uç havuzundaki sanal makineleri için yapılandırılmış. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
