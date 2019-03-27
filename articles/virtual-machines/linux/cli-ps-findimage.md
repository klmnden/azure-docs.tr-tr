---
title: Azure CLI ile Linux VM görüntülerini seçme | Microsoft Docs
description: Yayımcı, teklif, SKU ve sürümü için Market VM görüntülerini belirlemek için Azure CLI'yı kullanmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/25/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 453628dca04fbc3c48564f15b6cf61802165b0cf
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499701"
---
# <a name="find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a>Azure CLI ile Azure Marketi'nde, Linux VM görüntüleri bulma

Bu konuda, VM görüntüleri Azure Market'te bulmak için Azure CLI kullanmayı açıklar. Bir VM CLI ile programlı olarak oluşturduğunuzda, bir Market görüntüsü belirtmek için bu bilgileri kullanın. Resource Manager şablonları ya da başka araçlar.

Kullanılabilir görüntüleri ve teklifler kullanarak da göz [Azure Marketi](https://azuremarketplace.microsoft.com/) vitrini, [Azure portalında](https://portal.azure.com), veya [Azure PowerShell](../windows/cli-ps-findimage.md). 

En son yüklü olduğundan emin olun [Azure CLI](/cli/azure/install-azure-cli) ve bir Azure hesabına kaydedilir (`az login`).

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="list-popular-images"></a>Popüler görüntüleri listeleme

Çalıştırma [az vm görüntüsü listesi](/cli/azure/vm/image) olmadan komut `--all` seçeneği, popüler VM görüntüleri Azure Market'te bir listesini görmek için. Örneğin, önbellekteki popüler görüntülerin listesini tablo biçiminde görüntülemek için aşağıdaki komutu çalıştırın:

```azurecli
az vm image list --output table
```

Çıktı URN görüntüsünü içerir (değer *Urn* sütunu). Alternatif olarak bu popüler Market görüntüleri biri ile bir VM oluştururken, belirtebilirsiniz *UrnAlias*, kısaltılmış gibi *UbuntuLTS*.

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.5                 OpenLogic:CentOS:7.5:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.3                SUSE:openSUSE-Leap:42.3:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7-RAW               RedHat:RHEL:7-RAW:latest                                        RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Belirli görüntüleri bulma

Belirli bir sanal makine görüntü Marketi'nde bulmak için kullanın `az vm image list` komutunu `--all` seçeneği. Komutun bu sürümü ve tamamlanması biraz zaman alır. genellikle listeyi filtrelemek için uzun çıkış dönüş `--publisher` veya başka bir parametre. 

Örneğin, aşağıdaki komutu tüm Debian teklifleri görüntüler (unutmayın `--all` geçiş, yalnızca yerel önbelleğe genel görüntü arama):

```azurecli
az vm image list --offer Debian --all --output table 

```

Kısmi çıkış: 

```
Offer              Publisher    Sku                  Urn                                                    Version
-----------------  -----------  -------------------  -----------------------------------------------------  --------------
Debian             credativ     7                    credativ:Debian:7:7.0.201602010                        7.0.201602010
Debian             credativ     7                    credativ:Debian:7:7.0.201603020                        7.0.201603020
Debian             credativ     7                    credativ:Debian:7:7.0.201604050                        7.0.201604050
Debian             credativ     7                    credativ:Debian:7:7.0.201604200                        7.0.201604200
Debian             credativ     7                    credativ:Debian:7:7.0.201606280                        7.0.201606280
Debian             credativ     7                    credativ:Debian:7:7.0.201609120                        7.0.201609120
Debian             credativ     7                    credativ:Debian:7:7.0.201611020                        7.0.201611020
Debian             credativ     7                    credativ:Debian:7:7.0.201701180                        7.0.201701180
Debian             credativ     8                    credativ:Debian:8:8.0.201602010                        8.0.201602010
Debian             credativ     8                    credativ:Debian:8:8.0.201603020                        8.0.201603020
Debian             credativ     8                    credativ:Debian:8:8.0.201604050                        8.0.201604050
Debian             credativ     8                    credativ:Debian:8:8.0.201604200                        8.0.201604200
Debian             credativ     8                    credativ:Debian:8:8.0.201606280                        8.0.201606280
Debian             credativ     8                    credativ:Debian:8:8.0.201609120                        8.0.201609120
Debian             credativ     8                    credativ:Debian:8:8.0.201611020                        8.0.201611020
Debian             credativ     8                    credativ:Debian:8:8.0.201701180                        8.0.201701180
Debian             credativ     8                    credativ:Debian:8:8.0.201703150                        8.0.201703150
Debian             credativ     8                    credativ:Debian:8:8.0.201704110                        8.0.201704110
Debian             credativ     8                    credativ:Debian:8:8.0.201704180                        8.0.201704180
Debian             credativ     8                    credativ:Debian:8:8.0.201706190                        8.0.201706190
Debian             credativ     8                    credativ:Debian:8:8.0.201706210                        8.0.201706210
Debian             credativ     8                    credativ:Debian:8:8.0.201708040                        8.0.201708040
Debian             credativ     8                    credativ:Debian:8:8.0.201710090                        8.0.201710090
Debian             credativ     8                    credativ:Debian:8:8.0.201712040                        8.0.201712040
Debian             credativ     8                    credativ:Debian:8:8.0.201801170                        8.0.201801170
Debian             credativ     8                    credativ:Debian:8:8.0.201803130                        8.0.201803130
Debian             credativ     8                    credativ:Debian:8:8.0.201803260                        8.0.201803260
Debian             credativ     8                    credativ:Debian:8:8.0.201804020                        8.0.201804020
Debian             credativ     8                    credativ:Debian:8:8.0.201804150                        8.0.201804150
Debian             credativ     8                    credativ:Debian:8:8.0.201805160                        8.0.201805160
Debian             credativ     8                    credativ:Debian:8:8.0.201807160                        8.0.201807160
Debian             credativ     8                    credativ:Debian:8:8.0.201901221                        8.0.201901221
...
```

İle benzer filtreler uygulamak `--location`, `--publisher`, ve `--sku` seçenekleri. Kısmi eşleşmeler arama gibi bir filtre gerçekleştirebileceğiniz `--offer Deb` tüm Debian görüntüler bulunamadı.

Belirli bir konumla belirtmezseniz `--location` seçeneği, varsayılan konumu değerleri döndürülür. (Çalıştırarak farklı varsayılan konumu ayarlayın `az configure --defaults location=<location>`.)

Örneğin, aşağıdaki komut, Batı Avrupa konumu tüm Debian 8 Sku'da listeler:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Kısmi çıkış:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
Debian   credativ     8                  credativ:Debian:8:8.0.201708040                  8.0.201708040
Debian   credativ     8                  credativ:Debian:8:8.0.201710090                  8.0.201710090
Debian   credativ     8                  credativ:Debian:8:8.0.201712040                  8.0.201712040
Debian   credativ     8                  credativ:Debian:8:8.0.201801170                  8.0.201801170
Debian   credativ     8                  credativ:Debian:8:8.0.201803130                  8.0.201803130
Debian   credativ     8                  credativ:Debian:8:8.0.201803260                  8.0.201803260
Debian   credativ     8                  credativ:Debian:8:8.0.201804020                  8.0.201804020
Debian   credativ     8                  credativ:Debian:8:8.0.201804150                  8.0.201804150
Debian   credativ     8                  credativ:Debian:8:8.0.201805160                  8.0.201805160
Debian   credativ     8                  credativ:Debian:8:8.0.201807160                  8.0.201807160
Debian   credativ     8                  credativ:Debian:8:8.0.201901221                  8.0.201901221
...
```

## <a name="navigate-the-images"></a>Görüntüleri gidin
 
Bir konumda bir görüntü bulmak için başka bir yol [az vm görüntüsü listesi-publishers](/cli/azure/vm/image), [az vm görüntüsü listesi-offers](/cli/azure/vm/image), ve [az vm görüntüsü listesi-skus](/cli/azure/vm/image) komutları dizisi. Bu komutları ile bu değerleri belirler:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.

Ardından, seçilen SKU için bir sürümünün dağıtılacağını seçebilirsiniz.

Örneğin, aşağıdaki komut, Batı ABD konumunda görüntü yayımcılarını listeler:

```azurecli
az vm image list-publishers --location westus --output table
```

Kısmi çıkış:

```
Location    Name
----------  ----------------------------------------------------
westus      128technology
westus      1e
westus      4psa
westus      5nine-software-inc
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      accessdata-group
westus      accops
westus      Acronis
westus      Acronis.Backup
westus      actian-corp
westus      actian_matrix
westus      actifio
westus      activeeon
westus      advantech-webaccess
westus      aerospike
westus      affinio
westus      aiscaler-cache-control-ddos-and-url-rewriting-
westus      akamai-technologies
westus      akumina
...
```

Belirli bir yayımcıdan teklifler bulmak için bu bilgileri kullanın. Örneğin, *Canonical* çalıştırarak Batı ABD konumunda yayımcı teklifleri bulun `azure vm image list-offers`. Konum ve yayımcıyı aşağıdaki örnekteki gibi geçirin:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Çıkış:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
```
Batı ABD bölgesinde Canonical yayımlar gördüğünüz *UbuntuServer* Azure'da sunar. Peki bunların SKU’su ne? Bu değerleri almak için çalıştırın `azure vm image list-skus` ve konumu, yayımcı ve teklif keşfettiğiniz ayarlayın:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Çıkış:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-LTS
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      18.04-DAILY-LTS
westus      18.04-LTS
westus      18.10
westus      18.10-DAILY
westus      19.04-DAILY
```

Son olarak, `az vm image list` istediğiniz, örneğin, belirli bir SKU sürümünü bulmak için komutu *18.04 LTS*:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 18.04-LTS --all --output table
```

Kısmi çıkış:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201804262  18.04.201804262
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201805170  18.04.201805170
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201805220  18.04.201805220
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201806130  18.04.201806130
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201806170  18.04.201806170
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201807240  18.04.201807240
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201808060  18.04.201808060
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201808080  18.04.201808080
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201808140  18.04.201808140
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201808310  18.04.201808310
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201809110  18.04.201809110
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201810030  18.04.201810030
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201810240  18.04.201810240
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201810290  18.04.201810290
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201811010  18.04.201811010
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201812031  18.04.201812031
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201812040  18.04.201812040
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201812060  18.04.201812060
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201901140  18.04.201901140
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201901220  18.04.201901220
...
```

Artık tam olarak URN değeri not yararlanarak kullanmak istediğiniz görüntüyü seçebilirsiniz. Bu değer ile `--image` ile bir VM oluşturduğunuzda, parametre [az vm oluşturma](/cli/azure/vm) komutu. "Son" URN sürüm numarasını isteğe bağlı olarak değiştirebilirsiniz unutmayın. Bu her zaman görüntüsünün son sürümünü sürümüdür. 

Resource Manager şablonu ile bir VM dağıtırsanız, görüntü parametrelerini içinde tek tek ayarlayın `imageReference` özellikleri. Bkz. [şablon başvurusu](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Plan özelliklerini görüntüleme

Görüntünün satın alma planı bilgilerini görüntülemek için çalıştırın [az vm görüntüsü show](/cli/azure/image) komutu. Varsa `plan` çıktıda özelliği değil `null`, koşulları resimle önce programlamalı dağıtım'ı kabul etmeniz gerekir.

Örneğin, Canonical Ubuntu Server 18.04 LTS görüntüsünü ek koşullar için sahip `plan` bilgileri `null`:

```azurecli
az vm image show --location westus --urn Canonical:UbuntuServer:18.04-LTS:latest
```

Çıkış:

```
{
  "dataDiskImages": [],
  "id": "/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/18.04-LTS/Versions/18.04.201901220",
  "location": "westus",
  "name": "18.04.201901220",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

Bitnami görüntüsünün tarafından sertifikalı RabbitMQ aşağıdaki gösterir benzer bir komut çalıştıran `plan` özellikleri: `name`, `product`, ve `publisher`. (Bazı görüntüleri de bir `promotion code` özellik.) Bu görüntü dağıtmak için koşullarını kabul edin ve programlamalı dağıtımını etkinleştirmek için aşağıdaki bölümlere bakın.

```azurecli
az vm image show --location westus --urn bitnami:rabbitmq:rabbitmq:latest
```
Çıkış:

```
{
  "dataDiskImages": [],
  "id": "/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/bitnami/ArtifactTypes/VMImage/Offers/rabbitmq/Skus/rabbitmq/Versions/3.7.1901151016",
  "location": "westus",
  "name": "3.7.1901151016",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": {
    "name": "rabbitmq",
    "product": "rabbitmq",
    "publisher": "bitnami"
  },
  "tags": null
}
```

### <a name="accept-the-terms"></a>Koşulları kabul et

Lisans koşullarını kabul edin ve görüntülemek için kullanın [az vm görüntüsü kabul koşulları](/cli/azure/vm/image?) komutu. Koşulları kabul ettiğinde, aboneliğinizde programlamalı dağıtım etkinleştirin. Görüntü için abonelik başına bir kez kabul etmek yeterlidir. Örneğin:

```azurecli
az vm image accept-terms --urn bitnami:rabbitmq:rabbitmq:latest
``` 

Çıktı içerir bir `licenseTextLink` için lisans koşulları ve gösterir değerini `accepted` olan `true`:

```
{
  "accepted": true,
  "additionalProperties": {},
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.MarketplaceOrdering/offertypes/bitnami/offers/rabbitmq/plans/rabbitmq",
  "licenseTextLink": "https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_BITNAMI%253a24RABBITMQ%253a24RABBITMQ%253a24IGRT7HHPIFOBV3IQYJHEN2O2FGUVXXZ3WUYIMEIVF3KCUNJ7GTVXNNM23I567GBMNDWRFOY4WXJPN5PUYXNKB2QLAKCHP4IE5GO3B2I.txt",
  "name": "rabbitmq",
  "plan": "rabbitmq",
  "privacyPolicyLink": "https://bitnami.com/privacy",
  "product": "rabbitmq",
  "publisher": "bitnami",
  "retrieveDatetime": "2019-01-25T20:37:49.937096Z",
  "signature": "XXXXXXLAZIK7ZL2YRV5JYQXONPV76NQJW3FKMKDZYCRGXZYVDGX6BVY45JO3BXVMNA2COBOEYG2NO76ONORU7ITTRHGZDYNJNXXXXXX",
  "type": "Microsoft.MarketplaceOrdering/offertypes"
}
```

### <a name="deploy-using-purchase-plan-parameters"></a>Satın alma planı parametreleri kullanarak dağıtma

Görüntü için koşulları kabul ettikten sonra Abonelikteki bir VM dağıtabilirsiniz. Görüntüyü kullanarak dağıtmak için `az vm create` komutunu, parametrelerini satın alma planı için ayrıca bir URN görüntüsünün sağlamak. Örneğin, sertifikalı RabbitMQ ile bir VM tarafından Bitnami görüntüsünün dağıtmak için şunu yazın:

```azurecli
az group create --name myResourceGroupVM --location westus

az vm create --resource-group myResourceGroupVM --name myVM --image bitnami:rabbitmq:rabbitmq:latest --plan-name rabbitmq --plan-product rabbitmq --plan-publisher bitnami
```

## <a name="next-steps"></a>Sonraki adımlar
Görüntü bilgileri kullanarak hızla bir sanal makine oluşturmak için bkz: [oluşturma ve Azure CLI ile Linux sanal makineleri yönetme](tutorial-manage-vm.md).
