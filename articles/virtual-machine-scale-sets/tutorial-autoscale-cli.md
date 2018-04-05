---
title: Öğretici - Azure CLI 2.0 ile ölçek kümesini otomatik ölçeklendirme| Microsoft Docs
description: CPU talepleri arttıkça ve azaldıkça, sanal makine ölçek kümesini Azure CLI 2.0 ile otomatik olarak ölçeklendirmeyi öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 10e5b1a261f28391bed8cf3f111b1124b52d7816
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="tutorial-automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Öğretici: Azure CLI 2.0 ile sanal makine ölçek kümesini otomatik olarak ölçeklendirme
Ölçek kümesi oluşturduğunuzda, çalıştırmak istediğiniz sanal makine örneği sayısını tanımlarsınız. Uygulamanızın talebi değiştikçe, sanal makine örneklerinin sayısını otomatik olarak artırabilir veya azaltabilirsiniz. Otomatik ölçeklendirme özelliği, uygulamanızın yaşam döngüsü boyunca uygulama performansındaki değişikliklere veya müşteri taleplerine ayak uydurmanıza olanak tanır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Ölçek kümesi ile otomatik ölçeklendirmeyi kullanma
> * Otomatik ölçeklendirme kuralları oluşturma ve kullanma
> * Sanal makine örneklerinde stres testi yapma ve otomatik ölçeklendirme kurallarını tetikleme
> * Talep düştüğünde geriye doğru otomatik ölçeklendirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.29 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
Otomatik ölçeklendirme kuralları oluşturmanıza yardımcı olması için, abonelik kimliğiniz, kaynak grubunuz, ölçek kümesi adınız ve konumunuz için bazı parametreleri tanımlayın:

```azurecli-interactive
sub=$(az account show --query id -o tsv)
resourcegroup_name="myResourceGroup"
scaleset_name="myScaleSet"
location_name="eastus"
```

Aşağıdaki adımları uygulayarak [az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name $resourcegroup_name --location $location_name
```

Bu adımda [az vmss create](/cli/azure/vmss#create) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek, *2* örnek sayısı ile bir ölçek kümesini ve yoksa SSH anahtarlarını oluşturur:

```azurecli-interactive
az vmss create \
  --resource-group $resourcegroup_name \
  --name $scaleset_name \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --instance-count 2 \
  --admin-username azureuser \
  --generate-ssh-keys
```


## <a name="define-an-autoscale-profile"></a>Otomatik ölçeklendirme profilini tanımlama
Otomatik ölçeklendirme kuralları, Azure CLI 2.0 ile JSON (JavaScript Nesne Gösterimi) olarak dağıtılır. Şimdi bu otomatik ölçeklendirme profilinin her bir kısmına bakıp tam bir örnek oluşturalım.

Otomatik ölçeklendirme profilinin başlangıcı, varsayılan, en düşük ve en yüksek ölçek kümesi kapasitesini tanımlar. Aşağıdaki örnekte, varsayılan ve en düşük sanal makine örneği kapasitesi *2* ve en yüksek kapasite de *10* olarak ayarlanır:

```json
{
  "name": "autoscale rules",
  "capacity": {
    "minimum": "2",
    "maximum": "10",
    "default": "2"
  }
}
```


## <a name="create-a-rule-to-autoscale-out"></a>Otomatik ölçeklendirme ölçeğini genişletmek için kural oluşturma
Uygulamanızın talebi artarsa, ölçek kümenizdeki sanal makine örneklerinde üzerindeki yük de artar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. CPU veya disk gibi hangi ölçümlerin izleneceğini, uygulama yükünün belirli bir eşiği ne kadar süre karşılaması gerektiği ve ölçek kümesine kaç tane sanal makine örneği ekleneceğini denetlersiniz.

Şimdi ortalama CPU yükü 5 dakika boyunca %70’in üzerine çıktığında bir ölçek kümesindeki sanal makine örneği sayısını artıran bir kural oluşturalım. Kural tetiklendiğinde, sanal makine örneği sayısı üç artırılır.

Bu kural için aşağıdaki parametreler kullanılır:

| Parametre         | Açıklama                                                                                                         | Değer           |
|-------------------|---------------------------------------------------------------------------------------------------------------------|-----------------|
| *metricName*      | İzlenecek ve ölçek kümesi eylemlerinin uygulanmasında temel alınacak performans ölçümü.                                                   | CPU yüzdesi  |
| *timeGrain*       | Analiz için ne sıklıkla ölçümlerin toplanacağı.                                                                   | 1 dakika        |
| *timeAggregation* | Toplanan ölçümlerin analiz için nasıl bir araya getirileceğini tanımlar.                                                | Ortalama         |
| *timeWindow*      | Ölçüm ve eşik değerleri karşılaştırılmadan önce izlenecek süre.                                   | 5 dakika       |
| *operator*        | Ölçüm verilerini eşikle karşılaştırmak için kullanılan işleç.                                                     | Büyüktür    |
| *threshold*       | Otomatik ölçeklendirme kuralının bir eylemi tetiklemesine neden olan değer.                                                      | %70             |
| *direction*       | Kural geçerli olduğunda ölçek kümesinin ölçeğinin genişletileceğini veya daraltılacağını tanımlar.                                              | Artır        |
| *type*            | Sanal makine örneği sayısının belirli bir değerle değiştirilmesi gerektiğini belirtir.                                    | Değiştirme Sayısı    |
| *value*           | Kural geçerli olduğunda kaç tane sanal makine örneğinin ölçeğinin genişletileceği veya daraltılacağı.                                             | 3               |
| *cooldown*        | Otomatik ölçeklendirme eylemlerinin geçerli olması için kural tekrar uygulanmadan önceki bekleme süresi. | 5 dakika       |

Aşağıdaki örnek, sanal makine örneği sayısını azaltmak için kuralı tanımlar. *metricResourceUri*, daha önce abonelik kimliği, kaynak grubu adı ve ölçek kümesi adı için tanımlanan değişkenleri kullanır:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
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
    "value": "3",
    "cooldown": "PT5M"
  }
}
```


## <a name="create-a-rule-to-autoscale-in"></a>Otomatik ölçeklendirme ölçeğini daraltmak için kural oluşturma
Bir akşam veya hafta sonu uygulama talebiniz azalabilir. Belirli bir süreye yayılarak tutarlı şekilde yük azalıyorsa, ölçek kümesindeki sanal makine örneği sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Mevcut talebi karşılamak için gerekli örnek sayısını yalnızca siz çalıştırdığınızdan, bu ölçeği daraltma eylemi, ölçek kümenizi çalıştırma maliyetini azaltır.

Ortalama CPU yükü 5 dakika boyunca %30’un altına düştüğünde bir ölçek kümesindeki sanal makine örneği sayısını azaltan başka bir kural oluşturun. Aşağıdaki örnek, sanal makine örneği sayısını bir artırmak için kuralı tanımlar. *metricResourceUri*, daha önce abonelik kimliği, kaynak grubu adı ve ölçek kümesi adı için tanımlanan değişkenleri kullanır:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
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
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Otomatik ölçeklendirme kurallarını ölçek kümesine uygulama
Son adım, otomatik ölçeklendirme profilini ve kurallarını ölçek kümenize uygulamaktır. Bundan sonra ölçek kümenizin ölçeği, uygulamanın talebine bağlı olarak daraltılabilir veya genişletilebilir. Aşağıdaki adımları uygulayarak [az monitor autoscale-settings create](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create) ile otomatik ölçeklendirme profilini uygulayın. Aşağıdaki tam JSON, önceki bölümlerde belirtilen kuralları ve profili kullanır:

```azurecli-interactive
az monitor autoscale-settings create \
    --resource-group $resourcegroup_name \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "'$location_name'",
      "notifications": [],
      "profiles": [
        {
          "name": "autoscale by percentage based on CPU usage",
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
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
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
                "value": "3",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
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
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'"
    }'
```


## <a name="generate-cpu-load-on-scale-set"></a>Ölçek kümesinde CPU yükü oluşturma
Otomatik ölçeklendirme kurallarını test etmek için, ölçek kümesindeki sanal makine örneklerinde biraz CPU yükü oluşturun. Bu benzetimi yapılan CPU yükü, otomatik ölçeklendirmenin ölçeği genişletmesine ve sanal makine örneği sayısını artırmasına neden olur. Benzetimi yapılan CPU yükü daha sonra azaldığında otomatik ölçeklendirme kuralları ölçeği daraltır ve sanal makine örneği sayısını azaltır.

İlk olarak, [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info) komutunu kullanarak bir ölçek kümesindeki sanal makine örneklerine bağlanacak bağlantı noktalarını ve adresi listeleyin:

```azurecli-interactive
az vmss list-instance-connection-info \
  --resource-group $resourcegroup_name \
  --name $scaleset_name
```

Aşağıdaki örnek çıktıda, yük dengeleyicinin genel IP adresi, örnek adı ve Ağ Adresi Çevirisi (NAT) kurallarının trafiği ilettiği bağlantı noktası numarası gösterilmektedir:

```json
{
  "instance 1": "13.92.224.66:50001",
  "instance 3": "13.92.224.66:50003"
}
```

Birinci sanal makine örneğinizde SSH oturumu açın. Önceki komutta gösterildiği gibi, `-p` parametresiyle kendi genel IP adresinizi ve bağlantı noktası numaranızı belirtin:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50001
```

Oturum açtıktan sonra **stres** yardımcı programını yükleyin. CPU yükü oluşturan *10* **stress** çalışanıyla başlayın. Bu çalışanlar *420* saniye çalışır; bu, otomatik ölçeklendirme kurallarının istenen eylemi uygulamasını sağlamak için yeterlidir.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

**stress**, *stress: info: [2688] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd* benzeri bir çıktı gösterdiğinde, isteme geri dönmek için *Enter* tuşuna basın.

**stress** yardımcı programının CPU yükü oluşturduğunu onaylamak için, **top** yardımcı programını kullanarak etkin sistem yükünü inceleyin:

```azuecli-interactive
top
```

**top** yardımcı programından çıkın ve sanal makine örneğiyle bağlantınızı kapatın. **stress** yardımcı programı, sanal makine örneğinde çalışmaya devam eder.

```azurecli-interactive
Ctrl-c
exit
```

Önceki [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info) komutunda yer alan bağlantı noktası numarası ile ikinci sanal makine örneğine bağlanın:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50003
```

**stress** yardımcı programını yükleyip çalıştırın ve sonra bu ikinci sanal makine örneğinde on çalışan başlatın.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

Tekrar **stress**, *stress: info: [2713] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd* benzeri bir çıktı gösterdiğinde, isteme geri dönmek için *Enter* tuşuna basın.

İkinci sanal makine örneğiyle bağlantınızı kapatın. **stress** yardımcı programı, sanal makine örneğinde çalışmaya devam eder.

```azurecli-interactive
exit
```

## <a name="monitor-the-active-autoscale-rules"></a>Etkin otomatik ölçeklendirme kurallarını izleme
Ölçek kümenizdeki sanal makine örneği sayısını izlemek için **watch** yardımcı programını kullanın. Otomatik ölçeklendirme kurallarının, sanal makine örneklerinin her birinde **stress** tarafından oluşturulan CPU yüküne yanıt olarak ölçeği genişletme işlemini başlatması 5 dakika sürer.

```azurecli-interactive
watch az vmss list-instances \
  --resource-group $resourcegroup_name \
  --name $scaleset_name \
  --output table
```

CPU eşiği karşılandıktan sonra otomatik ölçeklendirme kuralları, ölçek kümesindeki sanal makine örneği sayısını artırır. Aşağıdaki çıktıda, ölçek kümesi ölçeği genişlettiğinde oluşturulan üç sanal makine gösterilmektedir:

```bash
Every 2.0s: az vmss list-instances --resource-group myResourceGroup --name myScaleSet --output table

  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup    VmId
------------  --------------------  ----------  ------------  -------------------  ---------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUP  4f92f350-2b68-464f-8a01-e5e590557955
           2  True                  eastus      myScaleSet_2  Succeeded            MYRESOURCEGROUP  d734cd3d-fb38-4302-817c-cfe35655d48e
           4  True                  eastus      myScaleSet_4  Creating             MYRESOURCEGROUP  061b4c90-0d73-49fc-a066-19eab0b3d95c
           5  True                  eastus      myScaleSet_5  Creating             MYRESOURCEGROUP  4beff8b9-4e65-40cb-9652-43899309da27
           6  True                  eastus      myScaleSet_6  Creating             MYRESOURCEGROUP  9e4133dd-2c57-490e-ae45-90513ce3b336
```

**stress** yardımcı programı ilk sanal makine örneklerinde durduğunda ortalama CPU yükü normale döner. Bir 5 dakika daha geçtikten sonra otomatik ölçeklendirme kuralları, ölçeği daraltarak sanal makine örneklerinin sayısını azaltır. Ölçeği daraltma eylemleri önce en yüksek kimliğe sahip sanal makine örneklerini kaldırır. Aşağıdaki örnek çıktıda, ölçek kümesi otomatik olarak ölçeği daralttığında tek bir sanal makine örneğinin silindiği gösterilmektedir:

```bash
           6  True                  eastus      myScaleSet_6  Deleting             MYRESOURCEGROUP  9e4133dd-2c57-490e-ae45-90513ce3b336
```

`Ctrl-c` ile *watch* yardımcı programından çıkın. Ölçek kümesi, minimum 2 örnek sayısına ulaşılıncaya kadar her 5 dakikada bir ölçeği daraltmaya ve bir sanal makine örneğini kaldırmaya devam eder.


## <a name="clean-up-resources"></a>Kaynakları temizleme
Ölçek kümenizi ve ek kaynaklarınızı kaldırmak için [az group delete](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu ve bu kaynak grubunun tüm kaynaklarını silin. `--no-wait` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür. `--yes` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar.

```azurecli-interactive
az group delete --name $resourcegroup_name --yes --no-wait
```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure CLI 2.0 ile otomatik olarak bir ölçek kümesinin ölçeğini daraltma veya genişletme işleminin nasıl yapılacağını öğrendiniz.

> [!div class="checklist"]
> * Ölçek kümesi ile otomatik ölçeklendirmeyi kullanma
> * Otomatik ölçeklendirme kuralları oluşturma ve kullanma
> * Sanal makine örneklerinde stres testi yapma ve otomatik ölçeklendirme kurallarını tetikleme
> * Talep düştüğünde geriye doğru otomatik ölçeklendirme

Çalışır haldeki sanal makine ölçek kümelerinin daha fazla örneğini görmek için aşağıdaki Azure CLI 2.0 örnek betiklerine bakın:

> [!div class="nextstepaction"]
> [Azure CLI 2.0 için ölçek kümesi betik örnekleri](cli-samples.md)