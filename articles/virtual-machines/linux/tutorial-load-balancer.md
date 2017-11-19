---
title: "Nasıl yükleneceğini Linux sanal makineleri azure'da Bakiye | Microsoft Docs"
description: "Üç Linux sanal makineyi yüksek oranda kullanılabilir ve güvenli bir uygulama oluşturmak için Azure yük dengeleyici kullanmayı öğrenin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/13/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: dc25d6106ad67710660b1a5c48270a7082688d51
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Nasıl yükleneceğini Linux sanal makineleri yüksek oranda kullanılabilir bir uygulama oluşturmak için Azure Bakiye
Yük Dengeleme, birden çok sanal makine genelinde gelen istekleri yayarak daha yüksek düzeyde kullanılabilirlik sağlar. Bu öğreticide, trafiği dağıtmak ve yüksek kullanılabilirlik sağlayan farklı bileşenleri, Azure yük dengeleyici hakkında bilgi edinin. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Azure yük dengeleyici oluşturma
> * Bir yük dengeleyici durum araştırması oluştur
> * Yük Dengeleyici trafiği kuralları oluşturma
> * Temel bir Node.js uygulama oluşturmak için bulut init kullanın
> * Sanal makineler oluşturun ve bir yük dengeleyiciye ekleyin
> * Bir yük dengeleyici eylemde görüntüleyin
> * Ekleme ve sanal makineleri yük dengeleyiciden kaldırın


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="azure-load-balancer-overview"></a>Azure yük dengeleyici genel bakış
Bir Azure yük dengeleyici gelen trafiği sağlıklı VM'ler arasında dağıtarak yüksek kullanılabilirlik sağlayan bir katman 4 (TCP, UDP) yük dengeleyicidir. Bir yük dengeleyici durum araştırması her VM üzerinde belirli bir bağlantı noktasını izler ve yalnızca trafiği işletimsel bir VM dağıtır.

Bir veya daha fazla ortak IP adreslerini içeren bir ön uç IP yapılandırmasını tanımlayın. Bu ön uç IP yapılandırmasını yük dengeleyici ve uygulamaların Internet üzerinden erişilebilir olmasını sağlar. 

Sanal makineler kendi sanal ağ arabirim kartı (NIC) kullanarak bir yük dengeleyiciye bağlayın. VM'ler trafiğini dağıtmak için bir arka uç adres havuzu IP içeren sanal (NIC'ler) adreslerini yük dengeleyiciye bağlı.

Trafik akışını denetlemek için belirli bağlantı noktalarını ve Vm'leriniz için eşleme protokolleri için yük dengeleyici kuralları tanımlayın.

Önceki öğretici izlediyseniz [bir sanal makine ölçek kümesi oluşturma](tutorial-create-vmss.md), bir yük dengeleyici sizin için oluşturuldu. Tüm bu bileşenlerin, Ölçek kümesinin bir parçası olarak yapılandırıldı.


## <a name="create-azure-load-balancer"></a>Azure yük dengeleyici oluşturma
Bu bölümde, nasıl oluşturma ve her bileşenin yük dengeleyicinin yapılandırma ayrıntıları. Yük Dengeleyici oluşturmadan önce bir kaynak grubuyla oluşturmanız [az grubu oluşturma](/cli/azure/group#create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupLoadBalancer* içinde *eastus* konumu:

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma
Uygulamanızı Internet'te erişmek için yük dengeleyici için bir ortak IP adresi gerekir. Bir ortak IP adresiyle oluşturma [az ağ genel IP oluşturun](/cli/azure/network/public-ip#create). Aşağıdaki örnek adlı ortak IP adresi oluşturur *myPublicIP* içinde *myResourceGroupLoadBalancer* kaynak grubu:

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma
Bir yük dengeleyici ile oluşturma [az ağ lb oluşturma](/cli/azure/network/lb#create). Aşağıdaki örnek, adlandırılmış bir yük dengeleyici oluşturur *myLoadBalancer* ve atar *myPublicIP* ön uç IP yapılandırmasını adresi:

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Bir sistem durumu araştırması oluştur
Uygulamanızın durumunu izlemek yük dengeleyici izin vermek için bir sistem durumu araştırması kullanın. Sistem durumu araştırma dinamik olarak ekler veya bunların yanıtını durumu denetimleri için temel yük dengeleyici döndürme VM'ler kaldırır. Varsayılan olarak, bir VM yük dengeleyici dağıtımı 15 saniyelik aralıklarda iki ardışık hatadan sonra kaldırılır. Bir protokol veya belirli bir sistem onay sayfasında uygulamanız için temel bir sistem durumu araştırması oluşturun. 

Aşağıdaki örnekte bir TCP araştırması oluşturur. Daha fazla hassas sistem durumu denetimlerinin özel HTTP araştırmalara de oluşturabilirsiniz. Özel bir HTTP araştırma kullanırken, sistem durumu denetimi sayfası gibi oluşturmalısınız *healthcheck.js*. Araştırma döndürmelidir bir **HTTP 200 Tamam** dönüş konak tutmak yük dengeleyici için yanıt.

TCP durumu araştırması oluşturmak için kullandığınız [az ağ lb araştırma oluşturmak](/cli/azure/network/lb/probe#create). Aşağıdaki örnek adlı bir sistem durumu araştırması oluşturur *myHealthProbe*:

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma
Yük Dengeleyici kuralı trafiğin Vm'lere nasıl dağıtıldığını tanımlamak için kullanılır. Gelen trafiği ve gerekli kaynak ve hedef bağlantı noktası ile birlikte trafiği almak için arka uç IP havuzu için ön uç IP yapılandırmasını tanımlayın. Yalnızca sağlıklı VM'ler trafiği aldığınızdan emin olmak için aynı zamanda kullanmak için sistem durumu araştırma tanımlar.

Yük Dengeleyici kuralı ile oluşturma [az ağ lb kuralını](/cli/azure/network/lb/rule#create). Aşağıdaki örnek, adında bir kural oluşturur *myLoadBalancerRule*, kullanan *myHealthProbe* durum araştırması ve bağlantı noktası bakiyelerini trafiğine *80*:

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma
Bazı sanal makineleri dağıtmak ve, dengeleyici sınayabilirsiniz önce destekleyici sanal ağ kaynakları oluşturun. Sanal ağlar hakkında daha fazla bilgi için bkz: [Azure Sanal Ağları Yönet](tutorial-virtual-network.md) Öğreticisi.

### <a name="create-network-resources"></a>Ağ kaynakları oluşturun
Bir sanal ağ ile oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#create). Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVnet* adlı bir alt ağ ile *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

Bir ağ güvenlik grubu eklemek için kullandığınız [az ağ nsg oluşturma](/cli/azure/network/nsg#create). Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNetworkSecurityGroup*:

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

Ağ güvenlik grubu kural oluştururken [az ağ nsg kuralını](/cli/azure/network/nsg/rule#create). Aşağıdaki örnek adlı bir ağ güvenlik grubu kural oluşturur *myNetworkSecurityGroupRule*:

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Sanal NIC ile oluşturulan [az ağ NIC oluşturma](/cli/azure/network/nic#create). Aşağıdaki örnek, üç sanal NIC oluşturur. (Her VM için bir sanal NIC için aşağıdaki adımları uygulamayı oluşturduğunuz). Ek sanal NIC ve sanal makineleri herhangi bir zamanda oluşturabilir ve bunları yük dengeleyiciye ekleyin:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

Tüm üç sanal NIC oluşturduğunuzda, sonraki adıma devam edin.


## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

### <a name="create-cloud-init-config"></a>Bulut init yapılandırma oluşturma
Önceki bir öğretici içinde [Linux sanal bir makinede ilk önyükleme özelleştirmek nasıl](tutorial-automate-vm-deployment.md), bulut init VM özelleştirmesiyle otomatikleştirmek öğrendiniz. NGINX yüklemek ve bir basit 'Hello World' Node.js uygulaması sonraki adımda çalıştırmak için aynı bulut init yapılandırma dosyası kullanabilirsiniz. Yük Dengeleyici öğreticinin sonunda uygulamada görmek için bu basit uygulama bir web tarayıcısında erişin.

Geçerli kabuğunuzu adlı bir dosya oluşturun *bulut init.txt* ve aşağıdaki yapılandırma yapıştırın. Örneğin, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. Girin `sensible-editor cloud-init.txt` dosyası oluşturun ve kullanılabilir düzenleyicileri listesini görmek için. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı:

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

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma
Uygulamanızı yüksek kullanılabilirliğini artırmak için bir kullanılabilirlik kümesine Vm'leriniz yerleştirin. Kullanılabilirlik kümeleri hakkında daha fazla bilgi için önceki bkz [yüksek oranda kullanılabilir sanal makineleri nasıl oluşturacağınızı](tutorial-availability-sets.md) Öğreticisi.

Kullanılabilirlik kümesi oluştur [az vm kullanılabilirlik kümesi oluşturma](/cli/azure/vm/availability-set#create). Aşağıdaki örnek, kullanılabilirlik adlandırılmış kümesi oluşturur *myAvailabilitySet*:

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

VM'lerin oluşturabileceğiniz artık [az vm oluşturma](/cli/azure/vm#create). Aşağıdaki örnekte, üç VM'ler oluşturur ve zaten mevcut değilse SSH anahtarları üretir:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

Azure CLI sorusu döndükten sonra çalışmaya devam arka plan görevleri vardır. `--no-wait` Parametresi tüm görevlerinin tamamlanması için bekleyin değil. Başka bir birkaç uygulamaya erişmek için dakika olabilir. Uygulamayı her VM çalıştıran yük dengeleyici durum araştırması otomatik olarak algılar. Uygulama çalışmaya başladıktan sonra Yük Dengeleyici kuralı trafiğini dağıtmak başlatır.


## <a name="test-load-balancer"></a>Test yük dengeleyici
Yük Dengeleyici ile genel IP adresi elde [az ağ ortak IP Göster](/cli/azure/network/public-ip#show). Aşağıdaki örnek IP adresi alacağı *myPublicIP* daha önce oluşturduğunuz:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Bir web tarayıcısı ortak IP adresi girebilirsiniz. Unutmayın - bunlara trafiğini dağıtmak yük dengeleyici başlamadan önce hazır olmasını VM'ler için birkaç dakika sürer. Yük Dengeleyici trafiği için aşağıdaki örnekteki gibi dağıtılmış VM konak adı dahil olmak üzere uygulama gösterilir:

![Çalışan Node.js uygulaması](./media/tutorial-load-balancer/running-nodejs-app.png)

Uygulamanızı çalıştıran tüm üç VM'ler arasında trafiği dağıtmak yük dengeleyici görmek için zorla-web tarayıcınızı yenileyin.


## <a name="add-and-remove-vms"></a>Sanal makineleri ekleyip
İşletim sistemi güncelleştirmelerini yükleme gibi uygulamanızı çalıştıran VM'ler bakım yapmak gerekebilir. Uygulamanıza artan trafiği ile mücadele etmek için ek VM'ler eklemeniz gerekebilir. Bu bölümde kaldırın veya VM yük dengeleyiciden nasıl ekleneceğini gösterir.

### <a name="remove-a-vm-from-the-load-balancer"></a>VM yük dengeleyiciden kaldırın
Arka uç adres havuzu ile bir VM kaldırabilirsiniz [az ağ NIC IP-config adres havuzu kaldırmak](/cli/azure/network/nic/ip-config/address-pool#remove). Aşağıdaki örnek, sanal bir NIC'ye kaldırır **myVM2** gelen *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

Uygulamanızı çalıştıran diğer iki VM arasında trafiği dağıtmak yük dengeleyici görmek için zorla-web tarayıcınızı yenileyin. İşletim sistemi güncelleştirmeleri yükleme veya VM yeniden başlatma gerçekleştirme gibi VM üzerinde bakım artık gerçekleştirebilirsiniz.

Yük dengeleyiciye bağlı sanal NIC içeren VM'ler listesini görüntülemek için kullanın [az ağ lb adres havuzu Göster](/cli/azure/network/lb/address-pool#show). Sorgulamak ve sanal NIC kimliği üzerinde şu şekilde filtreleyin:

```azurecli-interactive
az network lb address-pool show \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myBackEndPool \
    --query backendIpConfigurations \
    --output tsv | cut -f4
```

Çıktı VM 2 sanal NIC artık arka uç adres havuzu bir parçası olduğunu gösteren bir aşağıdaki örneğe benzer:

```bash
/subscriptions/<guid>/resourceGroups/myResourceGroupLoadBalancer/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/ipconfig1
/subscriptions/<guid>/resourceGroups/myResourceGroupLoadBalancer/providers/Microsoft.Network/networkInterfaces/myNic3/ipConfigurations/ipconfig1
```

### <a name="add-a-vm-to-the-load-balancer"></a>VM yük dengeleyiciye ekleyin
VM bakım gerçekleştirdikten sonra veya kapasitesini genişletmek gerekiyorsa, bir VM ile arka uç adres havuzu ekleyebileceğiniz [az ağ NIC IP-config adres havuzu ekleme](/cli/azure/network/nic/ip-config/address-pool#add). Aşağıdaki örnek, sanal bir NIC'ye ekler **myVM2** için *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```

Sanal NIC arka uç adres havuzuna bağlı olduğunu doğrulamak için kullanılan [az ağ lb adres havuzu Göster](/cli/azure/network/lb/address-pool#show) önceki adımdaki.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir yük dengeleyici oluşturulur ve sanal makineleri bağlı. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir Azure yük dengeleyici oluşturma
> * Bir yük dengeleyici durum araştırması oluştur
> * Yük Dengeleyici trafiği kuralları oluşturma
> * Temel bir Node.js uygulama oluşturmak için bulut init kullanın
> * Sanal makineler oluşturun ve bir yük dengeleyiciye ekleyin
> * Bir yük dengeleyici eylemde görüntüleyin
> * Ekleme ve sanal makineleri yük dengeleyiciden kaldırın

Azure sanal ağı bileşenleri hakkında daha fazla bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [VM’leri ve sanal ağları yönetme](tutorial-virtual-network.md)
