---
title: Yük Dengelemesi Azure CLI kullanarak bölge olarak yedekli VM'ler | Microsoft Docs
description: Bir genel yük dengeleyiciye standart Azure CLI kullanarak bölge olarak yedekli ön uç ile oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/09/2018
ms.author: kumud
ms.openlocfilehash: 29dcfaad840b5498dd859082ce11655a4f1fe8af
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
#  <a name="load-balance-vms-across-all-availability-zones-using-azure-cli"></a>Azure CLI kullanarak tüm kullanılabilirlik bölgeler arasında Yük Dengeleme VM'ler

Bu makalede adımları genel oluşturmada size [yük dengeleyici standart](https://aka.ms/azureloadbalancerstandard) bölge artıklık birden çok DNS kaydı bağımlılığını olmadan elde etmek için bir bölge olarak yedekli ön ile. Tek bir ön uç IP adresi otomatik olarak bölge olarak yedekli ' dir.  Tek bir IP adresi ile yük dengeleyici için bir bölge olarak yedekli ön kullanarak tüm kullanılabilirlik bölgeler arasında olan bir bölge içindeki bir sanal ağdaki tüm VM artık ulaşabilir. Uygulamalarınızı beklenmeyen hatalardan veya tüm veri merkezinin kaybedilmesinden korumak için kullanılabilirlik alanlarından yararlanın.

Kullanılabilirlik bölgeleri standart yük dengeleyici ile kullanma hakkında daha fazla bilgi için bkz: [standart yük dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.17 çalıştırmasını gerektirir ya da daha yüksek.  Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Kullanılabilirlik bölgeler için destek, select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupSLB* içinde *westeurope* konumu:

```azurecli-interactive
az group create \
--name myResourceGroupSLB \
--location westeurope
```

## <a name="create-a-zone-redundant-public-ip-standard"></a>Bir bölge oluşturmanız yedekli ortak IP standart
Uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. Bir bölge olarak yedekli ön uç bir bölgedeki tüm kullanılabilirlik bölgeler tarafından eşzamanlı olarak sunulur. Bölge olarak yedekli genel bir IP adresi ile oluşturma [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Bir standart genel IP adresi oluşturduğunuzda, bölge olarak yedekli varsayılan olarak kullanılabilir.

Aşağıdaki örnek adlı bir bölge olarak yedekli genel IP adresi oluşturur *myPublicIP* içinde *myResourceGroupLoadBalancer* kaynak grubu.

```azurecli-interactive
az network public-ip create \
--resource-group myResourceGroupSLB \
--name myPublicIP \
--sku Standard
```

## <a name="create-azure-load-balancer-standard"></a>Azure yük dengeleyici standart oluşturma
Bu bölümde, nasıl oluşturma ve yük dengeleyici aşağıdaki bileşenlerden yapılandırma ayrıntıları:
- Yük dengeleyicideki gelen ağ trafiğini alan bir ön uç IP havuzu.
- ön uç havuzu yük yere gönderir bir arka uç IP havuzu, ağ trafiğini dengeli.
- arka uç VM örnekleri durumunu belirleyen bir durumu araştırması.
- trafiğin Vm'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici kuralı.

### <a name="create-the-load-balancer"></a>Yük Dengeleyici oluşturma
Bir standart yük dengeleyici ile oluşturma [az ağ lb oluşturma](/cli/azure/network/lb#az_network_lb_create). Aşağıdaki örnek, adlandırılmış bir yük dengeleyici oluşturur *myLoadBalancer* ve atar *myPublicIP* adres ön uç IP yapılandırmasını.

```azurecli-interactive
az network lb create \
--resource-group myResourceGroupSLB \
--name myLoadBalancer \
--public-ip-address myPublicIP \
--frontend-ip-name myFrontEnd \
--backend-pool-name myBackEndPool \
--sku Standard
```

## <a name="create-health-probe-on-port-80"></a>Bağlantı noktası 80 üzerinde durumu araştırması oluştur

Sistem durumu araştırması tüm sanal makine örneklerini denetleyerek ağ trafiği gönderdiklerinden emin olur. Sistem durumu denetimi başarısız olan sanal makine örnekleri tekrar çevrimiçi olana ve sistem durumu denetimi iyi olduğuna karar verene kadar yük dengeleyiciden kaldırılır. Bir sistem durumu araştırması oluşturmak sanal makinelerin sağlığını izlemek için az ağ lb araştırmasıyla oluşturun. TCP durum araştırması oluşturmak için [az network lb probe create](/cli/azure/network/lb/probe#az_network_lb_probe_create) komutunu kullanın. Aşağıdaki örnek *myHealthProbe* adında bir durum araştırması oluşturur:

```azurecli-interactive
az network lb probe create \
--resource-group myResourceGroupSLB \
--lb-name myLoadBalancer \
--name myHealthProbe \
--protocol tcp \
--port 80
```

## <a name="create-load-balancer-rule-for-port-80"></a>Bağlantı noktası 80 için yük dengeleyici kuralı oluşturma
Yük Dengeleyici kuralı gelen trafiği ve gerekli kaynak ve hedef bağlantı noktası ile birlikte trafiği almak için arka uç IP havuzu için ön uç IP yapılandırmasını tanımlar. Yük Dengeleyici kuralı oluşturma *myLoadBalancerRuleWeb* ile [az ağ lb kuralını](/cli/azure/network/lb/rule#az_network_lb_rule_create) ön uç havuzunda 80 numaralı bağlantı noktasını dinlemek için *myFrontEndPool* ve gönderme Yük dengeli ağ trafiği için arka uç adres havuzuna *myBackEndPool* ayrıca 80 numaralı bağlantı noktasını kullanıyor.

```azurecli-interactive
az network lb rule create \
--resource-group myResourceGroupSLB \
--lb-name myLoadBalancer \
--name myLoadBalancerRuleWeb \
--protocol tcp \
--frontend-port 80 \
--backend-port 80 \
--frontend-ip-name myFrontEnd \
--backend-pool-name myBackEndPool \
--probe-name myHealthProbe
```

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma
Bazı sanal makineleri dağıtmak ve yük dengeleyici sınayabilirsiniz önce destekleyici sanal ağ kaynakları oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Adlı bir sanal ağ oluşturma *myVnet* adlı bir alt ağ ile *mySubnet* myResourceGroup kullanarak [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create).


```azurecli-interactive
az network vnet create \
--resource-group myResourceGroupSLB \
--location westeurope \
--name myVnet \
--subnet-name mySubnet
```

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Adlı ağ güvenlik grubu oluşturun *myNetworkSecurityGroup* gelen bağlantıları sanal ağınızla tanımlamak için [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create).

```azurecli-interactive
az network nsg create \
--resource-group myResourceGroupSLB \
--name myNetworkSecurityGroup
```

Adlı ağ güvenlik grubu kural oluşturma *myNetworkSecurityGroupRule* bağlantı noktası 80 ile [az ağ nsg kuralını](/cli/azure/network/nsg/rule#az_network_nsg_rule_create).

```azurecli-interactive
az network nsg rule create \
--resource-group myResourceGroupSLB \
--nsg-name myNetworkSecurityGroup \
--name myNetworkSecurityGroupRule \
--protocol tcp \
--direction inbound \
--source-address-prefix '*' \
--source-port-range '*' \
--destination-address-prefix '*' \
--destination-port-range 80 \
--access allow \
--priority 200
```
### <a name="create-nics"></a>NIC’leri oluşturma
İle üç sanal Nıcs'yi oluşturmak [az ağ NIC oluşturmak](/cli/azure/network/nic#az_network_nic_create) ve genel IP adresi ve ağ güvenlik grubu ile ilişkilendirin. Aşağıdaki örnekte, altı sanal NIC oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC). İstediğiniz zaman ek sanal NIC’ler ve VM’ler oluşturabilir ve bunları yük dengeleyiciye ekleyebilirsiniz:

```azurecli-interactive
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupSLB \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```
## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma
Bu örnekte, bölge 1, 2 bölge ve bölge için yük dengeleyici arka uç sunucuları olarak kullanılacak 3 bulunan üç sanal makine oluşturun. Ayrıca sanal makinelere yük dengeleyici başarıyla oluşturulduğunu doğrulamak için NGINX yükleyin.

### <a name="create-cloud-init-config"></a>cloud-init yapılandırması oluşturma

NGINX yüklemek ve 'Hello World' Node.js uygulaması Linux sanal makine üzerinde çalıştırmak için bir bulut init yapılandırma dosyası kullanabilirsiniz. Geçerli Kabuğu'nda bulut init.txt ve kopyalama adlı bir dosya oluşturun ve aşağıdaki yapılandırma kabuğundan yapıştırın. Bütün kopyaladığınızdan emin olun bulut init dosya doğru özellikle ilk satırı:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-the-zonal-virtual-machines"></a>Zonal sanal makineler oluşturma
VM'lerin oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create) bölge 1, 2 bölge ve bölge 3. Aşağıdaki örnekte, her bölgede bir VM oluşturur ve zaten mevcut değilse SSH anahtarları üretir:

1 bölgesinde VM'ler oluşturma

```azurecli-interactive
 az vm create \
--resource-group myResourceGroupSLB \
--name myVM$i \
--nics myNic$i \
--image UbuntuLTS \
--generate-ssh-keys \
--zone $i \
--custom-data cloud-init.txt
```
## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Yük Dengeleyici kullanarak genel IP adresi al [az ağ ortak IP Göster](/cli/azure/network/public-ip#az_network_public_ip_show). 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupSLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
``` 
Daha sonra genel IP adresini bir web tarayıcısına girebilirsiniz. Yük dengeleyicinin VM’lere trafik dağıtmaya başlamadan önce VM’lerin hazır olması için birkaç dakika geçmesi gerektiğini unutmayın. Uygulama, yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adı ile aşağıdaki gibi görüntülenir:

![Node.js uygulaması çalıştırma](./media/load-balancer-standard-public-zone-redundant-cli/running-nodejs-app.png)

Trafik uygulamanızı çalıştıran tüm üç kullanılabilirlik bölgelerinde sanal makineleri dağıtmasına yük dengeleyici görmek için belirli bir bölgedeki bir VM'yi durdurmaya ve tarayıcınızı yenileyin.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [standart yük dengeleyici](./load-balancer-standard-overview.md)



