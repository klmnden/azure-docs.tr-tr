---
title: "Azure’da Linux için Sanal Makine Ölçek Kümeleri Oluşturma | Microsoft Belgeleri"
description: "Bir sanal makine ölçek kümesini kullanarak Linux VM'leri üzerinde yüksek oranda kullanılabilir bir uygulama oluşturma ve dağıtma"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 263983017e08dcc9a8e614c159ef5afaaf1d924e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Sanal Makine Ölçek Kümesi oluşturma ve Linux üzerinde yüksek oranda kullanılabilir bir uygulama dağıtma
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki VM sayısını el ile ölçeklendirebilir veya CPU, bellek isteği ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu öğreticide, Azure’da bir sanal makine ölçek kümesi dağıtılmaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Cloud-init kullanarak ölçeklendirilecek bir uygulama oluşturma
> * Sanal makine ölçek kümesi oluşturma
> * Ölçek kümesindeki örnek sayısını artırma veya azaltma
> * Otomatik ölçeklendirme kuralları oluşturma
> * Ölçek kümesi örneklerine ait bağlantı bilgilerini görüntüleme
> * Ölçek kümesinde veri diskleri kullanma


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.22 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="scale-set-overview"></a>Ölçek Kümesine genel bakış
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesindeki VM’ler, bir veya daha fazla *yerleştirme grubu* şeklinde mantık hatası ve güncelleme etki alanlarında dağıtılır. Bu gruplar, [kullanılabilirlik kümeleri](tutorial-availability-sets.md) gibi benzer şekilde yapılandırılmış VM’lerdir.

VM’ler, ölçek kümesinde gerektiğinde oluşturulur. Ölçek kümesinde VM eklenmesi veya kaldırılması işlemlerinin nasıl ve ne zaman gerçekleştirileceğini denetlemek için otomatik ölçeklendirme kurallarını tanımlanır. Bu kurallar, CPU yükü, bellek kullanımı veya ağ trafiği gibi ölçümlere dayalı olarak tetiklenebilir.

Bir Azure platform görüntüsü kullanılırsa ölçek kümeleri 1.000 adede kadar VM'i destekler. Ciddi derecede yükleme veya VM özelleştirme gereksinimleri olan iş yükleri için [özel bir VM görüntüsü oluşturulması](tutorial-custom-images.md) iyi olabilir. Özel bir görüntü kullanırken ölçek kümesinde 300 adede kadar VM oluşturabilirsiniz.


## <a name="create-an-app-to-scale"></a>Ölçeklendirilecek bir uygulama oluşturma
Üretim kullanımı için uygulamanızı yüklenmiş ve yapılandırılmış olarak içeren [özel bir VM görüntüsü oluşturmak](tutorial-custom-images.md) isteyebilirsiniz. Bu öğreticide, ölçek kümesini hemen iş başında görmek için VM’leri ilk önyüklemede özelleştirelim.

Daha önceki bir öğreticide cloud-init ile [ilk önyüklemede Linux sanal makinelerini özelleştirmeyi](tutorial-automate-vm-deployment.md) öğrendiniz. Aynı cloud-init yapılandırma dosyasını kullanarak NGINX’i yükleyebilir ve basit bir “Merhaba Dünya” Node.js uygulaması çalıştırabilirsiniz. 

Geçerli kabuğunuzda *cloud-init.txt* adlı bir dosya oluşturup aşağıdaki yapılandırmayı yapıştırın. Örneğin, dosyayı yerel makinenizde değil Cloud Shell’de oluşturun. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud-init.txt` adını girin. Başta birinci satır olmak üzere cloud-init dosyasının tamamının doğru bir şekilde kopyalandığından emin olun:

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


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
Ölçek kümesi oluşturabilmek için [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumunda *myResourceGroupScaleSet* adlı bir kaynak grubu oluşturur:

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

Bu adımda [az vmss create](/cli/azure/vmss#az_vmss_create) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek, *myScaleSet* adlı bir ölçek kümesi oluşturur, cloud-init dosyasını kullanarak VM’yi özelleştirir ve yoksa SSH anahtarlarını oluşturur:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer. Azure CLI sizi isteme geri döndürdükten sonra çalışmaya devam eden arka plan görevleri vardır. Uygulamaya erişmeniz birkaç dakika sürebilir.


## <a name="allow-web-traffic"></a>Web trafiğine izin verme
Sanal makine ölçek kümesinin bir parçası olarak otomatik olarak bir yük dengeleyici oluşturuldu. Yük dengeleyici, yük dengeleyici kurallarını kullanarak trafiği tanımlı bir VM'ler kümesi arasında dağıtır. Yük dengeleyiciye ilişkin kavramlar ve yapılandırma hakkında [Azure’da sanal makinelerin yükünü dengeleme](tutorial-load-balancer.md) adlı sıradaki öğreticide daha fazla bilgi edinebilirsiniz.

Trafiğin web uygulamanıza ulaşmasına izin vermek için [az network lb rule create](/cli/azure/network/lb/rule#az_network_lb_rule_create) ile bir kural oluşturun. Aşağıdaki örnek *myLoadBalancerRuleWeb* adlı bir kural oluşturur:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a>Uygulamanızı test etme
Node.js uygulamanızı Web’de görmek için [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) ile yük dengeleyicinizin genel IP adresini alın. Aşağıdaki örnek ölçek kümesinin bir parçası olarak oluşturulan *myScaleSetLBPublicIP* için IP adresini alır:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Genel IP adresini bir web tarayıcısına girin. Uygulama, yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adı ile birlikte görüntülenir:

![Node.js uygulaması çalıştırma](./media/tutorial-create-vmss/running-nodejs-app.png)

Ölçek kümesini çalışırken görmek için web tarayıcınızı yenilemeye zorlayarak yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM’ye dağıtmasını görebilirsiniz.


## <a name="management-tasks"></a>Yönetim görevleri
Ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Azure CLI 2.0 tüm bunları hızlıca yapabilmenizi sağlar. Aşağıda birkaç yaygın görev açıklanmaktadır.

### <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Ölçek kümenizde çalışan VM’lerin listesini görüntülemek için [az vmss list-instances](/cli/azure/vmss#az_vmss_list_instances) komutunu şu şekilde kullanın:

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

Çıktı aşağıdaki örneğe benzer:

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>VM örneklerinin sayısını artırma veya azaltma
Ölçek kümesinde şu anda yer alan örneklerin sayısını görmek için [az vmss show](/cli/azure/vmss#az_vmss_show) komutunu kullanarak *sku.capacity* üzerinde bir sorgu çalıştırın:

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Ardından [az vmss scale](/cli/azure/vmss#az_vmss_scale) ile ölçek kümesindeki sanal makinelerin sayısını elle artırabilir veya azaltabilirsiniz. Aşağıdaki örnek, ölçek kümenizdeki VM'lerin sayısını *3* olarak ayarlar:

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 3
```


### <a name="configure-autoscale-rules"></a>Otomatik ölçeklendirme kuralları yapılandırma
Ölçek kümenizdeki örneklerin sayısını el ile ölçeklendirmek yerine otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu kurallar, ölçek kümenizdeki örnekleri izler ve tanımladığınız ölçüm ve eşiklere göre müdahale eder. Aşağıdaki örnek, ortalama CPU yükü 5 dakika süresince %60’dan yüksek olursa örnek sayısını bir tane artırır. Ortalama CPU yükü daha sonra 5 dakika süresince %30’dan düşük olursa örnek sayısı bir tane azaltılır. Çeşitli ölçek kümesi bileşenlerinin kaynak URI’lerini oluşturmak için abonelik kimliğiniz kullanılır. Bu kuralları [az monitor autoscale-settings create](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create) ile oluşturmak için aşağıdaki otomatik ölçeklendirme komut profilini kopyalayıp yapıştırın:

```azurecli-interactive 
sub=$(az account show --query id -o tsv)

az monitor autoscale-settings create \
    --resource-group myResourceGroupScaleSet \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "East US",
      "notifications": [],
      "profiles": [
        {
          "name": "Auto created scale condition",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
    }'
```

Otomatik ölçeklendirme profilini yeniden kullanmak için bir JSON (JavaScript Nesne Gösterimi) dosyası oluşturup bu dosyayı `--parameters @autoscale.json` parametresiyle `az monitor autoscale-settings create` komutuna geçirebilirsiniz. Otomatik ölçeklendirme kullanımıyla ilgili diğer tasarım bilgileri için [otomatik ölçeklendirmeye yönelik en iyi yöntemlere](/azure/architecture/best-practices/auto-scaling) bakın.


### <a name="get-connection-info"></a>Bağlantı bilgilerini alma
Ölçek kümelerinizdeki VM'lere ilişkin bağlantı bilgilerini almak için [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info) komutunu kullanın. Bu komut, çıktı olarak her bir VM’nin genel IP adresini ve bağlantı noktasını verir. Bu bilgiler, SSH ile bağlanmanıza olanak sağlar:

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a>Ölçek kümeleri ile veri diskleri kullanma
Veri diskleri oluşturup ölçek kümeleri ile kullanabilirsiniz. Daha önceki bir öğreticide [Azure disklerini yönetmeyi](tutorial-manage-disks.md) öğrenirken işletim sistemi diski yerine veri diskleri üzerinde uygulama oluşturmaya yönelik en iyi yöntemleri ve performans geliştirmelerini genel hatlarıyla gördünüz.

### <a name="create-scale-set-with-data-disks"></a>Veri diskleri ile ölçek kümesi oluşturma
Ölçek kümesi oluşturup veri diskleri eklemek için [az vmss create](/cli/azure/vmss#az_vmss_create) komutuna `--data-disk-sizes-gb` parametresini ekleyin. Aşağıdaki örnek, her bir örneğe *50* GB’lık bir veri diski eklenmiş şekilde bir ölçek kümesi oluşturur:

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

Örnekler ölçek kümesinden kaldırıldığında ekli tüm veri diskleri de kaldırılır.

### <a name="add-data-disks"></a>Veri diski ekleme
Ölçek kümenizdeki örneklere bir veri diski eklemek için [az vmss disk attach](/cli/azure/vmss/disk#az_vmss_disk_attach) komutunu kullanın. Aşağıdaki örnek, her bir örneğe *50* GB’lık bir veri diski ekler:

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a>Veri diskini çıkarma
Ölçek kümenizdeki örneklerden veri diskini kaldırmak için [az vmss disk detach](/cli/azure/vmss/disk#az_vmss_disk_detach) komutunu kullanın. Aşağıdaki örnek, her bir örnekten LUN *2*’deki veri diskini kaldırır:

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sanal makine ölçek kümesi oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Cloud-init kullanarak ölçeklendirilecek bir uygulama oluşturma
> * Sanal makine ölçek kümesi oluşturma
> * Ölçek kümesindeki örnek sayısını artırma veya azaltma
> * Otomatik ölçeklendirme kuralları oluşturma
> * Ölçek kümesi örneklerine ait bağlantı bilgilerini görüntüleme
> * Ölçek kümesinde veri diskleri kullanma

Sanal makinelere ilişkin yük dengeleme kavramları hakkında daha fazla bilgi edinmek için sıradaki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Sanal makinelerde yük dengeleme](tutorial-load-balancer.md)
