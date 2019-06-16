---
title: Azure CLI ile bölgesel olarak yedekli VMs Yük Dengelemesi
titlesuffix: Azure Load Balancer
description: Azure CLI kullanarak bölge yedekli ön uç ile genel bir Standard Load Balancer oluşturma konusunda bilgi edinin
services: load-balancer
documentationcenter: na
author: KumudD
ms.custom: seodec18
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/09/2018
ms.author: kumud
ms.openlocfilehash: 8f1bf9b9070f2db2376de9cb0a0602eaea98b47e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66147688"
---
#  <a name="load-balance-vms-across-all-availability-zones-using-azure-cli"></a>Azure CLI kullanarak tüm kullanılabilirlik alanları genelinde Yük Dengeleme sanal makineleri

Bu makalede adımları genel oluşturma işleminde [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) bölge artıklığı birden çok DNS kayıtlarını bağımlılığı olmadan elde etmek için bölgesel olarak yedekli bir ön uç ile. Tek bir ön uç IP adresi otomatik olarak bölgesel olarak yedekli.  Tek bir IP adresi ile yük dengeleyiciniz için bir bölge yedekli ön uç kullanarak, tüm kullanılabilirlik alanları genelinde bir bölgedeki bir sanal ağdaki herhangi bir VM artık ulaşabilirsiniz. Uygulamalarınızı beklenmeyen hatalardan veya tüm veri merkezinin kaybedilmesinden korumak için kullanılabilirlik alanlarından yararlanın.

Standart Yük Dengeleyici ile Kullanılabilirlik alanlarını kullanma hakkında daha fazla bilgi için [Standart Yük Dengeleyici ve Kullanılabilirlik Alanları](load-balancer-standard-availability-zones.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici, Azure CLI Sürüm 2.0.17 çalıştırdığınız gerektirir veya üzeri.  Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Kullanılabilirlik bölgeleri, seçili Azure kaynakları ve bölgeler ve sanal makine boyutu aileleri için kullanılabilir. Kullanmaya başlamak nasıl daha fazla bilgi ve hangi Azure kaynakları, bölgeleri ve kullanılabilirlik alanları ile deneyebilirsiniz sanal makine boyutu aileleri için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroupSLB* içinde *westeurope* konumu:

```azurecli-interactive
az group create \
--name myResourceGroupSLB \
--location westeurope
```

## <a name="create-a-zone-redundant-public-ip-standard"></a>Bir bölge oluşturduğunuzu yedekli genel IP standart
Uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. Bölge yedekli ön uç bir bölgede tüm kullanılabilirlik alanları ile aynı anda sunulur. Bölge yedekli genel IP adresi ile oluşturma [az network public-IP oluşturma](/cli/azure/network/public-ip#az-network-public-ip-create). Bir standart genel IP adresi oluşturduğunuzda, varsayılan olarak yedekli bölgesidir.

Aşağıdaki örnekte adlı bir bölge yedekli genel IP adresi oluşturur *Mypublicıp* içinde *Mypublicıp* kaynak grubu.

```azurecli-interactive
az network public-ip create \
--resource-group myResourceGroupSLB \
--name myPublicIP \
--sku Standard
```

## <a name="create-azure-standard-load-balancer"></a>Azure Standard Load Balancer oluşturma
Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:
- Yük dengeleyicideki gelen ağ trafiğini alan bir ön uç IP havuzu.
- Ön uç havuzunun yük dengelemesi yapılmış ağ trafiğini gönderdiği bir arka uç IP havuzu.
- Arka uç sanal makine örneklerinin durumunu belirleyen bir durum araştırması.
- Trafiğin sanal makinelere dağıtımını tanımlayan bir yük dengeleyici kuralı.

### <a name="create-the-load-balancer"></a>Yük dengeleyiciyi oluşturma
Bir Standard load balancer ile oluşturma [az ağ lb oluşturma](/cli/azure/network/lb#az-network-lb-create). Aşağıdaki örnekte adlı bir yük dengeleyici oluşturur *myLoadBalancer* ve atar *Mypublicıp* ön uç IP yapılandırmasını adresi.

```azurecli-interactive
az network lb create \
--resource-group myResourceGroupSLB \
--name myLoadBalancer \
--public-ip-address myPublicIP \
--frontend-ip-name myFrontEnd \
--backend-pool-name myBackEndPool \
--sku Standard
```

## <a name="create-health-probe-on-port-80"></a>80 numaralı bağlantı noktasında durum araştırması oluşturma

Sistem durumu araştırması tüm sanal makine örneklerini denetleyerek ağ trafiği gönderdiklerinden emin olur. Sistem durumu denetimi başarısız olan sanal makine örnekleri tekrar çevrimiçi olana ve sistem durumu denetimi iyi olduğuna karar verene kadar yük dengeleyiciden kaldırılır. Durum araştırması oluşturun sanal makinelerin durumunu izlemek için az ağ lb araştırma ile oluşturun. TCP durum araştırması oluşturmak için [az network lb probe create](/cli/azure/network/lb/probe#az-network-lb-probe-create) komutunu kullanın. Aşağıdaki örnek *myHealthProbe* adında bir durum araştırması oluşturur:

```azurecli-interactive
az network lb probe create \
--resource-group myResourceGroupSLB \
--lb-name myLoadBalancer \
--name myHealthProbe \
--protocol tcp \
--port 80
```

## <a name="create-load-balancer-rule-for-port-80"></a>Bağlantı noktası 80 için yük dengeleyici kuralı oluşturun
Yük dengeleyici kuralı, gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırmasını ve trafiği almak için arka uç IP havuzunu tanımlar. *myFrontEndPool* ön uç havuzunda 80 numaralı bağlantı noktasını dinlemek ve yine 80 numaralı bağlantı noktasını kullanarak *myBackEndPool* arka uç adres havuzuna yük dengelemesi yapılmış ağ trafiğini göndermek için [az network lb rule create](/cli/azure/network/lb/rule#az-network-lb-rule-create) ile *myLoadBalancerRuleWeb* yük dengeleyici kuralı oluşturun.

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
VM’leri dağıtmadan ve dengeleyicinizi test etmeden önce yardımcı sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Adlı bir sanal ağ oluşturma *myVnet* adlı bir alt ağ ile *mySubnet* myResourceGroup kullanarak [az ağ sanal ağ oluşturma](/cli/azure/network/vnet#az-network-vnet-create).


```azurecli-interactive
az network vnet create \
--resource-group myResourceGroupSLB \
--location westeurope \
--name myVnet \
--subnet-name mySubnet
```

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Adlı ağ güvenlik grubu oluşturma *Vm2* sanal ağınıza gelen bağlantıları tanımlamak için [az ağ nsg oluşturma](/cli/azure/network/nsg#az-network-nsg-create).

```azurecli-interactive
az network nsg create \
--resource-group myResourceGroupSLB \
--name myNetworkSecurityGroup
```

Adlı bir ağ güvenlik grubu kuralı oluşturma *myNetworkSecurityGroupRule* bağlantı noktası 80 ile [az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule#az-network-nsg-rule-create).

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
Üç sanal NIC ile oluşturma [az ağ NIC oluşturup](/cli/azure/network/nic#az-network-nic-create) ve bunları genel IP adresi ve ağ güvenlik grubu ile ilişkilendirin. Aşağıdaki örnek, altı sanal NIC oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC). İstediğiniz zaman ek sanal NIC’ler ve VM’ler oluşturabilir ve bunları yük dengeleyiciye ekleyebilirsiniz:

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
Bu örnekte, bölge 1, bölge 2 ve bölge 3 yük dengeleyici için arka uç sunucular olarak kullanılacak bulunan üç sanal makine oluşturun. Yük dengeleyicinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere NGINX de yükleyin.

### <a name="create-cloud-init-config"></a>cloud-init yapılandırması oluşturma

cloud-init yapılandırma dosyasını kullanarak NGINX’i yükleyin ve Linux sanal makinesinde 'Merhaba Dünya' Node.js uygulaması çalıştırın. Geçerli kabuğunuzda cloud-init.txt adlı bir dosya oluşturup aşağıdaki yapılandırmayı kopyalayıp kabuğa yapıştırın. Başta birinci satır olmak üzere cloud-init dosyasının tamamını doğru bir şekilde kopyaladığınızdan emin olun:

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

### <a name="create-the-zonal-virtual-machines"></a>Bölgesel bir sanal makine oluşturma
İle Vm'leri oluşturmak [az vm oluşturma](/cli/azure/vm#az-vm-create) bölge 1, bölge 2 ve bölge 3. Aşağıdaki örnek, her bölgede bir VM oluşturur ve zaten mevcut değilse SSH anahtarlarını oluşturur:

Her bölgesinde (bölge 1, bölge 2 ve bölge 3) bir VM oluşturma *westeurope* konumu.

```azurecli-interactive
for i in `seq 1 3`; do
  az vm create \
    --resource-group myResourceGroupSLB \
    --name myVM$i \
    --nics myNic$i \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --zone $i \
    --custom-data cloud-init.txt
done
```
## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Yük Dengeleyici kullanarak genel IP adresini alın [az ağ public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show). 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupSLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
``` 
Sonra da genel IP adresini bir web tarayıcısına girebilirsiniz. Yük dengeleyicinin VM’lere trafik dağıtmaya başlamadan önce VM’lerin hazır olması için birkaç dakika geçmesi gerektiğini unutmayın. Uygulama, yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adı ile aşağıdaki gibi görüntülenir:

![Node.js uygulaması çalıştırma](./media/load-balancer-standard-public-zone-redundant-cli/running-nodejs-app.png)

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran tüm üç kullanılabilirlik bölgelerinde sanal makineleri dağıtmak görmek için belirli bir bölgedeki bir sanal Makineyi durdurun ve tarayıcınızı yenileyin.

## <a name="next-steps"></a>Sonraki adımlar
- [Standart Yük Dengeleyici](./load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin



