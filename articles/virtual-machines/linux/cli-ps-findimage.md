---
title: "Azure CLI ile Linux VM görüntüleri seçin | Microsoft Docs"
description: "Yayımcı, teklif, SKU ve sürümü Market VM görüntüleri belirlemek için Azure CLI kullanmayı öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/24/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e0c27a7ee9e9a7ab1a3b004e070fa556b56a36a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a>Linux VM görüntüleri Azure CLI ile Azure Marketi bulmak nasıl
Bu konuda, Azure Marketi'nde VM görüntüleri bulmak için Azure CLI 2.0 kullanmayı açıklar. Bir Linux VM oluşturduğunuzda bir Market görüntüsü belirtmek için bu bilgileri kullanın.

En son yüklendiğinden emin olmak [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabına günlüğe kaydedilir (`az login`).

## <a name="terminology"></a>Terminoloji

Market görüntülerini CLI ve diğer Azure araçlarını bir hiyerarşi göre tanımlanır:

* **Yayımcı** -görüntüyü oluşturan kuruluş. Örnek: kurallı
* **Teklif** -bir yayımcı tarafından oluşturulan ilgili görüntüler grubudur. Örnek: Ubuntu Server
* **SKU** - bir dağıtım büyük sürümü gibi bir teklif ait bir örnek. Örnek: 16.04-LTS
* **Sürüm** -SKU görüntü sürüm numarası. Görüntü belirtirken, hangi dağıtım en son sürümünü seçer sürüm numarasıyla "son" değiştirebilirsiniz.

Bir Market görüntüsü belirtmek için genellikle görüntünün kullanın *URN*. İki nokta üst üste (:) karakteriyle ayrılmış bu değerleri URN birleştirir: *yayımcı*:*teklif*:*Sku*:*sürüm*. 


## <a name="list-popular-images"></a>Liste popüler görüntüleri

Çalıştırma [az vm görüntü listesi](/cli/azure/vm/image#list) olmadan komutu `--all` Azure Marketi popüler VM görüntüleri listesini görmek için seçeneği. Örneğin, tablo biçiminde popüler görüntüleri önbelleğe alınmış bir listesini görüntülemek için aşağıdaki komutu çalıştırın:

```azurecli
az vm image list --output table
```

Çıktı URN içerir (değeri *Urn* sütun), görüntüyü belirtmek için kullanın. Bir VM Bu popüler Market görüntülerden birini oluştururken, alternatif olarak, URN diğer adı gibi belirleyebilirsiniz *UbuntuLTS*.

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Belirli görüntüleri bulma

Belirli bir VM görüntüsü markette bulmak için `az vm image list` komutunu `--all` seçeneği. Komutun bu sürümü tamamlamak ve biraz zaman alabilir genellikle listeyi filtrelemek için uzun output dönüş `--publisher` veya başka bir parametre. 

Örneğin, aşağıdaki komut tüm Debian teklifleri görüntüler (olmadan unutmamanız `--all` geçiş, yalnızca yerel önbelleğe ortak görüntülerinin arar):

```azurecli
az vm image list --offer Debian --all --output table 

```

Kısmi çıktı: 
```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  --------------
Debian   credativ     7                  credativ:Debian:7:7.0.201602010                  7.0.201602010
Debian   credativ     7                  credativ:Debian:7:7.0.201603020                  7.0.201603020
Debian   credativ     7                  credativ:Debian:7:7.0.201604050                  7.0.201604050
Debian   credativ     7                  credativ:Debian:7:7.0.201604200                  7.0.201604200
Debian   credativ     7                  credativ:Debian:7:7.0.201606280                  7.0.201606280
Debian   credativ     7                  credativ:Debian:7:7.0.201609120                  7.0.201609120
Debian   credativ     7                  credativ:Debian:7:7.0.201611020                  7.0.201611020
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
...
```

Benzer filtreleri ile uygulama `--location`, `--publisher`, ve `--sku` seçenekleri. Kısmi eşleşmeler arama gibi bir filtre bile gerçekleştirebileceğiniz `--offer Deb` tüm Debian görüntüleri bulunamıyor.

Belirli bir konumla belirtmezseniz `--location` seçeneğini değerlerini `westus` varsayılan olarak döndürülür. (Çalıştırarak farklı varsayılan konumunu ayarla `az configure --defaults location=<location>`.)

Örneğin, aşağıdaki komut, tüm Debian 8 SKU listeler `westeurope`:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Kısmi çıktı:

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
...
```

## <a name="navigate-the-images"></a>Görüntüleri gidin 
Görüntüyü bir konumda bulmak için başka bir yolu çalıştırmaktır [az vm görüntü listesi-yayımcılar](/cli/azure/vm/image#list-publishers), [az vm görüntü listesi-teklifleri](/cli/azure/vm/image#list-offers), ve [az vm görüntü listesi-SKU'ları](/cli/azure/vm/image#list-skus) komutları sırayla. Bu komutları ile bu değerler belirler:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.


Örneğin, aşağıdaki komut, Batı ABD konumunda görüntü yayımcılar listeler:

```azurecli
az vm image list-publishers --location westus --output table
```

Kısmi çıktı:

```
Location    Name
----------  ----------------------------------------------------
westus      1e
westus      4psa
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      Acronis
westus      Acronis.Backup
westus      actian_matrix
westus      actifio
westus      activeeon
westus      adatao
...
```
Belirli bir yayımcıdan teklifleri bulmak için bu bilgileri kullanın. Canonical Batı ABD konumunda bir görüntü yayımcı ise, örneğin, kendi teklifleri çalıştırarak bulabileceğiniz `azure vm image list-offers`. Konum ve yayıncıya aşağıdaki örnekteki gibi geçirin:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Çıktı:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
westus      Ubuntu_Snappy_Core
westus      Ubuntu_Snappy_Core_Docker
```
Batı ABD bölgesi Canonical yayımlar gördüğünüz **UbuntuServer** Azure üzerinde sunar. Peki bunların SKU’su ne? Bu değerleri almak için şunu çalıştırın `azure vm image list-skus` ve konumu, yayımcı ve bulunan teklif ayarlayın:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Çıktı:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-DAILY-LTS
westus      12.04.5-LTS
westus      12.10
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-beta
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      16.10
westus      16.10-DAILY
westus      17.04
westus      17.04-DAILY
westus      17.10-DAILY
```

Son olarak, `az vm image list` istediğiniz, örneğin, belirli bir SKU sürümünü bulmak için komutu **16.04 LTS**:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

Çıktı:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611220  16.04.201611220
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611300  16.04.201611300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612050  16.04.201612050
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612140  16.04.201612140
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612210  16.04.201612210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201701130  16.04.201701130
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702020  16.04.201702020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702200  16.04.201702200
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702210  16.04.201702210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702240  16.04.201702240
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703020  16.04.201703020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703030  16.04.201703030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703070  16.04.201703070
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703270  16.04.201703270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703280  16.04.201703280
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703300  16.04.201703300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705080  16.04.201705080
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705160  16.04.201705160
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706100  16.04.201706100
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706191  16.04.201706191
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707210  16.04.201707210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707270  16.04.201707270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708030  16.04.201708030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708110  16.04.201708110
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708151  16.04.201708151
```
## <a name="next-steps"></a>Sonraki adımlar
Artık tam olarak URN değeri not gerçekleştirerek kullanmak istediğiniz görüntü seçebilirsiniz. Bu değer ile geçirmek `--image` bir VM ile oluşturduğunuzda parametresi [az vm oluşturma](/cli/azure/vm#create) komutu. "Son" URN sürüm numarası isteğe bağlı olarak değiştirebilirsiniz unutmayın. Bu her zaman en son sürümünü dağıtım sürümüdür. URN bilgileri kullanarak hızlı bir şekilde bir sanal makine oluşturmak için bkz: [oluşturma ve yönetme Linux VM'ler Azure CLI ile](tutorial-manage-vm.md).
